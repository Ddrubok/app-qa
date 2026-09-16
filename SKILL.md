---
name: app-qa
description: Review an application's user journeys for functional defects, usability friction, and misleading or nonfunctional presentation (AI slop), with evidence and prioritized fixes. Use for app/web QA, usability audits, post-fix regression review, or requests such as "앱 QA", "사용하면서 불편한 점", and "AI slop 점검". Do not trigger a full audit for an ordinary isolated bug fix, general QA questions, or game presentation work.
---

# App QA

Produce a review the user can act on: what fails, under which conditions, how it affects a person, what evidence supports it, and the smallest adequate correction. Match the user's language and requested depth. Do not invent a target number of findings.

## Scope and evidence

- Identify the actual project, platform, build, and important user journeys. Read applicable project instructions and existing QA/fix reports before treating an old issue as current.
- A QA-only request permits inspection, local checks, and report artifacts; it does not itself request app changes or deployment. If the user also requests fixes, carry out that authorized scope rather than stopping at a report or asking again.
- Use available test accounts, mocks, or emulators for actions that would create accounts, send messages/mail, charge money, submit reports, or delete data. Do not treat a live product review as authorization for those effects. Mocked success demonstrates only the mocked portion of the flow.
- If credentials or devices are unavailable, continue independent checks. Describe the missing coverage rather than fabricating a reproduction or declaring the whole review blocked.
- When comparing deployed and local behavior, identify the tested build if practical. A matching web bundle, active function, successful build, or passing unit suite is not proof that all workflows work.

## Trace complete user journeys

Choose relevant journeys rather than mechanically auditing every feature. Typical paths include first visit → signup/recovery → discovery → join/approval → conversation → cancellation/settings/account exit.

For a suspected defect, follow the visible control through state, service, backend, and access rules as applicable. Check sibling callers and alternative entry points, including notifications and profile previews. A missing UI guard may be enforced by server rules; distinguish a bypass from an unfriendly error. A timer label is not server expiry enforcement.

Cover meaningful boundaries for the feature being inspected:

- First-time and returning users; partial signup or interrupted requests.
- Loading, empty, error, retry, duplicate taps, and stale screens.
- Capacity, deadlines, cancellation, role changes, and concurrent requests.
- Separation of identities/permissions across screens, alerts, and shared records.
- Platform-specific expectations such as mobile keyboards, background behavior, browser refresh/back, and notifications.

Use browser/device interaction for visible behavior when available. Inspect screenshots for actual clipping, readability, hierarchy, and action visibility; do not infer visual defects solely from widget names. Check labels, keyboard/focus access, and text scaling where relevant. Use existing test tooling; add a small local reproduction only when it provides useful evidence. Do not install a large QA stack for a few checks.

## AI slop and trust review

Judge observable quality, not whether AI authored the product. Look for:

- Buttons that claim to load, save, purchase, or submit but only display a toast or placeholder.
- Verification/safety badges that lack evidence or treat missing data as a positive result.
- Rewards, refunds, guarantees, or availability claims unsupported by the current implementation and operating policy.
- Metrics whose label differs from the calculation, fabricated sample values, or permanent placeholder statistics presented as working features.
- Warnings or "unverified" labels with no available way for the user to resolve them.
- Repeated explanatory copy, decorative statuses, and competing calls to action that demonstrably obscure the user's next step.

Rounded cards, icons, gradients, whitespace, or a familiar layout are not defects on their own. Do not infer that a brand name is obsolete from the repository name. Hidden legacy code is not automatically a user-facing issue. Treat operational promises as needing policy verification when the relevant external process is unavailable.

Prefer removing unsupported controls or correcting copy over adding a speculative subsystem. Do not replace one unsupported promise with another, such as inventing a reward or cancellation deadline. Preserve needed error handling, accessibility, and safety behavior.

## Validate findings

Classify evidence explicitly:

- **Reproduced:** observed behavior, steps, environment, and actual result. State any mocked dependency.
- **Code-confirmed:** traced reachable behavior and relevant enforcement, with file/line evidence; not claimed as runtime reproduction.
- **Needs verification:** a plausible issue requiring an account, device, policy, or missing context. Keep separate from confirmed defect counts.

Test the strongest alternative explanation before retaining a finding. For example, a host leaving a chat is not necessarily permanent exclusion if they can rejoin; an ordinary member's over-capacity write may already be rejected by rules. Recheck worker findings against source and evidence before using them. Follow project delegation rules when workers are available; this skill does not require a particular team or tool.

Compare prior findings with current code and evidence: resolved, still present, residual path, or not retested. Do not relabel all earlier issues as current or imply regression coverage that was not run.

Prioritize by user harm and likelihood, not by how dramatic a title sounds. Use existing project severity conventions when available; otherwise P1 for major functional/trust harm, P2 for ordinary failures/friction, and P3 for minor presentation issues. Mark an urgent blocker separately only when evidence warrants it.

## Deliverable

For a substantial review, write a Markdown report in the project's existing report location or a clearly named QA file. Avoid overwriting a prior review's evidence. A short review can be delivered inline.

Include:

1. Tested version/platform, scope, successful checks, mocks, and coverage limits.
2. Prioritized findings with stable IDs, affected journey, trigger, expected/actual behavior, user impact, evidence, and minimal correction.
3. Distinguish functional defects, usability concerns, and AI slop examples without counting the same root issue multiple times.
4. A practical recheck for each correction, and separate unresolved policy/device questions.

Use a compact summary table when useful: `ID | Priority | Problem / impact | Evidence level | Minimum fix`. Put longer source references and reproduction steps beneath it. Do not force headings or lengthy reports for small tasks.

End with the highest-impact findings, a report link when created, and the important validation limits. Explicitly distinguish review artifacts from implemented fixes. Do not call the application QA-passed based only on compilation, no console errors, or a mocked happy path.
