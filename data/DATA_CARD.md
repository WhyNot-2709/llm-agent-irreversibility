# Canonical Dataset Card

- Dataset: `data/reversibility_dataset_2000_corrected.jsonl`
- Records / unique IDs: 2000 / 2000
- Labels: `{"irreversible": 400, "partial": 600, "reversible": 1000}`
- Environments: `{"ScienceWorld": 1, "sandbox": 1999}`
- Corrected labels: 200
- Verified minimal pairs: 999 groups; 1998/2000 records (99.9%)
- Meets 30% minimal-pair target: True
- Required-field coverage: `{"action_text": 2000, "environment": 2000, "horizon": 2000, "id": 2000, "label": 2000, "source": 2000, "state_text": 2000}`
- Scenario split: 1600 train / 400 test; no scenario or pair overlap

## Human validation

- Same 100 examples; 12 automatic labels updated after correction.

| Comparison | Exact agreements | Cohen's kappa |
|---|---:|---:|
| annotator_A_vs_B | 99/100 | 0.9831 |
| annotator_A_vs_C | 100/100 | 1.0000 |
| annotator_B_vs_C | 99/100 | 0.9831 |
| annotator_A_vs_automatic | 59/100 | 0.3844 |
| annotator_B_vs_automatic | 59/100 | 0.3853 |
| annotator_C_vs_automatic | 59/100 | 0.3844 |

## Limitations

- ScienceWorld contains one record, which is too small for meaningful cross-environment conclusions.
- Sandbox action coverage is concentrated in edit_file, delete_file, and send_email.
- The 799-row synthetic expansion is identified in source metadata.
- Previously saved model outputs use a different dataset and are only an unaligned pilot.
- Corrected labels are 50% reversible, 30% partial, and 20% irreversible.
