---
name: Animated dice concurrency
description: Concurrency and state-machine constraints for Telegram animated-dice games.
---

Telegram can deliver several animated-dice updates while bot animations, payout work, or callback-generated rolls are still awaiting network operations. Each game's roll stream must be serialized and guarded by explicit bot/player state flags; a wall-clock delay is not a substitute for sequencing.

**Why:** A time-based throttle rejected valid rapid rolls, while concurrent handlers could increment shared round counters out of order or leave an auto-roll flag stuck after an exception. Callback-generated synthetic dice also need a guard separate from the normal player-processing flag so the synthetic roll does not reject itself.

**How to apply:** Route all manual and synthetic rolls through the per-game serialization boundary, accept configured rapid rolls in order, silently ignore extra/out-of-turn dice, and make every bot-roll/error path clear its processing flags or restart the round safely.