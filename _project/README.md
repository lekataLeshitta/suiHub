# suiHub — Move Smart Contract Auditor

> Fork of [MystenLabs/sui](https://github.com/MystenLabs/sui). This branch (`dev`) is my working space for building a static analysis tool for Move smart contracts on Sui.

---

## What this is

A tool that scans Move code and flags:
- Common vulnerabilities (missing abort codes, unchecked arithmetic, reentrancy patterns)
- Gas inefficiencies
- Code quality issues (unused variables, dead code, inconsistent error handling)

The goal is something devs can run before deploying — a quick sanity check, not a formal audit. Think of it as a linter with security awareness.

---

## Status

**Stage:** Pre-MVP / research & design

- [x] Project direction decided
- [x] Tech stack chosen
- [x] Wiki + dev notes set up
- [ ] First Move module written
- [ ] CLI prototype (takes `.move` file, outputs issues)
- [ ] Web UI (paste code, get results)
- [ ] Testnet deployment
- [ ] Public beta

---

## Tech stack

| Layer | Choice |
|-------|--------|
| Smart contracts | Move (Sui) |
| Frontend | React + Vite + TypeScript |
| SDK | `@mysten/sui` |
| Backend (analysis engine) | Node.js or FastAPI (TBD) |
| Deployment | Sui testnet → mainnet |

---

## Getting started (for contributors)

```bash
# Clone
git clone https://github.com/lekataLeshitta/suiHub.git
cd suiHub
git checkout dev

# Install Sui CLI
curl -fsSL https://raw.githubusercontent.com/MystenLabs/suiup/refs/heads/main/install.sh | bash
suiup install sui

# Verify
sui --version
```

---

## Syncing with upstream

```bash
git remote add upstream https://github.com/MystenLabs/sui.git
git fetch upstream
git checkout dev
git merge upstream/main
```

---

## Project docs

- [Wiki home](https://github.com/lekataLeshitta/suiHub/wiki) — dev notes, resources, project ideas
- [PLAN.md](./PLAN.md) — full project plan, milestones, architecture decisions
- [Sui Developer Docs](https://docs.sui.io)
- [Move language book](https://move-book.com)

---

## Why this / what I'm trying to do

I started trading SUI, got interested in actually building on it, forked the repo to learn from source. The auditor idea came from seeing how few accessible security tools exist for Move developers — most tooling requires deep formal methods knowledge. The gap is a simple, usable tool that catches the obvious stuff before it becomes a bug on mainnet.

---

*Work in progress. Updated as the project moves.*
