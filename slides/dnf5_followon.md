# DNF5 — Continuation from the RPM demo

---

## Slide 1 — Title / Intro

Title: DNF5 — Continuation from the RPM demo

What is DNF?
- DNF (Dandified YUM) is the high-level package manager for RPM-based systems: it manages repos/metadata, resolves dependencies, downloads packages, and delegates installs to rpm (via libdnf).

Speaker notes:
- One-liner opener (≤30s): "DNF is the conductor; RPM is the engine — RPM handles low-level package files and the rpmdb; DNF manages repos, dependency resolution, transactions and provides a programmatic API."
- Keep this short and explicitly reference the earlier RPM demo so the audience sees continuity.

---

## Slide 2 — Comparison (compact table)

Title: DNF5 vs RPM — Quick comparison

Feature | DNF5 (short) | RPM (short)
---|---:|---
Repository management | Installs/queries from configured repos (remote metadata) | Only local .rpm files; no repo metadata
Dependency resolution | Automatic solver (libsolv) picks and fetches deps | No auto-resolve — manual provision of deps
Transactions & history | Records multi-package transactions; can inspect/attempt rollback (dnf5 history) | Single-package operations; no transaction history/rollback tooling
Parallel downloads & performance | Parallel downloads & libdnf5 performance improvements | No download/solver stage (single-package tool)
Programmatic API | libdnf (libdnf5): high-level API for tools & plugins | Low-level package APIs; no repo/solver/transaction orchestration
Repo-centered queries | check-update, repoquery, upgrade planning | Focused on installed/package-file queries
Auditability | Transaction audit trail (who/when/what) | No transaction-level audit trail

Speaker notes:
- For the live demo, prioritize showing three rows live: Repos, Solver (install), History/Rollback.
- Use rpm -q before and after a dnf5 install to prove DNF5 delegates to RPM (rpmdb updated).

Demo command short list (for speaker notes or slide footer):
- rpm -q htop
- dnf5 --version
- sudo dnf5 install -y htop
- rpm -q htop
- dnf5 history
- dnf5 history info <txid>
- sudo dnf5 history rollback <txid>  (only if safe)

---

## Slide 3 — Architecture diagram (inserted image)

Title: DNF5 architecture — core components

Speaker notes:
- Walk the flow: Repos → librepo fetches metadata & packages → libdnf orchestrates, calls libsolv to solve → libdnf uses librpm to perform low-level installs that update rpmdb and system files.
- Call out: libdnf5 provides transaction manager + history; libsolv does SAT-based solving; librepo handles downloads and mirrors.
