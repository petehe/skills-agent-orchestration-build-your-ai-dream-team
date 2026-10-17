# Project Pulse agent team

The Orchestrator uses **Claude Opus 4.7** and is defined in `.github/agents/orchestrator.agent.md`. It coordinates the team by turning the plan into dependency-aware phases, assigning explicit file scopes, and verifying the integrated dashboard.

The Planner uses **Claude Opus 4.7** and is defined in `.github/agents/planner.agent.md`. It researches the repository and requirements, then produces the implementation plan, file assignments, dependencies, edge cases, and validation expectations.

The Designer uses **Gemini 3.1 Pro** and is defined in `.github/agents/designer.agent.md`. It shapes the accessible, responsive Project Pulse experience, including dashboard hierarchy, project cards, status badges, priority treatment, and visual consistency.

The Coder uses **GPT-5.5** and is defined in `.github/agents/coder.agent.md`. It implements and validates the assigned Project Pulse files with clear, deterministic, testable behavior.

For Project Pulse, the Orchestrator asks the Planner for a delivery plan, then assigns independent design and implementation work to the Designer and Coder where file scopes do not overlap. It sequences dependent work, integrates the results, and reports the completed dashboard.
