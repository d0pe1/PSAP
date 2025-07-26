# **PSAP – PowerShell Agent Preconfig**

### *A self-scoping, self-planning, self-iterating blueprint for PowerShell projects*  

PSAP is a template repository designed to bootstrap **Agentic DevOps for PowerShell development**.  
It’s built around a self-organizing architecture adapted for **enterprise-grade PowerShell tooling**.  

---

## **Core Principles**

1. **Zero-Step Planning**  
   - Reads `goal.md` → detects scope drift (via MD5 hash).  
   - Expands or reprioritizes `agent_tasks.md` on-the-fly.  
   - Supports enumerated **Additions** (`Add 1`, `Add 2`, etc.) for human-driven scope injection.

2. **Recursive Task Decomposition**  
   - NP-complete goals are split into discrete P-complete subtasks.  
   - Uses `agent_tasks.md` and `agent_prio.md` with **indentation-based hierarchy** to track dependencies.  

3. **Self-Iterating Agents**  
   - Specialized agents handle planning, coding, testing, and documentation.  
   - Each pass updates `DevDiary.md`, `CHANGELOG.md`, and cleans up completed tasks.

4. **PowerShell-First**  
   - Implements standard PowerShell dev practices:
     - Module + function scaffolding.
     - Pester testing baked in.
     - CI/CD ready (GitHub Actions or Azure Pipelines).  

---

## **Repo Structure**

📂 PSAP/
├─ 📄 goal.md # Defines project intent + user Additions
├─ 📄 agent_tasks.md # Weighted task list (top-level)
├─ 📄 agent_prio.md # Blocking task detail & subtasks
├─ 📄 agents.md # Agent roles + recursive logic
├─ 📄 DevDiary.md # Prescope notes + implementation history
├─ 📄 CHANGELOG.md # 1-line summaries per task
├─ 📄 usertodo.md # Manual follow-ups / test instructions
├─ 📂 src/ # PowerShell modules & scripts
│ └─ ModuleName.psm1
├─ 📂 tests/ # Pester test files
│ └─ ModuleName.Tests.ps1
├─ 📂 docs/ # Design docs & diagrams
│ └─ api_map.md
└─ 📂 runtime_crashes/ # Logs for crash triage

markdown
Copy
Edit

---

## **How It Works**

### **Step 0: Goal Processing**
- On startup, **ZeroStepPlannerAgent** hashes `goal.md`.
- If changed:
  1. Reads enumerated Additions.  
  2. Updates/creates tasks in `agent_tasks.md`.  
  3. Logs changes to `DevDiary.md` + `CHANGELOG.md`.

### **Step 1: Task Execution**
- Finds highest-weight task (`[ ]`) or resolves `[a]` blockers.
- Expands NP-complete tasks → creates indented subtasks.

### **Step 2: Implementation**
- Modifies only **`src/`** and related test/docs files.  
- Comments every function.  
- Updates Pester specs as it builds modules.  

### **Step 3: Cleanup**
- Marks finished tasks `[x]`, removes from `agent_prio.md` when appropriate.  
- Records results in `DevDiary.md` + `CHANGELOG.md`.

---

## **Key Features**

- **Scope Drift Detection**: auto-replans when `goal.md` changes.  
- **Indentation-Based Hierarchy**: agents always tackle deepest subtasks first.  
- **Weighted Task Scheduling**: highest-impact tasks handled first.  
- **Self-Documenting**: prescope notes, test plans, and changelogs are maintained without manual effort.  

---

## **Example Usage**

1. Clone PSAP and write your **initial goal** in `goal.md`.  
2. Run the first agent iteration:  
   ```powershell
   Invoke-PSAPAgent
Review agent_tasks.md and agent_prio.md → add Additions if needed.

Let agents iterate until project is built & tested.

Built-in Agents
ZeroStepPlannerAgent: Goal hashing, scope drift detection.

DiffAnalysisAgent: Baseline analysis & task planning.

ModuleBuilderAgent: Builds PowerShell modules & scaffolds functions.

TestAgent: Writes & runs Pester tests.

DocumentationAgent: Updates DevDiary.md, CHANGELOG.md, and /docs.

Why PSAP?
Because you shouldn’t spend your week manually breaking down projects, updating docs, and wiring CI pipelines.
PSAP gives you:

Predictable, agent-driven progress.

A universal project skeleton ready for PowerShell dev.

Human-in-the-loop control over scope without micromanaging.

yaml
Copy
Edit

---

👉 Want me to also write a **`goal.md` template** (with sample Additions + project objectives) so the repo feels 100% plug-and-play? Or should we jump straight to the first **agents.md** for PSAP?







You said:
lol i don't plan on running it with powershell xD are you mad, this is to preconfig sticky codex repos like i did xD


ChatGPT said:
Ahhhh got it – so PSAP is basically the meta-preconfig factory for Codex repos, not a PowerShell runner. Even better. Here's the corrected README.md for that use case (drop-in for the repo):

markdown
Copy
Edit
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

Next Steps
Add your own agent profiles in agents.md (define roles + responsibilities).

Write your first goal.md and let Codex plan the entire project.

Build actual module or source directories after PSAP lays the groundwork.

✨ PSAP makes Codex feel like a project manager that never sleeps.

pgsql
Copy
Edit

---
