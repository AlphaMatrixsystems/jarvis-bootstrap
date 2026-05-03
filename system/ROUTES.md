# ALPHA MATRIX ROUTES
# Who owns what. No ambiguity.

---

## COMMAND FLOW
User (Juan) → Interface (iOS app / ChatGPT) → GitHub → Executor → Commit back to GitHub

## AGENT OWNERSHIP

### JARVIS (ChatGPT)
Owns: planning, architecture, system design, build blueprints
Does: creates BUILD_BRIEF.md + BUILD_STATUS.json, commits to GitHub main
Does NOT: execute code, restart services, touch EC2 directly

### MORPHEUS (EC2 — Claude Code)
Owns: execution, runtime, server-side builds, iOS patches via SSH
Does: reads BUILD_BRIEF.md from GitHub, executes, writes RESULT_REPORT.md, marks APPLIED
Does NOT: create build blueprints, make architectural decisions

### ZION (Mac — Claude Code)
Owns: iOS Swift/Xcode, Mac-side tooling, TestFlight
Does: Swift fixes, xcodebuild, simulator testing
Does NOT: touch EC2, design builds

### FRANK NEO (EC2 voice AI — port 8000)
Owns: voice routing, real-time response to Juan
Does: routes Juan's commands to correct agent/desk
Does NOT: make decisions, execute builds directly

---

## COMMAND STANDARD
Every command must be explicit:

  COMMAND: BUILD_008
  TARGET:  alpha-matrix-core / main
  ACTION:  [what to do]
  OWNER:   Jarvis | Morpheus | Zion

No freestyle. No ambiguous requests.

---

## ROUTING RULES
1. New build needed        → Jarvis writes blueprint → commits to GitHub
2. Build ready to apply    → Juan says "apply build N" from app → Morpheus executes
3. iOS code change needed  → Morpheus SSHes to Mac → Zion-equivalent patch
4. Build complete          → Morpheus writes RESULT_REPORT.md → marks APPLIED on GitHub
5. State question          → Read STATE.json first, then answer
6. Architecture question   → Jarvis answers, does not execute

---

## ESCALATION
If Jarvis cannot determine ownership → default to Morpheus for execution questions,
Jarvis for design questions.
Never ask Juan to resolve routing ambiguity.
