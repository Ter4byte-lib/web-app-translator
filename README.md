# Language Translator

A simple, lightweight language translator web app built to test out Flask API integrations and explore client-side asynchronous UI states.

**🔗 Live Demo:** TODO: Add your PythonAnywhere or deployment link here
**📅 Built:** Summer 2026

---

## What It Does

The project provides a clean, single-page interface for text translation across multiple languages. Users can input text, let the app automatically detect the source language, choose a target language, and view translations instantly. It also supports text-to-speech audio playback of the translated output, basic sample text quick-actions, and a dynamic dark/light/system theme switcher.

I built this specifically as a learning project to sharpen my Python backend skills, understand how Flask handles asynchronous JSON routing, and practice clean API consumption.

---

## Tech Stack

* **Backend & API Routing:** Flask (Python)
* **Frontend UI:** HTML5, CSS3 (Custom styles with theme toggles), JavaScript (Fetch API)
* **Translation & TTS Engines:** `deep-translator` (Google Translator backend) and `gTTS` (Google Text-to-Speech)
* **Hosting:** PythonAnywhere (WSGI configuration)

---

## Features

* **Auto-Language Detection:** Users don't need to guess the input language; the backend automatically identifies the source text profile.
* **Asynchronous Translating:** Leverages JavaScript `Fetch` calls to hit Flask routes (`/translate`) without forcing a clunky full-page browser refresh.
* **Instant Audio Synthesis:** Synthesizes translated strings into downloadable/playable MP3 playback queues directly on the frontend using standard HTML5 audio elements.
* **Dynamic Client-Side Themes:** Includes a clean UI mode toggle supporting explicit **Light**, **Dark**, or **Auto** (matching the host operating system's prefers-color-scheme).
* **Translate-as-you-Type Toggle:** An optional mode that hooks text input elements to real-time events, firing minor debounced API updates instead of waiting for an explicit click.

---

## What I Learned Building It

This was an excellent exercise in balancing frontend JavaScript lifecycles with standard Python backend rules. 

I learned how Flask interprets explicit POST body structures via `request.get_json()` and how crucial it is to return properly formatted stringified payloads using `jsonify()`. On the frontend, wrestling with async states taught me how to handle UI feedback states gracefully (like disabling inputs and modifying button text to "Translating..." so users don't break the active execution loops by spamming requests).

---

## Revisiting It with a Security Lens

### Credential Handling

* **Where do API keys/secrets live in this code?** Right now, there are actually no explicit API credentials hardcoded in the codebase because it relies entirely on free, unauthenticated scraping wrappers via `deep-translator` and `gTTS`. If I were to switch this to official paid production routes (like Google Cloud Translation API or DeepL API), those keys would need to live securely in environment variables or a `.env` file rather than being committed.
  
* **If someone cloned this repo, could they accidentally commit secrets?** Because no keys are currently utilized, a standard clone won't leak private tokens. However, the repository lacks a `.gitignore` template. If I transition to an authenticated corporate API later, I risk accidentally pushing a `.env` file or Python cache files (`__pycache__/`) directly to GitHub.
  
* **If this deployed to production, how would you inject credentials securely?** Instead of editing files directly on a remote server like PythonAnywhere, I would utilize the environment variable dashboard provided by the host (or read from a secure configuration wrapper initialized at the WSGI entry level).

### Access Rules & Permissions

* **Who can access this app right now?** Anyone on the internet can hit the public routes if it's running live on PythonAnywhere. No user registration walls or login portals exist.
  
* **Should there be rate limiting?** **Yes, absolutely.** Because the application uses public endpoints without auth limits, a simple automated bot script could endlessly loop against the `/translate` route. This would quickly cause Google's endpoints to flag my server IP for scraping abuse, breaking the application for all real human visitors. I need to plug in a tool like `Flask-Limiter` to restrict transactions per IP address.
  
* **If users can create accounts or save state, what access controls exist?** There is no database engine hooked up and no concept of sessions. Every API transaction is strictly stateless—text enters, text leaves, and the server instantly forgets it happened. No data leak between separate human browser sessions is possible.

### Input Validation

* **What user inputs does this app accept?** The app consumes simple text strings through the main input window, string identifiers for the target/source language selectors, and raw interaction triggers for the quick-sample text boxes.
  
* **What happens if someone sends malicious input?** If someone sends a massive chunk of text containing millions of characters, the server might hang or time out trying to process it. Furthermore, sending script tags or raw structural syntax might break downstream string rendering if the frontend elements aren't escaping text data properly.
  
* **Are you sanitizing or validating inputs before passing them to the translation API or database?** The current setup passes raw string data directly to the translation libraries with zero filtration or validation checks. While JavaScript's `.innerText` assignment shields the interface from standard client-side DOM XSS loops, the backend itself is fully exposed to oversized structural payloads.

---

## Next Steps

To make this production-ready from a design and security standpoint, I need to knock out the following steps:

- [ ] Add a comprehensive `.gitignore` file to isolate system cache files (`__pycache__/`, `.DS_Store`) and prepare for eventual secrets file usage.
- [ ] Install and configure **Flask-Limiter** to prevent automated scrapers from spamming the backend API routes and triggering upstream IP bans.
- [ ] Implement robust character length limits and string truncation checks inside the Flask endpoints to safely catch bloated payloads before processing them.
- [ ] Standardize try/except handling blocks to gracefully return clean, structured JSON error codes (`500` status flags) back to the UI instead of risking trace errors leaking into frontend alerts.
