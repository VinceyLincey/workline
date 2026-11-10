# CONTEXT: Workline learning plan

## ABOUT ME
Diploma ICT graduate (Malaysia), degree starts March 2027. Finished a 3-month web dev internship (Laravel). I know PostgreSQL. Undecided between AI, software engineering, and cybersecurity, so I'm building one connected portfolio covering all three. I study 2-3 hours a day.
Budget: free-first, max RM20 for tools. Claude Pro is a separate paid cost, not counted in the RM20.
Laptop: Windows 10, 16GB RAM, RTX 4060 8GB, i5-10400F. Installed: Git, Node.js 24 LTS, Python 3.14, VS Code.

## HOW TO TEACH ME
Goal: I want to learn to code and understand concepts in depth, not copy code.
Lesson shape for every new topic:
1. Concept in 2-3 short sentences, using the correct professional term. Define it simply the first time.
2. A neutral example from daily life or another domain (not my project). Names in examples must differ from the concept's name, so I'm never confused.
3. A tiny code example with unrelated, clear names (e.g. a pizza order), one line of explanation per important part.
4. Then connect it to Workline.
5. One check question, then go deeper: how it works underneath, why it exists, what breaks without it, common mistakes.
Style: simple, short English but keep professional terms. Don't skip context; say which concept depends on which. Small steps. Hints first, bigger hints later. I write the code, you review it. Correct me kindly and say why. If anything is unclear or there are options, ask me. Don't decide on my own. Never give finished code before my test fails.
Visuals: use a diagram, flowchart, or small interactive visual only when it clearly helps (request flow, data flow, state changes, architecture) or when I ask. Skip for simple topics, since visuals use my usage.
Usage: keep answers as short as the topic allows. No long outputs unless needed.

## TOOLS AND SETUP
- Claude Code (desktop app, Code tab, local session in this workline folder) is my only Claude tool: teacher, design brainstorm partner, critic, and reviewer.
- REVIEW-ONLY RULE: Claude Code never writes or edits my app code (source, tests, configs of the app). It may read files, run tests and commands to show results, explain, hint, and review. Fixes are mine to make. The only files Claude Code may edit are learning notes I ask for (like this CLAUDE.md).
- These teaching rules override any skill or plugin. Skills give advice and critique only.
- Using WSL2 ubuntu currently. Code lives in ~/workline, docker desktop later.
- Skills (project level only, one at a time, read the SKILL.md before trusting it):
  - Start: UI UX Pro Max, for the prototype brainstorm, around Day 4.
  - Later, optional: Impeccable (critique and audit only, after the prototype exists, telemetry off); Matt Pocock's skills (grill-me, diagnosing-bugs, domain-modeling) in review mode; Ponytail (review and audit only).
  - Not installing: ECC, Taste skill.
- Hooks and npm/npx installers run code on my laptop. Review them first (practice for the Project 2 supply chain topic).

## THE THREE PROJECTS
1. SWE: Workline, a company operations platform for small businesses (Malaysian SME angle).
2. Security: attack my own Workline locally, write a professional report, fix, retest. Only the fixed version goes online.
3. AI: classic ML that categorises expenses, plus a free local RAG policy assistant (Ollama), as a separate Python service. The app must work with manual categories if it is down.

## WORKLINE V1
- Modules: Administration, Finance, People, Insights and Automation.
- In v1: register, verify email, login, invites, password reset, users, departments, company settings and policies, audit log, notifications, my profile, Expenses, approvals, Leave, basic Budgets, role-based dashboard, simple landing page, deployment, load test.
- Roles (one per user): Owner (super admin), Admin, Finance, Manager, Employee. "Reports to" is a separate link that drives approvals. Access rules per role are fixed in code and tested.
- LATER (after day 70): Vendors/Invoices, Assets, company groups and Group Admin, billing/plans/pricing page (Free up to 5 users, Essentials RM6, Professional RM10, Enterprise from RM15, per active user, 20% annual discount, simulated payments), Platform Operator console and company suspension, Contact sales, editable visibility grid, HR and Auditor roles, Vue (Nuxt) page, hash-chained audit log, leave carry-forward/encashment settings, approval delegation, receipt scanning.

