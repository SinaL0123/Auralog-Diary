# AI Diary System 📔
<div align="center">

###  [**Try Live Demo at auralogdiary.com**](https://auralogdiary.com) 

*Transform your voice into beautiful diary entries with AI*

[![Deploy to DigitalOcean](https://github.com/swe-students-fall2025/5-final-finally/actions/workflows/deploy.yml/badge.svg)](https://github.com/swe-students-fall2025/5-final-finally/actions/workflows/deploy.yml)
[![AI Service CI](https://github.com/swe-students-fall2025/5-final-finally/actions/workflows/ai-service-ci.yml/badge.svg)](https://github.com/swe-students-fall2025/5-final-finally/actions/workflows/ai-service-ci.yml)
[![AI Service Release](https://github.com/swe-students-fall2025/5-final-finally/actions/workflows/ai-service-release.yml/badge.svg)](https://github.com/swe-students-fall2025/5-final-finally/actions/workflows/ai-service-release.yml)
[![Web App CI](https://github.com/swe-students-fall2025/5-final-finally/actions/workflows/web-app-ci.yml/badge.svg)](https://github.com/swe-students-fall2025/5-final-finally/actions/workflows/web-app-ci.yml)
[![Web App Release](https://github.com/swe-students-fall2025/5-final-finally/actions/workflows/web-app-release.yml/badge.svg)](https://github.com/swe-students-fall2025/5-final-finally/actions/workflows/web-app-release.yml)

</div>

An AI-powered intelligent diary system that transforms your voice into beautifully written diary entries. Talk naturally to your AI companion, and watch as it creates personalized, reflective diary entries that capture your thoughts, feelings, and experiences.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Team Members](#-team-members)
- [Docker Images](#-docker-images)
- [Features](#-features)
- [System Architecture](#-system-architecture)
- [Technology Stack](#-technology-stack)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Configuration](#configuration)
  - [Running the Application](#running-the-application)
- [Development](#-development)
- [Testing](#-testing)
- [CI/CD Pipeline](#-cicd-pipeline)
- [API Documentation](#-api-documentation)
- [Database Schema](#-database-schema)
- [Contributing](#-contributing)

---

## 🌟 Overview

**AI Diary System** is a full-stack web application that revolutionizes the traditional diary-writing experience. Instead of typing, users can simply speak to an AI companion that listens, responds warmly, and automatically generates well-structured diary entries with emotion analysis and personalized styling.

### What Makes It Special?

- 🎙️ **Voice-First Interface**: Record your thoughts naturally through your browser
- 🤖 **Intelligent Conversation**: Powered by Google Gemini, the AI asks thoughtful follow-up questions
- 📝 **Automatic Diary Generation**: Converts conversations into first-person diary entries
- 😊 **Emotion Analysis**: Tracks your mood with sentiment analysis (mood & mood_score)
- 🎨 **Personalized Styles**: Choose themes and writing styles (reflective, humorous, poetic, etc.)
- 🔐 **Privacy First**: Complete user isolation with secure authentication

---

## 👥 Team Members

- [**Harrison Gao**](https://github.com/HTK-G)
- [**Ivan Wang**](https://github.com/Ivan-Wang-tech)
- [**Junhao Chen**](https://github.com/JunHaoChen16)
- [**Sina Liu**](https://github.com/SinaL0123)
- [**Sophia Fu**](https://github.com/Sophiaaa430)

---

## 🐳 Docker Images

Our containerized services are available on Docker Hub under the **sophiafujy** account:

> 💡 **Note for Users**: These images are pre-built and hosted by our team. You can pull and use them directly **without needing your own Docker Hub account**. The username `sophiafujy` is our team's Docker Hub account - you don't need to change it unless you're forking the project and publishing your own images.

### Production Images
- **Web Application**: [`sophiafujy/web-app:latest`](https://hub.docker.com/r/sophiafujy/web-app)
  - Flask-based web application serving the frontend and API
  - Handles user authentication and diary management
  
- **AI Service**: [`sophiafujy/ai-service:latest`](https://hub.docker.com/r/sophiafujy/ai-service)
  - FastAPI service for speech-to-text and AI conversation
  - Powered by Faster-Whisper and Google Gemini

### Pull Commands
```bash
# Pull web application image
docker pull sophiafujy/web-app:latest

# Pull AI service image
docker pull sophiafujy/ai-service:latest
```

### Usage Guide

**For End Users (Most Common)** ✅
- Just want to run the application? **Keep `sophiafujy` as-is**
- Our images are public and ready to use
- Simply set your `GEMINI_API_KEY` and you're good to go!

**For Developers/Contributors** 🛠️
- Want to build and push your own images?
- Create your own Docker Hub account
- Update `DOCKER_USERNAME` in your `.env` file
- Configure your GitHub Secrets accordingly

---

## ✨ Features

### Core Functionality
- 🎤 **Real-time Voice Recording**: Browser-based audio capture and processing
- 💬 **AI Conversation**: Natural, warm responses powered by Google Gemini Flash
- 📖 **Automatic Diary Generation**: Transform conversations into structured diary entries
- 📊 **Emotion Analysis**: Automatic mood detection and scoring
- 🔍 **Diary Search & Browsing**: View all your entries with date-based navigation
- 📅 **Calendar View**: Visual timeline of your diary entries

### Advanced Features
- 🎨 **Custom Diary Styles**: Choose from reflective, humorous, poetic, or professional tones
- 🎯 **Theme Focusing**: Emphasize specific aspects (work, travel, relationships, etc.)
- 🗣️ **Voice Input for Preferences**: Speak your preferences instead of typing

---

### Data Flow: Voice Message → Diary Entry

```
1. User Records Audio
   ↓
2. Browser → Web-App: POST /api/conversations/<cid>/audio
   ↓
3. Web-App → AI-Service: POST /api/chat/audio
   ↓
4. AI-Service:
   • Faster-Whisper transcribes audio → text
   • Fetches conversation history from ai_diary
   • Gemini generates warm reply
   • Saves context to ai_diary.conversations
   ↓
5. AI-Service → Web-App: { reply, transcript }
   ↓
6. Web-App saves to diary_db.conversations
   ↓
7. User clicks "Complete" → Generate Diary
   ↓
8. Web-App → AI-Service: POST /api/generate-diary
   ↓
9. Gemini analyzes conversation → structured diary
   ↓
10. Web-App saves to diary_db.diaries
```

---

##  Technology Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | HTML5, CSS3, JavaScript (Vanilla), Web Audio API |
| **Web Framework** | Flask (Python 3.10) |
| **AI Service** | FastAPI (Python 3.10) |
| **LLM** | Google Gemini 2.5 Flash Lite |
| **Speech-to-Text** | Faster-Whisper (OpenAI Whisper, tiny model) |
| **Database** | MongoDB 6 |
| **Containerization** | Docker, Docker Compose |
| **CI/CD** | GitHub Actions |
| **Deployment** | DigitalOcean Droplet |
| **Testing** | Pytest, Coverage.py |
| **Package Management** | Pipenv |

---

##  Getting Started

### Prerequisites

Before you begin, ensure you have the following installed on your system:

- **Docker** (v20.10 or higher) - [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose** (v2.0 or higher) - [Install Docker Compose](https://docs.docker.com/compose/install/)
- **Git** - [Install Git](https://git-scm.com/downloads)

**Optional for development:**
- Python 3.10+
- Pipenv

### Installation

#### Step 1: Clone the Repository

```bash
# Clone the repository
git clone https://github.com/swe-students-fall2025/5-final-finally.git

# Navigate to project directory
cd 5-final-finally
```

#### Step 2: Configure Environment Variables

The project requires a Google Gemini API key for AI functionality.

**Create the AI Service environment file:**

```bash
# Copy the example file
cp ai-service/.env.example ai-service/.env

# Edit the file with your API key
nano ai-service/.env  # or use your preferred editor
```

**Update `ai-service/.env` with your credentials:**
GEMINI_API_KEY=your_gemini_api_key_here

#### Step 3: Verify Configuration Files

Ensure the following files exist:

- `ai-service/.env` - Contains your Gemini API key
- `docker-compose.yaml` - Orchestrates local development services

---

### Configuration

#### Environment Variables Explained

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `GEMINI_API_KEY` | Google Gemini API key for LLM | Yes | - |
| `MONGO_URI` | MongoDB connection string | No | `mongodb://mongo:27017` |
| `MONGO_URL` | Alternative MongoDB URI format | No | `mongodb://mongo:27017/` |
| `AI_SERVICE_URL` | URL of the AI service | No | `http://ai-service:8000` |
| `DOCKER_USERNAME` | Docker Hub username (for production) | No | `sophiafujy` |

#### Getting a Gemini API Key

1. Visit [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Sign in with your Google account
3. Click "Create API Key"
4. Copy the key and paste it into `ai-service/.env`

---

### Running the Application

#### Option 1: Quick Start (Recommended)

Run all services with a single command:

```bash
docker-compose up --build
```

This will:
- Build Docker images for web-app and ai-service
- Pull MongoDB 6 image
- Start all three services
- Create a persistent volume for MongoDB data

#### Option 2: Pull Pre-built Images

If you prefer to use our pre-built images from Docker Hub:

```bash
# Pull images from Docker Hub (sophiafujy is our team's account)
docker pull sophiafujy/web-app:latest
docker pull sophiafujy/ai-service:latest

# Run with docker-compose
docker-compose -f docker-compose.prod.yaml up
```

> **Note**: For production deployment, you'll need to create a `.env` file in the root directory:
> ```bash
> cp .env.example .env
> # Edit .env to add your GEMINI_API_KEY
> # DOCKER_USERNAME is already set to 'sophiafujy' - no need to change it!
> ```

---

### Accessing the Application

Once the services are running:

- **Web Application**: http://localhost:5001
- **AI Service API**: http://localhost:8000
- **AI Service Docs**: http://localhost:8000/docs (FastAPI auto-generated)
- **MongoDB**: localhost:27017 (not exposed in production)

#### First Time Setup

1. Navigate to http://localhost:5001
2. You'll be redirected to the login page
3. Enter a username and password to create a new account
4. Start your first conversation!

---

### Stopping the Application

```bash
# Stop all services (Ctrl+C in the terminal running docker-compose)
# Or in detached mode:
docker-compose down

# Stop and remove all data (⚠️ deletes all conversations and diaries)
docker-compose down -v
```

---

## 💻 Development

### Local Development Setup

For active development without Docker:

#### 1. Set Up Web-App

```bash
cd web-app

# Install dependencies
pipenv install --dev

# Run development server
pipenv run python app.py
# Server runs on http://localhost:5000
```

#### 2. Set Up AI-Service

```bash
cd ai-service

# Create .env file with your API key
cp .env.example .env
nano .env  # Add your GEMINI_API_KEY

# Install dependencies
pipenv install --dev

# Run development server
pipenv run uvicorn app.main:app --reload --host 0.0.0.0 --port 8001
# API docs available at http://localhost:8001/docs
```

#### 3. Set Up MongoDB

```bash
# Run MongoDB in Docker (recommended)
docker run -d -p 27017:27017 --name mongo-dev mongo:6

# Or install MongoDB locally
# https://www.mongodb.com/docs/manual/installation/
```

### Development Workflow

```bash
# Create a feature branch
git checkout -b feature/your-feature-name

# Make changes and test locally
docker-compose up --build

# Run tests
cd web-app && pipenv run pytest
cd ai-service && pipenv run pytest

# Commit and push
git add .
git commit -m "feat: your feature description"
git push origin feature/your-feature-name

# Create pull request on GitHub
```

---

## 🧪 Testing

Our project maintains **≥80% test coverage** enforced by CI/CD.

### Running Tests

#### Web-App Tests

```bash
cd web-app

# Run all tests
pipenv run pytest

# Run with coverage report
pipenv run pytest --cov=app --cov-report=term-missing

# Run with coverage requirement (CI mode)
pipenv run pytest --cov=app --cov-report=term-missing --cov-fail-under=80
```

#### AI-Service Tests

```bash
cd ai-service

# Run all tests
pipenv run pytest

# Run with coverage
pipenv run pytest --cov=app --cov-report=term-missing --cov-fail-under=80
```

### Test Structure

```
web-app/tests/
├── conftest.py           # Pytest fixtures (client, fake_db, login_user)
└── test_app.py           # 650+ lines of comprehensive tests
    ├── Unit tests (analyze_mood_and_summary)
    ├── Auth tests (login, logout, session)
    ├── Conversation tests (CRUD, audio handling)
    ├── Diary tests (generation, retrieval)
    └── API integration tests

ai-service/app/tests/
├── test_main.py          # FastAPI endpoint tests
├── test_db.py            # Database operations
├── test_geminiclient.py  # Gemini client mocking
└── test_sttservice.py    # Speech-to-text service
```

### Writing Tests

Example test with mocking:

```python
import pytest
from unittest.mock import patch

def test_add_audio_message(client, fake_db, login_user):
    """Test audio message processing with AI service mock"""
    user_id = login_user()
    
    # Create a conversation
    conv_id = create_test_conversation(client, fake_db, user_id)
    
    # Mock AI service response
    with patch("requests.post") as mock_post:
        mock_post.return_value.status_code = 200
        mock_post.return_value.json.return_value = {
            "reply": "That sounds great!",
            "history": [{"role": "user", "text": "I went for a walk"}]
        }
        
        # Send audio file
        audio_data = BytesIO(b"fake audio content")
        response = client.post(
            f"/api/conversations/{conv_id}/audio",
            data={"audio": (audio_data, "test.wav")}
        )
    
    assert response.status_code == 200
    data = response.json()
    assert "ai_response" in data
```

---

## 🔄 CI/CD Pipeline

Our project uses **GitHub Actions** for automated testing, building, and deployment.

### Workflows

#### 1. **Continuous Integration (CI)** - Tests Only

**Triggers**: Push or PR to service directories

**Web-App CI** (`.github/workflows/web-app-ci.yml`):
```yaml
- Runs on: push to web-app/**, pull_request to web-app/**
- Steps:
  1. Checkout code
  2. Setup Python 3.10
  3. Install pipenv and dependencies
  4. Run pytest with ≥80% coverage requirement
```

**AI-Service CI** (`.github/workflows/ai-service-ci.yml`):
```yaml
- Runs on: push to ai-service/**, pull_request to ai-service/**
- Steps: Same as web-app-ci.yml
```

#### 2. **Continuous Delivery (CD)** - Build & Push Images

**Triggers**: PR merged to `main` branch

**Web-App Release** (`.github/workflows/web-app-release.yml`):
```yaml
- Runs on: pull_request closed and merged to main
- Steps:
  1. Checkout repository
  2. Login to Docker Hub
  3. Build Docker image
  4. Push to sophiafujy/web-app:latest
```

**AI-Service Release** (`.github/workflows/ai-service-release.yml`):
```yaml
- Runs on: pull_request closed and merged to main
- Steps: Same as web-app-release.yml
- Pushes to: sophiafujy/ai-service:latest
```

#### 3. **Continuous Deployment (CD)** - Deploy to Production

**Deploy to DigitalOcean** (`.github/workflows/deploy.yml`):
```yaml
- Runs on: push to main branch
- Steps:
  1. Checkout code
  2. Copy docker-compose.prod.yaml to server via SCP
  3. SSH into DigitalOcean Droplet
  4. Create .env with secrets
  5. Login to Docker Hub
  6. Pull latest images
  7. Restart services with docker-compose up -d
```
---

## 📚 API Documentation

### Web-App Endpoints (Flask)

#### Authentication
```
POST   /login              # User login/registration
GET    /logout             # User logout
```

#### Conversations
```
POST   /api/conversations                   # Create new conversation
GET    /api/conversations                   # Get all user conversations
POST   /api/conversations/<cid>/messages    # Add text message
POST   /api/conversations/<cid>/audio       # Add audio message
POST   /api/conversations/<cid>/complete    # Complete conversation & generate diary
```

#### Diaries
```
GET    /api/diaries                         # Get all user diaries
GET    /api/diaries/<did>                   # Get specific diary by ID
```

#### Transcription
```
POST   /api/transcribe                      # Transcribe audio (no chat)
```

### AI-Service Endpoints (FastAPI)

Full interactive documentation available at `/docs` when running.

#### Health Check
```
GET    /health
Response: {"status": "ok"}
```

#### Chat Endpoints
```
POST   /api/chat
Body:  {"user_id": "string", "text": "string"}
Response: {"reply": "string", "history": [...]}

POST   /api/chat/audio
Query: ?user_id=string
Body:  multipart/form-data with "file" field
Response: {"reply": "string", "history": [...]}
```

#### Transcription
```
POST   /api/transcribe
Body:  multipart/form-data with "file" field
Response: {"text": "string"}
```

#### Diary Generation
```
POST   /api/generate-diary
Body:  {
  "messages": [{"role": "user", "text": "..."}, ...],
  "preferences": {
    "theme": "daily life",
    "style": "reflective",
    "custom_instructions": "Focus on..."
  }
}
Response: {
  "title": "string",
  "content": "string",
  "summary": "string",
  "mood": "positive|negative|neutral",
  "mood_score": -5 to 5
}
```

---

## 🗄️ Database Schema

### Database 1: `diary_db` (User-Facing Data)

#### Collection: `users`
```javascript
{
  "_id": ObjectId("..."),
  "username": "alice",
  "password": "hashed_password",  // Plain text in development, should use bcrypt
  "created_at": ISODate("2024-12-09T00:00:00Z")
}
```

#### Collection: `conversations`
```javascript
{
  "_id": ObjectId("..."),
  "user_id": ObjectId("..."),      // Reference to users._id
  "date": "2024-12-09",
  "messages": [
    {
      "role": "user",
      "text": "I went for a walk today",
      "timestamp": ISODate("...")
    },
    {
      "role": "ai",
      "text": "That sounds lovely! How was the weather?",
      "timestamp": ISODate("...")
    }
  ],
  "created_at": ISODate("..."),
  "completed": false               // true when diary is generated
}
```

#### Collection: `diaries`
```javascript
{
  "_id": ObjectId("..."),
  "user_id": ObjectId("..."),      // Reference to users._id
  "conversation_id": ObjectId("..."), // Reference to conversations._id
  "date": "2024-12-09",
  "time": "14:30",
  "title": "A Peaceful Walk in the Park",
  "content": "Today I took a long walk in the park...",
  "summary": "Enjoyed a peaceful walk in nature",
  "mood": "positive",              // "positive" | "negative" | "neutral"
  "mood_score": 3,                 // -5 to 5
  "preferences": {
    "theme": "daily life",
    "style": "reflective",
    "custom_instructions": "Focus on sensory details"
  },
  "created_at": ISODate("...")
}
```

### Database 2: `ai_diary` (AI Internal Cache)

#### Collection: `conversations`
```javascript
{
  "_id": ObjectId("..."),
  "user_id": "alice",              // String, not ObjectId
  "date": "2024-12-09",
  "messages": [
    {
      "role": "user",
      "text": "I went for a walk today"
    },
    {
      "role": "ai",
      "text": "That sounds lovely! How was the weather?"
    }
  ],
  "updated_at": ISODate("...")
}
```
