<div align="center">

# 🧭 Pusula

### Privacy-first career workspace for job discovery, CV analysis and application tracking

A modern career platform built with **Next.js 16, React 19 and TypeScript** that combines job discovery, local CV analysis, skill matching, application tracking and browser-assisted job aggregation in a single workspace.

<br />

![Next.js](https://img.shields.io/badge/Next.js-16.3-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-19.2-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Chrome Extension](https://img.shields.io/badge/Chrome_Extension-Manifest_V3-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white)

</div>

---

## About the Project

**Pusula** is a personal career workspace designed to help users discover job opportunities, compare them with their CV and track the application process from one interface.

The application combines:

- Job discovery
- CV parsing
- Skill extraction
- CV-to-job matching
- Job filtering
- Saved jobs
- Applied jobs
- Manual job importing
- Platform-specific searches
- Browser-extension assisted job collection
- Local profile persistence

The project is especially focused on the Turkish job market while also supporting broader remote and international searches.

---

## Main Features

### Career Workspace

- Discover jobs
- Save jobs
- Mark jobs as applied
- Hide already-applied jobs
- Filter by location
- Filter by workplace type
- Filter by experience level
- Filter by employment type
- Filter by source
- Sort by skill match
- Sort by publication date

### CV Analysis

- PDF support
- DOCX support
- TXT support
- Maximum 10 MB upload
- Skill extraction
- Role detection
- CV report
- Automatic search suggestion
- Local browser processing

### Job Matching

- Matches job descriptions with CV skills
- Normalizes Turkish text
- Scores jobs using matched skills
- Sorts relevant jobs ahead of weaker matches

### Browser Integration

- Chrome / Edge extension
- LinkedIn parsing
- Indeed parsing
- Kariyer.net parsing
- İşin Olsun parsing
- Yenibiriş parsing
- Eleman.net parsing
- Manual re-scan
- Multi-platform search launch

---

## Architecture

```text
                       Pusula
                          │
          ┌───────────────┴────────────────┐
          │                                │
          ▼                                ▼
     Career Workspace                CV Analysis
          │                                │
          │                                ├── PDF.js
          │                                ├── Mammoth
          │                                └── TXT
          │
          ├── Live Job Sources
          ├── Platform Search
          ├── Manual Jobs
          └── Browser Extension
                    │
                    ▼
             Normalized Job Model
                    │
                    ▼
             Skill Matching Engine
                    │
                    ▼
        Save / Apply / Filter / Sort
```

---

## Tech Stack

| Technology | Purpose |
| --- | --- |
| **Next.js 16.3** | Full-stack framework |
| **React 19.2** | UI architecture |
| **TypeScript 5.9** | Type safety |
| **Tailwind CSS 4** | Styling |
| **shadcn/ui** | UI component system |
| **Base UI** | UI primitives |
| **Lucide React** | Icons |
| **PDF.js** | PDF CV parsing |
| **Mammoth** | DOCX CV parsing |
| **Cheerio** | HTML parsing |
| **Recharts** | Charting foundation |
| **Vinext** | Vite-based Next-compatible runtime |
| **Cloudflare Workers** | Alternative deployment/runtime target |
| **Vite 8** | Build tooling |
| **Wrangler** | Cloudflare development/deployment |
| **Oxlint** | Linting |
| **Oxfmt** | Formatting |

---

## CV Processing

CV files are processed directly in the browser.

Supported formats:

```text
PDF
DOCX
TXT
```

Current upload limit:

```text
10 MB
```

---

## PDF Parsing

PDF documents are read using:

```text
pdfjs-dist
```

The application extracts text from up to:

```text
30 pages
```

and sends the resulting text into the local CV analysis pipeline.

---

## DOCX Parsing

Word documents are parsed through:

```text
mammoth
```

using raw text extraction.

---

## Privacy-First CV Flow

The application explicitly keeps CV parsing on the client side.

```text
CV File
   │
   ▼
Browser
   │
   ├── PDF.js
   ├── Mammoth
   └── Text Reader
   │
   ▼
CV Analysis
```

The file itself does not need to be uploaded to the application server for parsing.

---

## CV Analysis

CV analysis logic is implemented in:

```text
lib/cv-analysis.ts
```

The generated report contains information such as:

- Skills
- Potential roles
- Other detected terms
- Character count
- Document statistics

The report is cached locally so the user does not need to reprocess the same CV on every visit.

---

## Skill Matching

Job matching logic lives in:

```text
lib/matching.ts
```

The application compares:

```text
Job Title
+
Job Tags
+
Job Description
```

against extracted CV skills.

The number of matching skills can then be used to sort jobs.

---

## Matching Flow

```text
CV
 │
 ▼
Extract Skills
 │
 ▼
Job Description
 │
 ▼
Normalize Text
 │
 ▼
Skill Matching
 │
 ▼
Match Count
 │
 ▼
Relevance Sorting
```

---

## Job Discovery

Jobs can originate from multiple sources:

```text
Live sources
Platform search
Browser extension
Manual entries
Previously viewed jobs
```

They are normalized into a shared job model before entering the user interface.

---

## Job Model

The application works with normalized job information including:

```text
id
url
title
company_name
location
country
description
source
tags
category
salary
publication_date
job_type
workplace
level
```

This lets different job providers behave consistently inside the UI.

---

## Search & Filtering

Available filters include:

### Location

- Türkiye
- Individual Turkish cities
- Worldwide remote
- All countries

### Workplace

- Remote
- Hybrid
- On-site
- Unknown

### Employment Type

- Full-time
- Part-time
- Internship
- Contract
- Other supported types

### Experience Level

Jobs can also be grouped by inferred seniority/experience level.

---

## Saved Jobs

Users can save opportunities directly inside Pusula.

Saved job IDs are persisted in:

```text
localStorage
```

so they remain available after page refreshes.

---

## Application Tracking

Jobs can be marked as:

```text
Applied
```

allowing the interface to act as a lightweight personal application tracker.

The user can also hide applied positions from the discovery view.

---

## Local Storage

The project stores personal workspace data under local browser storage.

Examples include:

```text
pusula-local
pusula-history
pusula-manual
pusula-cv-analysis
```

Stored data includes:

- Saved jobs
- Applied jobs
- Skills
- CV filename
- CV analysis
- Imported jobs
- Job history

---

## Platform Search

The application contains a dedicated platform search layer:

```text
components/platform-search.tsx
lib/platform-search.ts
```

This enables supported job platforms to be queried through a common UI.

---

## Turkish Job Portals

The application also provides direct access to major Turkish career portals.

Users can:

- Open supported search pages
- Copy a prepared search query
- Add discovered jobs manually
- Paste job descriptions for CV comparison

---

## Manual Job Import

A job can be added manually with:

```text
Job URL
Position
Company
City
Workplace Type
Employment Type
Description
```

The description is automatically processed for skills.

---

## Browser Extension

Pusula includes a separate Chrome / Edge extension located under:

```text
extension/
```

The extension uses:

```text
Manifest V3
```

and communicates with the main Pusula application.

---

## Supported Browser Sources

The extension currently contains readers for:

```text
LinkedIn
Indeed
Kariyer.net
İşin Olsun
Yenibiriş
Eleman.net
```

---

## Browser Search Flow

```text
Pusula
   │
   │ Search request
   ▼
Browser Extension
   │
   ├── Opens selected platforms
   │
   ▼
Search Result Tabs
   │
   ▼
Listing Card Parser
   │
   ▼
Normalized Jobs
   │
   ▼
Pusula
```

---

## Extension Security Model

The extension does not attempt to bypass login, verification or CAPTCHA systems.

If a site requires authentication:

```text
User completes authentication
        │
        ▼
Extension re-scans visible results
```

The extension reads listing data from the open page rather than trying to automate account credentials.

---

## Page Parsing

The extension can use multiple approaches depending on the platform.

### JSON-LD

It checks:

```html
<script type="application/ld+json">
```

for `JobPosting` structured data.

### Listing Cards

Platform-specific selectors are also used for visible job cards.

---

## Extension Session Isolation

Search state is stored using:

```text
chrome.storage.session
```

instead of persistent Chrome storage.

Search sessions are invalidated after approximately:

```text
1 hour
```

or when the Pusula tab is closed.

---

## Browser Extension Safety Checks

The background worker validates:

- Pusula origin
- Search request ID
- Query length
- City length
- Selected platforms
- Platform domain
- Source tab ID

This prevents unrelated tabs from submitting arbitrary results.

---

## Extension Installation

The built extension can be downloaded from:

```text
/pusula-extension.zip
```

Installation steps:

1. Extract the ZIP archive.
2. Open `chrome://extensions` or `edge://extensions`.
3. Enable Developer Mode.
4. Select **Load unpacked**.
5. Choose the extracted extension directory.
6. Reload Pusula.

---

## Extension Build

The project includes:

```text
scripts/build-extension.mjs
```

for packaging extension files.

---

## API Routes

The application currently exposes:

```text
/api/jobs
/api/platform-search
```

---

## Jobs API

```http
GET /api/jobs
```

provides normalized jobs and source status information for the application.

The frontend uses this endpoint during initial job discovery.

---

## Platform Search API

```http
/api/platform-search
```

provides the backend integration layer for platform-specific search operations where server-side retrieval is supported.

---

## User Interface

The main workspace includes:

```text
İş keşfet
Kaydedilenler
Başvurularım
CV ve profilim
Bağlantılar
```

The dashboard follows a persistent sidebar architecture.

---

## Career Dashboard

The interface includes:

- Sidebar navigation
- Search tools
- Filters
- CV onboarding
- Match reports
- Job cards
- Job detail modal
- Saved/application counters
- Privacy messaging

---

## CV Report

The:

```text
CvReport
```

component displays analysis results and lets the user trigger searches from detected skills and roles.

---

## Browser-Aware Integration

The frontend also checks for:

```text
document.modelContext
```

and can register a:

```text
search_jobs
```

tool when that environment is available.

This adds another interaction layer without changing the main search architecture.

---

## Testing

The repository includes dedicated tests for:

```text
CV Analysis
Browser Extension
Platform Logic
Türkiye Utilities
```

Test files include:

```text
tests/cv-analysis.test.mjs
tests/extension.test.mjs
tests/platforms.test.mjs
tests/turkey.test.mjs
```

---

## Extension Tests

The extension tests cover areas such as:

- Tab authorization
- Simultaneous results
- Cancellation
- Invalid URLs
- Source verification
- Search state

---

## Türkiye Utilities

Turkish-specific normalization and career data logic are separated into:

```text
lib/turkey.ts
```

This includes:

- Turkish cities
- Text normalization
- Workplace labels
- Employment labels
- Seniority inference
- Job portal utilities

---

## Responsive UI

The project includes responsive behavior for:

- Sidebar
- Job results
- Filters
- CV onboarding
- Modals
- Job cards
- Platform search
- Mobile layouts

---

## Project Structure

```text
kariyerApp/
│
├── app/
│   ├── api/
│   │   ├── jobs/
│   │   │   └── route.ts
│   │   └── platform-search/
│   │       └── route.ts
│   │
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx
│
├── components/
│   ├── ui/
│   ├── browser-search.tsx
│   ├── cv-report.tsx
│   ├── platform-search.tsx
│   └── turkey-portals.tsx
│
├── extension/
│   ├── background.js
│   ├── bridge.js
│   ├── manifest.json
│   ├── popup.html
│   ├── popup.js
│   └── reader.js
│
├── lib/
│   ├── browser-jobs.ts
│   ├── cv-analysis.ts
│   ├── jobs.ts
│   ├── matching.ts
│   ├── platform-search.ts
│   ├── platforms.ts
│   ├── turkey.ts
│   └── utils.ts
│
├── docs/
│   ├── browser-extension.md
│   ├── platform-sources.md
│   └── vercel-deployment.md
│
├── scripts/
│   ├── build-extension.mjs
│   └── generate-icons.mjs
│
├── tests/
│   ├── cv-analysis.test.mjs
│   ├── extension.test.mjs
│   ├── platforms.test.mjs
│   └── turkey.test.mjs
│
├── public/
│   ├── pusula-extension.zip
│   └── pdf.worker.min.mjs
│
├── package.json
├── vercel.json
├── vite.config.ts
└── tsconfig.json
```

---

## Getting Started

Clone the repository:

```bash
git clone https://github.com/seyitbugraerden/kariyerApp.git
```

Navigate into the project:

```bash
cd kariyerApp
```

Install dependencies:

```bash
npm install
```

Create your environment file:

```bash
cp .env.example .env.local
```

Start development:

```bash
npm run dev
```

---

## Node.js Requirement

The project specifies:

```text
Node.js >= 22.13.0
```

---

## Available Scripts

### Development

```bash
npm run dev
```

Runs the project through Vinext.

### Build

```bash
npm run build
```

### Vercel Build

```bash
npm run build:vercel
```

### Vercel Start

```bash
npm run start:vercel
```

### Cloudflare Runtime

```bash
npm start
```

Uses Wrangler and the generated Cloudflare configuration.

### Lint

```bash
npm run lint
```

Uses Oxlint.

### Format

```bash
npm run format
```

Uses Oxfmt.

---

## Deployment Architecture

The project contains support for multiple deployment paths.

### Vercel

```text
Next.js
   ↓
next build
   ↓
Vercel
```

### Cloudflare

```text
Vinext
   ↓
Vite
   ↓
Cloudflare Workers
```

This gives the project a more flexible deployment architecture than a standard single-target Next.js application.

---

## Privacy

Privacy is one of the central design decisions of Pusula.

CV parsing happens locally.

The browser extension reads only job listing information required for the application workflow.

It does not intentionally transfer:

- Passwords
- Browser cookies
- Full page HTML
- CV files
- Application history from job sites

The user's saved/application state is primarily stored in the user's browser.

---

## Current Limitations

The current implementation should still account for several limitations:

- External job site HTML can change
- Browser parsers may require maintenance
- CAPTCHA and authentication are not bypassed
- Full pagination is not guaranteed
- Historical applications on external platforms are not synchronized
- Browser extension currently targets desktop Chrome / Edge
- Extension is manually installed rather than store-distributed
- Skill matching is rule/text based rather than semantic embedding based

---

## Development Roadmap

Potential next steps include:

- Persistent account system
- Server-side user profiles
- Database-backed application history
- Semantic CV/job matching
- Embedding-based relevance scoring
- Saved search alerts
- Email notifications
- Job deduplication improvements
- Company tracking
- Interview pipeline
- Notes per application
- Application deadlines
- Salary analytics
- Chrome Web Store release
- Firefox extension
- Extension auto-update
- Background job refresh
- AI-assisted CV feedback
- Cover letter generation
- Automated test expansion

---

## Technical Highlights

The project demonstrates experience with:

- Next.js 16
- React 19
- TypeScript
- Client-side document parsing
- PDF.js
- DOCX parsing
- Job data normalization
- CV skill extraction
- Search and filtering
- Local persistence
- Chrome Extension Manifest V3
- Cross-tab communication
- Platform-specific DOM parsing
- REST route handlers
- Multi-runtime deployment
- Cloudflare Workers
- Vercel
- Automated tests

---

## Developer

<div align="center">

### Seyit Buğra Erden

**Full Stack Developer · Software Engineer**

[GitHub](https://github.com/seyitbugraerden) ·
[LinkedIn](https://www.linkedin.com/in/sbugraerden/)

<br />

Built with **Next.js · React · TypeScript · Chrome Extension APIs · Cloudflare**

</div>
