# Running a case

This requires HADDOCK3 with the modified-RNA forcefield.

Each case directory contains a ready-to-run HADDOCK3 configuration, `X_b_dock.cfg`. Run it from inside that directory, because the file paths in the configuration are relative:

```bash
cd 4RCJ
haddock3 4RCJ_b_dock.cfg
```

Each configuration runs this pipeline:

```text
topoaa → rigidbody → seletop (400) → flexref → emref
       → clustfcc → seletopclusts → caprieval (reference: X_target.pdb)
```

The defaults are `mode = "local"` and `ncores = 125`. Adjust `ncores` (or switch to a batch/HPC `mode`) for your machine before running. Results are written to `run_dock/` inside the case directory. The repository's `.gitignore` excludes `run_dock/`, so results are never committed.
