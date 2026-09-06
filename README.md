# ProductPulse AI: Customer Feedback Intelligence and Product Planning Copilot

ProductPulse AI is an AI-driven workspace for product managers to analyze customer feedback and uncover actionable insights.

## Architecture

The system is broken down into two main components:
- **Frontend**: A React application built with Vite and styled with Tailwind CSS. It provides a premium, responsive SaaS dashboard.
- **Backend**: A FastAPI (Python) service that handles data ingestion, processing, and storage.

### Data Flow
1. **Input**: Users upload a CSV file or manually enter feedback via the frontend forms.
2. **Preprocessing**: The backend receives the data, cleans the text (removing extra spaces, ignoring empty inputs).
3. **Categorization & Theme Extraction**: A deterministic, rule-based keyword analyzer processes each feedback item to:
   - Assign a **Category** (e.g., Bug, Feature Request).
   - Extract a **Theme** (e.g., Stability, Dark Mode).
   - Determine **Sentiment** (Positive, Negative, Neutral).
   - Calculate a **Priority** score (Low, Medium, High).
4. **Database**: The processed item is stored in MongoDB (or falls back to an in-memory mock if a connection string is not provided).
5. **Dashboard Insights**: The `/api/insights` and `/api/dashboard-summary` endpoints aggregate this data to surface the most critical themes and populate the frontend charts.

## Prerequisites

- Node.js (v18+)
- Python (v3.11+)
- MongoDB (optional, falls back to in-memory store if not available)

## Setup and Run Instructions

### 1. Backend Setup

Open a terminal and navigate to the backend directory:
```bash
cd backend
```

Create a virtual environment and install dependencies:
```bash
# Windows
python -m venv venv
.\venv\Scripts\activate
pip install fastapi uvicorn motor pandas pydantic python-multipart

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
pip install fastapi uvicorn motor pandas pydantic python-multipart
```

Start the FastAPI server:
```bash
uvicorn main:app --reload
```
The backend API will be available at `http://localhost:8000`. On first startup, it will seed the database with 20 realistic feedback records.

### 2. Frontend Setup

Open a new terminal and navigate to the frontend directory:
```bash
cd frontend
```

Install dependencies:
```bash
npm install
```

Start the development server:
```bash
npm run dev
```
The application will be available at `http://localhost:5173`.
