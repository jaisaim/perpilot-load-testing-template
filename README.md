# PerPilot Load Testing Pipeline — Template

> **AI-Powered End-to-End Load Testing | 6 Agent Pipeline | GitHub Actions**
> 
> Use this template to instantly set up a complete load testing pipeline for any project.
> Simply fill in your app URL and run — no infrastructure required.

---

## Quick Start

1. Click **Use this template** (top right) to create your own repo
2. Go to **Actions** tab in your new repo
3. Select **PerPilot | AI Load Testing Pipeline Template**
4. Click **Run workflow** and fill in the inputs
5. Watch all 6 stages complete automatically

---

## Pipeline Overview

```
Stage 01: Target Application  →  Validates your app URL
Stage 02: NFR Gathering       →  Sets performance thresholds
Stage 03: Workload Design     →  Designs load scenarios
Stage 04: Script Generation   →  Generates k6 test script
Stage 05: Load Test Execution →  Runs the test (k6)
Stage 06: Store Results       →  Saves results to GitHub
```

---

## What to Enter in Each Agent (Stage-by-Stage Guide)

When you click **Run workflow** in GitHub Actions, you will see these input fields:

---

### Agent 01 — Target Application
**Field:** `Target App URL`

Enter the base URL of the application you want to load test.

| Example | When to use |
| ------- | ----------- |
| `https://api.mycompany.com` | Your production/staging REST API |
| `https://staging.myapp.io/api/v1` | A staging environment |
| `https://jsonplaceholder.typicode.com` | Free sample API for testing the pipeline itself |
| `https://reqres.in/api` | Another free sample REST API |
| `https://httpbin.org` | Free HTTP testing API |

> **Tip:** The URL should be the base domain — the script will append endpoint paths like `/posts`, `/users` etc.
> **Note:** If your API requires authentication headers, add them inside the k6 script in Stage 04.

---

### Agent 02 — NFR Gathering (Non-Functional Requirements)
**Field:** `Environment`

Select which environment you are testing. This automatically sets the performance thresholds:

| Environment | P95 Response Time | Max Error Rate | Min Throughput |
| ----------- | ----------------- | -------------- | -------------- |
| `production` | < 1000ms (strict) | < 0.5% | 200 req/s |
| `staging` | < 2000ms (moderate) | < 1% | 100 req/s |
| `development` | < 5000ms (relaxed) | < 5% | 20 req/s |
| `sandbox` | < 10000ms (minimal) | < 10% | 5 req/s |

> **Tip:** Start with `staging` or `development` for initial testing.
> Use `production` only when you are confident the app can handle the load.
> **Customise:** Edit the `case` block in Stage 02 of the YAML to add your own thresholds.

---

### Agent 03 — Workload Design
**Fields:** `Virtual Users` and `Test Duration`

**Virtual Users (VUs)** — How many concurrent users to simulate:

| Value | Test Type | Use Case |
| ----- | --------- | -------- |
| `1-5` | Smoke Test | Verify the script works, sanity check |
| `10-50` | Load Test | Normal expected traffic |
| `50-200` | Stress Test | Find the breaking point |
| `200+` | Spike Test | Sudden traffic burst simulation |
| `10` | Default (this template) | Good starting point for most APIs |

**Test Duration** — How long to run the test:

| Value | Use Case |
| ----- | -------- |
| `1m` | Quick sanity check |
| `5m` | Standard load test (default) |
| `10m` | Extended stability test |
| `30m` | Soak / endurance test |

> **Tip:** For a first run, use `10` VUs and `5m` duration (the defaults).
> For soak testing (memory leaks, connection pool exhaustion), use `30m` or longer.

---

### Agent 04 — Script Generation
**Field:** `Test tool`

Choose your load testing tool:

| Tool | Language | Best For |
| ---- | -------- | -------- |
| `k6` | JavaScript | REST APIs, lightweight, cloud-native |
| `jmeter` | Java/XML | Complex test plans, GUI-based design |

> **Default:** `k6` is recommended for most projects.

**What the script tests (3 scenarios):**

The generated script distributes load across 3 scenarios:

| Scenario | % of Load | What it tests |
| -------- | --------- | ------------- |
| A — Browse | 50% | GET requests (list/read endpoints) |
| B — User Journey | 30% | Multi-step flow (GET detail + POST create) |
| C — CRUD | 20% | PUT update + DELETE operations |

**To customise endpoints for your project:**
Edit `.github/workflows/load-test-template.yml`, Stage 04, and replace:
- `/posts` → your list endpoint (e.g. `/api/v1/products`)
- `/posts/{id}` → your detail endpoint (e.g. `/api/v1/products/{id}`)
- The POST payload → your actual request body
- The PUT/DELETE endpoints → your actual update/delete routes

---

### Agent 05 — Load Test Execution
No manual input required — this stage runs automatically.

It installs k6 and runs the generated script with your configured VUs and duration.
Results are saved as a JSON artifact (`k6-raw-results`).

---

### Agent 06 — Store Results to GitHub
No manual input required — this stage runs automatically.

Results are stored in 3 places:
- **GitHub Step Summary** — visible in the Actions run summary tab
- **Artifact: `load-test-results`** — downloadable JSON file (kept 30 days)
- **Artifact: `load-test-script`** — the generated k6 script

---

## Repository Structure

```
perpilot-load-testing-template/
├── .github/
│   └── workflows/
│       └── load-test-template.yml   ← The 6-stage pipeline
└── README.md                        ← This guide
```

---

## How to Use This Template for a New Project

**Option A — GitHub Template (Recommended)**
1. Click the green **Use this template** button at the top of this page
2. Name your new repository
3. The pipeline file will be copied automatically
4. Go to Actions > Run workflow and fill in your project's details

**Option B — Copy the workflow file manually**
1. Copy `.github/workflows/load-test-template.yml` to your existing repo
2. Commit and push
3. Go to your repo's Actions tab and run it

---

## Customisation Checklist

When adapting this template for a real project, update these sections in the YAML:

- [ ] `target_url` default value → your app's base URL
- [ ] `env.P95_THRESHOLD_MS` → your acceptable response time
- [ ] `env.ERROR_RATE_THRESHOLD` → your acceptable error rate
- [ ] `env.MIN_THROUGHPUT` → your minimum req/s target
- [ ] Stage 04 Scenario A endpoint → your GET list endpoint
- [ ] Stage 04 Scenario B endpoints → your user journey endpoints + request body
- [ ] Stage 04 Scenario C endpoints → your update/delete endpoints
- [ ] Stage 06 notifications → add Slack/Teams webhook if needed

---

## About PerPilot

PerPilot is an AI-Powered Performance Engineering platform that orchestrates end-to-end load testing through 6 intelligent agents — from application URL to GitHub-stored results.

This template replicates the PerPilot agent pipeline as a fully automated GitHub Actions workflow.

---

*Built for performance professionals | Template by jaisaim*
