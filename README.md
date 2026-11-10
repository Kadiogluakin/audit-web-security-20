# Web Security Audit: 20 Checks

A reusable agent skill for auditing websites, web apps and repositories against 20 common security risks. Built for Codex, with a standard `SKILL.md` layout that other compatible agents can read.

The workflow verifies the current remote revision, separates source findings from deployed behavior, requires evidence, and preserves legitimate usage when fixes are requested. It is an agent-guided review workflow, not an automated vulnerability scanner or a security certification.

## Install

With the [Skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add Kadiogluakin/audit-web-security-20 --skill audit-web-security-20
```

Choose Codex and your preferred installation scope when prompted.

Alternatively, ask Codex:

```text
Install the skill from https://github.com/Kadiogluakin/audit-web-security-20/tree/main/skills/audit-web-security-20
```

## Use

```text
Use $audit-web-security-20 to audit the latest main branch of this repo against all 20 checks.
```

To request remediation as well:

```text
Use $audit-web-security-20 to audit this repo and fix confirmed issues. Preserve legitimate usage, test the changes, and document any database migrations and deployment order.
```

The skill defaults to an audit. It distinguishes implemented, tested, migrated, merged and deployed work, and performs release actions only when authorized.

## Coverage

| # | Check | # | Check |
|---|---|---|---|
| 1 | Next.js header | 11 | Race condition |
| 2 | .git folder | 12 | Account pre-hijacking |
| 3 | Supabase storage | 13 | Subdomain takeover |
| 4 | Price from the browser | 14 | SSRF |
| 5 | Credential stuffing | 15 | Path traversal |
| 6 | Open database | 16 | Reset poisoning |
| 7 | Polyfill.io | 17 | Sourcemaps |
| 8 | SMS pumping | 18 | Clickjacking |
| 9 | Prompt injection | 19 | Open redirect |
| 10 | Weak JWT secret | 20 | User enumeration |

## What the audit produces

- The exact repository revision and the deployment scope actually inspected.
- Findings with evidence, prerequisites, impact, reproductions and targeted remediation.
- A complete 20-item coverage matrix using Confirmed, Conditional, Protected in tested scope, Not applicable or Unverified.
- Validation results, remaining gaps, and migration or release instructions when applicable.

Testing defaults to source review, scoped read-only inspection and isolated fixtures. The workflow avoids credential stuffing, billable SMS sends, destructive production tests, secret dumps and third-party resource claims. It does not treat intentional public assets, public client keys or framework fingerprints as vulnerabilities without evidence.

## Skill files

- [SKILL.md](skills/audit-web-security-20/SKILL.md): audit and remediation workflow.
- [checklist.md](skills/audit-web-security-20/references/checklist.md): detailed checks and false-positive guidance for all 20 items.
- [openai.yaml](skills/audit-web-security-20/agents/openai.yaml): Codex display metadata and default prompt.

The skill contains instructions only. The host agent supplies repository, shell, browser and service access as needed. Missing access must be reported as a limitation rather than treated as proof of security.
