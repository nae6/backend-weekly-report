# Backend Report Project

An AI-powered backend engineering learning system that automatically collects, analyzes, and transforms weekly backend industry news into actionable learning plans — and tracks how much of that plan actually gets done.

The project consists of two scheduled Claude agents:

- **Saturday:** Backend Weekly Industry Report (+ Learning Progress Sync)
- **Sunday:** Sunday Learning Planner (+ Learning Checklist generation)

Together, they provide a continuous learning cycle from **industry analysis** → **personalized weekly learning** → **progress tracking** → back into the learner's own profile.

---

# Overview

Backend Report Project is an automated learning support system for backend engineers.

Every **Saturday**, the system:

- Syncs last week's completed learning checklist items back into the learner's profile (Step 0)
- Collects backend-related articles from multiple trusted sources
- Analyzes their technical importance
- Generates a Backend Weekly Industry Report
- Saves the report to Notion

Every **Sunday**, the system:

- Reads the latest Industry Report
- Analyzes the current learning sprint
- Selects the highest-priority learning theme
- Generates a practical one-week learning plan
- Saves the Learning Report to Notion
- Breaks the plan down into a checkable Learning Checklist

The objective is not simply to summarize news, but to transform industry trends into structured, practical learning — and to close the loop by feeding what was actually completed back into the learner's own profile.

---

# Features

## Saturday – Backend Weekly Industry Report

- Automated weekly execution (fully unattended, no human approval required mid-run)
- Multi-source article discovery via search (no raw feed fetching)
- AI-based industry analysis, written by Claude itself
- Duplicate topic consolidation
- Beginner-friendly explanations
- Technology trend summary
- Dynamic Priority / Tags assignment
- Automatic Notion integration
- Duplicate-report prevention (same-day idempotency)

## Learning Progress Sync (runs as Step 0 of the Saturday agent)

- Reads all completed-but-unreflected Learning Checklist items, across every past week — not just the most recent one
- Updates the learner's Notion profile (Current Skills / Current Focus) based on what was actually finished
- Updates the active Current Sprint context with the same progress
- Marks a week's checklist "Learning Completed" once every item in it is done

---

## Sunday – Sunday Learning Planner

- Current Sprint analysis
- Learning priority evaluation
- Practical weekly learning plan generation
- Step-based learning roadmap
- Learning scope control
- Mentor-style learning advice
- Automatic Notion integration
- Learning Checklist generation (3–6 checkable action items per week)

---

# System Architecture

```text
                Web Search (5 official sources)
                         │
                         ▼
        Backend Weekly Industry Report
                 (+ Learning Progress Sync)
                         │
                         ▼
                   Notion Database
                         │
                         ▼
              Sunday Learning Planner
                         │
                         ▼
     Sunday Learning Report + Learning Checklist
                         │
                         ▼
                   Notion Database
                         │
             (checked off by the learner
              during the week)
                         │
                         ▼
        fed back into next Saturday's Step 0
```

---

# Workflow

## Saturday (Claude Scheduled Task, 20:00 JST every Saturday)

```text
Scheduled Trigger
        │
        ▼
Step 0: Sync completed Learning Checklist items
   → Learner Profile / Current Sprint / "Learning Completed" flag
        │
        ▼
Duplicate-report check (same-day idempotency)
        │
        ▼
WebSearch each of 5 official sources (parallel, domain-restricted)
        │
        ▼
Claude writes the Backend Weekly Industry Report
        │
        ▼
Assign Priority + Tags (existing options only — no schema changes)
        │
        ▼
Save to Notion
```

---

## Sunday (Claude Scheduled Task, 20:00 JST every Sunday)

```text
Scheduled Trigger
        │
        ▼
Get Backend Weekly Industry Report
        │
        ▼
Get Current Sprint / Current Focus / Learner Profile / Current Skills
        │
        ▼
Get AI Prompt (Notion AI Prompts Database)
        │
        ▼
Claude writes the Sunday Learning Report
        │
        ▼
Save to Notion
        │
        ▼
Extract 3–6 action items → create Learning Checklist rows
```

---

# RSS / Search Sources

## Official Sources (discovered via domain-restricted web search, not raw feed fetching)

- PHP Official
- Laravel News
- Symfony Blog
- AWS What's New
- Docker Blog

Three sources from the original design (LY Tech Blog, Money Forward Developers, OpenAI Developer Blog) were dropped: they were either too broad, off the target stack (Ruby/Rails), or off-axis from the learning goal (AI/LLM rather than backend fundamentals).

---

# Reports

## Backend Weekly Industry Report

- Top 3 Most Important News
- Technology Trends
- Related Technologies
- Weekly Summary

(Mermaid diagrams were part of the original design but are explicitly excluded to keep generation fast and reliable.)

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

## Learning Checklist (new)

- 3–6 concrete, checkable action items derived from that week's Learning Report
- Checked off manually by the learner in Notion during the week
- Read back every Saturday and reflected into the learner's profile

---

# Technology Stack

| Category | Technology |
| ----------- | ------------ |
| Workflow Automation | Claude Scheduled Tasks (Claude Code Remote triggers) |
| AI | Claude itself (no external AI API) |
| Article Discovery | WebSearch (domain-restricted) |
| Knowledge Management | Notion API |
| Prompt Management | Notion Database (Sunday only — Saturday's prompt is embedded in its trigger) |
| Programming Language | n/a (no custom workflow code; trigger prompts + Notion schema only) |

n8n was used for the original v1.0/v1.1 implementation. As of v2.0.0, both workflows run unattended as Claude scheduled tasks; the n8n workflows are disabled but kept in this repo (`workflows/*.json`) for historical reference.

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
│   │   └── specification/
│   └── workflow/
│
└── workflows/
    ├── backend-weekly-report-v1.0.json      (legacy n8n export)
    └── sunday-learning-planner-v1.1.json    (legacy n8n export)
```

Claude scheduled tasks have no exportable JSON equivalent to `workflows/*.json` — their prompts are documented directly in `docs/prompts/implementations/*-v2.0.md`, which is the source of truth from v2.0.0 onward.

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
- Prefer official sources whenever possible
- Generate practical, achievable weekly learning plans
- Never let an unattended run block on an interactive approval (no schema-altering writes, no raw feed fetches — see [Design Principles: Unattended Safety](docs/architecture/architecture.md))
- Close the loop: what actually gets done should change what the system knows about the learner

---

# Version

Current Version

```text
v2.0.0
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

## v2.0.0 ✅

- Migrated both workflows from n8n to Claude Scheduled Tasks
- Replaced the external AI API with Claude itself
- Replaced raw RSS/WebFetch collection with domain-restricted WebSearch
- Removed Mermaid diagram generation
- Made Priority/Tags dynamic while forbidding unattended schema changes
- Added the Learning Checklist database and generation step
- Added the Learning Progress Sync step (Saturday Step 0)

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

Instead of reading dozens of articles, this project automatically identifies the most valuable backend topics, explains their practical impact, and converts them into a structured weekly learning plan — and then checks whether that plan was actually followed, feeding the answer back into the learner's own profile.

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
        │
        ▼
Checked-off Progress
        │
        ▼
Updated Learner Profile
```

This allows learners to stay informed while maintaining focus on their current learning goals, and gives the system an honest picture of how much of the plan was actually completed.

---

# License

This project is published for educational and portfolio purposes.

---

# Author

Created as a personal backend engineering learning project. Originally built on **n8n** + **OpenAI API** + **Notion**; migrated in v2.0.0 to run entirely on **Claude Scheduled Tasks** + **Notion**, with Claude itself performing the analysis and writing.
