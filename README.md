# 📱 Resale Genie

### AI-Powered Mobile Device Valuation & Resale Marketplace

**DeviceIQ** is a full-stack AI-powered mobile resale platform that combines **computer vision, machine learning, dynamic pricing, payments, and Retrieval-Augmented Generation (RAG)** to create a trustworthy peer-to-peer device marketplace.

The platform analyzes phone images and specifications to estimate resale value, detects screen damage using deep learning models, enables buyers and sellers to transact through a verified marketplace, and provides policy-driven customer support through a multi-agent RAG pipeline.

> **Core Value Proposition:** Reduce pricing uncertainty in mobile phone resales through AI-powered damage detection, intelligent valuation, dynamic pricing, and policy-aware customer support.

---

## 🚀 Key Features

### 📸 AI-Powered Phone Valuation

- **YOLOv8** for phone screen detection
- **ResNet50 CNN** for screen damage classification
- **XGBoost** for device quality scoring
- Hybrid AI + depreciation-based valuation
- End-to-end inference in **41.8 ms per image**

### 💰 Intelligent Dynamic Pricing

- Hybrid depreciation + machine learning pricing model
- Damage-weighted price adjustments
- Confidence-aware valuation
- Device specification-based scoring
- Real-time marketplace pricing adjustments

### 🛒 P2P Marketplace

- Browse available mobile devices
- View AI-generated condition assessments
- Buyer and seller workflows
- Razorpay payment integration
- Listing lifecycle tracking: `on_sale → sold`

### 💳 Credit-Based Valuation System

- **1 credit = 1 phone valuation**
- Credit packages:
  - 5 credits → ₹50
  - 10 credits → ₹90
  - 20 credits → ₹150
- Razorpay-powered payments
- Automatic credit deduction after valuation

### 🤖 RAG-Based Customer Support

- Multi-agent support pipeline
- FAISS vector retrieval
- 8 policy document types
- Groq LLaMA-powered response generation
- Structured, policy-aware decisions
- Automated issue classification and escalation

### 🔐 Security

- JWT-based authentication
- bcrypt password hashing
- Razorpay signature verification
- Buyer/seller role-based access control
- Configurable CORS
- Secure environment variable management

---

# 🏗️ System Architecture

```text
                         ┌──────────────────────────────┐
                         │       React Frontend         │
                         │      Vite + Tailwind CSS     │
                         │        Framer Motion         │
                         └──────────────┬───────────────┘
                                        │
                                  REST API / HTTPS
                                        │
                         ┌──────────────▼───────────────┐
                         │       FastAPI Backend        │
                         │                              │
                         │  ┌────────────────────────┐  │
                         │  │ API Routes             │  │
                         │  │                        │  │
                         │  │ /auth                  │  │
                         │  │ /predict               │  │
                         │  │ /marketplace           │  │
                         │  │ /payments              │  │
                         │  │ /support                │  │
                         │  └────────────────────────┘  │
                         │                              │
                         │  ┌────────────────────────┐  │
                         │  │ AI / ML Pipeline        │  │
                         │  │                        │  │
                         │  │ YOLOv8                 │  │
                         │  │ ResNet50               │  │
                         │  │ XGBoost                │  │
                         │  │ Price Engine            │  │
                         │  └────────────────────────┘  │
                         │                              │
                         │  ┌────────────────────────┐  │
                         │  │ RAG Support System      │  │
                         │  │                        │  │
                         │  │ Groq LLaMA             │  │
                         │  │ FAISS                  │  │
                         │  │ Policy Documents       │  │
                         │  └────────────────────────┘  │
                         └──────────────┬───────────────┘
                                        │
                         ┌──────────────┴───────────────┐
                         │                              │
              ┌──────────▼─────────┐        ┌──────────▼─────────┐
              │ PostgreSQL / Neon  │        │    FAISS Index     │
              │                    │        │                    │
              │ Users              │        │ Policy Documents   │
              │ Phones             │        │ Embeddings         │
              │ Predictions        │        │                    │
              │ Payments           │        │                    │
              └────────────────────┘        └────────────────────┘
```

