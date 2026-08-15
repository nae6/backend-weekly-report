# Backend Weekly Report

An AI-powered workflow that automatically collects, analyzes, and summarizes backend engineering news every week, then stores a beginner-friendly report in Notion.

---

# Overview

Backend Weekly Report is an automated knowledge aggregation system designed for backend engineering learning.

Every Saturday at **20:00**, the workflow:

* Collects the latest backend-related articles
* Evaluates their importance using OpenAI
* Generates a beginner-friendly weekly report
* Automatically saves the report to Notion

The goal of this project is **not to collect every article**, but to identify and summarize the information that is most valuable for backend engineers.

---

# Features

* Automated weekly execution with n8n
* Multi-source RSS aggregation
* AI-based importance analysis
* Duplicate topic consolidation
* Beginner-friendly technical explanations
* Automatic Notion integration
* Structured weekly learning report

---

# Workflow

```text
Schedule Trigger
        │
        ▼
RSS Read
        │
        ▼
Merge
        │
        ▼
Aggregate
        │
        ▼
Edit Fields
        │
        ▼
OpenAI
        │
        ▼
Code
        │
        ▼
Notion
```

---

# RSS Sources

## Official Sources

* PHP Official
* Laravel News
* Symfony Blog
* AWS What's New
* Docker Blog
* OpenAI Developer Blog

## Engineering Blogs

* Money Forward Developers
* LY Tech Blog

These sources were carefully selected to maximize information quality while minimizing duplicate or low-value articles.

---

# Weekly Report Structure

Each report contains:

* Top 3 Most Important News
* Weekly Technology Trends
* Mermaid Diagram (when necessary)
* Learning TODOs
* Short Learning Advice

The report is intentionally designed to be readable in approximately **3 minutes**.

---

# Technology Stack

| Category             | Technology |
| -------------------- | ---------- |
| Workflow Automation  | n8n        |
| AI                   | OpenAI API |
| Knowledge Management | Notion API |
| Data Source          | RSS        |
| Programming Language | JavaScript |

---

# Project Structure

```text
backend-weekly-report/

├── README.md
│
├── docs/
│   ├── backend-weekly-report-design-v1.0.md
│   ├── architecture.md
│   └── version-history.md
│
└── workflows/
    └── backend-weekly-report-v1.0.0.json
```

---

# Version

Current Version

```text
v1.0.0
```

Status

```text
Released
```

---

# Roadmap

## Version 1.0 ✅

* Backend Weekly Report
* Automated RSS Collection
* AI-based Article Analysis
* Weekly Report Generation
* Notion Integration

## Version 1.1

* Role Model Analyzer
* Technology Trend Analyzer
* Learning Planner

## Future Plans

* Smarter article ranking
* Additional high-quality backend sources
* Report quality improvements
* Enhanced learning recommendations

---

# Design Documents

Detailed documentation is available in the `docs/` directory.

* System Design
* Architecture
* Version History

---

# Why This Project?

Many developers follow numerous technical blogs and official announcements, making it difficult to determine which information truly matters.

This project addresses that challenge by allowing AI to filter, prioritize, and explain backend-related news from a beginner's perspective.

Instead of reading dozens of articles every week, users can review a concise report and immediately understand:

* What happened
* Why it matters
* How it is used in real-world development
* Whether it is worth learning now

---

# License

This project is published for educational and portfolio purposes.

---

# Author

Created as a personal backend engineering learning project using **n8n**, **OpenAI API**, and **Notion**.
