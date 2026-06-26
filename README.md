# QueueStorm Investigator - SupportOps AI Assistant

An automated, AI-powered digital finance support copilot and forensic investigator built for the **SUST CSE Carnival 2026 (Codex Community Hackathon)**. The system dynamically cross-references user complaints with transaction histories to produce structured, safe, and context-aware operational assessments.

---

## 🚀 Features & Endpoints

### 1. Readiness Health Check
* **Endpoint:** `GET /health`
* **Description:** Used by the automated scoring system to verify server availability.
* **Success Response:** `200 OK` with `{"status": "ok"}`

### 2. Support Ticket Forensic Analyzer
* **Endpoint:** `POST /analyze-ticket`
* **Description:** Consumes ticket metadata, user descriptions, and logs to execute automated investigative reasoning.
* **Payload Format:** Strict JSON contract containing transaction matching, risk severity routing, categorization, and contextual language translation.

---

## 🛠️ Tech Stack & Architecture

* **Runtime:** Node.js (ECMAScript Modules)
* **Framework:** Express.js
* **AI Infrastructure:** Groq Cloud SDK
* **Underlying Model:** `llama-3.3-70b-versatile` (Configured for deterministic JSON formatting output)

---

## 📋 Prerequisites & Local Setup

### 1. Installation
Clone your repository or navigate to your local working directory and install the required production dependencies:

```bash
npm install express dotenv groq-sdk
```
### 2. Environment Configuration
Create a `.env` file in the root directory of your project to manage secrets securely:

```env
PORT=3000
GROQ_API_KEY=your_groq_api_key_here
```
### 3. Running the Service
To spin up the service locally in development mode with auto-reloads:
```bash
npm run dev
```

## 🧠 Core AI & Safety Logic Strategy

The implementation heavily targets the hackathon's automated evaluation metrics across several critical operational layers:

### 1. Contract & Schema Reliability
The service utilizes Groq's low-latency inference system paired with strict schema structures. The system configuration enforces an exact schema structure (`OBJECT` structures with restricted string enums), effectively neutralizing structural anomalies or payload drift during high-concurrency judge scoring.

### 2. Evidence-Based Reasoning Engine
The integration pipeline explicitly mandates the model to trace explicit correlations. The model checks balances, stamps, status fields, and accounts before classifying the `evidence_verdict` (`consistent`, `inconsistent`, or `insufficient_data`).

### 3. Multi-Tier Safety Guardrails
* **Prompt Injection Safeguards:** System boundaries insulate operational decisions, ensuring external text payloads inside user complaints cannot modify routing states or force-override severe classifications.
* **Financial Credential Leak Countermeasures:** Built-in semantic policies block the model from asking for customer PINs, OTPs, or financial secrets. 
* **Regex Fallback Interceptor:** A hardcoded safety layer intercepts the model's generated output strings. If the phrase contains critical credential variables (such as "pin", "otp", or "password"), it overwrites the field instantly with an immutable safety warning.
* **Conditional Compliance Phrasing:** The system is explicitly blocked from making concrete financial liability promises (e.g., promising direct refunds or immediate reversals). It replaces them with defensive legal terminology ("any eligible amount will be processed through official channels").

### 4. Multilingual & Localization Handling
The pipeline dynamically evaluates language contexts. If a customer complaint uses native Bengali text or mixed Banglish syntax, the agent summary and routing infrastructure process natively in English for internal operations, while seamlessly translating the `customer_reply` field into Bengali to maximize user accessibility.

---

## ⚠️ Architectural Limitations

* **Context Window Boundaries:** While highly performant for high-throughput operational pipelines, extreme trace transaction histories exceeding massive volume arrays may require pre-filtering optimization stages to prevent truncation.
* **Stateless Execution:** The current endpoint evaluates tickets independently. To support long-term systemic account histories across sequential billing tickets, an external persistence cache layer (e.g., Redis) would be required.