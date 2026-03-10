# **THE FORK** 🌐

![The Fork Logo](https://github.com/CassieMarie0728/the-fork/blob/main/assets/overview-image-one.png-365dm79r.png?raw=true)

> **Speak to the you who lived the life you didn’t choose.** One session. No history. No comfort.

---

## 🌟 What is The Fork?

The Fork is a **stateless, single-session AI chat experience** that lets you explore what your life might have been like if you had made a different decision. By providing a defining fork in your life and choosing an intensity level, you can engage in a conversation with an AI persona that embodies the version of you who took the alternate path.

### Key Features:
✅ **Stateless Design** – No accounts, no history, no emotional hoarding
✅ **Intensity Levels** – Choose from mild, savage, or brutal responses
✅ **Ephemeral Sessions** – Every conversation burns clean
✅ **LLM-Powered** – Uses Emergent LLM for authentic, character-driven responses
✅ **Safety-First** – Built-in guardrails to prevent harmful content

---

## 🎯 Why This Project?

The Fork is designed for **exploration, curiosity, and emotional reflection**—not therapy, not emotional crutches, and not a repository for personal data. It's a **safe space to ask "what if?"** without the baggage of real-life consequences.

### Who is this for?
- People curious about alternate paths in life
- Writers and creatives exploring character development
- Therapists and psychologists looking for a safe exploration tool
- Anyone interested in AI-driven storytelling

---

## 🛠️ Tech Stack

### Frontend:
- **React** (Create React App)
- **Tailwind CSS** for styling
- **Playwright** for end-to-end testing
- **CRACO** for advanced configuration

### Backend:
- **FastAPI** for the API
- **Uvicorn** for serving the API
- **Motor/PyMongo** (MongoDB optional)
- **Pydantic** for data validation

### LLM:
- **Emergent LLM** via `emergentintegrations.llm.chat`
- Requires `EMERGENT_LLM_KEY` for functionality

### DevOps:
- **Docker** for containerization
- **Docker Compose** for multi-service orchestration
- **GitHub Actions** for CI/CD

---

## 🚀 Quick Start

### Prerequisites:
- **Node.js** 18+
- **Python** 3.11+
- **Docker & Docker Compose** (optional)

### Option 1: Local Development

#### Backend Setup:
```bash
cd backend
cp .env.example .env
# Edit .env with your EMERGENT_LLM_KEY and MongoDB settings if needed
pip install -r requirements.txt
uvicorn server:app --reload
```

#### Frontend Setup:
```bash
cd frontend
cp .env.example .env
# Ensure REACT_APP_BACKEND_URL points to your backend
yarn install
yarn start
```

### Option 2: Docker Compose (Recommended for Development)

```bash
# Build and start all services
docker-compose up --build

# Access the application:
# Frontend: http://localhost:3000
# Backend API: http://localhost:8000
# API Docs: http://localhost:8000/api/docs
```

### Stop Services:
```bash
docker-compose down
```

---

## 📁 Project Structure

```
the-fork/
├── backend/                  # FastAPI backend service
│   ├── server.py             # Main application
│   ├── requirements.txt      # Python dependencies
│   ├── .env.example          # Environment variables template
│   └── API_DOCUMENTATION.md  # API reference
│
├── frontend/                 # React frontend application
│   ├── src/                  # Source files
│   │   ├── components/       # Modular React components
│   │   ├── hooks/            # Custom React hooks
│   │   └── pages/            # Page-level components
│   ├── e2e/                  # Playwright E2E tests
│   └── Dockerfile            # Docker configuration
│
├── docker-compose.yml        # Multi-service orchestration
├── .gitignore                # Git configuration
├── README.md                 # This file
└── ...
```

---

## 🔧 Configuration

### Backend Environment Variables (`.env`):

```
MONGO_URL=mongodb://admin:password@mongo:27017
DB_NAME=fork_database
EMERGENT_LLM_KEY=your_emergent_llm_key_here
CORS_ORIGINS=*
SERVER_HOST=0.0.0.0
SERVER_PORT=8000
```

### Frontend Environment Variables (`.env`):

```
REACT_APP_BACKEND_URL=http://localhost:8000
WDS_SOCKET_PORT=443
ENABLE_HEALTH_CHECK=false
```

---

## 🤝 Contributing

We welcome contributions! Here's how you can help:

1. **Fork** the repository
2. **Clone** your fork
3. **Create** a new branch for your feature or bugfix
4. **Commit** your changes
5. **Push** to your branch
6. **Open** a Pull Request

### Development Setup:

```bash
# Clone the repository
git clone https://github.com/CassieMarie0728/the-fork.git
cd the-fork

# Install dependencies
cd backend && pip install -r requirements.txt
cd ../frontend && yarn install

# Run tests
cd backend && pytest
cd ../frontend && yarn test:e2e
```

### Code Style Guidelines:
- Follow **ESLint** for frontend code
- Follow **PEP 8** for backend code
- Use **black** for Python code formatting
- Use **Prettier** for JavaScript formatting

---

## 📝 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---

## 👥 Authors & Contributors

- **CassieMarie0728** – Project Maintainer

### Special Thanks:
- Emergent for the LLM integration
- The React and FastAPI communities

---

## 🐛 Issues & Support

### Reporting Issues:
- Open an issue on GitHub with clear steps to reproduce the problem
- Include screenshots, logs, and any relevant code snippets

### Getting Help:
- Join our [Discussions](https://github.com/CassieMarie0728/the-fork/discussions) for general questions
- For urgent issues, contact the maintainer directly

---

## 🗺️ Roadmap

### Planned Features:
- [ ] Add more intensity levels
- [ ] Implement user analytics (optional)
- [ ] Add more safety checks and moderation
- [ ] Create a mobile app version

### Known Issues:
- [ ] Docker Compose health checks need improvement
- [ ] MongoDB integration needs more documentation

---

## 🌟 Star & Share

If you find The Fork useful, please consider **starring** this repository and sharing it with others who might benefit from exploring alternate paths in life.

---

## 📚 Documentation

For more detailed information, check out our comprehensive documentation:

- [Quick Start Guide](QUICKSTART.md)
- [Enhancement Summary](ENHANCEMENT_SUMMARY.md)
- [File Structure](FILE_STRUCTURE.md)
- [Implementation Report](IMPLEMENTATION_REPORT.md)
- [Docker Guide](DOCKER_GUIDE.md)
- [API Documentation](backend/API_DOCUMENTATION.md)

---

## 📊 Badges

[![GitHub Stars](https://img.shields.io/github/stars/CassieMarie0728/the-fork?style=flat-square)](https://github.com/CassieMarie0728/the-fork/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/CassieMarie0728/the-fork?style=flat-square)](https://github.com/CassieMarie0728/the-fork/network)
[![GitHub Issues](https://img.shields.io/github/issues/CassieMarie0728/the-fork?style=flat-square)](https://github.com/CassieMarie0728/the-fork/issues)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)