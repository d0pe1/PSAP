# **Agent Profiles for PSAP (PowerShell Agent Preconfig)**

This defines the roles and operating procedures for autonomous agents in **PSAP-powered repos**.  
Agents must implement features, integrate atomic modules, and maintain a fully modular PowerShell workflow.

---

## **Atomic Module Design**

- Each reusable function (or logically tight function group) is its own module in `Modules/<FunctionName>/<FunctionName>.psm1`.  
- Each module has its own `psd1` manifest.  
- Function duplication is forbidden: common patterns must be factored out into their own atomic module.

**Example:**
📂 Modules/
📂 Get-Config/
├─ Get-Config.psm1
└─ Get-Config.psd1
📂 Write-Log/
├─ Write-Log.psm1
└─ Write-Log.psd1
📂 New-TaskPlan/
├─ New-TaskPlan.psm1
└─ New-TaskPlan.psd1

---

## **Task Selection Workflow (Goal-Driven + Priority + Recursive Splitting)**

### **Step 0: Pre-Planning from `goal.md` (Drift Detection)**

1. **Hash `goal.md`:** Compare against previously saved hash.  
   - **Hash changed:**  
     - Regenerate high-level project plan in `agent_tasks.md` (new tasks go **on top**).  
     - Mark existing tasks `[a]` if they conflict with the new scope.  
   - **Hash unchanged:**  
     - Proceed as usual.
2. Parse any `Add n:` entries into new actionable tasks (P-complete subtasks if atomic, NP-split otherwise).

> Every drift detection event must be logged in `DevDiary.md` and `CHANGELOG.md` with what scope changed.

---

### **Step 1: Scan `agent_tasks.md`**
1. If there’s any `[a]` task: jump into `agent_prio.md`.
2. If there’s no `[a]` task: pick the highest-weight `[ ]` task and start work.

---

### **Step 2: Scan `agent_prio.md`**
1. Identify the child tasks of the parent `[a]` task (sorted by indentation depth: deepest first).  
2. Remove `[x]` tasks that are logged in `DevDiary.md` and `CHANGELOG.md`.  
3. Execute the deepest, highest-weight `[ ]` task.  

---

### **Step 3: If a task explodes in scope**
1. Mark current task `[a]`.  
2. Split into atomic `[ ]` subtasks directly below:  
   - Indent each child one level deeper (2 spaces).  
   - Assign weights.  
3. STOP. Future runs pick these subtasks.

---

### **Step 4: Finishing child tasks**
1. Copy the task into `DevDiary.md` with indentation preserved and **verbose description**.  
2. Add a one-liner to `CHANGELOG.md`.  
3. Remove it from `agent_prio.md`.  
4. If a parent `[a]` task now has no children, flip it to `[ ]` and document the unblock.

---

## **Weighting + Indentation Rules**

- **Weights:**  
  - 1000 = Foundational systems (framework or planning-level tasks)  
  - 500 = Core modules that other modules depend on  
  - 100 = Feature modules or atomic utilities  
  - 10 = Bugfixes/patches  

- **Indentation:**  
  - 0 spaces = Top-level task from `agent_tasks.md`  
  - 2 spaces = Child  
  - 4 spaces = Sub-child  

Agents must:
- Process deepest tasks first.  
- Pick highest-weight tasks first.  
- Clean up `[x]` tasks immediately.

---

## **Documentation & Commenting Standards**

1. **Every function must have:**
   - Verbose **header block**:
     ```powershell
     <#
     .SYNOPSIS
        Short description of what the function does.
     .DESCRIPTION
        Detailed explanation of its purpose and behavior.
     .PARAMETER ParamName
        [Type] Description
     .OUTPUTS
        [Type] Description
     .EXAMPLE
        Example usage and expected result.
     #>
     ```
2. **Inline comments** explaining key logic, assumptions, and edge cases.
3. Each module must have its own `.psd1` manifest with versioning, author, and dependency info.
4. Update `Docs/<FunctionName>.md` to document how/where the function is used.

---

## **Agents**

### **1. GoalPlannerAgent**
- Reads `goal.md`, hashes it, and regenerates `agent_tasks.md` from scratch if scope changed.
- Creates high-level NP tasks first, then generates atomic P subtasks.

### **2. DiffAnalysisAgent**
- Compares existing `.psm1` modules and scripts for drift/conflicts.
- Outputs `docs/runtime_vs_modules.md` and spawns new tasks if mismatches found.

### **3. ModuleBuilderAgent**
- Creates new atomic modules:  
  - Generates `.psm1` with verbose header comments.  
  - Generates `.psd1` manifest.  
  - Adds starter `Docs/<FunctionName>.md`.
- Splits functions into their own modules if they are bundled together.

### **4. ScriptIntegratorAgent**
- Wires modules together, ensuring dependencies are loaded and entry points are well-defined.

### **5. TestingAgent**
- Uses Pester for all modules. Blocks the pipeline if tests fail.

### **6. DocumentationAgent**
- Updates all docs, cross-links Task IDs, regenerates `Docs/Index.md`.

---

## **Example: Priority Cascade (Aligned with Your Rules)**

**agent_tasks.md**
[a] CORE-1 Build "Write-Log" module (weight=800)

**agent_prio.md**
[a] CORE-1 Build "Write-Log" module
  [ ] CORE-2 Define logging levels (weight=400)
  [ ] CORE-3 Create Write-Log.psm1 with verbose headers (weight=400)

> **Notes:**  
> - Indentation shows hierarchy (2 spaces per depth).  
> - Child tasks (CORE-2, CORE-3) are **P-complete** atomic tasks spawned from the NP parent (CORE-1).  
> - The parent stays `[a]` until **all children are complete**.  

**DevDiary.md entry (after completing CORE-2):**
## CORE-2
- **Context:** Spawned from CORE-1 (NP task: Build Write-Log module)
- **Summary:** Defined logging levels and constants for use across modules. 
- **Details:** Added `LoggingConstants.psm1` in `Modules/Write-Log/` with full verbose header docs and inline comments.  
- **Complexity:** [P-complete]
- **Dependencies:** None remaining.
- **Date:** 2025-07-26

> **Next run behavior:**  
> - CORE-2 is removed from `agent_prio.md` (because it’s now in the DevDiary and Changelog).  
> - CORE-1 stays `[a]` because CORE-3 is still open.  
> - Once CORE-3 is complete and no children remain, CORE-1 flips to `[ ]` and can be executed.

---
