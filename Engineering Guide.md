
#  **Engineering Guideline**

  

This document defines the required engineering workflow for all projects under the seanslab stack. The standard is “firmware bring-up discipline”: plan first, instrument the work, verify each stage, and only then declare completion.

---

## **0) Workspace and Project Structure**

1. **Root work folder (authoritative):** ROOTDIR - project by project
    
2. **New project setup:**
    
    - Create a new folder under ROOTDIR
        
    - Initialize git
        
    - Create and configure the GitHub remote
        
    
3. **Every project must have a GitHub repo — no exceptions.**
    

---

## **1) Planning and Design First (No Blind Execution)**

1. **Plan mode by default**
    
    - Enter plan mode for any non-trivial task (≥3 steps, or any architectural decision).
        
    - Plan must include: interfaces, constraints, failure modes, rollback plan, and verification steps.
        
    
2. **Design first**
    
    - Start with a written SDD (System Design Document) before meaningful implementation.
        
    - Reduce ambiguity up front; do not “discover requirements” in the middle of coding.
        
    
3. **Re-plan immediately when reality diverges**
    
    - If assumptions break, behavior is unexpected, or requirements are unclear: stop and re-plan.
        
    - Do not keep pushing forward in a broken state.
        
    

---

## **2) Execution Model (Parallelize, Keep Context Clean)**

1. **Subagent strategy**
    
    - Use subagents liberally to keep the main context clean.
        
    - Offload research, exploration, tradeoff analysis, and parallel debugging hypotheses.
        
    - One subagent = one tack. No mixed missions.
        
    - For complex problems, allocate more compute rather than polluting the main thread.

    - Subagents must work hard toward the truth: no lazy conclusions, no performative progress, no fabricated certainty. Require evidence, checked reasoning, and an honest account of uncertainty or failed paths.
        
    
2. **Parallel execution after planning**
    
    - For complex tasks, once the plan is locked, scan it for steps that are independent and can run simultaneously instead of sequentially.
        
    - Run independent tool calls in a single batch (file reads, greps, builds, tests, network probes) — never serialize work that has no data dependency.
        
    - Hire subagents when a sub-task is large, context-heavy, or specialized (deep research, exploring an unfamiliar codebase, comparing tradeoffs across options). Each subagent gets one focused mission so its result is clean and reusable.
        
    - Only hire subagents for sub-jobs that are clearly scoped, small enough to verify independently, and safe to run in parallel.
        
    - Every delegated sub-job must include its own success check, and the parent task is not done until that check was executed and reviewed.
        
    - **Quality bar — parallelism must not degrade execution quality.** Before parallelizing, verify:
        
        - No write conflicts: parallel tasks must not touch the same files, branches, processes, or shared state.
            
        - No hidden order dependency: a step that depends on another step’s output must wait for it — never guess or use placeholders.
            
        - Each parallel branch is independently verifiable: results can be checked in isolation, and a failure in one does not silently corrupt the others.
            
        - If any of these constraints are unclear, run sequentially. Speed never beats correctness.
            
        
    
3. **Autonomous problem solving**
    
    - Solve issues independently whenever possible.
        
    - If uncertain, raise the issue with recommended solution options and let the user choose.
        
    

---

## **3) Test-First Verification and Definition of Done**

1. **Unit tests are mandatory**
    
    - Write unit tests for each feature and run them.
        
    - For engineering tasks, break the work into small, clear sub-jobs and define at least one concrete verification step for each sub-job before implementation.
        
    - Execute the test/check for each sub-job before marking it complete.
        
    - If tests pass: proceed.
        
    - If tests fail: fix and re-run until all tests pass, or explicitly report the error, impact, and blocker if it cannot be resolved in the current task.
        
    
2. **Never mark “done” without proof**
    
    - Run tests, inspect logs, demonstrate expected behavior.
        
    - Diff behavior between baseline and changes when relevant.
        
    - Staff-engineer bar: _Would a staff engineer sign off on this?_ If not, it’s not done.
        
    
3. **Always test**
    
    - No feature ships without verification (unit tests minimum; integration/system tests when applicable).
        
    

---

## **4) Source Control Discipline (Progress Must Be Recoverable)**

1. **Commit and push periodically**
    
    - Push to GitHub regularly to preserve progress and enable rollback.
        
    - Commits should be small enough to isolate cause/effect.
        
    
2. **Minimal impact changes**
    
    - Touch only what’s necessary.
        
    - Avoid broad refactors unless explicitly planned and justified.
        
    

---

## **5) Logging, Reporting, and Operational Visibility**

