# NutritionAgent_IBM-final-project

# 🥗 NutriAI Agent

**AI-powered Nutrition Agent powered by IBM Watsonx.ai (Granite LLM)**

A full-stack web application providing personalized nutrition planning, calorie analysis, Indian superfoods guidance, family diet recommendations, and a conversational AI interface — all powered by IBM Granite language models.

---

## 🌟 Features

| Feature | Description |
|---------|-------------|
| 💬 **AI Chat** | Conversational nutrition assistant (IBM Granite) |
| 📊 **Nutrition Dashboard** | Daily calorie, macro targets, meal analyzer |
| 🗓️ **Meal Planner** | 7-day personalized Indian meal plans |
| ⚖️ **BMI Calculator** | BMI, TDEE, macro targets (South Asian adjusted) |
| 👨‍👩‍👧 **Family Profiles** | Per-member nutrition plans for all ages |
| 🌿 **Indian Superfoods** | AI guide to turmeric, moringa, amla, and more |
| 🌙 **Dark Mode** | Full dark/light theme toggle |
| 📱 **Mobile Responsive** | Works on all screen sizes |

---

## 📁 Project Structure

```
NutriAI/
├── app.py                  # Flask backend + Watsonx.ai integration
├── agent_instructions.py   # ⭐ CUSTOMIZE AGENT BEHAVIOR HERE
├── requirements.txt        # Python dependencies
├── .env.example            # Environment variables template
├── .env                    # Your actual credentials (git-ignored)
├── templates/
│   └── index.html          # Full SPA frontend
├── static/
│   ├── css/style.css       # Custom styles + dark mode
│   └── js/app.js           # Frontend JavaScript
└── README.md
```

---

## 🚀 Quick Start

### 1. Clone / Open the project
```bash
cd "Nutrition agent"
```

### 2. Create a virtual environment
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Set up IBM Watsonx.ai credentials
Copy `.env.example` to `.env` and fill in your credentials:

```bash
copy .env.example .env
```

Edit `.env`:
```env
IBM_API_KEY=your_ibm_cloud_api_key
IBM_PROJECT_ID=your_watsonx_project_id
IBM_REGION=us-south
FLASK_SECRET_KEY=change-this-to-a-random-secret
```

### 5. Run the application
```bash
python app.py
```

Visit **http://localhost:5000** in your browser.

---

## 🔑 Getting IBM Watsonx Credentials

1. Go to [https://cloud.ibm.com](https://cloud.ibm.com) and sign up / log in
2. Search for **"Watson Machine Learning"** and create an instance
3. Go to **Watsonx.ai** → [https://dataplatform.cloud.ibm.com](https://dataplatform.cloud.ibm.com)
4. Create a **Project** and note the **Project ID**
5. Generate an **API Key** from IBM Cloud → IAM → API Keys

### Required Watsonx Permissions
- Watson Machine Learning (Reader or Editor)
- Access to `ibm/granite-3-3-8b-instruct` model

---

## 🎛️ Customizing Agent Behavior

All agent customization lives in **`agent_instructions.py`**:

```python
# Change the tone
AGENT_TONE = "friendly"        # professional | friendly | casual | motivational

# Change diet specialization
AGENT_DIET_PHILOSOPHY = "balanced"  # balanced | keto | ayurvedic | mediterranean

# Enable/disable Indian food focus
AGENT_INDIAN_FOOD_ENABLED = True

# Add your own superfoods
AGENT_INDIAN_SUPERFOODS = ["Turmeric", "Amla", ...]

# Edit the system prompt function for deep customization
def get_system_prompt(user_profile=None):
    ...
```

### Key Customization Points

| Setting | File Location | Purpose |
|---------|--------------|---------|
| `AGENT_TONE` | `agent_instructions.py` | Communication style |
| `AGENT_DIET_PHILOSOPHY` | `agent_instructions.py` | Primary diet approach |
| `AGENT_PRIMARY_MODEL` | `agent_instructions.py` | Granite model to use |
| `AGENT_TEMPERATURE` | `agent_instructions.py` | Response creativity (0–1) |
| `AGENT_MEDICAL_DISCLAIMER` | `agent_instructions.py` | Safety disclaimer text |
| `get_system_prompt()` | `agent_instructions.py` | Core AI instructions |

---

## 🌐 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET`  | `/` | Main application UI |
| `POST` | `/api/chat` | Send chat message |
| `POST` | `/api/meal-plan` | Generate meal plan |
| `POST` | `/api/analyze-meal` | Analyze meal nutrition |
| `POST` | `/api/bmi` | Calculate BMI & TDEE |
| `POST` | `/api/family-plan` | Generate family nutrition plan |
| `GET`  | `/api/superfoods` | Indian superfoods guide |
| `POST` | `/api/clear-chat` | Clear chat session |
| `GET`  | `/api/health` | App health check |

### Example Chat API Call
```bash
curl -X POST http://localhost:5000/api/chat \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Create a 7-day vegetarian meal plan for weight loss",
    "profile": {
      "name": "Priya",
      "age": 28,
      "weight": 68,
      "height": 162,
      "diet_type": "vegetarian",
      "goal": "weight loss"
    }
  }'
```

---

## 🔒 Demo Mode (No Credentials)

If no IBM credentials are configured, the app runs in **demo mode** with rich rule-based responses. This allows full testing of the UI without requiring an IBM account.

Set `IBM_API_KEY=your_ibm_cloud_api_key_here` in `.env` to trigger demo mode.

---

## ☁️ Deployment

### Option 1: IBM Cloud Code Engine
```bash
# Build container
docker build -t nutriai-agent .

# Push to IBM Container Registry
ibmcloud cr namespace-add nutriai
ibmcloud cr push us.icr.io/nutriai/agent:latest

# Deploy to Code Engine
ibmcloud ce application create --name nutriai \
  --image us.icr.io/nutriai/agent:latest \
  --env IBM_API_KEY=xxx \
  --env IBM_PROJECT_ID=xxx
```

### Option 2: Gunicorn (Production)
```bash
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:8080 app:app
```

### Option 3: Railway / Render / Heroku
1. Create a `Procfile`:
   ```
   web: gunicorn app:app
   ```
2. Set environment variables in the platform dashboard
3. Deploy from Git repository

---

## 🧪 Testing Without IBM Credentials

The app includes a comprehensive demo mode:
- Chat responses cover meal plans, calories, diabetes diet, family nutrition, etc.
- All UI features work completely
- BMI calculator uses local Python logic (no Watsonx needed)

---

## 📦 Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `flask` | 3.0.3 | Web framework |
| `flask-cors` | 4.0.1 | Cross-origin support |
| `python-dotenv` | 1.0.1 | .env file loading |
| `ibm-watsonx-ai` | 1.1.2 | IBM Granite LLM |
| `requests` | 2.32.3 | HTTP client |
| `gunicorn` | 22.0.0 | Production server |

---

## ⚕️ Medical Disclaimer

NutriAI is an **AI-powered educational tool** and is NOT a substitute for professional medical advice. Always consult a licensed healthcare provider, registered dietitian, or physician before making significant dietary changes — especially if you have medical conditions, are pregnant, or are on medication.

---

## 🤝 Built With

- **IBM Watsonx.ai** — Granite-3.3 language model
- **Flask** — Python web framework  
- **Bootstrap 5** — UI components
- **Bootstrap Icons** — Icon library
- **Python** — Backend logic

---

*Made with ❤️ for healthy living · Powered by IBM Granite AI*
