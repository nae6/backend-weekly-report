# Setting Up Your Own Instance

This project is not a service you sign up for — it's a design you run yourself, in your own Notion workspace and your own Claude account. This document explains what that involves and points you to a single copy-paste prompt that does most of the work for you.

---

## What you get

- Two weekly scheduled Claude agents (no server, no n8n, no API keys to manage):
  - **Saturday**: reads what you finished learning last week and updates your profile, then researches your stack and writes an industry report.
  - **Sunday**: turns that report + your current sprint into a one-week learning plan, and breaks it into a checklist.
- 5 Notion databases that hold your profile, your sprint context, and the generated reports/checklist. (No prompt database — both scheduled tasks carry their own prompt.)
- A closed loop: you check off checklist items in Notion during the week → next Saturday reads what you finished → updates your profile → informs next Sunday's plan.

## What you need before starting

1. A Notion workspace, with the **Notion connector** enabled for your Claude account.
2. A Claude account with access to **scheduled tasks / routines** (this feature is called different things depending on where you're using Claude — ask Claude "can you create a scheduled task?" if you're not sure).
3. About 10–15 minutes and a rough idea of your current learning goals (what you're working on, what tech stack you use) — you'll be asked for this in Step 1 below.

## How to set it up

1. Open a **new** chat with Claude (not this one — a fresh session in whatever product you use day-to-day: claude.ai, the desktop app, etc.).
2. Make sure the Notion connector is connected in that session.
3. Open [`docs/setup/onboarding-prompt.md`](docs/setup/onboarding-prompt.md), copy the entire prompt block, and paste it into the new chat.
4. Answer Claude's questions (your learning goals, current skills, current focus, etc.).
5. Claude will create the 5 Notion databases, seed them with your answers, and create the two scheduled tasks. This takes a few minutes.
6. Review what got created — the databases, and the two scheduled tasks' next-run times.

That's it. From then on, it runs on its own every Saturday/Sunday at 20:00 in your local time (or whatever time you asked for).

## What you'll need to do weekly

- Check off items in the **Learning Checklist** database as you actually complete them during the week. Nothing else reads or infers completion for you — it's genuinely manual, on purpose (see [`docs/architecture/architecture.md`](docs/architecture/architecture.md), Unattended Safety, for why).
- Occasionally glance at the generated reports and adjust your **Current Sprint** (in Learning Contexts) when your focus shifts.

## If something goes wrong

- If a scheduled run seems to hang or never produces a page, the most common cause is a Notion write that's waiting on a permission approval nobody answered — see the "Unattended Safety" section of [`docs/architecture/architecture.md`](docs/architecture/architecture.md) for the two operations this design deliberately avoids at runtime (schema changes, raw feed fetches) and why.
- The full, exact prompt text each scheduled task runs is documented in [`docs/prompts/implementations/`](docs/prompts/implementations/) — useful if you want to tweak wording, add more sources, or change the schedule.

## Adapting it further

The onboarding prompt leaves your article sources open-ended (it asks Claude to pick sources matching your stack, rather than hardcoding PHP/Laravel/AWS/Docker/Symfony). If you want to pin specific sources, edit the Saturday scheduled task's prompt directly after setup — no need to redo onboarding.
