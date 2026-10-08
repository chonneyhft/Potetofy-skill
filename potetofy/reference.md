# Poteto's method, condensed

This reference is for the potetofy skill. It covers Lauren Tan (@poteto), engineering lead for Grok Bot and author of pstack. Every line cites a source.

## Sources

- **[talk]** "here's how i shipped 2,500 PRs last month to production". The talk itself says 2,000. https://x.com/poteto/status/2102050467505430555 (timestamps are mm:ss into the video)
- **[ep]** Behind the Craft with Peter Yang, "We Built Grok Bot. Here Are Our 14 Best Bots". https://youtu.be/xZ5TEaleUdg (timestamps mark speaker-turn starts)
- **[pstack]** https://github.com/cursor/plugins/blob/main/pstack/ followed by the file path
- **[x:ID]** https://x.com/poteto/status/ID
- **[eggbot]** Dr. Eggbot marketplace page. https://x.ai/bot/marketplace/bots/dr-eggbot-v2

## Thesis

- Trust caps how many agents you can run. Without trust, spawning 100 agents yields "a ton of slop requests and a bunch of regressions". [talk 06:15–06:44]
- Her goal is a "Michelin kitchen": quality at scale, not a slop factory. [talk 00:54–01:57] [ep 28:47]

## 1. Trust ladder

- Watch the bot work, correct it, and turn the result into a skill at the end of the conversation. When the slash command works in one shot, make it a routine. [ep 39:56] She confirmed this is her flow. [ep 42:27]
- Overnight loops have to earn trust first. You've done the task once by hand. The agent has your tools. Every stage proves its work. Repeated failures are already encoded. [pstack: docs/guide/07-overnight.md]
- She auto-merges on an agent-friendly codebase and sometimes reads a PR only after it lands. [ep 28:03]
- Her bug loop: watch the feedback channel, file a ticket, a cloud agent reproduces it, it fixes only on a clean repro, a small agent swarm fuzzes the PR, she gets pinged, and it auto-merges after 1 hour unless she objects. [x:2107510472601985336]

## 2. One job per bot, with anti-jobs

- Start with one bot. Add bots later, for specialization and organization. [x:2107830403549831186]
- Split a bot when its context gets "very diverse... maybe confusing". [ep 18:09]
- Bot bar: "one job, one voice, explicit anti-jobs, no leftover tools". For example, "a mentions scout does not post, a drafter does not send, and they remain quiet when there's nothing to say". [eggbot]

## 3. Managers delegate, cloud agents do the work

- Her chain runs: she talks to the chief of staff, which talks to the eng lead, which runs the engineer bots, which launch cloud agents. The eng lead's instructions list the bot IDs and say "never do work on your own and always delegate". [ep 25:33]
- The engineer bots are "supervisors" too. They always spawn cloud agents and pick models per task. [ep 25:33]
- Cloud agents "each have their own computer, it frees up your bot to be more of a manager". [x:2105377066942349794]
- Coordinators own "the program, never the code". Workers run in the cloud by default. Use fewer, broader workers, one writer per branch, and roughly 10 in flight at most. [pstack: skills/poteto-mode/playbooks/orchestrate.md]

## 4. Independent checks on the real product

- A verification skill has two parts: a CLI in the skill directory, and a feature map, which is "materialized memory" of what the app does. Together they became "critical infrastructure". [talk 08:41–12:20]
- "Verify every task output by checking the real thing directly", not proxies, self-reports or "it compiles". [pstack: skills/principle-prove-it-works/SKILL.md]
- "the agent that judges a change is never the one that wrote it." "Green is not the same as safe." [pstack: docs/guide/06-verify-and-ship.md]
- Run the verifier on a different model family from the worker. Verdicts are keyed by PR and head SHA, and a new SHA voids the verdict. "CI green is an input to a verdict, not a verdict." Size verification to the unit. [pstack: skills/poteto-mode/playbooks/orchestrate.md]
- Every stage can stop the line, and every stage hands over evidence. [pstack: docs/guide/07-overnight.md]

## 5. Correction ladder

