# 🚛 CityFlow — Intelligent Routes. Predictable Journeys.

> **SIH 2026 Project | Backend Engineering | AI-Assisted Route Intelligence for Commercial Vehicles**

CityFlow is an AI-assisted route intelligence platform designed for commercial vehicles. Instead of optimizing only for distance or travel time, CityFlow considers **vehicle constraints, infrastructure clearance, traffic conditions, weather, predicted ETA, delay risk, and emissions** to provide more operationally informed route alternatives.

## 🏆 Smart India Hackathon 2026

**CityFlow was developed as a solution for Smart India Hackathon (SIH) 2026.**

### My Role — Backend Engineer

I worked primarily on the **backend architecture and service integration layer**, connecting the frontend with routing, database, external APIs, vehicle constraints, route analysis, and machine-learning inference.

### Backend responsibilities

* Designed and implemented REST API endpoints using **Node.js, Express and TypeScript**
* Built the route-analysis backend workflow
* Integrated **OSRM** for road-network routing
* Integrated **Open-Meteo** for weather information
* Integrated **Nominatim** for geocoding
* Implemented vehicle and infrastructure constraint validation
* Integrated the Python **FastAPI + XGBoost** prediction service
* Implemented ETA and delay prediction request handling
* Implemented route scoring and ranking logic
* Integrated **MongoDB/Mongoose** for application data
* Added fallback mechanisms for external-service and ML-service failures
* Structured backend services into routes, models, configuration and service layers

> **Note:** The ML models themselves are documented separately. My primary contribution was the backend layer that consumes and integrates their predictions into the route-analysis workflow.

---

# 🎯 Problem Statement

Conventional navigation systems generally optimize for distance or travel time.

For commercial vehicles, that is not always sufficient.

A route can be short or fast while still being operationally unsuitable because of:

* Low-clearance underpasses
* Bridge weight restrictions
* Vehicle dimensions
* Traffic congestion
* Weather conditions
* Road restrictions
* Fuel and emissions impact

For example, a heavy truck may receive a mathematically shorter route that passes through infrastructure unsuitable for its height or weight.

CityFlow introduces a **vehicle-aware route intelligence layer** to address this problem.

---

# 💡 Solution

Instead of asking only:

> **"What is the fastest route?"**

CityFlow evaluates:

> **"Which available route is appropriate for this vehicle considering travel time, infrastructure constraints, safety, delay risk, weather and emissions?"**

### Core workflow

```text
┌───────────────────────────┐
│ Vehicle + Journey Input   │
│ Source / Destination      │
│ Vehicle Dimensions        │
└─────────────┬─────────────┘
              ↓
┌───────────────────────────┐
│ Route Generation          │
│ OSRM                      │
└─────────────┬─────────────┘
              ↓
┌──────────────────────────────────────────┐
│        Backend Route Analysis             │
├──────────────────────────────────────────┤
│ Vehicle Constraints                      │
│ Infrastructure Validation                │
│ Traffic Profiling                        │
│ Weather Analysis                         │
│ ML Prediction                            │
│ Emission Estimation                      │
└─────────────────────┬────────────────────┘
                      ↓
              Route Scoring
                      ↓
             Ranked Alternatives
                      ↓
          Frontend Route Results
```

---

# 🚀 Live Demo

**Live Application:**
https://cityflow-4py4.vercel.app/

The application provides:

* Dashboard
* Route analysis / RouteShield
* Fleet management
* What-if simulation
* Analytics
* Alerts
* Vehicle-aware route analysis

---

# 🔑 Key Features

## 1. Vehicle-Aware Routing

Users can provide:

* Height
* Width
* Length
* Weight
* Vehicle type
* Fuel type

The backend uses these parameters during route analysis.

---

## 2. Infrastructure Constraint Validation

CityFlow evaluates route compatibility against constraints such as:

* Overhead clearance
* Bridge weight capacity
* Infrastructure type
* Clearance margin

A route that violates an implemented physical constraint receives a strong route penalty.

---

## 3. Road-Network Routing

The backend integrates **OSRM** to obtain road-network routes.

Route information includes:

* Distance
* Duration
* Route geometry
* Direct Haversine distance
* Route circuity

---

## 4. Weather Analysis

CityFlow integrates **Open-Meteo** to obtain current weather information.

