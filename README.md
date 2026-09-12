# Financial-Decision-Agent
A smart Agent which will not only looks to account balance but also review user history and other scenerios to guide on their financial decision. This automated agent will help to built crucial decisions and maintain cash flow
Here is your complete, A-to-Z, production-grade **`README.md`** file. It contains everything from project overview and folder structures to setup, execution, testing, GCP deployment, and submission packaging in one single, continuous document.


# 💳 Buy or Wait? — AI-Powered FinTech Affordability & Decision Agent

An enterprise-grade financial reasoning system built on a **Hybrid Neuro-Symbolic Architecture** designed for high-throughput affordability evaluation, multimodal OCR perception, and real-time interactive decision-making.

The system determines whether a user can safely afford a requested purchase or financial commitment by evaluating baseline cash flows, recurring commitments, pending payments, essential spending limits, and unstructured multimodal receipts/messages over a 90-day forecast window.

---

## 📺 Interactive Dashboard Demo

Here is a quick walkthrough of the **Streamlit Interactive UI**, demonstrating real-time 90-day cash flow simulation, balance trajectory charts, and instant affordability verdicts:

![Streamlit Demo Walkthrough](docs/streamlit_demo.mp4)
*(Replace or link your recorded video demonstration here)*

---

## 🏗️ Architectural Highlights

1. **Neuro-Symbolic Separation**:
   - **Symbolic Core (Deterministic Solver)**: Handles daily cash flow forecasting, 90-day balance trajectories ($B(t) \ge B_{\text{min}}$), foreign exchange normalization, and plan candidate ranking. Guaranteed mathematical accuracy with zero hallucination risk.
   - **Neural Core (Vertex AI Gemini 2.5 Flash)**: Dedicated strictly to untrusted data perception (multimodal OCR on receipt images and NLP message parsing for subscription updates or explicit cancellations).
2. **Prompt Injection & Security Shielding**:
   - System instructions isolate untrusted text and images within secure delimiters.
   - Pydantic schema enforcement (`response_schema`) forces structured JSON outputs and prevents embedded instruction overrides.
3. **6-Step Decision Ranking Matrix**:
   When multiple safe payment plans exist, candidates are ranked and selected by:
   - Priority 1: Complete full request by `desired_completion_date`.
   - Priority 2: Require zero spending changes (`spending_changes_needed = none`).
   - Priority 3: Minimize total payable amount.
   - Priority 4: Start payment as early as possible.
   - Priority 5: Use fewer payments.
   - Priority 6: Deterministic tie-breaker selecting lowest `payment_option_id`.

---

## 📁 Complete Project Directory Structure (A to Z)

```text
Financial-Decision-Agent/
├── README.md                      # Comprehensive system documentation & execution guide
├── app.py                         # Interactive Streamlit Web Dashboard UI
├── pyproject.toml                 # Frozen dependency specifications & build system
├── Dockerfile                     # Container image definition for serverless deployment
├── run_solution.py                # Master batch pipeline execution entrypoint
├── output.csv                     # Generated predictions output for dataset/requests.csv
├── sample_output.csv              # Benchmark evaluation predictions for sample data
│
├── dataset/                       # Benchmark data files & unstructured media artifacts
│   ├── requests.csv               # Core evaluation test requests
│   ├── sample_requests.csv        # Example preview requests with ground truth
│   ├── financial_profiles.csv     # User balances, minimum thresholds, and preferences
│   ├── financial_events.csv       # Inflows, recurring expenses, and commitments
│   ├── exchange_rates.csv         # Historical daily FX rates for multi-currency normalization
│   ├── request_payment_options.csv# Pre-configured installment and financing choices
│   ├── messages.csv               # Unstructured user SMS/chat messages for NLP parsing
│   ├── images.csv                 # Metadata links for receipt images
│   └── media/images/              # Raw receipt images (.png artifacts)
│
├── evaluation/                    # Verification, testing, and auditing deliverables
│   ├── evaluate_accuracy.py       # Automated accuracy comparison script against sample truth
│   ├── usage_report.md            # Generated token usage, model breakdown, and cost analysis
│   └── validate_output.py         # Schema compliance, format, and invariant auditor
│
├── scripts/                       # Automation scripts for packaging and cloud deployment
│   ├── build_container.sh         # Docker build automation script
│   ├── deploy_gcp.sh              # Google Cloud Run deployment automation script
│   └── generate_submission.sh     # Submission bundler script for code.zip creation
│
├── src/                           # Core application source code modules
│   ├── __init__.py                # Package initialization marker
│   ├── api_server.py              # FastAPI REST backend service for live HTTP requests
│   ├── ledger/                    # Ledger management & lifecycle reconciliation
│   │   ├── __init__.py
│   │   ├── fx_converter.py        # O(1) hashed FX rate normalization engine
│   │   └── state_builder.py       # P1-P4 conflict resolution and state builder
│   ├── engine/                    # Simulation and optimization solvers
│   │   ├── __init__.py
│   │   ├── cash_flow.py           # 90-day balance trajectory solver (B(t) >= B_min)
│   │   └── plan_optimizer.py      # Flexible spending permutator & 6-step ranker
│   ├── perception/                # Multimodal AI perception client
│   │   ├── __init__.py
│   │   └── client.py              # Vertex AI Gemini client with token tracking & security
│   └── utils/                     # Shared utilities & compliance schemas
│       ├── __init__.py
│       ├── schema_validation.py   # Pydantic OutputPrediction invariant guardrails
│       └── token_tracker.py       # Thread-safe token accountant and cost estimator
│
└── tests/                         # Automated unit test suite (pytest)
    ├── test_engine.py             # Unit tests for cash flow simulation and capacity checks
    └── test_perception.py         # Mocked unit tests for Vertex AI perception and security

```

