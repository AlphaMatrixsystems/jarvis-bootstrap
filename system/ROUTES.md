# ALPHA MATRIX ROUTES
# Who owns what. No ambiguity.

---

## COMMAND FLOW
User (Juan) → iOS App (voice mic OR home screen text bar) → EC2 port 8000 /voice_ingest → Morpheus

IMPORTANT: Both voice and text bar go to the SAME endpoint: port 8000 /voice_ingest
There is NO other active input path. Port 8011 is deprecated for user input.

---

## AGENT OWNERSHIP

### JARVIS (ChatGPT)
Owns: planning, architecture, system design, build blueprints
Does: creates BUILD_BRIEF.md + BUILD_STATUS.json, commits to GitHub main
Does NOT: execute code, restart services, touch EC2 directly

### MORPHEUS (EC2 — Claude Code)
Owns: execution, runtime, server-side builds, iOS patches via SSH
Does: reads BUILD_BRIEF.md from GitHub, executes, writes RESULT_REPORT.md, marks APPLIED
Does NOT: create build blueprints, make architectural decisions
Auto-execution: Build 009 enables Claude agent to auto-execute on YES confirmation

### ZION (Mac — Claude Code)
Owns: iOS Swift/Xcode, Mac-side tooling, TestFlight
Does: Swift fixes, xcodebuild, simulator testing
Does NOT: touch EC2, design builds

### FRANK NEO (EC2 voice AI — port 8000)
Owns: voice routing, real-time response to Juan
Does: routes Juan's commands to correct desk via /voice_ingest
Entry points: voice mic (FrankVoiceManager) + home screen text bar (sendCommandBar)
Does NOT: make decisions, execute builds directly

---

## BUILD EXECUTION FLOW (post Build 009)

1. Jarvis writes BUILD_BRIEF.md + BUILD_STATUS.json → commits to GitHub main
2. Juan says "apply build N" from app (voice or text bar)
3. Frank Neo returns brief + "Say YES to execute. Say NO to cancel."
4. Juan says "yes"
5. build_auto_executor.py fires in background — Claude agent reads brief, executes, marks APPLIED
6. Push notification fires in app when done

NO manual Morpheus session required after Build 009.

---

## COMMAND STANDARD
Every command must be explicit:

  COMMAND: BUILD_010
  TARGET:  alpha-matrix-core / main
  ACTION:  [what to do]
  OWNER:   Jarvis | Morpheus | Zion

No freestyle. No ambiguous requests.

---

## ROUTING RULES
1. New build needed        → Jarvis writes blueprint → commits to GitHub main
2. Build ready to apply    → Juan says "apply build N" from app → auto-executes via build_auto_executor.py
3. iOS code change needed  → Morpheus SSHes to Mac → Zion-equivalent patch
4. Build complete          → RESULT_REPORT.md written → marked APPLIED → push notification sent
5. State question          → Read STATE.json first, then answer
6. Architecture question   → Jarvis answers, does not execute

---

## ESCALATION
If auto-executor cannot complete a build → notification sent → Juan opens Morpheus session manually.
Never ask Juan to resolve routing ambiguity.
