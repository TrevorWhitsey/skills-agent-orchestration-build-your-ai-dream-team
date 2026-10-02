# Agent team

I will use this custom agent team to build Mona's Project Pulse dashboard:

- **Orchestrator** (Claude Opus 4.7): Coordinates the work, delegates scoped tasks to specialists, manages dependencies and parallel work, and verifies the integrated result. Definition: `.github/agents/orchestrator.agent.md`.
- **Planner** (Claude Opus 4.7): Researches the repository and produces an implementation plan, including file assignments, dependencies, risks, edge cases, and validation expectations. Definition: `.github/agents/planner.agent.md`.
- **Coder** (GPT-5.5): Implements assigned code tasks, fixes bugs, and validates changes while staying within the assigned file scope. Definition: `.github/agents/coder.agent.md`.
- **Designer** (Gemini 3.1 Pro): Shapes the dashboard's user experience, accessibility, information hierarchy, responsive behavior, and visual design. Definition: `.github/agents/designer.agent.md`.

I am using GitHub Copilot CLI in a Codespace to orchestrate the work.
