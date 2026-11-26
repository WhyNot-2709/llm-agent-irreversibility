# Analysis plan and decision rules

Prepared and frozen: 2026-11-16, before the full real-data/model run. This plan was written after code development and synthetic fixture checks; it is **not a preregistration**. Do not revise these decision rules after examining full-run held-out scores. Record any necessary deviation and its reason.

## Primary claims

- **RQ1:** primary estimate is binary balanced accuracy of the linear probe on held-out, explicitly matched minimal-pair records. Report the estimate and cluster-bootstrap interval. Do not claim a statistically significant best layer from the maximum held-out score.
- **RQ1 secondary:** all-data binary and three-class scores, macro-F1, AUROC where defined, token-position ablation, and cross-environment transfer. Show full layer curves; treat them as descriptive/exploratory. Do not make significance claims for layer rankings, binary-versus-three-class scores, or Qwen-versus-Llama differences.
- **RQ2/RQ4:** select downstream layers only on grouped validation rows drawn from each training partition. Keep held-out rows out of layer selection.
- **RQ3:** report the paired blindness gap K−A, K, A, task success, careful-behavior components, intervals, and the predeclared McNemar tests. Identify whether careful labels came from the detector or adjudicated manual labels.

## RQ3 detector acceptance and fallback

- Before the full run, inspect the genuine Qwen and Llama transcripts produced by the small preflight. Check that output formatting, refusals, and actual tool results follow the annotation rubric. This is a qualitative implementation check, not the detector acceptance audit.
- After the full run, two annotators label a deterministic sample of 20 episodes per model (10 complete task pairs), then resolve disagreements into `adjudicated_careful`.
- Accept the structured detector for the primary careful-behavior metric only when the sample has at least 20 episodes and, against adjudicated labels, **both precision and recall are at least 0.80 for that model**. Undefined precision or recall fails the rule. Report counts and metrics for both annotators and the adjudicated labels.
- If the sample is under 20, classify the result as insufficient; do not call the detector accepted. If either metric is below 0.80, manually annotate every complete RQ3 episode for that model and rerun `run_agent.py` with `--manual_careful_labels`. The resulting K−A is the primary result and must be labeled as human-adjudicated. Component rates remain detector-derived and must be labeled accordingly.
- The 0.80 threshold is a predeclared operational quality bar for this small project, not a universal standard. The 20-episode audit is limited evidence; include the confusion counts and small-sample limitation.

## Splits and uncertainty

- Use the fixed three scenario/pair-grouped 80/20 splits with seeds 42, 43, and 44.
- For probe metrics, compute 1,000-resample cluster-bootstrap intervals over held-out scenario groups within each split; report those intervals separately from the mean and standard deviation across the three split scores.
- For direct-answer and cross-environment estimates, resample the scenario/pair cluster. For paired agent/steering deltas, resample task pairs.

## Multiple-comparison scope

- The three RQ3 McNemar tests (careful behavior, any irreversible action, target irreversible action) form one BH family **within each model**. Report exact p-values and adjusted q-values. Do not make a pooled Qwen-versus-Llama significance claim.
- RQ1 layer curves and secondary probe/transfer scores are descriptive estimates with intervals; no layer-wise or cross-condition p-value claims are planned. Binary and three-class scores concern different prediction targets and are reported separately.
- If an additional inferential comparison is needed, define its hypotheses, family, and correction before inspecting the relevant results. Apply BH across every test in that declared family and report the complete family.
- Report every model, feature position, label, steering condition, failed/incomplete run, and ScienceWorld search omission.

## Data quality checks

- Verify exact pair invariants and dataset/feature alignment before analysis.
- Report background-scenario coverage and the number/fraction of sampled ScienceWorld candidates omitted at the node limit.
- Complete the genuine transcript preflight and the full human audit decision rule above before interpreting K−A.
