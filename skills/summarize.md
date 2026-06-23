# Summarize SLURM Jobs

Analyze and summarize SLURM jobs provided by the user. The user will paste `sacct` output or job IDs with statuses.

## Instructions

1. **Identify the shell scripts**: Use the job names to find the launching script (e.g., `full_pipeline` → `run_0All.sh`, `world_size` → `run_0All_world.sh`). Read the script to understand the pipeline steps, config, and logdir paths.

2. **Check each job's logs** in `logs/` directory:
   - `.out` file: `grep "STEP"` to see which pipeline steps completed, then `tail` to see current activity
   - `.err` file: `tail` to check for errors, OOM kills, crashes
   - `head` the `.out` to identify the run config (logdir, settings, seed, etc.)

3. **For FAILED/OOM jobs**: Determine the exact failure point:
   - Which pipeline step was running
   - Root cause (CUDA crash, Python OOM, SLURM OOM kill, timeout, etc.)
   - Whether results are salvageable (checkpoints exist, partial outputs saved)

4. **For RUNNING jobs**: Report which pipeline step they're currently on and approximate progress (e.g., training step number).

5. **For COMPLETED jobs**: Confirm all steps finished (look for "ALL N STEPS COMPLETE" in output).

6. **For array jobs**: Map task IDs to their configs using the array mapping in the script (e.g., `TASK_ID / N` for one axis, `TASK_ID % N` for another). Use a table format.

## Output Format

- Group jobs by their launching script
- For each group, briefly describe the experiment (what's being swept, key settings)
- For individual jobs or small groups: narrative format with job ID, config, status, current step, and failure reason if applicable
- For array jobs: table format (Task | Config1 | Config2 | Status | Notes)
- End with "Key Observations" section: notable patterns, best/worst results, salvageable failures, actionable next steps

## Key Paths

- SLURM logs: `logs/` directory, named `{jobname}_{jobid}.out` or `{jobname}_{arrayid}_{taskid}.out`
- Shell scripts: repo root `run_*.sh`
- Logdirs: typically `./logdir/{run_name}/`
- Checkpoints: `{logdir}/ckpt/`
- Pipeline steps are numbered (e.g., STEP 1/9, STEP 2/9, etc.) in stdout

## Common Failure Modes

- **C++ abort during training**: `terminate called without an active exception` → transient GPU/CUDA fault, check if checkpoint was saved recently
- **SLURM OOM kill**: `Detected oom_kill event` in `.err` → identify which step/script was running. `analyze_tuning.py --mode autocorr` is a known OOM offender
- **Timeout**: job hit SLURM time limit before completing all steps — check how far it got
- **Python error**: traceback in `.err` — report the exception type and location
