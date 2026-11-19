Halal Accreditation Platform

An AI-assisted Halal certification and accreditation platform consisting of a mobile-first Applicant App, a desktop Creditor/Reviewer Dashboard, and a Node.js + Gemini AI Backend.

Features

1. Mobile Applicant App (index.html)

Guided 3-Step Application Wizard: Captures business profile, certification scope (Food manufacturing, Meat processing, Restaurant/catering, Beverage, Cosmetics), and certified product lists.

AI Document Pre-Check: Supports uploading Images (JPG/PNG), PDFs, and Word Documents (DOCX). Real-time status badges update dynamically (Missing $\rightarrow$ Processing $\rightarrow$ Ready or Needs a fix).

Dynamic Progress Pipeline: Automatically tracks milestones from Submit application $\rightarrow$ AI pre-check $\rightarrow$ Audit visit $\rightarrow$ Committee decision $\rightarrow$ Certificate issued.

Interactive Audit Preparation: Checklist for on-site readiness that unlocks audit visit completion once all requirements are met.

AI Compliance Assistant: Embedded Q&A chat on the Home screen powered by Gemini, context-aware of the applicant's business name and live document statuses.

Payment-Gated Certificate & Renewal: Unlocks the official QR-verifiable Halal Certificate only after all accreditation steps are finished and the ₱500.00 issuance fee is settled, with a separate ₱500.00 renewal flow.

2. Node.js & Gemini AI Backend (server.js)

Multimodal Document Analysis (/api/analyze):

Automatically compresses and resizes large images using sharp for faster inference.

Passes PDFs natively to Gemini.

Extracts text from .docx files using mammoth.

Enforces strict JSON schema responses (ok, fix, missing) with exponential backoff retries for API traffic spikes (503 errors).

Context-Aware AI Assistant (/api/ask): Answers applicant questions about Halal compliance and missing documentation.

Creditor Hub Sync (/api/applications): Receives submitted applications from the mobile frontend and exposes them to the creditor dashboard.

Tech Stack

Frontend: Vanilla HTML5, CSS3 (Mobile-first responsive layout), JavaScript (ES6+)

Backend: Node.js, Express.js

AI Integration: @google/generative-ai (gemini-3.8-flash)

File Processing: multer (in-memory uploads), sharp (image optimization), mammoth (DOCX text extraction)

Prerequisites

Node.js (v18 or higher recommended)

A Google Gemini API Key from Google AI Studio

Installation & Setup

1. Backend Setup

Navigate to your backend directory and install the required dependencies:

npm init -y
npm install express multer cors dotenv @google/generative-ai sharp mammoth


Create a .env file in the root of your backend folder and add your Gemini API key:

GEMINI_API_KEY=your_actual_gemini_api_key_here
PORT=3000


Start the Node.js server:

node server.js


You should see: Server running on port 3000.

2. Frontend Setup

Open index.html directly in any modern web browser (Chrome, Edge, Safari) or serve it using a local development server (such as VS Code Live Server).

API Endpoints

Method

Endpoint

Description

POST

/api/analyze

Accepts multipart/form-data (document file + documentType string) and returns { state, note }.

POST

/api/ask

Accepts { question, docs, businessName } and returns { reply }.

POST

/api/applications

Receives a completed application payload from the mobile app and assigns a reference ID.

GET

/api/applications

Retrieves all submitted applications for the creditor dashboard.

Accreditation Workflow

Apply: Applicant fills out business details, selects certification scopes, adds products, and submits the application to the creditor hub.

Documents: Applicant uploads the 5 required compliance documents (Business registration, HACCP plan, Supplier halal certificates, Cleaning procedure, Ingredient specification sheets). Gemini AI verifies each file.

Audit Visit: Applicant completes the 5-point audit preparation checklist and confirms completion of the site audit.

Certificate & Payment: Once steps 1–3 are complete, the ₱500.00 payment button unlocks on the Certificate tab, granting access to the live QR code, PDF download, and renewal options.
