# Bibel-Begleiter

An open-source, multilingual AI Bible study companion that runs as a single static web page.

Ask a question about a verse, a person or a whole book, and the assistant answers with
Bible references. Each reference becomes a 📖 button that shows the **real verse text** from
the bundled Bible files, so the AI never has to "recite" Scripture, which is where language
models tend to make things up.

## Features

- **Chat**: Bible questions answered with clickable references.
- **Today**: a daily verse, a reflection, an exercise, a question to think about and a prayer.
- **Bible**: a full reader for all 66 books.
- **150 Questions**: the most common Bible study questions, grouped by topic and book.
- **7 Questions**: a guided method for studying any passage, with personal notes.
- **6 UI languages** (de, en, es, pt, fr, it), with language detection by IP or browser.
- **Private by design**: chats and notes stay on the user's device (localStorage).
- **Safe API key handling**: the OpenAI key lives only in a Supabase Edge Function secret,
  never in the browser.

## Project structure

```
app/
  index.html        The whole app (no build step, no framework)
  bibles/           Public-domain Bible texts, 66 books each
    de.json         Schlachter 1951
    en.json         King James Version
    fr.json         Louis Segond 1910
supabase/functions/bibel-chat/index.ts
                    Edge Function that proxies requests to OpenAI
```

The Bible files load from `bibles/<lang>.json`. For Spanish, Portuguese or Italian, add a
**public-domain** translation in the same format (for example Reina-Valera 1909 as `es.json`).
If a file is missing, the app still works; only the reader for that language is unavailable.

## Setup

1. **Backend (Supabase, free tier)**
   - Create a project at https://supabase.com.
   - Deploy `supabase/functions/bibel-chat/index.ts` as an Edge Function named `bibel-chat`.
   - Add the secrets `OPENAI_API_KEY` (required) and `OPENAI_MODEL` (optional, default `gpt-4o-mini`).
   - Set a monthly spending limit in your OpenAI account.
2. **Connect the app**: in `app/index.html`, fill in the `CONFIG` block:
   ```js
   API_URL: "https://YOUR-PROJECT.supabase.co/functions/v1/bibel-chat",
   API_KEY: "your Supabase anon public key",
   ```
3. **Deploy**: drag the `app` folder onto https://app.netlify.com/drop, or use any static host.
   Opening `index.html` straight from disk won't work, because browsers block local `fetch`.

Deep links: `#today`, `#bible`, `#faq`, `#study`. Force a language with `?lang=en`.

## License

The code is released under the [MIT License](LICENSE).
The bundled Bible translations (Schlachter 1951, KJV, Louis Segond 1910) are in the public domain.

Contributions are welcome: open an issue or a pull request.
