# Grim

A sarcastic, snarky, broody AI chatbot that insults you every time you open it, fires back savage comebacks, and still gives you the correct answer. Think of the smartest friend you have, the one who is also unbearable.

**Live site:** https://bhavyabhagat12-innovator.github.io/grim-bot/

## What it does

- Greets every visitor with a fresh roast (a local fallback roast appears instantly, then gets replaced by an AI-written one).
- Replies to anything with sarcasm, dry jokes and moody one-liners.
- Keeps the roasts on your choices and habits, never on identity or appearance.
- Drops the act and gets genuinely supportive if you sound stressed or sad.
- Remembers the last 12 messages of the conversation.

## How it works

```
Browser (GitHub Pages)  ->  Cloudflare Worker  ->  Groq API (openai/gpt-oss-120b)
     index.html               worker.js            (API key lives here as a secret)
```

GitHub Pages only serves static files, so an API key placed in `index.html` would be public. The Cloudflare Worker keeps the key secret and only accepts requests from this site's origin.

## Project structure

| File | Purpose |
|------|---------|
| `index.html` | The whole frontend: HTML, CSS and JavaScript in one file |
| `worker.js` | Cloudflare Worker code, kept here for reference (it runs on Cloudflare, not GitHub) |

## Tech stack

- HTML, CSS, vanilla JavaScript
- GitHub Pages for hosting
- Cloudflare Workers for the backend proxy
- Groq API running `openai/gpt-oss-120b`

## Run your own copy

1. **Get a Groq API key** at [console.groq.com](https://console.groq.com).
2. **Create a Cloudflare Worker.** Go to Workers & Pages, create a Worker from the Hello World template, and paste in the contents of `worker.js`.
3. **Add the secret.** In the Worker's Settings, under Variables and Secrets, add a secret named `GROQ_API_KEY` with your key.
4. **Set the allowed origin.** In `worker.js`, change `ALLOWED_ORIGIN` to `https://YOUR-USERNAME.github.io` (no trailing slash, no repo path), then deploy.
5. **Connect the frontend.** In `index.html`, set the `API` constant to your Worker URL (`https://your-worker.your-subdomain.workers.dev`).
6. **Deploy the site.** Push to GitHub, then go to Settings, Pages, and choose Deploy from a branch, `main`, `/ (root)`.

## Customize the personality

Edit the `SYSTEM` prompt in `worker.js`. You can make Grim meaner, gentler, more broody, or more joke-heavy. The `MOODS` array adds a random mood to each reply so answers stay varied.

## Troubleshooting

| Problem | Likely cause |
|---------|--------------|
| Only canned roasts appear | The `API` URL in `index.html` is still the placeholder or is wrong |
| CORS error in the console | `ALLOWED_ORIGIN` in the Worker does not match your site exactly |
| `GROQ_API_KEY is missing` | The secret has the wrong name or was not saved and deployed |
| `model does not exist` | Groq retired the model. Check [console.groq.com/docs/models](https://console.groq.com/docs/models) and update the `model` field |
| Old version still showing | Hard refresh with `Ctrl+Shift+R` |

## Ideas for later

- Roast-level slider
- Saved chat history with `localStorage`
- A "roast my resume" mode

## Disclaimer

Grim is a joke. The insults are for fun, aimed at your choices and never at who you are. Verify anything important before relying on it.
