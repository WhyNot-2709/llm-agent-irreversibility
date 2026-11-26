# Do AI Agents Know Which Actions Cannot Be Undone?

This project studies whether large language models represent the reversibility of actions, state that information when asked, and act more carefully when a tool action is harder to undo. It is a measurement study using a controlled tool sandbox and a small ScienceWorld sample. The sandbox never acts on real files, email, money, or user accounts.

**Team:** Himan Aakanksh Reddy, Ananya Agrawal, and P Sishir Kundan Reddy

## Research questions

- **RQ1: Hidden-state probes.** Can a linear classifier predict action reversibility from model hidden states, and at which layers?
- **RQ2: Direct answers.** How accurately do the models label reversibility when asked directly?
- **RQ3: Paired behavior.** Do agents act more carefully in a task version where the tempting tool action is harder to undo?

The project also considered two optional questions: activation steering (RQ4) and next-state uncertainty (RQ5). Those bonus experiments were not run.

## Dataset

The corrected dataset contains 2,000 state-action records with three labels:

| Label | Records | Share |
|---|---:|---:|
| Reversible | 1,000 | 50% |
| Partially reversible | 600 | 30% |
| Irreversible | 400 | 20% |

There are 1,999 sandbox records and one ScienceWorld record. The sandbox records cover edit_file (1,199), delete_file (400), and send_email (400); the ScienceWorld record is a pick up picture action. The data includes 1,200 rows tagged as bounded-BFS sandbox data and 799 tagged as synthetic sandbox data.

The structural audit verified 999 minimal-pair groups, covering 1,998 records (99.9% of the dataset). A pair keeps the action and task context comparable while changing a state condition that affects reversibility. The audit checks pair structure; it does not establish that every individual label is semantically correct. See [the dataset card](data/DATA_CARD.md) and the files under [data/validation](data/validation).

### Label rule

Labels use a bounded restoration search with a horizon of up to three recovery actions after the candidate action:

- **Reversible:** the complete canonical state can be restored exactly within the horizon.
- **Partially reversible:** visible or content state can be restored, but the complete state is not exact or the recovery is approximate.
- **Irreversible:** a complete search finds no qualifying restoration within the horizon.

This is an operational, bounded definition. It does not claim that recovery is impossible with unlimited time or tools. Invalid actions and incomplete searches are excluded rather than labeled irreversible.

## Models and evaluation

The study uses Qwen2.5-7B-Instruct and Llama-3.1-8B-Instruct.

- **RQ1** uses last-action-token and mean-action-token hidden-state features with linear logistic-regression probes. Analyses use grouped splits with seeds 42, 43, and 44.
- **RQ2** evaluates direct forced-choice answers on the corrected dataset.
- **RQ3** uses 60 paired sandbox tasks per model, with two versions per task, for 120 episodes per model. The simulated agents use greedy decoding and have up to 15 steps per episode.

Saved results are in [results/rq1_rq2](results/rq1_rq2) and [results/rq3](results/rq3). The paper figures are in [report/figures](report/figures).

## Main findings and limits

The selected three-class probe reached 1.000 balanced accuracy for both models. The unrelated random action-verb control also reached 1.000, so the measured selectivity over that control is zero. The probe result shows decodability in this dataset; by itself it does not show that the models learned a reversibility-specific signal.

Mean held-out direct-answer balanced accuracy was 0.456 for Qwen and 0.333 for Llama. Llama answered “reversible” for all 2,000 records.

On RQ3, the direct-answer pair measure K was 0.450 for Qwen and 0 for Llama. The detector-based careful-behavior measure A was 0.017 for Qwen and 0.433 for Llama, giving gaps K-A of 0.433 and -0.433. The planned quantitative human audit of the careful-behavior detector was not completed, so A, careful-behavior rates and tests, and the gap estimates are provisional.

The single ScienceWorld record is not meaningful second-testbed coverage. Cross-environment transfer was not estimated, and the project does not support cross-testbed conclusions. The current report describes three completed ablation comparisons; the required minimum of four has not been met. Comparing restoration horizons is not an informative ablation here because the saved labels are identical across horizons.

Human annotation agreement was assessed on a 100-record sample. It does not mean that all 2,000 labels were independently human-validated.

## Repository contents

- data/: corrected dataset, dataset card, and validation summaries.
- environment/: pinned CPU-side requirements.
- notebooks/: the user-confirmed project notebook.
- protocol/: analysis plan and decision rules.
- report/: editable IEEE-style paper source, the current compiled report PDF, its BibTeX bibliography, and figures.
- results/rq1_rq2/: probe and direct-answer artifacts.
- results/rq3/: paired-task logs and derived metrics.

The notebook contains the project workflow and historical exploration. It currently includes Kaggle-specific paths and older diagnostic cells, so it may need path updates and careful cell selection before it can be rerun outside the original Kaggle workspace. The compiled report PDF in `report/ADNN_Irreversibility_Report.pdf` is the current report copy. The notebook and editable LaTeX source may continue to change independently.

## Reproduction notes

The included requirements file pins a CPU environment. The saved model feature extraction used GPU PyTorch; the recorded protocol lists PyTorch 2.11.0+cu130. The CPU requirements alone are not enough to reproduce GPU extraction. Confirm the matching Python, CUDA, driver, and package versions before describing the full pipeline as reproducible.

Model downloads may require Hugging Face access. Supply credentials through a local environment variable or a notebook secret store. Never commit access tokens, private annotation keys, or other credentials.

The committed figures and results are saved outputs. The notebook has a plotting cell for the RQ1 layer profiles, but it does not currently regenerate the final named paper figures or the RQ3 gap plot from the files in this repository. Add those plotting cells to the notebook; a separate script is optional. A complete GPU environment specification is not currently included.

## License and reuse

No project-wide license is included yet. Check the source terms for the dataset and any third-party material, and agree on a license with the project authors before granting reuse rights.