Weather analysis considers factors such as:

* Rainfall
* Wind
* Weather conditions

The backend converts these conditions into a weather-risk factor used during route analysis.

---

## 5. Traffic Profiling

The current implementation estimates congestion using:

* Time of day
* Weekday/weekend
* Road type
* Free-flow speed
* Average speed

> The current implementation is a CityFlow traffic profiler rather than a direct third-party live traffic feed.

---

## 6. ETA & Delay Prediction

CityFlow integrates a separate Python ML service containing:

* `cityflow-eta-v1` — ETA regression
* `cityflow-delay-v1` — delay classification

The architecture is:

```text
Node.js Backend
      │
      │ HTTP Request
      ↓
Python FastAPI ML Service
      │
      ├── ETA Model
      └── Delay Model
      │
      ↓
Prediction
      │
      ↓
Node.js Route Analysis
```

If the ML service is unavailable, the backend can use deterministic fallback inference.

---

## 7. Route Scoring

The backend supports multiple route-selection modes:

* Balanced
* Fastest
* Reliable
* Eco
* Clearance

The route-analysis layer combines different operational factors before presenting ranked alternatives.

> The route-ranking system uses application-level scoring heuristics; it is not a learned route-ranking model.

---

## 8. Emission Estimation

CityFlow estimates:

* Fuel consumption
* CO₂ emissions
* Baseline emissions
* Potential CO₂ savings

These values can be incorporated into route comparison.

---

# 🏗️ Backend Architecture

```text
                    React Frontend
                          │
                          │ REST API
                          ↓
              ┌────────────────────────┐
              │ Node.js + Express      │
              │ TypeScript Backend     │
              └───────────┬────────────┘
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ↓                 ↓                 ↓
   Route Services    Vehicle Services   ML Services
        │                 │                 │
        ↓                 ↓                 ↓
      OSRM             MongoDB       Python FastAPI
        │                                   │
        ↓                                   ↓
 Infrastructure                       XGBoost Models
 Validation
        │
        └──────────────┬────────────────────┘
                       ↓
                Route Analysis
                       ↓
                 Route Scoring
                       ↓
                Ranked Results
```

---

# 🔧 Backend Technology Stack

| Layer                | Technology                    |
| -------------------- | ----------------------------- |
| Runtime              | Node.js                       |
| Framework            | Express.js                    |
| Language             | TypeScript                    |
| Database             | MongoDB                       |
| ODM                  | Mongoose                      |
| Routing              | OSRM                          |
| Geocoding            | OpenStreetMap Nominatim       |
| Weather              | Open-Meteo                    |
| ML Integration       | FastAPI + XGBoost             |
| API Style            | REST                          |
| Frontend Integration | HTTP/JSON                     |
| Deployment           | Vercel + Render configuration |

---

# 🧩 Backend Project Structure

```text
server/
└── src/
    ├── config/
    │   └── Database configuration
    │
    ├── models/
    │   ├── User
    │   ├── Vehicle
    │   ├── Route
    │   ├── Trip
    │   └── Alert
    │
    ├── routes/
    │   ├── Vehicle APIs
    │   ├── Route APIs
    │   ├── ML APIs
    │   └── Other REST APIs
    │
    ├── services/
    │   ├── Routing
    │   ├── Weather
    │   ├── Traffic
    │   ├── Infrastructure
    │   ├── Emissions
    │   └── Route Analysis
    │
    ├── ml/
    │   └── ML service integration
    │
    └── index.ts
```

---

# 🔄 Main Backend Request Flow

For a route-analysis request:

```text
POST /api/routes/analyze
              │
              ↓
      Validate Request
              │
              ↓
      Validate Vehicle
              │
              ↓
        Geocode Input
              │
              ↓
       Request OSRM Routes
              │
              ↓
    Analyze Route Candidates
              │
       ┌──────┼─────────┐
       ↓      ↓         ↓
   Traffic Weather Infrastructure
       │      │         │
       └──────┼─────────┘
              ↓
       Generate ML Features
              │
              ↓
      Request ETA/Delay
              │
              ↓
       Estimate Emissions
              │
              ↓
        Route Scoring
              │
              ↓
       Ranked Route Data
              │
              ↓
          JSON Response
```

This is the main backend flow I would discuss in an interview.

---

# 🗄️ Database