---

## ⚙️ Setup Guide & Environment Installation

### Prerequisites

* Python >= 3.10
* Google Cloud SDK (for Vertex AI perception and GCP deployment)
* Docker (optional, for local container builds)

### Installation Steps

1. Clone the repository and navigate into the project directory:
```bash
git clone [https://github.com/NandiniJaiswal05/Financial-Decision-Agent.git](https://github.com/NandiniJaiswal05/Financial-Decision-Agent.git)
cd Financial-Decision-Agent

```


2. Install frozen Python dependencies in editable mode:
```bash
pip install -e .

```


3. Configure your Google Cloud environment variables (required for live multimodal OCR/NLP extraction):
```bash
export GOOGLE_CLOUD_PROJECT="your-gcp-project-id"
export GOOGLE_CLOUD_LOCATION="us-central1"

```



---

## 🚀 Execution Guide & Workflow

### 1. Run the Batch Evaluation Pipeline

To execute the engine across all benchmark requests in `dataset/requests.csv`:

```bash
python3 run_solution.py

```

*This writes structured predictions to `output.csv` and compiles token usage metrics into `evaluation/usage_report.md`.*

### 2. Validate Output Predictions

To verify strict compliance with schema invariants, data types, and relational rules:

```bash
python3 evaluation/validate_output.py output.csv dataset/requests.csv

```

### 3. Run Automated Unit Tests

To verify the stability and mathematical correctness of the simulation engine and perception mocks:

```bash
pytest tests/

```

### 4. Run the Interactive Streamlit Web UI

To launch the visual dashboard locally and test purchase requests interactively:

```bash
streamlit run app.py --server.port 8501 --server.address 0.0.0.0 --server.enableCORS=false --server.enableXsrfProtection=false

```

*(If running in Google Cloud Shell, click **Web Preview -> Preview on port 8501** to view the live app).*

---

## ☁️ GCP Deployment (Cloud Run & Artifact Registry)

To containerize and deploy this application as a high-throughput, autoscaling serverless microservice on Google Cloud Platform:

1. **Authenticate Docker with Artifact Registry**:
```bash
gcloud auth configure-docker us-central1-docker.pkg.dev --quiet

```


2. **Build and Tag the Container Image**:
```bash
docker build -t us-central1-docker.pkg.dev/your-gcp-project-id/fintech-agent-repo/buy-or-wait-engine:latest .

```


3. **Push Image to Google Artifact Registry**:
```bash
docker push us-central1-docker.pkg.dev/your-gcp-project-id/fintech-agent-repo/buy-or-wait-engine:latest

```


4. **Deploy to Google Cloud Run**:
```bash
gcloud run deploy buy-or-wait-api \
  --image us-central1-docker.pkg.dev/your-gcp-project-id/fintech-agent-repo/buy-or-wait-engine:latest \
  --region us-central1 \
  --platform managed \
  --allow-unauthenticated \
  --memory 2Gi \
  --cpu 2 \
  --min-instances 1 \
  --max-instances 100 \
  --set-env-vars GOOGLE_CLOUD_PROJECT=your-gcp-project-id,GOOGLE_CLOUD_LOCATION=us-central1

```



---

## 📦 Submission Packaging

To automate validation and package the final `code.zip` submission bundle containing all required source files, predictions, and usage reports:

```bash
chmod +x scripts/generate_submission.sh
./scripts/generate_submission.sh

```

```

```
