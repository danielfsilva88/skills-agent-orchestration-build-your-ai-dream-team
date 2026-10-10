# Agent team

For Mona's Project Pulse dashboard, I am using a four-agent custom team orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 4.7 (copilot) — Researches the repo, identifies edge cases, and produces the implementation plan and file ownership map before work starts. Definition: [.github/agents/planner.agent.md](../.github/agents/planner.agent.md)
- Designer — Model: Gemini 3.1 Pro (copilot) — Designs the dashboard UX, information hierarchy, accessibility, and visual polish for the Project Pulse experience. Definition: [.github/agents/designer.agent.md](../.github/agents/designer.agent.md)
- Coder — Model: GPT-5.5 (copilot) — Implements the dashboard code, fixes bugs, and validates changes within the assigned file scope. Definition: [.github/agents/coder.agent.md](../.github/agents/coder.agent.md)
- Orchestrator — Model: Claude Opus 4.7 (copilot) — Coordinates the specialist agents, sequences work, delegates tasks, and verifies the final integration. Definition: [.github/agents/orchestrator.agent.md](../.github/agents/orchestrator.agent.md)

All agent definitions live under the repository's agent folder, [.github/agents](../.github/agents), and the workflow is coordinated through GitHub Copilot CLI in a Codespace so each specialist can focus on one part of the dashboard while the orchestrator keeps the effort aligned.
