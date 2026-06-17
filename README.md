# Language Translator

A lightweight, single-page translation web app built to sharpen my Flask API integration skills and practice client-side asynchronous UI states.

**🔗 Live Demo:** [https://ter4by2e.eu.pythonanywhere.com/]  
**📅 Built:** Summer 2025

---

## What It Does

This project is a clean, interactive translator that takes user text, communicates with a backend translation API asynchronously, and renders the result instantly without a clunky full-page reload. It automatically tracks your character count, supports quick-action sample texts, includes an optional "translate-as-you-type" mode, and comes with a full Bootstrap dark/light theme toggle. 

The main reason I built this was to push my Flask skills forward—specifically figuring out how to handle incoming JSON request bodies on the backend and make clean outbound HTTP requests to external APIs using Python.

---

## Tech Stack

* **Backend Framework:** Flask (Python)
* **Frontend UI:** HTML5, Bootstrap 5 (Styling & Layout), Vanilla JavaScript (Fetch API)
* **Outbound HTTP Client:** `requests` library
* **Configuration Management:** `python-dotenv`

---

## Features

### 🎨 Custom UI Themes
[PLACEHOLDER: Add a screenshot here showing your app with the Dark/Light theme toggle active]
Users can toggle between Light, Dark, or System Auto themes dynamically. The system hooks into Bootstrap’s data attributes to swap colors on the fly without refreshing the page.

### 🌐 Auto-Language Detection
The frontend features a "Detect language" flag. When checked, it lets the backend query string handle identifying what language the text was originally written in before translating it.

### ⚡ Translate-as-you-Type
An option in the settings panel allows real-time asynchronous translating. The app watches the text area and updates the output box dynamically as the user types, rather than forcing them to wait and click an explicit "Translate" button.

---

## What I Learned Building It

This was a massive lesson in linking frontend JavaScript execution lifecycles with standard Python backend routing rules. 

I had to map out how to grab data sent via JS `fetch()` on the Python side using `request.get_json()`, parse out specific keys safely with `.get()`, and ensure that my backend always spat back a proper, stringified JSON payload using Flask's native `jsonify()`. 

On the frontend, writing the asynchronous `translateText` function taught me how crucial UI loading states are. Setting the text box status to `"Translating..."` while waiting for the API to respond keeps the user experience seamless and obvious.

---

## Revisiting It with a Security Lens

### Credential Handling

* **Where do API keys/secrets live in this code?** They live completely outside the repository code. I used `python-dotenv` to read `RAPID_API_KEY`, `RAPID_API_HOST`, and the base `URL` from a local, uncommitted `.env` file. The values are pulled at runtime using Python’s `os.getenv()`.
  
* **If someone cloned this repo, could they accidentally commit secrets?** Yes, they easily could. While the code reads from `os.getenv()`, there isn't a `.gitignore` file included in this directory. If a developer sets up a local `.env` file or generates a `.pyc` Python cache directory, they risk accidentally bundling those private keys into a standard `git commit` and pushing them straight to public GitHub.
  
* **If this deployed to production, how would you inject credentials securely?** Instead of uploading or writing local `.env` files on the host server (like PythonAnywhere or Render), I would inject these values directly via the hosting provider's environment variables dashboard or secrets manager console, completely isolating production environment keys from development.

### Access Rules & Permissions

* **Who can access this app right now?** Right now, anyone on the internet can hit the application routes if it's running live. There are no authentication walls, no login portals, and no API user sessions.
  
* **Should there be rate limiting?** **Yes, desperately.** Because the `/translate` route forwards requests directly to RapidAPI, an attacker could easily target this open endpoint with a script loop. Since it doesn't limit inbound traffic, someone could rack up my API usage costs or trigger an upstream IP block within seconds. I need to drop in a library like `Flask-Limiter` to cap requests per IP.
  
* **If users can create accounts or save state, what access controls exist?** There is no database or storage layer connected. Every single request processed through the `/translate` route is stateless. The backend processes the string, returns it, and instantly forgets it. There is zero risk of data leaking between completely separate human browser sessions.

### Input Validation

* **What user inputs does this app accept?** The application consumes text from the source text area along with the selected values from the language `<select>` dropdown tables.
  
* **What happens if someone sends malicious input?** If a user sends a massive payload (e.g., millions of characters), my outbound `requests.post()` call has a strict 5-second `timeout=5` safety block, which prevents the server from hanging indefinitely. However, passing unvalidated text strings downstream means a corrupted payload could trigger an application crash.
  
* **