---

# 🧠 Machine Learning Pipeline

DeviceIQ combines three machine learning components to generate a final resale valuation.

```text
Phone Image
     │
     ▼
┌──────────────────────┐
│ YOLOv8               │
│ Screen Detection     │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ ResNet50             │
│ Damage Classification│
└──────────┬───────────┘
           │
           │
Phone Specifications
(RAM, Storage, Age,
 Brand, Body Damage)
           │
           ▼
┌──────────────────────┐
│ XGBoost              │
│ Device Scoring       │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│     Price Engine     │
│                      │
│ Damage Weight        │
│ CNN Confidence       │
│ ML Device Score      │
│ Base Depreciation    │
└──────────┬───────────┘
           │
           ▼
     Resale Value
```

---

# 📊 Model Performance

| Component | Model | Accuracy / Output | Inference |
|---|---|---:|---:|
| Screen Detection | YOLOv8 | 73% | 12 ms |
| Damage Classification | ResNet50 CNN | 82% | 18 ms |
| Device Scoring | XGBoost | 0–1 score | 11.8 ms |
| **Full Pipeline** | Optimized Pipeline | — | **41.8 ms** |

### YOLOv8 — Screen Detection

- Framework: PyTorch / Ultralytics
- Model: `ml_models/yolo.pt`
- Confidence threshold: `0.73`
- Input: Variable-resolution images
- Output: Screen bounding box
- Purpose: Isolate the phone screen before damage analysis

### ResNet50 — Damage Classification

- Framework: TensorFlow / Keras
- Model: `ml_models/cnn_model.h5`
- Input: `224 × 224` RGB image
- Output: Four-class probability distribution

| Class | Damage |
|---|---|
| `no_broken` | 0% damage |
| `light_broken` | <30% damage |
| `moderately_broken` | 30–70% damage |
| `severe_broken` | >70% damage |

### XGBoost — Device Scoring

The model considers:

- RAM
- Storage
- Device age
- Brand
- Body damage

The output is a normalized **device quality score between 0 and 1**.

---

# 💰 Valuation Engine

DeviceIQ uses a hybrid **depreciation + AI scoring** approach to generate stable resale estimates.

### Pricing Components

| Component | Formula | Purpose |
|---|---|---|
| Base Depreciation | `0.8 × MRP` | Establishes resale baseline |
| Damage Weight | Damage-dependent | Applies damage penalty |
| Confidence Factor | `0.5 + (0.5 × CNN_Confidence)` | Accounts for model certainty |
| ML Factor | `0.7 + (0.3 × ML_Score)` | Incorporates device specifications |

### Damage Multipliers

| Condition | Multiplier |
|---|---:|
| No Damage | `1.00` |
| Light Damage | `0.85` |
| Moderate Damage | `0.65` |
| Severe Damage | `0.45` |

### Final Formula

```text
Resale Price =
    MRP
    × 0.8
    × DamageWeight
    × ConfidenceFactor
    × MLFactor
```

### Example

```text
MRP:             ₹30,000
Damage:          Light Broken
CNN Confidence:  0.92
Device Score:    0.85

Price =
30,000 × 0.8 × 0.85 × 0.96 × 0.955

≈ ₹18,760
```

---

# 🤖 RAG Customer Support

DeviceIQ includes an automated customer-support pipeline using **retrieval-augmented generation**.

```text
Customer Issue
      │
      ▼
┌──────────────────────┐
│ Triage Agent         │
│ Is issue relevant?   │
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ Classification Agent│
│ Identify issue type  │
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ FAISS Retriever      │
│ Retrieve top policies│
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ Groq LLaMA           │
│ Generate response    │
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ Compliance Agent     │
│ Validate response    │
└──────────┬───────────┘
           ▼
      Final Response
```