CityFlow uses **MongoDB with Mongoose** for application persistence.

Backend models include:

```text
User
Vehicle
Route
Trip
Alert
VerificationCode
DemoLead
```

MongoDB is preferred when configured, with an in-memory fallback for selected application flows.

---

# 🌐 API Overview

### Health

```http
GET /api/health
GET /api/db-status
```

### Vehicles

```http
GET    /api/vehicles
POST   /api/vehicles
DELETE /api/vehicles/:id
```

### Routes

```http
GET    /api/routes
POST   /api/routes
DELETE /api/routes/:id
```

### Route Analysis

```http
POST /api/routes/analyze
```

### Machine Learning

```http
GET  /api/ml/status
POST /api/ml/predict-eta
POST /api/ml/predict-delay
```

---

# 🤖 Machine Learning

## Models

### ETA

**Algorithm:** XGBoost Regressor
**Model:** `cityflow-eta-v1`

### Delay

**Algorithm:** XGBoost Classifier
**Model:** `cityflow-delay-v1`

The backend does not train these models. It acts as the integration layer between the application and the Python ML service.

---

# 📊 Dataset

The repository contains a **5,000-row generated telemetry dataset** created by `ml/generate_dataset.py`.

It simulates:

* Traffic
* Weather
* Incidents
* Commercial corridors
* Vehicle types
* Travel times

### Split

```text
70% → Training   = 3,500
15% → Validation =   750
15% → Test       =   750
```

The split is chronological to reduce temporal leakage.

---

# 📈 ML Evaluation

| Model         | Metric    |    Result |
| ------------- | --------- | --------: |
| ETA XGBoost   | MAE       | 2.029 min |
| ETA XGBoost   | RMSE      | 2.628 min |
| ETA XGBoost   | MAPE      |     5.27% |
| ETA XGBoost   | R²        |    0.9865 |
| Delay XGBoost | Precision |     0.867 |
| Delay XGBoost | Recall    |     0.822 |
| Delay XGBoost | F1        |     0.844 |
| Delay XGBoost | ROC-AUC   |    0.8903 |

### Important limitation

These metrics are measured on **generated telemetry**, not real-world fleet data.

Therefore, they should not be presented as real-world transportation prediction accuracy.

A production system would require training and validation using real historical fleet/GPS/traffic data.

---

# 🧠 Backend Engineering Decisions

## Why Node.js + Express?

The application requires an API layer capable of handling:

* Frontend requests
* External API calls
* Database operations
* ML-service communication
* Route-analysis orchestration

Node.js with Express provides a lightweight REST architecture suitable for this integration-heavy backend.

---

## Why TypeScript?

TypeScript provides:

* Static typing
* Better maintainability
* Safer API contracts
* Improved IDE support
* Easier refactoring

This becomes particularly useful when handling complex objects such as vehicle specifications, route results and ML prediction responses.

---

## Why MongoDB?

CityFlow works with entities whose structures can evolve during development, such as:

```text
Vehicle
Route
Trip
Alert
User
```

MongoDB provides a flexible document-oriented persistence layer for these entities.

---

## Why Separate the ML Service?

The backend and ML components have different responsibilities:

```text
Node.js
↓
Application + APIs + Integration

Python
↓
ML inference + model management
```

This separation allows the ML service to be independently updated or scaled.

---

## Why Fallbacks?

External services can fail.

CityFlow therefore uses fallback mechanisms for selected services so that a temporary dependency failure does not necessarily terminate the entire route-analysis workflow.

---

# ⚙️ Challenges & Solutions

### Challenge 1 — Fastest route may not be vehicle-compatible

**Solution:** Added vehicle and infrastructure constraint validation before route ranking.

### Challenge 2 — Multiple external services

**Solution:** Centralized external-service interactions in backend service modules.

### Challenge 3 — ML service dependency

**Solution:** Added ML-service status checking and deterministic fallback inference.

### Challenge 4 — Time-dependent ETA prediction

**Solution:** Used chronological dataset splitting for model evaluation.

### Challenge 5 — Combining heterogeneous data

**Solution:** Backend route analysis combines routing, vehicle, weather, traffic, infrastructure and ML information into a common route representation.

---

# 🔐 Security

Before making the repository public:

