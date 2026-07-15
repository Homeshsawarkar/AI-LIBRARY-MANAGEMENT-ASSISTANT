# Alexandria — AI-Powered Library Assistant

> An intelligent literary companion powered by **IBM watsonx.ai** and **IBM Granite models**, built with Python Flask.

---

## ✨ Features

| Feature | Description |
|---|---|
| 💬 **Chat Agent** | Full conversational AI with IBM Granite — ask anything literary |
| 🔍 **AI Discovery** | Personalised book recommendations based on your reading profile |
| 📚 **Virtual Bookshelf** | Track reading progress, rate books, add notes |
| 📝 **Study Guide Generator** | AI-generated study guides, analysis, and discussion questions |
| 🌙 **Night Mode** | Smooth warm-cream ↔ obsidian dark theme toggle |
| ⚙️ **Reader Profile** | Preferences (genres, level, intent, format) injected into every AI prompt |

---

## 🚀 Quick Start

### Prerequisites

- Python 3.10+
- [`uv`](https://github.com/astral-sh/uv) (recommended) or `pip`
- An **IBM Cloud** account with a **watsonx.ai** project

---

### 1. Clone the project

```bash
cd library-assistant
```

### 2. Create a virtual environment & install dependencies

**Using `uv` (recommended):**
```bash
uv venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate

uv pip install -r requirements.txt
```

**Using `pip`:**
```bash
python -m venv .venv
source .venv/bin/activate   # or .venv\Scripts\activate on Windows
pip install -r requirements.txt
```

---

### 3. Configure IBM watsonx.ai credentials

Copy the example environment file and fill in your credentials:

```bash
cp .env.example .env
```

Edit `.env`:

```dotenv
IBM_API_KEY=your_ibm_cloud_api_key_here
WATSONX_PROJECT_ID=your_watsonx_project_id_here
WATSONX_URL=https://us-south.ml.cloud.ibm.com
FLASK_SECRET_KEY=your_long_random_secret_here
FLASK_ENV=development
WATSONX_MODEL_ID=ibm/granite-3-3-8b-instruct
```

> **Where to find credentials:**
> - **IBM_API_KEY**: [IBM Cloud → IAM → API Keys](https://cloud.ibm.com/iam/apikeys)
> - **WATSONX_PROJECT_ID**: Open your project in [watsonx.ai](https://dataplatform.cloud.ibm.com/wx/home) → Project Settings → General → Project ID
> - **WATSONX_URL**: Choose your region endpoint (default: `https://us-south.ml.cloud.ibm.com`)

---

### 4. Run the application

```bash
python app.py
```

Open your browser at: **http://127.0.0.1:5000**

---

## 📁 Project Structure

```
library-assistant/
├── app.py                    # Flask backend — routes, watsonx.ai integration
├── .env.example              # Environment variable template (copy to .env)
├── requirements.txt          # Python dependencies
├── README.md                 # This file
│
├── templates/
│   ├── base.html             # Base layout (nav, modals, scripts)
│   ├── index.html            # Dashboard / landing page
│   ├── chat.html             # Conversational chat agent interface
│   ├── bookshelf.html        # Virtual bookshelf & reading tracker
│   ├── discover.html         # AI recommendation discovery feed
│   └── study.html            # Study guide & summary generator
│
└── static/
    ├── css/
    │   └── main.css          # Complete design system (Editorial theme + dark mode)
    └── js/
        ├── app.js            # Global: theme, preferences modal, nav, toast, markdown
        ├── chat.js           # Chat agent — send/receive, history, export, sidebar
        ├── bookshelf.js      # Shelf CRUD, progress tracking, book cards
        ├── discover.js       # Recommendation fetching, parsing, card rendering
        └── study.js          # Study guide & summary generation, recent guides
```

---

## 🧠 Agent Configuration

The AI agent's behaviour is defined in the **`AGENT_INSTRUCTIONS`** block inside [`app.py`](app.py). Customise this to change Alexandria's:

- **Persona and tone** — literary guide, domain expert, tutor, etc.
- **Competencies** — recommendations, summaries, study guides, author research
- **Response format** — structured lists, markdown sections, length
- **Boundaries** — what topics to accept or redirect

```python
# app.py — lines ~75-115
AGENT_INSTRUCTIONS = """
You are Alexandria — an expert AI Library Assistant...
"""
```

---

## 🎨 Design System

**Theme:** "Minimalist Editorial" — Premium literary journal aesthetic

| Token | Light Mode | Dark Mode |
|---|---|---|
| Background | `#FDFBF7` (warm cream) | `#0D0D0F` (deep obsidian) |
| Text | `#1C1C1C` (charcoal) | `#F0EDE8` (off-white) |
| Accent | `#3B82D4` (blue) | `#5B9BD4` (soft blue) |
| Sage (availability) | `#5C7A5E` | `#7A9E7C` |
| Sepia (warmth) | `#8B6F47` | `#B8946A` |

**Fonts:**
- **Headings/Titles**: `Playfair Display` (elegant serif)
- **UI & Body**: `Plus Jakarta Sans` (clean, readable)

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Dashboard |
| `GET` | `/chat` | Chat agent page |
| `GET` | `/discover` | Discovery feed page |
| `GET` | `/bookshelf` | Bookshelf page |
| `GET` | `/study` | Study guide page |
| `POST` | `/api/chat` | Send message → AI response |
| `POST` | `/api/recommendations` | Get book recommendations |
| `POST` | `/api/study-guide` | Generate study guide |
| `POST` | `/api/summary` | Generate book summary |
| `GET/POST` | `/api/preferences` | Get/set reader preferences |
| `GET` | `/api/bookshelf` | Get shelf contents |
| `POST` | `/api/bookshelf/add` | Add book to shelf |
| `PUT` | `/api/bookshelf/update/<id>` | Update book progress/status |
| `DELETE` | `/api/bookshelf/remove/<id>` | Remove book from shelf |
| `GET` | `/api/health` | Health check & config status |

---

## 🛠️ Supported Granite Models

The default model is `ibm/granite-3-3-8b-instruct`. Override via `.env`:

```dotenv
WATSONX_MODEL_ID=ibm/granite-3-3-8b-instruct
# Other options:
# ibm/granite-3-2-8b-instruct
# ibm/granite-13b-chat-v2
# ibm/granite-20b-multilingual
```

---

## 🚢 Production Deployment

```bash
# Set environment
FLASK_ENV=production

# Run with gunicorn (installed via requirements.txt)
gunicorn app:app --workers 4 --bind 0.0.0.0:8000
```

---

## 🔒 Security Notes

- **Never commit your `.env` file** — it's listed in `.gitignore`
- Generate a strong `FLASK_SECRET_KEY`: `python -c "import secrets; print(secrets.token_hex(32))"`
- Session data (bookshelf, preferences) is stored server-side in Flask sessions
- For production, switch to Redis-backed sessions or a database for persistence

---

## 📦 Dependencies

| Package | Purpose |
|---|---|
| `flask` | Web framework |
| `flask-cors` | Cross-origin request handling |
| `ibm-watsonx-ai` | IBM Granite model inference |
| `python-dotenv` | `.env` file loading |
| `pydantic` | Data validation |
| `gunicorn` | Production WSGI server |

---

## 📄 License

MIT License — see `LICENSE` for details.

---

*Built with IBM watsonx.ai · Granite Models · Python Flask*