- "whenever you correct your agent: 1. codebase 2. static analysis (lint/compiler/ci) 3. rules/bugbot 4. skills 5. 'style guide'". Work down that list in order. [talk 15:40–18:40, 36:07–37:09]
- Agents copy workarounds. "Each copy makes the next copy likelier." Teams need gardeners who delete tech debt, keep one paved path, and lint against anti-patterns. [talk 21:04–26:30]
- When you write the same instruction twice, turn it into a lint, a flag, a check or a script. "The instruction is the symptom." [pstack: skills/principle-encode-lessons-in-structure/SKILL.md]
- A mistake class counts after 2 repeats. Rank the fixes: architecture, then types or lint, then a test, then docs. [pstack: docs/guide/09-make-it-yours.md]
- Fleet audit: read the bots and their transcripts, then propose skills, new bots or routine changes. [ep 18:09] Dr. Eggbot runs a weekday friction scan and a weekly token and waste audit. Both stay quiet when there's nothing to report. [eggbot]

## 6. Goals and pass/fail checks

- "Give the agent a goal and a way to check it." Include the done check, the proof, what's already known, and the real constraints. Leave out the how and your theory. [pstack: docs/guide/02-poteto-mode.md]
- Brief fields: GOAL, SCOPE, CONTEXT, ACCEPTANCE, VERIFY, TIMEBOX, FORBIDDEN, REPORT, STANDING. "A field you cannot fill is a unit you have not scoped yet." [pstack: skills/poteto-mode/playbooks/orchestrate.md]
- Keep standing orders in one register and paste it into every spawn and resume. "Directives decay across resumes." [pstack: skills/poteto-mode/playbooks/orchestrate.md]
- Restate before acting: "restate in your own words what you think my goals are". [x:2104744961904394699]
- "a duration is not a finish condition." [pstack: docs/guide/07-overnight.md]
- Pilot one unit end to end before fanning out. [pstack: skills/poteto-mode/playbooks/orchestrate.md]

## 7. Human gates

- Proceed and present on reversible work. Confirm only irreversible actions. Product direction comes from the human. [pstack: skills/principle-never-block-on-the-human/SKILL.md]
- Escalate irreversible actions, product calls, and dead ends, batched into one status update. Never escalate retries, flake triage or "should I keep going". [pstack: skills/poteto-mode/playbooks/orchestrate.md]
- For purchases she asks the bot to "check in with me before you hit book". [ep 22:34]

## 8. Cost discipline

- Routines that run too often get expensive. "every 10 minutes, that's like a lot of wake ups." [ep 18:09]
- Use fast models for straightforward tasks and more reasoning for planning-heavy ones. [ep 25:33]
- "A strong model in the main chat with cheaper, faster models in the code roles is a good split." Save rigor for work that needs it. [pstack: docs/guide/01-setup.md]
- "A deterministic lever beats fan-out." [pstack: skills/principle-build-the-lever/SKILL.md] A control CLI means agents "spend fewer tokens". [pstack: docs/guide/06-verify-and-ship.md]
- Main contexts get summaries, not raw payloads. [pstack: skills/principle-guard-the-context-window/SKILL.md] Don't forward raw child reports. [pstack: skills/poteto-mode/playbooks/orchestrate.md]
- "A 4KB scaffold around a two-line edit costs more to write and obey than the edit." Probe status read-only instead of resuming an agent. [pstack: skills/poteto-mode/playbooks/orchestrate.md]

## Anti-patterns

- Scaling before trust. Babysitting every chat. [talk 05:28–06:44]
- Human style guides as the main gate. [talk 18:02]
- A "company brain" build-out. Connected tools are enough. [talk 33:09]
- Overloaded bots. Routines that fire too often. [ep 18:09]
- Listing skills or steps in a prompt. Vague done conditions. Polishing an abstract plan. Parallel agents sharing one worktree. Claiming success off a green build. Correcting the same mistake by hand. [pstack: docs/guide/10-recipes-and-pitfalls.md]
- "I'll keep that in mind" without recording it. Recording without routing it anywhere. [pstack: skills/principle-encode-lessons-in-structure/SKILL.md]