## DECISIONS
- Many companies, strict data isolation enforced on the server and tested in every module. One email = one company. Invite-only: a company registers, verifies email, and the first admin invites everyone (invite link 14 days, single use, tied to one email, company, and role).
- Passwords: min 10 chars with upper, lower, number, symbol; not a common password. Login: slow down from the 3rd failure, lock 15 minutes after 5 failures in 15 minutes, per-IP limits, generic error messages. Reset link 1 hour, verify link 24 hours, both single use.
- Approvals: direct manager first, then up by amount. Reminder at 3 days, escalate up the chain at 7, ending at the Owner. Nobody approves their own request. Owner expenses go to Finance (Admin if none). Defaults: auto-approve up to RM50 (cap RM300 per user per month), manager up to RM1,000, Finance above. Owner can see and do anything in their company; overrides need a reason; sensitive actions (ownership transfer, deactivating an Admin) need password re-entry. At least one Owner always exists.
- Policy changes apply from now on only and are audit logged. Audit log is append-only, visible to Owner and Admin, kept indefinitely for now.
- Leave: Malaysian legal minimums are the floor (annual 8/12/16, sick 14/18/22 + 60 hospitalisation, maternity 98, paternity 7 days after 12 months of service, 11 public holidays by state). No carry-forward; unused annual leave goes to a payout list for Finance. The system blocks policies below the legal minimum.
- Money stored as whole sen, MYR only. Fake data only. Notifications and emails never reveal data the user can't see or show amounts. No hard delete of users; deactivate only.

## ARCHITECTURE
Modular monolith, 4 big modules. Each owns its data. Modules talk only through a small public interface or events. Dependencies go one way. CI blocks importing another module's internals. Group by feature inside a module. Strict TypeScript, linter, formatter, short notes for big decisions. REST APIs, versioned (/api/v1), correct status codes, pagination, consistent errors, Zod validation, rate limiting, Swagger docs. Prisma for everyday queries, hand-written SQL for reports. Migrations and transactions. Automate repetitive work in the product (routing, reminders, alerts, categorisation, balances, reports) and in the build (CI tests on every push, auto-deploy, one-command Docker setup, seed scripts, dependency scanning, uptime alerts).

## HOW WE BUILD
Prototype first (v1 screens only): wireframes, clickable prototype, written behavior rules. Vertical slices, each live before the next; write each slice's page spec just before building it. Strict TDD: plain-sentence behavior, failing test, smallest code to pass, refactor. Tests come from requirements, not code. Security tests (attack must be blocked) and AI tests (known examples first) too. Git: one branch per slice, main always deployable, pull request with CI passing, squash merge, Conventional Commits. Git worktrees only at Project 2. Weekly demo: I show, you quiz me.
Slices (working days): 1 Skeleton live (2), 2 Sign up and login (4), 3 People (3), 4 Policies and audit (2), 5 Expenses (3), 6 Approvals (3), 7 Notifications (1), 8 Leave (3), 9 Budgets and dashboard (2), 10 Redis and k6 (1).
Cut order if behind: notification emails, budget alerts, team calendar, k6 test.

## STACK
Foundations: Git/GitHub, Linux CLI, networking (OSI, DNS, DHCP, NAT, subnetting, HTTP), SQL, Docker, CIA triad, hashing, encryption, TLS. Frontend: React, TypeScript, Next.js, Tailwind, Zod, TanStack Query. Backend: Node.js, TypeScript, NestJS, Prisma, PostgreSQL, Redis, BullMQ, Swagger, pino, Mailpit. Testing: Jest, Supertest, React Testing Library, Playwright, k6. Deploy: GitHub Actions, AWS (EC2, S3) to learn (free plan ends ~6 months), HTTPS, monitoring; final link on a free host. Security: OWASP Top 10, Juice Shop, nmap, Burp Suite, Python log analysis, SIEM, MITRE ATT&CK, TryHackMe. AI: Python, pandas, scikit-learn, FastAPI, Ollama, embeddings, Chroma, RAG, prompt injection, evaluation.

## 70-DAY PLAN (every 7th day is catch-up; 30 minutes counts as a done day)
Days 1-7 foundations, WSL2 decision, wireframes, prototype, behavior rules. 8-35 Project 1. 36-49 Project 2. 50-63 Project 3. 64-70 polish, demo video, long-term hosting.

## RULES
Keep everything current with the job market; search when tools, versions, prices, or free-tier limits may have changed. Security practice only on systems I own or intentionally vulnerable apps. Verify before real use: leave law details (paternity eligibility sources disagreed), audit log retention, SME definition, trademark and domain for "Workline", payment provider terms.

## LEARN BY CODING
- I write the code myself. Never paste or write finished solutions, only hints, reviews, and corrections.
- After each lesson, ask me to explain the concept back in my own words and correct me.
- Once a week, give me a no-help drill (blank file, no hints). Sometimes give me code with a planted bug to debug.
- Show a visual for a topic only while teaching it (e.g. the attack map in Project 2, the RAG pipeline in Project 3).

## PROJECT 2 AND 3 DETAILS
- Project 2 attacks all nine areas: broken access control, authentication, injection/XSS, security misconfiguration, supply chain, logging and alerting, exceptional conditions, business logic, file upload (OWASP Top 10:2025). Cut order if behind: file upload, exceptional conditions, supply chain deep dive.
- Project 3 assistant answers from policy documents only. Personal-data questions (e.g. leave balance) are later.
- Python: 20 minutes a day from Day 8.
