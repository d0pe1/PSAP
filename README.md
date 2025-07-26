# **PSAP – PowerShell-Agent Preconfig**

### *A self-scoping, self-planning, self-iterating blueprint for building Codex agent repos*

PSAP is a **meta-template repository** designed to bootstrap **sticky Codex agent repos** (like your G.A.M.M.A. Warfare project).  
It provides the full **preconfiguration + task orchestration layer** so Codex agents can self-plan, self-scope, and self-iterate without you doing the manual heavy lifting.  

---

## **What Does PSAP Do?**

1. **Zero-Step Planning**  
   - Reads `goal.md` on startup.  
   - MD5 hashes the file: if it changed → re-plans the entire project.  
   - Expands/updates `agent_tasks.md` and `agent_prio.md` automatically.  
   - Handles enumerated **Additions** (`Add 1`, `Add 2`, etc.) for new scope injection.

2. **Recursive NP→P Task Decomposition**  
   - Treats large NP-complete project goals as **parent tasks**.  
   - Splits them into smaller P-complete subtasks using **indentation + weights**.  
   - Maintains clean, prioritized task lists in `agent_tasks.md` and `agent_prio.md`.  

3. **Sticky Codex Workflow**  
   - Codex agents use PSAP's config files for every pass:  
     - Reads `agents.md` → knows its roles.  
     - Reads `agent_tasks.md` → finds highest-weight task.  
     - Reads `agent_prio.md` → resolves blockers.  
   - After each run, cleans up completed subtasks, updates `DevDiary.md` and `CHANGELOG.md`.  

---

## **Repo Structure**

📂 PSAP/
├─ 📄 goal.md # Defines project intent + user Additions
├─ 📄 agent_tasks.md # Weighted top-level tasks
├─ 📄 agent_prio.md # Expanded blockers + subtasks
├─ 📄 agents.md # Agent roles & recursive task logic
├─ 📄 DevDiary.md # Prescope notes & implementation history
├─ 📄 CHANGELOG.md # One-line summaries per task
├─ 📄 usertodo.md # Manual tasks / tests required
├─ 📂 docs/ # Generated docs: api_map.md, system_design.md
└─ 📂 runtime_crashes/ # Crash logs for triage (optional)

markdown
Copy
Edit

---

## **Key Features**

- **Goal Drift Detection:** Auto-replans tasks when `goal.md` changes.  
- **Indentation-Based Hierarchy:** Deepest subtasks always tackled first.  
- **Weighted Task Scheduling:** Highest-impact tasks first.  
- **Self-Documenting:** Every iteration updates `DevDiary.md`, `CHANGELOG.md` and `/docs`.  
- **Codex Native:** This structure is optimized for sticky Codex agents to run without babysitting.  

---

## **How to Use**

1. **Clone PSAP:**  
   ```bash
   git clone https://github.com/<your-org>/PSAP-Preconfig.git <NewProject>
Write your goal.md:

Describe the overall project objective.

Add enumerated Additions for new features or changes.

Run your first Codex pass:

Codex reads agents.md, agent_tasks.md and starts building subtasks.

It will prescope first (populate DevDiary.md) before touching any source files.

Repeat:

Each PR → merge → re-run Codex.

PSAP will keep the task list and docs clean and up-to-date.

Why PSAP?
Because manually managing Codex agent repos is a nightmare.
PSAP gives you:

Sticky project scaffolding that enforces discipline and order.

Automatic NP→P breakdown so Codex never gets lost.

Self-cleaning task lists that won’t spiral into chaos.

Zero cognitive overhead: you just review PRs.