The system retrieves relevant policies from a FAISS index containing **8 policy document types**, then uses Groq's LLaMA model to generate a structured response.

### Example Response

```json
{
  "classification": "Damaged on Delivery",
  "clarifying_questions": [],
  "decision": "approve",
  "rationale": "Policy allows refunds for qualifying damaged deliveries",
  "citations": [
    "08-disputes-damaged-incorrect-items.md"
  ],
  "customer_response": "We'll process your refund...",
  "next_steps": [
    "Contact support",
    "Return item",
    "Receive credit"
  ]
}
```

---

# 🔄 Core Workflows

## 1. Phone Valuation

```text
Upload Image + Device Specifications
                │
                ▼
        Deduct 1 Credit
                │
                ▼
       YOLOv8 Screen Detection
                │
                ▼
     ResNet50 Damage Classification
                │
                ▼
       XGBoost Device Scoring
                │
                ▼
          Price Engine
                │
                ▼
        Save Prediction
                │
                ▼
       Display Valuation
                │
                ▼
       Optional: List Phone
```

The complete valuation pipeline is designed to execute in approximately **41.8 ms per image**.

---

## 2. Marketplace Purchase

```text
Browse Available Phones
          │
          ▼
     Select Phone
          │
          ▼
   Create Razorpay Order
          │
          ▼
      Make Payment
          │
          ▼
    Razorpay Webhook
          │
          ▼
   Verify Signature
          │
          ▼
 Update Listing → SOLD
```

---

## 3. Credit Purchase

```text
Check Credit Balance
        │
        ▼
   Select Package
        │
        ├── 5 Credits  → ₹50
        ├── 10 Credits → ₹90
        └── 20 Credits → ₹150
        │
        ▼
   Razorpay Payment
        │
        ▼
 Signature Verification
        │
        ▼
    Add Credits
```

---

# 🛠️ Technology Stack

## Backend

- **Python 3.10+**
- **FastAPI**
- **PostgreSQL / Neon**
- **SQLAlchemy**
- **PyTorch**
- **TensorFlow / Keras**
- **XGBoost**
- **LangChain**
- **FAISS**
- **Hugging Face Transformers**
- **Groq LLaMA 3.1**
- **Razorpay Python SDK**
- **python-jose**
- **bcrypt**

The RAG pipeline uses LangChain, FAISS, and `sentence-transformers/all-MiniLM-L6-v2` for retrieval and embeddings.

## Frontend

- **React 18+**
- **Vite**
- **Tailwind CSS**
- **PostCSS**
- **Framer Motion**
- **Lucide React**

## Infrastructure

- PostgreSQL / Neon Cloud Database
- FAISS vector index
- Razorpay payment gateway
- Groq cloud inference
- Local `/uploads` storage

---

# 📁 Project Structure

```text
DeviceIQ/
│
├── frontend/
│   ├── components/
│   ├── pages/
│   ├── services/
│   ├── hooks/
│   └── ...
│
├── backend/
│   ├── routes/
│   ├── models/
│   ├── services/
│   ├── ml_models/
│   ├── rag/
│   ├── uploads/
│   ├── app.py
│   └── ...
│
├── datasets/
│   ├── dataset_yolov8/
│   ├── cnn_dataset_split/
│   └── mobile_resale_synthetic_dataset_v2.csv
│
├── ml_models/
│   ├── yolo.pt
│   ├── cnn_model.h5
│   └── xgboost_resale_model.pkl
│
├── requirements.txt
└── README.md
```

---

# 🚀 Getting Started

## Prerequisites

Make sure you have:

- Python 3.10+
- Node.js 18+
- npm
- PostgreSQL / Neon database
- Groq API key
- Razorpay API credentials

---

## Backend Setup

```bash
cd backend

python -m venv venv
```

### macOS / Linux

```bash
source venv/bin/activate
```

### Windows

```bash
venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirement.txt
```

Create a `.env` file:

