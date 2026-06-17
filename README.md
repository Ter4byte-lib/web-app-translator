# 🌐 Web Translator

A clean, responsive translation web app built to sharpen my Python backend skills and practice handling secure API integrations.

**🔗 Live Demo:** https://ter4by2e.eu.pythonanywhere.com/  
**📅 Built:** Summer 2026

---

## 📝 About

I built this translator specifically to focus on what it takes to design a reliable backend that mediates between a user and an external API. 

Instead of forcing a slow full-page browser refresh every time you want to translate something, the app takes your text, triggers an asynchronous background request to a Flask route, and displays the result instantly. The Flask backend acts as a secure intermediary—handling the translation logic server-side so third-party API credentials stay completely hidden and secure from the browser.

<p align="center">
  <img src="assets/home.png" width="900" alt="Web Translator interface">
</p>

---

## 🛠️ Tech Stack

* **Backend:** Flask (Python)
* **Frontend:** HTML5, Bootstrap 5, Vanilla JavaScript
* **Client Communication:** Fetch API (`async`/`await`)
* **Translation API:** RapidAPI Translation Services
* **HTTP Client:** `requests` (For outbound API calls)
* **Configuration:** `python-dotenv` for secure environment management
* **Hosting:** PythonAnywhere

---

## ✨ Features

### 🌐 Auto-Language Detection
By passing `"auto"` as the source language identifier, the backend query handles identifying what language the text was originally written in before translating it, so users don't have to guess.

### ⚡ Async UI Updates
The frontend uses JavaScript Fetch calls to hit my Flask route in the background. The script keeps the UI interactive, changing the output window to read `"Translating..."` or `"Detecting language..."` dynamically so the app feels smooth and responsive while waiting on the network.

<p align="center">
  <img src="assets/translating.png" width="900" alt="Translation in progress">
</p>

### 🎨 Live Theme Toggles
Users can instantly switch between Light, Dark, or System Auto themes. The JavaScript catches the click and updates Bootstrap’s `data-bs-theme` attribute on the fly without breaking the active page state or clearing the text areas.

<p align="center">
  <img src="assets/themes.png" width="900" alt="dark mode interface">
</p>

### ♻️ Reusable Jinja Templates
Instead of duplicating identical dropdown markup for the "Translate From" and "Translate To" boxes, both language select elements pull from a single shared Jinja template (`languages.html`) to keep the frontend dry and easy to maintain.

---

## 🔒 Technical & Security Reflection

### Credential Handling
* **Secrets Management:** Sensitive variables like `RAPID_API_KEY` and `RAPID_API_HOST` are kept completely out of the core logic. They are loaded from a local `.env` file at runtime using Python’s `os.getenv()`.
* **Git Practices:** A strict `.gitignore` file ensures my local virtual environment (`venv/`), secrets file (`.env`), system files (`.DS_Store`), and compiled Python paths (`__pycache__/`) never accidentally get tracked or pushed to public GitHub.
* **Production Deployment:** When deployed on PythonAnywhere, these keys are injected directly through the provider's native environment variables panel instead of relying on hardcoded configuration files.

### Backend API Brokerage
The client browser never communicates directly with RapidAPI. Instead, it hits my local `/translate` POST route. The backend catches the payload, attaches the required private authentication headers, and executes the outbound request. This keeps my API limits protected from users inspecting network requests in their browser developer tools.

### Error Handling
Outbound requests use `response.raise_for_status()` together with a strict `timeout=5` safety block. This catches API failures cleanly and ensures the backend worker thread doesn't hang indefinitely if the third-party service crashes.

### Stateless Request Processing
The app doesn't have a database or active sessions hooked up yet. Every API transaction is completely stateless—the backend receives the JSON string, requests a translation, passes it back, and instantly drops it from memory. There is zero risk of data leaking between separate human browser sessions.

---

## 🧠 What I Learned

* **Designing Secure Brokers:** I practiced decoupling the frontend from external API wrappers. The Flask backend acts as a secure intermediary, which is a fundamental pattern for production-grade web apps.
* **Working with JSON Payloads:** I learned how to use `request.get_json()` on the Flask side to cleanly extract incoming objects from a POST body, map those to parameters, and return a structured payload back using `jsonify()`.
* **Upgrading Network Logic:** I moved beyond simple, blank `try/except` templates by implementing explicit timeouts and status raises, which prevents the application from locking up during upstream network drops.
* **Taming Async UI States:** Writing the async frontend functions gave me a much better grasp on using `async`/`await` and handling UI loading feedback so users don't break execution loops by spamming input fields.

---

## 🚀 Future Improvements

- [ ] Install **Flask-Limiter** on the `/translate` route to guard against automated script loops exhausting my RapidAPI quota.
- [ ] Enforce an explicit character maximum check on the backend to instantly drop oversized or malicious text payloads.
- [ ] Implement a local caching layer or utilize session storage to reduce duplicate API latency and cut down request costs.
- [ ] Expand automated testing around the backend routes to handle edge cases and API failure scenarios gracefully.
