<div align="center">

# Meme Bot

### A small Discord bot I wrote while learning Python.

**Type `$meme` in chat, get a meme back.**

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![discord.py](https://img.shields.io/badge/discord.py-2.x-5865F2?style=flat-square&logo=discord&logoColor=white)](https://discordpy.readthedocs.io/)
[![MIT license](https://img.shields.io/badge/license-MIT-f4c430?style=flat-square)](LICENSE)

[Quick start](#quick-start) · [How it works](#how-it-works) · [FAQ](#faq)

<code>python bot.py</code>

</div>

---

Type `$meme` in any channel the bot can see and it posts a meme into the chat. That's the whole thing.

I made it to figure out how Python talks to the internet, so it stays small on purpose. One file, 24 lines, nothing to set up past your bot token.

## Quick start

### What you need

Python 3.8 or newer, a Discord account, and a server you can add a bot to.

### Get the code

```bash
git clone https://github.com/Dhanushgunji/discord-bot.git
cd discord-bot
pip install -r requirements.txt
```

### Make your bot

1. Open the [Discord Developer Portal](https://discord.com/developers/applications) and click **New Application**.
2. Go to the **Bot** tab and add a bot.
3. Scroll to **Privileged Gateway Intents** and turn on **Message Content Intent**. Your bot cannot read `$meme` without it, so don't skip this one.
4. Hit **Reset Token** and copy the token it gives you.
5. Under **OAuth2 → URL Generator**, tick `bot`, then tick **Send Messages**. Open the link it builds and pick your server.

### Run it

Put your token on the last line of `bot.py`:

```python
client.run('paste your token here')
```

Then start it:

```bash
python bot.py
```

Your terminal prints `Logged on as YourBot#1234!` once it connects. Go to your server and type `$meme`.

> **Keep your token to yourself.** It works like a password, so anyone holding it can run your bot. Don't put it in a commit, a screenshot, or a Discord message. If it does get out, press **Reset Token** and the old one dies right away.

## Commands

| Command | What happens |
| :--- | :--- |
| `$meme` | Posts a meme into the channel you typed it in. |

## How it works

Three small pieces stacked together.

**Getting the meme.** `requests` asks for one, `json` turns the reply into a Python dictionary, and the image link sits under the `url` key.

```python
def get_meme():
  response = requests.get(...)
  json_data = json.loads(response.text)
  return json_data['url']
```

**Hearing the message.** Building on `discord.Client` lets you hook into Discord's events. `on_ready` runs once when the bot connects. `on_message` runs every time anyone says anything the bot can see.

**Not talking to itself.** The first line inside `on_message` is the one that matters:

```python
if message.author == self.user:
  return
```

Without it the bot reads its own replies, answers them, reads those, and never stops. I found that out the fun way.

Everything is `async` because the bot has to keep listening to Discord while it waits on the meme.

## FAQ

<details>
<summary><strong>I typed $meme and nothing happened.</strong></summary>

Message Content Intent is almost always why. Open the **Bot** tab in the Developer Portal, turn it on under **Privileged Gateway Intents**, then restart the bot. Also check the bot can actually see the channel you're typing in.

</details>

<details>
<summary><strong>Can I change the command?</strong></summary>

Yes. Find `message.content.startswith("$meme")` in `bot.py` and put whatever you want in the quotes.

</details>

<details>
<summary><strong>Does it stay online when I close my laptop?</strong></summary>

No. It runs on your machine, so it goes offline with your terminal. Keeping it up means hosting it somewhere.

</details>

<details>
<summary><strong>Can I use this for my own bot?</strong></summary>

Go for it. MIT licensed, short enough to read in a minute. Fork it and break it.

</details>

## License

[MIT](LICENSE)
