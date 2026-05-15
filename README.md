# ChatGPT Web Gen

A single-file CLI tool that generates images via **ChatGPT Web** using CloakBrowser (stealth Playwright Chromium). No API key needed — just a ChatGPT Plus/Pro account.

```bash
pip install -r requirements.txt
python gen.py --login          # one-time: log into ChatGPT in visible browser
python gen.py "a cute orange cat, digital art"
python gen.py "put this on a beach" --ref product.jpg
```

## How it works

1. Launches a headless Chromium via CloakBrowser (anti-detection)
2. Restores your saved ChatGPT session cookies
3. Opens a fresh conversation
4. Uploads a reference image (optional) and sends your prompt
5. Waits for generation to finish and downloads the result

All in one command — no servers, no daemons, no API setup.

## Setup

### Requirements
- Python 3.10+
- A ChatGPT Plus, Pro, or Team account (image generation requires a paid plan)

### Install

```bash
git clone https://github.com/lunkerchen/chatgpt-web-gen.git
cd chatgpt-web-gen
pip install -r requirements.txt
playwright install chromium    # install browser driver
```

### Login

```bash
python gen.py --login
```

This opens a visible Chrome window to `chat.openai.com`. Log in manually, then press Enter in the terminal. The session cookies are saved to `cookies.json` for headless use.

Cookies last about 1-2 months before they expire. Re-run `--login` when they do.

## Usage

### Text-to-image

```bash
python gen.py "a cinematic shot of a cyberpunk city at night, neon lights"
```

### Image-to-image (with reference)

```bash
python gen.py "redesign this logo in a minimalist style" --ref logo.png
```

### Output

- Success → prints `IMAGE:/path/to/generated.png`
- Failure → prints `ERROR:<reason>` and exits with code 1

Generated images land in `./temp/`.

## Limitations

- **Requires a paid ChatGPT account** (Plus/Pro/Team)
- **Single-threaded** — one generation at a time (ChatGPT Web limitation)
- **Cookie-dependent** — session expires every 1-2 months
- **UI-fragile** — ChatGPT frontend changes may break selectors; update `S` dict in `gen.py` when that happens

## Selector maintenance

When ChatGPT's UI changes, edit the `S` dictionary at the top of `gen.py`:

| Key | DOM selector | Purpose |
|-----|-------------|---------|
| `chat_input` | `#prompt-textarea`, `div[contenteditable="true"]` | Prompt input |
| `send` | `button[data-testid="send-button"]` | Send button |
| `generated` | `img[alt*="已產生"]` | Generated image |
| `streaming` | `button[data-testid="stop-button"]` | Streaming indicator |
| `file_input` | `input[type="file"]` | Image upload |

## Related

- [chatgpt-image-bot](https://github.com/lunkerchen/chatgpt-image-bot) — Telegram Bot version with multi-user queue and admin controls

## License

MIT