1. **Maintain dev logs (required)**
    
    - Log achievements, discoveries, and mistakes continuously in the project folder:
        
        - devlog-YYYYMMDD.md (e.g., devlog-20260218.md)
            
        
    
2. **Visible progress**
    
    - When starting any code agent, keep a terminal window open so progress and prompts are visible in real time.
    - For big tasks, e.g. downloading big files, compile big source tree, etc, report at every 15% percent, keep user updated on the progress.
    - When downloading big files to a target system, select the faster path — possibly download on the working machine (e.g. MacBook) first, then transfer to the target machine, rather than downloading directly on the target over a slow connection.


3. **Proactive reporting**
    
    - Report progress periodically without waiting to be asked:
        
        - what was done
            
        - what’s next
            
        - blockers / risks
            
        - verification status (tests/logs)
            
        
    

---

## **6) Task Management (Checkable, Granular, Enforced)**

1. **Plan first**
    
    - Write the plan to tasks/todo.md with checkable items and acceptance criteria.
        
    - Break engineering work down until each sub-job is small enough to execute, verify, and reason about independently.
        
    - Each sub-job must state its verification method up front (test, build, log check, repro, benchmark, or manual validation).
        
    
2. **Verify plan before implementation**
    
    - Quick sanity check: scope, risks, test strategy.
        
    - Mark which sub-jobs can run in parallel and which must remain sequential because of data, state, or file dependencies.
        
    
3. **Track progress**
    
    - Mark items complete as you go; keep the list accurate.
        
    
4. **Explain changes**
    
    - Provide high-level summaries at meaningful steps (what changed, why).
        
    
5. **Document results**
    
    - Add a review section in tasks/todo.md:
        
        - tests run
            
        - outcomes
            
        - known limitations
            
        
    
6. **Capture lessons**
    
    - After any user correction, update tasks/lessons.md with a preventative rule to avoid repeating the mistake.
        
    - Review relevant lessons at session start.
        
    

---

## **7) Disk Hygiene (End-of-Day Cleanup)**

1. **Check for temp file growth**

    - At the end of each work session, audit all machines used for new temporary files, caches, build artifacts, and downloaded intermediates.

    - Common culprits: `__pycache__/`, `.cache/`, `*.pyc`, `pip` download caches, HuggingFace cache (`~/.cache/huggingface/`), Docker dangling images, `tmp/` directories, log files, `.nohup.out`.


2. **Delete aggressively, confirm if risky**

    - Delete all clearly temporary files (build artifacts, pip caches, intermediate outputs) without asking.

    - If a file or cache might be needed later (e.g., a large downloaded model, a dataset), confirm with the user before deleting.


3. **Disk space must not grow from daily work**

    - The goal is zero net disk growth from temporary/cache files across work sessions.

    - After cleanup, report disk usage before vs after so the user can verify.

    - On constrained devices (Jetson Orin, RK3588), this is critical — small root partitions fill up fast.


---

## **8) Engineering Standards (Embedded Mindset)**

1. **Simplicity first**
    
    - Minimal change, minimal surface area, minimal regression risk.
        
    
2. **No laziness**
    
    - Find root causes. Avoid temporary hacks unless explicitly triage with a follow-up item.
        
    
3. **Demand elegance (balanced)**
    
    - For non-trivial changes: pause and consider a cleaner, more robust solution.
        
    - If a fix feels hacky: implement the correct design given current knowledge.
        
    - For small obvious fixes: do not over-engineer.
        
    
4. **Autonomous bug fixing**
    
    - For bug reports: diagnose, patch, verify, and report without hand-holding.
        
    - Anchor on evidence: logs, errors, failing tests, reproduction steps.
        
    - If CI fails: fix it without being told.
        
    

---

## **9) Requirements Capture**

1. **Log every user requirement in `Requirements.md`**

    - When the user issues a requirement, briefly add it as its own entry in `Requirements.md`.
    - Keep newest on top (prepend above the most recent ID).
    - Put the agent's reply, in brief, into the same entry so the decision lives next to the requirement.


---

## **Summary: Non-Negotiables**

- Design + plan first (SDD + tasks/todo.md)
    
- GitHub repo for every project
    
- Tests written and passing before “done”
    
- Frequent commits/pushes
    
- Proactive devlog + progress reporting
    
- Lessons captured in tasks/lessons.md
    
- Minimal-impact changes, root-cause fixes, staff-engineer quality bar

- End-of-day disk cleanup: delete temp files/caches, confirm before removing anything risky

- Every user requirement logged in `Requirements.md` (newest on top) with the agent's brief reply in the same entry
