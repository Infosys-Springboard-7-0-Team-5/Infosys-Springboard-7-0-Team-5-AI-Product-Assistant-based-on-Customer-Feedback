# ProductPulse AI Architecture & Database Schema

## System Architecture

The application follows a modern client-server architecture, utilizing a React frontend (Vite) and a FastAPI backend, connected to a MongoDB database.

> [!NOTE]
> The backend defaults to an in-memory mock database if MongoDB is not provided in the environment variables, ensuring the application can always run locally out of the box.

```mermaid
graph TD
    %% Frontend Components
    subgraph Frontend [React + Vite Frontend]
        UI[User Interface Dashboard]
        Upload[CSV Upload & Forms]
        Charts[Insights & Analytics Charts]
    end

    %% Backend Components
    subgraph Backend [FastAPI Backend]
        API[API Router]
        Analyzer[Rule-Based NLP Analyzer]
        DB_Layer[Database Abstraction Layer]
        
        API -->|Processes Text| Analyzer
        Analyzer -->|Categorization & Sentiment| API
        API -->|CRUD Operations| DB_Layer
    end

    %% Data Storage
    subgraph Storage [Database]
        MongoDB[(MongoDB)]
        MockDB[(In-Memory Mock)]
    end

    %% Data Flow
    UI -->|HTTP GET / POST| API
    Upload -->|Uploads CSV| API
    DB_Layer -.->|Primary| MongoDB
    DB_Layer -.->|Fallback| MockDB
    API -->|Aggregates Data| Charts
```

---

## Database Schema

The database stores raw and processed feedback items, as well as aggregated insights. The data is modeled using Pydantic in `backend/models.py`.

```mermaid
erDiagram
    FeedbackItem {
        string _id PK "Unique Document ID"
        string workspace_id "Identifier for tenant/workspace (Default: 'default_workspace')"
        string customer_name "Name of the customer providing feedback"
        string source "Origin of the feedback (e.g., Zendesk, Email, Form)"
        string feedback_text "Raw feedback content"
        string category "Assigned category (e.g., Bug, Feature Request)"
        string theme "Extracted theme (e.g., Stability, Dark Mode)"
        string sentiment "Determined sentiment (Positive, Negative, Neutral)"
        string priority "Calculated Priority (High, Medium, Low)"
        int priority_score "Numeric score for sorting (3, 2, 1)"
        datetime created_at "Timestamp of record creation"
    }

    Insight {
        string _id PK "Unique Document ID"
        string workspace_id "Identifier for tenant/workspace"
        string theme "The aggregated theme (e.g., Stability)"
        int feedback_count "Total number of feedback items matching this theme"
        string priority "Calculated aggregate priority"
        int priority_score "Numeric score used for ranking insights"
        string short_explanation "Brief, AI-generated or rule-based summary"
        string recommended_action "Suggested next step for product managers"
    }
    
    FeedbackItem }|--|| Insight : "Contributes to"
```
