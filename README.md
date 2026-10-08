# /potetofy

An agent skill that audits any team of AI agents, or any single agent workflow, against the way [@poteto](https://x.com/poteto) (Lauren Tan) runs her bot fleet.

It reads how your agents are actually set up and how they actually behave, then checks them on eight questions: how much trust each one has earned, whether each has one clear job, whether managers delegate, whether work gets checked independently, whether corrections become structure, whether briefs give goals rather than scripts, when the human gets pulled in, and what it all costs. You get back a findings table and a plan split into reversible fixes and calls that need you. It works for coding teams, content, research, ops, or any mix, and it changes nothing until you pick which items to apply.

## Install

Copy the `potetofy/` folder (both `SKILL.md` and `reference.md`) into your agent's skills directory, or ask your agent to save it as a skill. Then run `/potetofy` and point it at your agent team or a workflow.

## Credit

All of the method comes from poteto. Watch her talk, [here's how i shipped 2,500 PRs last month to production](https://x.com/poteto/status/2102050467505430555), and check out [pstack](https://github.com/cursor/plugins/tree/main/pstack). Her [Dr. Eggbot](https://x.ai/bot/marketplace/bots/dr-eggbot-v2) bot runs ongoing health checks on a bot team; /potetofy is a one-shot audit against her full method and pairs well with it. Sources for every rule are cited in `reference.md`.
