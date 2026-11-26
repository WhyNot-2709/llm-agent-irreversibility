# RQ3: Paired Agent Behavior

This is the corrected analysis of the saved 60 paired sandbox tasks per model (120 episodes per model; 240 episodes total). No model inference was repeated. The pairwise measure \(K\) counts a task pair as correct when the answer for its irreversible version ranks as more irreversible than the answer for its reversible version. \(A\) is the fraction of task pairs where the agent was coded careful in the irreversible version and not careful in the reversible version. The blindness gap is \(K-A\).

| Model | K (95% CI) | A (95% CI; provisional) | Gap (95% CI; provisional) | Careful rate, reversible | Careful rate, irreversible | Any irreversible action, irreversible version | Target irreversible action, irreversible version | Task success, irreversible version |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Qwen2.5-7B-Instruct | 0.450 [0.333, 0.567] | 0.017 [0.000, 0.050] | 0.433 [0.300, 0.567] | 0.483 | 0.483 | 0.517 [0.400, 0.633] | 0.517 [0.400, 0.633] | 1.000 |
| Llama-3.1-8B-Instruct | 0.000 [0.000, 0.000] | 0.433 [0.300, 0.567] | -0.433 [-0.567, -0.300] | 0.500 | 0.933 | 0.067 [0.017, 0.133] | 0.067 [0.017, 0.133] | 1.000 |

The careful-behavior detector was not quantitatively validated against independent episode labels. The team completed an informal transcript read-through, but no independent labels or detector precision/recall counts were saved. Therefore, \(A\), careful-behavior rates and tests, and both gap estimates are provisional. Their confidence intervals do not include detector-misclassification uncertainty. The handout’s separate validation of 100 dataset labels is not this RQ3 detector audit.

## Predeclared McNemar Tests

The three tests form one Benjamini–Hochberg family within each model.

| Model | Outcome | Exact p-value | BH-adjusted q-value |
|---|---|---:|---:|
| Qwen2.5-7B-Instruct | Careful behavior (provisional) | 1.000 | 1.000 |
| Qwen2.5-7B-Instruct | Any irreversible action | 9.31323e-10 | 1.39698e-09 |
| Qwen2.5-7B-Instruct | Target irreversible action | 9.31323e-10 | 1.39698e-09 |
| Llama-3.1-8B-Instruct | Careful behavior (provisional) | 2.98023e-08 | 8.94070e-08 |
| Llama-3.1-8B-Instruct | Any irreversible action | 0.125 | 0.125 |
| Llama-3.1-8B-Instruct | Target irreversible action | 0.125 | 0.125 |

Task success was 1.000 for both models in both versions. ScienceWorld has one record, so this RQ3 analysis supports no cross-environment comparison.
