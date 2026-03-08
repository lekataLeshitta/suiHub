# suiHub — Project Plan

> Living document. Updated as decisions are made and milestones are hit.
> Last updated: March 2026

---

## 1. Project overview

**Name:** suiHub Move Auditor (working title)
**Type:** Developer tooling — static analysis for Move smart contracts on Sui
**Goal:** Build a usable, accessible tool that catches common Move vulnerabilities, gas issues, and bad patterns before deployment
**Target users:** Solo devs and small teams building on Sui who want a fast pre-deployment check

---

## 2. Problem being solved

Move smart contract development on Sui lacks accessible, automated security tooling:
- The Move Prover exists but requires formal specifications and significant expertise
- No simple CLI or web tool for catching common issues before deploying
- Developers either skip security checks or pay for manual audits
- The gap is a lightweight, opinionated linter with security awareness

---

## 3. Proposed solution

A two-layer tool:

### Layer 1: CLI tool
- Input: a `.move` file or directory
- Output: a structured list of issues with severity (error / warning / info), line numbers, and explanation
- Runs locally, no sign-in, no data sent anywhere
- Installable via `npm` or `cargo`

### Layer 2: Web UI
- Paste Move code, get instant analysis
- Shareable results (link to analysis)
- Free tier: core rules
- Pro tier: extended rule set, CI/CD integration, PDF reports

---

## 4. Detection rules (MVP scope)

### Security
- [ ] Missing `abort` codes (use of raw `abort 0` or no abort)
- [ ] Unchecked arithmetic (overflow/underflow risk)
- [ ] Missing access control on sensitive functions
- [ ] Reentrancy-style patterns
- [ ] Coin/balance handling without proper guards

### Code quality
- [ ] Unused variables or imports
- [ ] Functions with no return type where one is expected
- [ ] Missing module documentation
- [ ] Overly long functions (complexity signal)

### Gas
- [ ] Unnecessary vector copies in loops
- [ ] Repeated global reads (should cache in local)
- [ ] Unused objects not dropped properly

---

## 5. Tech stack decisions

| Component | Choice | Reason |
|-----------|--------|--------|
| Analysis engine | TypeScript (initially) | Faster to iterate; Move AST parsing via regex + heuristics first, full AST later |
| CLI wrapper | Node.js | Cross-platform, easy distribution via npm |
| Frontend | React + Vite + TypeScript | Standard modern stack, fast to build |
| Backend API | FastAPI (Python) or Node.js | TBD based on analysis engine choice |
| Sui integration | `@mysten/sui` SDK | Official, well-documented |
| Move parsing | move-compiler crate or custom parser | Phase 2 — start with text heuristics |

---

## 6. Milestones

### Phase 0 — Setup (current)
- [x] Fork MystenLabs/sui
- [x] Set up wiki and dev notes
- [x] Define project direction
- [x] Write project documentation (this file)
- [ ] Install Sui CLI locally (`suiup`)
- [ ] Set up local dev environment

### Phase 1 — Proof of concept (target: 4 weeks)
- [ ] Write first `.move` module (hello world on devnet)
- [ ] Build basic text-based scanner (regex rules for top 5 issues)
- [ ] CLI: takes a `.move` file, outputs issues to stdout
- [ ] Test against 10 real Move contracts from Sui examples

### Phase 2 — MVP (target: 8 weeks from Phase 1)
- [ ] Expand rule set to 15+ checks
- [ ] Basic web UI (paste code, get results)
- [ ] Hosted demo at a URL
- [ ] 20+ beta users from Sui community
- [ ] GitHub repo public with good README

### Phase 3 — Grant & scale (target: post-MVP)
- [ ] Apply for Sui Developer Grant ($10k–$100k)
- [ ] CI/CD integration (GitHub Action)
- [ ] Rule configuration file (`.auditrc`)
- [ ] Pro tier with extended features
- [ ] Mainnet deployment of any on-chain components

---

## 7. Monetization

| Model | Details |
|-------|---------|
| Free / open-source | Core CLI + web UI, basic rules, no sign-in |
| Pro subscription | Extended rules, CI/CD integration, PDF reports, priority support |
| Grant funding | Sui Foundation Developer Grant (non-dilutive, $10k–$100k) |
| Future: audit marketplace | Connect devs with auditors; tool provides pre-audit report |

---

## 8. Risks and how to handle them

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| Someone builds this first | Medium | Ship fast, focus on UX over features |
| Move AST parsing too complex | High | Start with heuristic text analysis, improve iteratively |
| No users find the tool | Medium | Embed in Sui Discord/Telegram naturally |
| Grant application rejected | Low–Medium | Apply early with working prototype |
| Scope creep | High | Stick to MVP definition, log ideas for later |

---

## 9. Community strategy

**Approach:** build in public, earn credibility through usefulness

**Phase 0–1:** Lurk and learn
- Join Sui Discord (#dev-help, #move-lang)
- Join Sui Telegram
- Answer questions where possible, take notes on pain points

**Phase 2:** Soft launch
- Share working demo in Discord/Telegram — no pitch, just "built this, here it is"
- Post devlog on X: what was built, what was hard, what's next
- Engage with feedback directly

**Phase 3:** Grow
- Open-source the CLI, invite contributions
- Write short posts on Sui forums about Move security patterns
- Build reputation as someone who knows Move security

---

## 10. Key resources

| Resource | URL |
|----------|-----|
| Sui docs | https://docs.sui.io |
| Move book | https://move-book.com |
| Sui SDK | https://sdk.mystenlabs.com |
| Sui examples | https://examples.sui.io |
| Sui forums | https://forums.sui.io |
| Grants | https://sui.io/programs-funding |
| RFPs | https://sui.io/request-for-proposals |
| Wiki | https://github.com/lekataLeshitta/suiHub/wiki |

---

## 11. Architecture notes (early thinking)

```
src/
  scanner/
    rules/          # individual detection rules
    parser.ts       # Move file reader + basic AST
    engine.ts       # runs rules against parsed code
    reporter.ts     # formats output
  cli/
    index.ts        # CLI entry point
  web/
    frontend/       # React app
    api/            # REST API wrapping scanner
```

Rule structure (each rule is a module):
```typescript
export interface Rule {
  id: string;           // e.g. "M001"
  name: string;         // e.g. "Missing abort code"
  severity: 'error' | 'warning' | 'info';
  check: (code: string) => Finding[];
}

export interface Finding {
  rule: string;
  line: number;
  message: string;
  suggestion?: string;
}
```

---

*Updated as the project progresses. Decisions logged here so context isn't lost between sessions.*
