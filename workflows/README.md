# Legacy n8n Workflows

The two `.json` files in this directory are exports of the original n8n workflows (v1.0 / v1.1). They are kept for historical reference only.

As of v2.0.0, both workflows have been migrated to Claude Scheduled Tasks (Claude Code Remote triggers) and are disabled in n8n. Claude scheduled tasks have no exportable JSON equivalent — their full trigger prompts (which now serve as the source of truth) are documented in [`docs/prompts/implementations/backend-weekly-report-v2.0.md`](../docs/prompts/implementations/backend-weekly-report-v2.0.md) and [`docs/prompts/implementations/sunday-learning-planner-v2.0.md`](../docs/prompts/implementations/sunday-learning-planner-v2.0.md).

See [`docs/architecture/version-history.md`](../docs/architecture/version-history.md) (v2.0.0) for the full migration rationale.
