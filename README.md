# Backend Report Project

An AI-powered backend engineering learning system that automatically collects, analyzes, and transforms weekly backend industry news into actionable learning plans.

The project consists of two independent workflows:

- **Saturday:** Backend Weekly Industry Report
- **Sunday:** Sunday Learning Planner

Together, they provide a continuous learning cycle from **industry analysis** to **personalized weekly learning**.

---

# Overview

Backend Report Project is an automated learning support system for backend engineers.

Every **Saturday**, the system:

- Collects backend-related articles from multiple trusted sources
- Analyzes their technical importance using OpenAI
- Generates a Backend Weekly Industry Report
- Saves the report to Notion

Every **Sunday**, the system:

- Reads the latest Industry Report
- Analyzes the current learning sprint
- Selects the highest-priority learning theme
- Generates a practical one-week learning plan
- Saves the Learning Report to Notion

The objective is not simply to summarize news, but to transform industry trends into structured, practical learning.

---

# Features

## Saturday – Backend Weekly Industry Report

- Automated weekly execution
- Multi-source RSS aggregation
- AI-based industry analysis
- Duplicate topic consolidation
- Beginner-friendly explanations
- Technology trend summary
- Automatic Notion integration

---

## Sunday – Sunday Learning Planner

- Current Sprint analysis
- Learning priority evaluation
- Practical weekly learning plan generation
- Step-based learning roadmap
- Learning scope control
- Mentor-style learning advice
- Automatic Notion integration

---

# System Architecture

```text
                RSS / Official Sources
                         │
                         ▼
        Backend Weekly Industry Report
                         │
                         ▼
                   Notion Database
                         │
                         ▼
              Sunday Learning Planner
                         │
                         ▼
             Sunday Learning Report
                         │
                         ▼
                   Notion Database
```

---

# Workflow

## Saturday

```text
Schedule Trigger
        │
        ▼
Collect RSS Articles
        │
        ▼
Normalize Articles
        │
        ▼
Remove Duplicate Topics
        │
        ▼
Get AI Prompt
        │
        ▼
Prepare OpenAI Request
        │
        ▼
OpenAI
        │
        ▼
Convert Properties
        │
        ▼
Notion
```

---

## Sunday

```text
Schedule Trigger
        │
        ▼
Get Backend Weekly Industry Report
        │
        ▼
Get Current Sprint
        │
        ▼
Current Sprint Ready?
        │
        ▼
Get AI Prompt
        │
        ▼
Prepare OpenAI Request
        │
        ▼
OpenAI
        │
        ▼
Convert Properties
        │
        ▼
Notion
```

---

# RSS Sources

## Official Sources

- PHP Official
- Laravel News
- Symfony Blog
- AWS What's New
- Docker Blog
- OpenAI Developer Blog

## Engineering Blogs

- Money Forward Developers
- LY Tech Blog

The source list focuses on official announcements and high-quality engineering blogs while minimizing duplicate information.

---

# Reports

## Backend Weekly Industry Report

- Top 3 Most Important News
- Technology Trends
- Related Technologies
- Mermaid Diagram (when necessary)
- Weekly Summary

---

## Sunday Learning Report

- Practical Reference
- Learning Theme
- Learning Reason
- Weekly Learning Plan
- Step-based Learning Tasks
- Do Not Learn This Week
- Next Learning Candidate
- Mentor's Advice

---

# Technology Stack

| Category | Technology |
| ----------- | ------------ |
| Workflow Automation | n8n |
| AI | OpenAI API |
| Knowledge Management | Notion API |
| Prompt Management | Notion Database |
| Data Source | RSS |
| Programming Language | JavaScript |

---

# Repository Structure

```text
backend-report-project/

├── README.md
│
├── docs/
│   ├── architecture/
│   ├── prompts/
│   │   ├── implementations/
│   │   ├── specification/
│   │   └── workflow/
│   │
│   └── workflows/
│
└── workflows/
    ├── backend-weekly-report-v1.0.json
    └── sunday-learning-planner-v1.1.json
```

---

# Design Documents

The `docs` directory contains the complete project documentation.

- Architecture
- Workflow Design
- Prompt Implementations
- Prompt Specifications
- Version History

---

# Design Principles

- Separate Industry Analysis from Learning Planning
- Prioritize Current Sprint over technology trends
- Manage AI Prompts outside workflows
- Prefer official sources whenever possible
- Generate practical, achievable weekly learning plans

---

# Version

Current Version

```text
v1.1.0
```

Status

```text
Stable
```

---

# Roadmap

## v1.0 ✅

- Backend Weekly Industry Report
- RSS Aggregation
- AI-based Industry Analysis
- Notion Integration

---

## v1.1 ✅

- Sunday Learning Planner
- Current Sprint Integration
- Prompt Management Database
- Workflow Separation
- Step-based Learning Plan

---

## Future

- Learning Backlog
- Monthly Learning Review
- Sprint Retrospective
- Portfolio Review
- Career Review

---

# Why This Project?

Backend engineers are exposed to a large amount of technical information every week.

Instead of reading dozens of articles, this project automatically identifies the most valuable backend topics, explains their practical impact, and converts them into a structured weekly learning plan.

The result is a continuous workflow that connects:

```text
Industry Trends
        │
        ▼
Industry Report
        │
        ▼
Learning Decision
        │
        ▼
Weekly Learning Plan
```

This allows learners to stay informed while maintaining focus on their current learning goals.

---

# License

This project is published for educational and portfolio purposes.

---

# Author

Created as a personal backend engineering learning project using **n8n**, **OpenAI API**, and **Notion**.