```env
DATABASE_URL=postgresql+psycopg2://username:password@host/neondb?sslmode=require

JWT_SECRET_KEY=your_secret_key
JWT_ALGORITHM=HS256
JWT_EXPIRATION_HOURS=24

GROQ_API_KEY=your_groq_api_key

RAZORPAY_KEY_ID=your_razorpay_key
RAZORPAY_KEY_SECRET=your_razorpay_secret

UPLOAD_DIR=./uploads
CORS_ORIGINS=["http://localhost:5173"]
```

Start the backend:

```bash
python app.py
```

Backend:

```text
http://localhost:8000
```

---

## Frontend Setup

```bash
cd frontend
npm install
```

Create `.env`:

```env
VITE_API_BASE_URL=http://localhost:8000
```

Start the development server:

```bash
npm run dev
```

Frontend:

```text
http://localhost:5173
```

The environment variables and local development configuration are based on the project's supplied setup instructions.

---

# 🔌 Core API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/predict` | Run AI phone valuation |
| `POST` | `/marketplace/mark-sold/{phone_id}` | Mark a phone listing as sold |
| `POST` | `/payments/verify-credit-payment` | Verify Razorpay credit payment |
| `POST` | `/support` | Process a customer-support request |
| `POST` | `/auth` | Authentication endpoints |

### Example Valuation Input

```text
Phone Image
MRP
RAM
Storage
Age
Brand
Body Condition
```

### Example Response

```json
{
  "status": "accepted",
  "damage": "moderately_broken",
  "cnn_score": 0.78,
  "ml_score": 0.83,
  "resale_price": 18600
}
```

---

# 📊 Performance

| Metric | Result |
|---|---:|
| AI Valuation Pipeline | **41.8 ms** |
| Screen Detection | **12 ms** |
| Damage Classification | **18 ms** |
| Device Scoring | **11.8 ms** |
| Average Database Query | **<5 ms** |
| Support Ticket Resolution | **<2 sec** |
| API Response Time (p95) | **<200 ms** |

These are the performance figures documented for the current implementation.

---

# 🔒 Security

DeviceIQ incorporates several security mechanisms:

- JWT authentication with configurable token expiration
- bcrypt password hashing
- Razorpay payment signature verification
- Buyer/seller role separation
- CORS configuration
- Environment-based secret management
- HTTPS recommended for production deployment



---

# 🧪 Model Training

### YOLOv8

```text
Dataset: dataset_yolov8/
Classes: phone_screen
Format: YOLO labels
Test Accuracy: 73%
```

### ResNet50

```text
Dataset: cnn_dataset_split/
Classes:
  - no_broken
  - light_broken
  - moderately_broken
  - severe_broken

Input: 224 × 224 RGB
Test Accuracy: 82%
Framework: TensorFlow/Keras
```

### XGBoost

```text
Dataset: mobile_resale_synthetic_dataset_v2.csv

Features:
  - RAM
  - Storage
  - Age
  - Brand
  - Body Damage
  - MRP

Target:
  - Normalized resale value
```

---

# 🔮 Future Improvements

- [ ] Real-time market price ingestion
- [ ] More device brands and models
- [ ] Improved damage detection with additional damage categories
- [ ] User reviews and seller ratings
- [ ] Image-based device model identification
- [ ] Cloud object storage for uploaded images
- [ ] Production deployment with CI/CD
- [ ] API rate limiting
- [ ] Automated model retraining pipeline
- [ ] Advanced marketplace recommendations

---

# 🙌 Acknowledgments

- **YOLOv8 / Ultralytics** — Object detection
- **ResNet** — Image classification architecture
- **XGBoost** — Device scoring
- **Groq** — LLM inference
- **Razorpay** — Payment infrastructure
- **FAISS** — Vector similarity search

---

## 👩‍💻 Author

**Pratheksha Kanagaraj**

Computer Science Graduate Student · Software Engineer · AI/ML Enthusiast

---

⭐ **If you found Resale Genie interesting, consider giving the repository a star!**
