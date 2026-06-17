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
