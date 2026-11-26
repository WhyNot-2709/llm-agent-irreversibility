# RQ3: Paired agent behavior
## RQ3: Paired agent behavior

This is the corrected analysis of the saved 60 paired sandbox tasks per model (120 episodes per model; 240 total). No model inference was repeated. K uses the existing project definition: the answer for the irreversible version must rank as more irreversible than the answer for its reversible counterpart. A counts pairs where the agent was careful in the irreversible version and not careful in the reversible version. The blindness gap is K − A.

| Model | K (95% CI) | A (95% CI) | Gap (95% CI) | Careful rate, rev | Careful rate, irr | Any irreversible action, irr | Target irreversible action, irr | Task success, irr |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| qwen7b | 0.450 [0.333, 0.567] | 0.017 [0.000, 0.050] | 0.433 [0.300, 0.567] | 0.483 [0.350, 0.600] | 0.483 [0.367, 0.600] | 0.517 [0.400, 0.633] | 0.517 [0.400, 0.633] | 1.000 [1.000, 1.000] |

**qwen7b predeclared McNemar tests:**

| Outcome | Exact p-value | BH-adjusted q-value |
|---|---:|---:|
| careful_behavior | 1 | 1 |
| any_irreversible_action | 9.31323e-10 | 1.39698e-09 |
| target_irreversible_action | 9.31323e-10 | 1.39698e-09 |
| llama8b | 0.000 [0.000, 0.000] | 0.433 [0.300, 0.567] | -0.433 [-0.567, -0.300] | 0.500 [0.367, 0.633] | 0.933 [0.867, 0.983] | 0.067 [0.017, 0.133] | 0.067 [0.017, 0.133] | 1.000 [1.000, 1.000] |

**llama8b predeclared McNemar tests:**

| Outcome | Exact p-value | BH-adjusted q-value |
|---|---:|---:|
| careful_behavior | 2.98023e-08 | 8.9407e-08 |
| any_irreversible_action | 0.125 | 0.125 |
| target_irreversible_action | 0.125 | 0.125 |

### Audit status and limitations

The team read through RQ3 transcripts informally. No independent episode labels or detector precision/recall counts were recorded, so the careful-behavior detector is not quantitatively human-validated. The handout’s separate 100-example dataset-label validation remains complete. RQ3 uses the sandbox; ScienceWorld coverage remains one row, so cross-testbed claims are limited.

The three predeclared McNemar tests are careful behavior, any irreversible action, and target irreversible action. Their q-values use Benjamini–Hochberg within each model. Task success is descriptive.
