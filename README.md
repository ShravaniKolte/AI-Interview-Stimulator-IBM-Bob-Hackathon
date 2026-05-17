# AI-Interview-Stimulator-IBM-Bob-Hackathon

🤖 AI Interview Platform 

Table of Contents

1. Overview

2. Features

3. Tech Stack

4. Project Structure

5. Getting Started

6. Environment Variables
 
7. API Endpoints

8. User Flow

9. Database Models

10. Health Check


Overview
This platform conducts intelligent mock interviews by parsing the user's resume, GitHub profile, and LinkedIn URL — then generating tailored, adaptive interview questions using the IBM BOB AI API. It tracks emotion and confidence in real time, detects potential dishonesty, adjusts difficulty on the fly, and delivers a full post-interview report.


Features

Real-Time AI Interviewer — Human-like follow-up and intervention during the session.

Emotion + Confidence Detection — Behavioral analysis on every answer.

Resume-to-Interview AI Engine — Questions generated directly from your own resume.

AI Lie / Confidence Analyzer — Flags suspicious, vague, or low-confidence responses.

Adaptive Difficulty — Harder or easier questions based on live performance.

AI Career Recommendation Engine — Suggests roles best suited to your profile.

AI Interview Report Dashboard — Full breakdown with scores and insights after the session.

AI "Why You Got Rejected" Prediction — Honest, data-driven rejection analysis.

Suggested Ideal Answers — Shows what you should have said after each question.

Company-Specific Interview Questions — Questions tailored to your target company.



Tech Stack

Backend

Python 3.x with FastAPI

SQLAlchemy and SQLite for the database

IBM BOB AI API for question generation and answer evaluation

pdfplumber for resume PDF parsing

passlib for password hashing

python-dotenv for environment configuration

Frontend

React 18 with Vite build tool

Plain JavaScript and JSX (no TypeScript)

Single-page app with manual routing via state


Project Structure

Final_Prototype/

├── Portfolio_Backend_v2/

│   └── project1/

│       ├── main.py               # FastAPI app entry point, CORS, router registration

│       ├── auth.py               # User registration & login

│       ├── upload.py             # Resume upload, GitHub/LinkedIn fetching

│       ├── interview.py          # Interview session management, Q&A flow

│       ├── feedback.py           # Post-interview report generation

│       ├── bob_integration.py    # IBM BOB API — question gen & answer scoring

│       ├── emotion_analyzer.py   # Emotion/confidence detection, adaptive logic

│       ├── github_fetcher.py     # Fetches GitHub repo data

│       ├── linkedin_fetcher.py   # Fetches LinkedIn profile data

│       ├── resume_parser.py      # Extracts text from uploaded PDF resumes

│       ├── requirements.txt      # Python dependencies

│       ├── .env.example          # Environment variable template

│       └── database/

│           ├── db.py             # SQLAlchemy engine & session setup

│           └── models.py         # ORM models: User, Session, Question, Attempt


│
└── Portfolio_Frontend_v2/

    └── project2/
    
        ├── index.html
        
        ├── vite.config.js
        
        ├── package.json
        
        └── src/
        
            ├── main.jsx          # React root
            
            ├── App.jsx           # Page router & global state
            
            ├── Home.jsx          # Landing page
            
            ├── Auth.jsx          # Login / Register
            
            ├── Upload.jsx        # Resume upload + interview configuration
            
            ├── Interview.jsx     # Live interview Q&A interface
            
            ├── Results.jsx       # Post-interview report dashboard
            
            └── History.jsx       # Past session history

Getting Started
Backend Setup
Navigate to the backend folder:
bashcd Portfolio_Backend_v2/project1
Create and activate a virtual environment:
bashpython -m venv venv
source venv/bin/activate
# On Windows: venv\Scripts\activate
Install dependencies:
bashpip install -r requirements.txt
Set up environment variables:
bashcp .env.example .env
# Edit .env and fill in your API keys
Start the server:
bashuvicorn main:app --reload --port 8000
The API will be live at http://localhost:8000
Interactive docs available at http://localhost:8000/docs

Frontend Setup
Navigate to the frontend folder:
bashcd Portfolio_Frontend_v2/project2
Install dependencies:
bashnpm install
Start the development server:
bashnpm run dev
The app will be live at http://localhost:5173
Note: Make sure the backend is running before using the frontend. The frontend calls the API at http://localhost:8000 by default.

Environment Variables
Copy .env.example to .env in Portfolio_Backend_v2/project1/ and fill in the following values:
envIBM_BOB_API_KEY=your_bob_api_key_here
IBM_BOB_API_URL=your_bob_api_url_here
GITHUB_TOKEN=your_github_token_here
DATABASE_URL=sqlite:///./database/portfolio.db
IBM_BOB_API_KEY — API key for IBM BOB, used for question generation and answer scoring.
IBM_BOB_API_URL — The endpoint URL for the IBM BOB API.
GITHUB_TOKEN — Personal access token for the GitHub API, used to fetch repository info.
DATABASE_URL — SQLAlchemy database connection string. Defaults to SQLite.

API Endpoints
Auth — /auth

POST /auth/register — Create a new user account.
POST /auth/login — Log in and get user details.

Upload — /upload

POST /upload/resume — Upload a PDF resume and create a new interview session.

Interview — /interview

POST /interview/confirm-field — Confirm or change the AI-detected field and level.
POST /interview/start — Generate questions and start the interview session.
POST /interview/answer — Submit an answer and receive scoring plus emotion analysis.
GET  /interview/next-question — Get the next adaptive question.
POST /interview/company-questions — Get questions tailored to a specific company.
POST /interview/finish — Mark the session as complete.

Feedback — /feedback

GET /feedback/report/{session_id} — Get the full post-interview report for a session.
GET /feedback/history/{user_id} — Get all past sessions for a user.


User Flow

User visits the Home Page.
User registers or logs in.
User uploads their resume as a PDF, and optionally provides a GitHub URL, LinkedIn URL, target company, and desired role.
The system detects the candidate's field (e.g. "Backend Developer") and experience level (e.g. "Mid"). The user can confirm or change this before continuing.
The interview starts. Adaptive questions are generated from the resume and profile data.
For each question, the user submits an answer. IBM BOB scores the answer. Emotion and confidence are analyzed. Lie risk is flagged if confidence is very low. A follow-up question may be triggered automatically.
When the session ends, the Results Dashboard is shown. It includes the overall score, a per-question breakdown, an emotion summary, ideal answers for each question, career role recommendations, and a rejection risk prediction.
All past sessions are accessible in the History view.


Database Models
User — Stores account credentials and profile info including name, email, hashed password, and LinkedIn URL.
Session — One record per interview attempt. Stores the uploaded resume text, AI-detected field and level, target role, company name, and interview mode.
Question — One row per question in a session. Stores the question text, the user's answer, the BOB score, emotion data, confidence score, lie risk flag, the ideal answer, and any follow-up question.
Attempt — Tracks overall attempt metadata linked to a user and session.

Health Check
GET http://localhost:8000/health
Expected response:
json{ "status": "healthy", "version": "2.0.0" }

Built with FastAPI + React + IBM BOB AI
