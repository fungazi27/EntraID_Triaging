# Entra ID ML-Based SOC Alert Analysis — POC

## Objective
Build a proof-of-concept ML-assisted engine that analyzes Microsoft Entra ID authentication activity and determines whether behavior appears normal or suspicious — eventually capable of recommending an analyst response or flagging situations where a predefined response could be automated.

The business problem: reduce the amount of time SOC analysts spend investigating common Entra ID authentication alerts by building a detection/decision engine that can triage sign-in activity automatically.

This project is about the **data analysis, ML, detection, and decision-making engine only**. Explicitly out of scope: cloud architecture, databases, API design, SIEM ingestion pipelines, Microsoft Graph integration, Kubernetes, production deployment, front-end development, enterprise infrastructure.

## Background
The goal is hands-on experience taking security data from its raw state through data analysis, feature engineering, model development, evaluation, and eventually an operationally useful security decision engine — applying existing cybersecurity/SOC domain knowledge to a real ML engineering workflow.

## Dataset
- **File:** `EntraID_logs.csv`
- **Shape:** 47,124 rows × 73 columns
- Synthetic Microsoft Entra ID `SigninLogs`-style data, modeled after Microsoft's documented schema
- **Intentionally unlabeled** — no ground truth. Treated as though a real organization exported raw sign-in logs with no pre-tagged malicious/benign field.
- Contains interactive and non-interactive authentication, successes and failures, MFA info, Conditional Access info, user/device/application/IP/geographic/session/risk fields, and realistic missing/sparse data.

## POC Detection Scenarios (Release 1)
1. **Impossible Travel** — authentication for the same user from geographically distant locations in a timeframe that makes legitimate physical travel unlikely.
2. **Repeated Failures Followed by Success** — sequences of failed logins followed by a success; determine when this represents suspicious behavior vs. normal user error.
3. **Unfamiliar Location or IP Address** — activity that deviates meaningfully from a user's historical authentication behavior (not just "IP not seen before").

## Release Roadmap
| Release | Goal |
|---|---|
| 1 — Detection Engine | Working detection/ML logic for the three scenarios above |
| 2 — Analyst Recommendations | Risk scoring, contributing factors, recommended disposition/actions, human-review flags |
| 3 — Simulated Response Automation | Identify high-confidence/low-risk actions that could be automated (simulated, not against a live tenant) |
| 4 — Evaluation & Tuning | Metrics appropriate to each detection method, not blanket classification metrics |
| 5 — Optional LLM Analyst Explanation | LLM explains structured detection output to SOC analysts; not a detection mechanism itself |

## Ultimate Goal
Understand — and be able to justify — the full path from raw Entra ID telemetry → data exploration → understanding authentication behavior → feature engineering → detection/model selection → model training or behavioral baselining → evaluation → risk scoring → response recommendations → optional AI-generated analyst explanations. The reasoning behind each decision matters as much as the working code.

## Current Status
Working on **Release 1 — Case 1: Impossible Travel**, currently at the exploratory data analysis / field validation stage on the dataset above.