* Never commit `.env` files.
* Never commit database credentials.
* Never commit API keys.
* Use environment variables.
* Rotate credentials that were previously exposed.
* Review Git history for accidentally committed secrets.

---

# ⚠️ Current Limitations

1. ML training data is generated rather than collected from real fleet telemetry.
2. Traffic estimation currently uses time-of-day and road-type heuristics.
3. Incident data currently uses a placeholder zero-incident response.
4. Infrastructure constraints are represented by corridor-level rules.
5. Fallback route geometry should not be treated as actual road geometry.
6. Production-grade authentication, rate limiting and observability require further hardening.
7. ML performance must be re-evaluated using real-world transportation data.

---

# 🚀 Future Improvements

* Real fleet/GPS telemetry
* Verified real-time traffic provider
* Verified road-incident feed
* Geospatial infrastructure database
* Segment-level clearance validation
* Dynamic rerouting
* Real-world ML retraining
* Model monitoring and drift detection
* Automated unit/integration testing
* CI/CD pipeline
* API authentication
* Rate limiting
* Fleet-level multi-vehicle optimization

---

# 🏆 SIH 2026 Project

CityFlow was developed for **Smart India Hackathon 2026** as a team project focused on improving commercial-vehicle route intelligence.

### Team

**Team:** `codeVedas`

### My Contribution

**Role: Backend Engineer**

My primary area of contribution was the backend engineering layer:

```text
REST APIs
   ↓
Route Analysis
   ↓
External Service Integration
   ↓
Vehicle Constraints
   ↓
MongoDB
   ↓
ML Service Integration
   ↓
Route Scoring
   ↓
Frontend Response
```

This backend layer acts as the bridge between the user-facing application, external data sources, persistent data and machine-learning services.

---

# 📸 Screenshots

Recommended screenshots for the repository:

1. Landing page
2. Route analysis interface
3. Vehicle constraint input
4. Top route comparison
5. Fleet dashboard
6. What-if simulation
7. Analytics / alerts

A short demo video/GIF is also recommended.

---

# 🛠️ Getting Started

## Prerequisites

* Node.js 20+
* npm
* Python 3.10+
* MongoDB / MongoDB Atlas
* Required API keys

## Frontend

```bash
npm install
npm run dev
```

Runs on:

```text
http://localhost:5173
```

## Backend

```bash
cd server
npm install
npm run dev
```

Runs on:

```text
http://localhost:5000
```

Health check:

```text
http://localhost:5000/api/health
```

## ML Service

```bash
cd ml

python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Install:

```bash
pip install fastapi uvicorn xgboost pandas numpy scikit-learn
```

Run:

```bash
python server.py
```

ML service:

```text
http://127.0.0.1:8000
```

---

# 🔑 Environment Variables

Use:

```text
.env.example
server/.env.example
```

Example:

```env
MONGODB_URI=
ML_SERVICE_URL=http://localhost:8000
GOOGLE_MAPS_API_KEY=
```

**Never commit actual credentials.**

---

# 📁 Repository Structure

```text
cityflow/
│
├── src/                       # React frontend
│   ├── components/
│   ├── context/
│   ├── pages/
│   └── services/
│
├── server/                    # Node.js backend
│   └── src/
│       ├── config/
│       ├── ml/
│       ├── models/
│       ├── routes/
│       └── services/
│
├── ml/                        # Python ML pipeline
│   ├── feature_engineering.py
│   ├── generate_dataset.py
│   ├── train.py
│   └── server.py
│
├── models/                    # XGBoost artifacts
├── data/                      # ML data
├── public/
├── .env.example
├── render.yaml
└── README.md
```

---

# 🔮 Why CityFlow?

```text
Traditional Routing
        │
        ▼
Fastest / Shortest Route
        │
        ▼
             CityFlow
                 │
                 ▼
     Vehicle-Aware Routing
                 +
      Infrastructure Checks
                 +
        Traffic & Weather
                 +
         ETA / Delay ML
                 +
          Route Scoring
                 +
          Fleet Context
```

CityFlow explores a shift from **"find the fastest route"** toward **"find a route that is operationally suitable for the vehicle and journey."**

---

# 👩‍💻 Author

**Sweta Jha**
B.Tech Computer Science

**Backend Engineer — CityFlow, SIH 2026**

GitHub: https://github.com/NeuralSweta
