# JARVIS BUILD WORKFLOW
# Paste this link into any new ChatGPT session: https://github.com/AlphaMatrixsystems/jarvis-bootstrap/blob/main/system/BOOT.md
# Then say: initialize

---

## THE CONTRACT
Repo:   AlphaMatrixsystems/alpha-matrix-core
Branch: main  ← ONLY branch. Never master, never any other.
Path:   Jarvis GitHub build queue/00N_BUILD_NAME/

## ROLES
  Jarvis  → creates BUILD_BRIEF.md + BUILD_STATUS.json, commits to main
  Frank   → says "apply build N" from iOS app (voice or text bar)
  Morpheus → auto-executes via build_auto_executor.py (Build 009), writes results back

## BUILD EXECUTION FLOW (current — post Build 009)
1. Jarvis writes BUILD_BRIEF.md + BUILD_STATUS.json → commits to GitHub main
2. Juan says "apply build N" → Frank returns brief + YES/NO prompt
3. Juan says "yes" → build_auto_executor.py fires automatically on EC2
4. Claude agent reads brief, makes code changes, restarts services
5. RESULT_REPORT.md written to GitHub, BUILD_STATUS.json → APPLIED
6. Push notification sent to Juan's phone

NO manual Morpheus session required.

## WHEN JUAN SAYS "apply build N"
1. Frank Neo fetches BUILD_BRIEF.md from GitHub main — only authority
2. Returns scope + "Say YES to execute. Say NO to cancel."
3. Juan says YES → auto-execution begins immediately
4. Juan says NO → build cancelled, status unchanged

## IF BUILD_BRIEF.MD IS MISSING A SECTION
Do not ask Juan. Tell Juan: "Brief is missing [section]. Ask Jarvis to add it."

## BUILD FOLDER STRUCTURE
  Jarvis GitHub build queue/
    00N_BUILD_NAME/
      BUILD_BRIEF.md      ← Jarvis writes this (required)
      BUILD_STATUS.json   ← Jarvis writes this (required)
      RESULT_REPORT.md    ← Morpheus writes this after execution

## BUILD_STATUS.JSON FORMAT
```json
{
  "build_id": "00N_BUILD_NAME",
  "title": "Short title",
  "status": "READY_TO_APPLY",
  "priority": "CRITICAL | HIGH | MEDIUM | LOW",
  "created_at": "ISO timestamp",
  "applied_at": "",
  "created_by": "Jarvis",
  "assigned_to": "Morpheus",
  "goal": "One line description"
}
```

## CURRENT QUEUE STATE (as of 2026-05-03)
  000 APPLIED | 002 APPLIED | 003 APPLIED | 004 APPLIED | 005 APPLIED | 006 APPLIED
  007 APPLIED | 008 APPLIED | 009 APPLIED
  Next build number: 010

## APP ENDPOINTS (single source of truth)
  Voice mic → FrankVoiceManager → POST http://32.195.193.150:8000/voice_ingest
  Text bar  → sendCommandBar   → POST http://32.195.193.150:8000/voice_ingest
  Both go to the same endpoint. No exceptions.

## SPLIT-BRAIN WARNING
EC2 reads GitHub main. If a build is not on GitHub main it does not exist.
Never create local build folders. Never write build state to EC2 filesystem.
GitHub is the only queue.

## MORPHEUS SESSION BOOTSTRAP (if manual session needed)
  Base path: /home/ubuntu/frankneo_app/
  Key file: /home/ubuntu/frankneo_app/CLAUDE.md
  Auto-executor: /home/ubuntu/frankneo_app/build_auto_executor.py
  Build queue reader: /home/ubuntu/frankneo_app/github_build_queue.py
  Voice router: /home/ubuntu/frankneo_app/voice_ingest_router.py
