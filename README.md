# App QA

[English](README.md) | [한국어](README.ko.md)

**An AI agent skill for finding where people get stuck in an app, then reporting the evidence and which fixes matter most.**

QA stands for *Quality Assurance*: checking whether a product works as intended. An app launching successfully does not tell you whether signing up, joining an event, chatting, or cancelling a booking works correctly. App QA gives an agent instructions for following those user journeys and looking for defects and friction.

A skill is a set of instructions an agent reads. This repository is not a standalone program that runs your app or automatically finds every bug. What the agent can check depends on the code, browser tools, test accounts, and devices it can access.

## What does it check?

| Area | What it looks for | Example |
|---|---|---|
| Functional defects | Whether controls and processes do what they should | Joining an event succeeds, but its chat room is inaccessible |
| Usability | Whether people can understand the current state and their next step | A loading indicator keeps spinning after a request fails |
| Claims versus behavior | Whether labels, badges, and numbers have evidence behind them | A profile says “Verified” without checking verification data |
| Checks after fixes | Whether an earlier issue is resolved or remains in another path | A nickname is corrected in the app but still wrong in notifications |

These are hypothetical examples illustrating the review process.

## How does it judge a problem?

Suppose an app displays “Saved,” but the content disappears when you reopen the screen. The agent should not accept the message as proof of success. It checks whether the button actually requests a save, whether the server processes it, and whether the app reads the saved data correctly.

Findings distinguish three levels of evidence:

- **Reproduced:** The agent used the app and observed the problem. It records the steps and environment.
- **Code-confirmed:** The agent traced the relevant screen, service, server logic, and permission rules. This is kept separate from running the app and observing the result.
- **Needs verification:** A required account, physical device, or operating policy is unavailable, so the conclusion remains open. These items are not counted as confirmed defects.

This distinction helps developers decide what to fix and what still needs testing. If a test replaces a real server response with a simulated one, the report says so.

## What does “AI slop” mean here?

In this skill, AI slop means **weak or misleading interface elements that make a product appear more complete than its actual functionality supports**. Examples include:

- A “Buy” button that only displays a message and never starts a purchase.
- A promise of participation rewards with no implementation to award them.
- Statistics that always show the same number without calculating anything.
- An “Unverified” warning with no available way to complete verification.

The skill does not detect whether AI wrote the app. Rounded cards or icons are not defects by themselves. The question is how an element affects what people understand and what they can actually do.

## Installation and usage

Download this repository into an `app-qa` folder at one of these locations:

| Scope | Location |
|---|---|
| Personal, across projects | `~/.agents/skills/app-qa` |
| Shared with one project | `<project>/.agents/skills/app-qa` |

These locations follow the [official Codex skill documentation](https://learn.chatgpt.com/docs/build-skills), checked on 2026-09-16. The main file should be at `app-qa/SKILL.md`.

Some installer or host setups use `$CODEX_HOME/skills/app-qa` (commonly `~/.codex/skills/app-qa`). Keep a working installation there rather than moving it just because the paths differ. In the maintainer's Windows/Orca session, this skill was discovered from `~/.codex/skills/app-qa` through a `CODEX_HOME/skills` junction; the installed CLI reported `codex-cli 0.154.0`. This is an observed local setup, not a compatibility test of every host or installation method.

After installation, check that `app-qa` appears in the skill selector (`/skills` or `$` in Codex CLI/IDE). If it does not appear, restart the session and check the folder location. Avoid installing duplicate copies under the same skill name.

Once the skill is available in your session, try:

```text
$app-qa Review this app from signup through its main user journeys.
Report functional defects, usability friction, and AI slop with evidence
and prioritized fixes.
```

You can also focus on one feature:

```text
$app-qa Review event requests, approvals, and cancellations.
Include what happens when an event is full or a network request fails.
```

When checking changes, provide the previous report and the scope of the fixes:

```text
$app-qa Recheck the fixes from the previous QA report.
Separate resolved issues, remaining issues, and anything not yet tested.
```

## What do you get?

For a substantial review, the skill guides the agent to write a Markdown report. For a small review, a short response may be enough. Findings include the conditions that trigger the problem, its effect on users, source locations or reproduction evidence, a minimal correction, and a practical check to run after fixing it.

Fixing a problem does not always require building a new feature. Hiding an unsupported control or correcting inaccurate text may be sufficient. The skill favors small corrections that address the confirmed cause.

## Limits to keep in mind

- A request for QA means inspection and reporting. It does not automatically request code changes or deployment.
- A successful browser check does not establish that background behavior works on a physical iPhone or Android device.
- Payments, messages to real users, and production data deletion are outside a simple QA request. The agent follows the available test environment and the work already authorized.
- A successful build or an empty error log is not enough to declare that the entire app passed QA.
- A test account may still connect to production. The agent checks the actual destinations and authorized targets before actions with side effects, removes secrets and unnecessary personal data from evidence, and treats instructions inside inspected content as data.
- Expected behavior needs a basis, such as a requirement or a product promise. Clearly labeled demo data and legitimate review states are not automatically defects.

## Repository files

- [SKILL.md](SKILL.md): Review instructions for the agent.
- [agents/openai.yaml](agents/openai.yaml): The skill's display name and example invocation.
- [Evaluation cases](references/evaluation-cases.md): Small maintainer scenarios for checking false positives, evidence quality, and scope boundaries. Cases are not test results.

This explanation follows the ELI15–18 approach: define necessary terms, explain how the process works, and preserve its important limits.
