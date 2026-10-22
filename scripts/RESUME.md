# Resuming an interrupted run

1. Run `colab sessions` and `colab usage`. Only use the `chunkfresh` session.
2. Run `python scripts/sync.py` (needs the `colab` CLI). It snapshots only `/content/chunkfresh` and prints the tail of the detached job log.
3. If `results/DONE` exists, run validate.py, analyze.py and figures.py locally, copy the final run log to figures/colab_run.txt, record usage and stop the `chunkfresh` session.
4. If incomplete, inspect remote `job.pid`, process existence and run.log before relaunching. Do not duplicate a live process. Completed embedding batches, retrieval files and answer keys are reused. `scripts/launch.py` can relaunch a dead job after an updated script archive is uploaded. The running job is independent of the notebook kernel.
5. Do not allocate a replacement session before checking existing ones. Compare fresh `colab usage` against the starting balance (127.05 CU; the shared-account difference is an upper bound). I stopped at a conservative 5.6-CU account decrease and never exceeded the 6-CU cap.

Preregistration: c9907fa. 3,600 fixed questions; the analysis must not use a smaller completed subset as the confirmatory test. Main hypothesis is budget-controlled 1024 minus 128. Six simultaneous contrasts across two policies and three comparisons. A truncated run is INCONCLUSIVE.

Local monitoring can be restarted using Python subprocess.Popen with start_new_session=True for scripts/budget_guard.py and scripts/watch.py. Inspect existing processes first to avoid duplicate monitors. The GPU job is independent of both monitors. After validation and analysis, run scripts/finalize_timing.py --eval-end; finish the writing, then scripts/finalize_timing.py, report.py, blog.py, export_log.py and check_publication.py.
