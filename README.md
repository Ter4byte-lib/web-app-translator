# 🌐 Web Translator

> A clean, responsive translation utility built to practice Python backend architecture and asynchronous API brokerage.

**🔗 Live Demo:** [https://your-app-link.com](https://your-app-link.com)

---

## 📝 About
This project was built to polish my Python skills, specifically focusing on building a reliable backend that mediates between a user and an external API. The app uses **Flask** to manage requests, handling the translation logic server-side to ensure API credentials remain hidden and secure.

## 🛠️ Tech Stack
* **Backend:** `Flask` (Python)
* **Frontend:** `HTML5`, `CSS3`, `JavaScript` (using `Fetch API`)
* **API Integration:** `RapidAPI` Translation Services
* **Configuration:** `python-dotenv` for secure environment management

## 🧠 What I Learned (Python Polish)
* **Backend "Brokerage":** I practiced decoupling the frontend from the API. The client never sees the API keys; the Flask backend acts as a secure broker, which is a fundamental pattern for production-ready apps.
* **Robust Error Handling:** I moved beyond simple `try/except` blocks, implementing `response.raise_for_status()` and timeout configurations to ensure the app doesn't hang if the third-party service fails.
* **Efficient Payload Handling:** I practiced working with JSON serialization and deserialization, ensuring the server-side routes correctly ingest request data and format outgoing API payloads.
* **Clean Environment Management:** I implemented best practices by using `.env` files to store sensitive variables, ensuring the logic stays strictly separated from configuration.

## ✨ Key Features
* **Async UX:** The frontend uses `async/await` to keep the UI interactive (e.g., showing "Detecting language...") while waiting for server responses.
* **Auto-Detect Logic:** A server-side integration that intelligently routes source-text detection to the translation engine.
* **Clean Routing:** Used Flask’s request methods to create a modular structure that separates template rendering from API logic.

---

## 🚀 Future Polish
* [ ] **Cache Layer:** Implement local caching for frequent translations to reduce API latency and cost.
* [ ] **History Logs:** Store previous translations in a simple local file or session storage.
* [ ] **Validation:** Add stricter server-side length constraints to prevent excessive API load.


# 🌐 Web Translator

A clean, responsive translation utility built to practice Python backend architecture and asynchronous API brokerage.

**🔗 Live Demo:** https://ter4by2e.eu.pythonanywhere.com/  
**📅 Built:** Summer 2026

---

## 📝 About

This project was built to polish my Python skills, specifically focusing on building a reliable backend that mediates between a user and an external API. Instead of forcing full-page browser refreshes, the app uses Flask to handle asynchronous routing server-side, ensuring third-party API credentials remain entirely hidden and secure.

---

## 🛠️ Tech Stack

* **Backend:** Flask (Python)
* **Frontend:** HTML5, Bootstrap 5, Vanilla JavaScript (Fetch API)
* **API Integration:** RapidAPI Translation Services (`requests`)
* **Configuration:** `python-dotenv` for secure environment management

---

## 🧠 What I Learned (Python Polish)

* **Backend "Brokerage":** I practiced decoupling the frontend from the external API wrapper. The client environment never exposes the sensitive `RAPID_API_KEY` or custom header blocks; the Flask backend acts as a secure broker, which is a fundamental pattern for production-grade web apps.
* **Robust Error Handling:** I moved beyond simple, generic `try/except` templates by implementing `response.raise_for_status()` and strict `timeout=5` parameter rules to make sure the core application worker thread doesn't hang indefinitely if the upstream RapidAPI route fails.
* **Efficient Payload Handling:** I practiced working with JSON serialization and deserialization, using `request.get_json()` to correctly ingest data payloads on the server and `jsonify()` to pass structured translations cleanly back to the client interface.
* **Clean Environment Management:** I implemented standard environment configurations by using `os.getenv()` paired with a local `.env` file to ensure the application logic stays strictly separated from private runtime values.

---

## ✨ Key Features

### 🎨 Theme Toggles
Swaps between Light, Dark, or System Auto themes instantly. The JavaScript catches theme dropdown selections and updates Bootstrap’s `data-bs-theme` attribute on the fly without breaking UI state.

### ⚡ Async UX
The frontend uses `async/await` and the Fetch API to keep the UI interactive. The script updates the translation panel to display `"Translating..."` (or `"Detecting language..."`) dynamically while waiting for background API tasks to resolve.

### 🌐 Auto-Detect Logic
A server-side implementation that routes an `"auto"` source-language parameter directly to the translation engine, letting the external API handle identifying what language the text was originally written in.

---

## 🔒 Security & Technical Reflection

### Credential Handling
Sensitive variables like `RAPID_API_KEY`, `RAPID_API_HOST`, and the base endpoint `URL` are kept completely safe out of source control. A strict local `.gitignore` rule is configured to permanently ignore `.env`, virtual environment folders (`venv/`), system artifacts (`.DS_Store`), and compiled Python paths (`__pycache__/`). When migrating this to production on PythonAnywhere, these keys are securely injected directly via the provider's native environment setup rather than storing hardcoded text configurations.

### Access Control & Validation
The application functions as a completely stateless translation pipe—it ingests text via a POST request to `/translate`, requests a transformation from the API broker, and immediately drops the data from server memory once resolved. While standard client-side text assignments protect the frontend against basic DOM XSS loops, the backend endpoints are currently unauthenticated and open. 

---

## 🚀 Future Polish

- [ ] **Rate Limiting:** Implement a package like `Flask-Limiter` on the `/translate` route to block automated scraper bots from spamming the broker and exhausting my RapidAPI quota.
- [ ] **Server-Side Constraints:** Enforce strict character-length constraints on the backend payload validation to instantly reject oversized or malicious text inputs before executing outbound network calls.
- [ ] **Cache Layer:** Introduce local translation caching or simple session storage logs to reduce duplicate API latency and cut down request costs.
