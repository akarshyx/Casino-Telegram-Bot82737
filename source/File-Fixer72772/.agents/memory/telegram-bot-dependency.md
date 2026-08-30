---
name: Telegram bot dependency
description: Preventing the PyPI telegram package from shadowing python-telegram-bot.
---

The project must depend on `python-telegram-bot` only; the unrelated PyPI package named `telegram` shadows the `telegram` namespace and breaks imports such as `InlineKeyboardButton`.

**Why:** Installing both packages can leave a partial namespace that makes the bot fail before startup.

**How to apply:** Check requirements and the import path before launching the bot after dependency changes; remove `telegram` if it appears.