# PTB-XL ablation files

These files patch the current `medtsllm-biomedcoop` combined model so MOMENT and
BioMedCoOp can be disabled independently without removing the BLIP-2-style
Q-Former.

## Install

From the repository root:

```bash
cp /path/to/medtsllm_ablation_files/models/tri_medtsllm.py models/tri_medtsllm.py
cp /path/to/medtsllm_ablation_files/configs/*.toml configs/
```

Back up your original `models/tri_medtsllm.py` first.

## Experiments

Full model (existing config):

```bash
RUN_ID="ptbxl_full_seed0"
python3 -u train.py configs/combined_ptbxl_full.toml "$RUN_ID" \
  2>&1 | tee "outputs/console/${RUN_ID}.log"
```

Without MOMENT:

```bash
RUN_ID="ptbxl_no_moment_seed0"
python3 -u train.py configs/combined_ptbxl_no_moment.toml "$RUN_ID" \
  2>&1 | tee "outputs/console/${RUN_ID}.log"
```

Without BioMedCoOp:

```bash
RUN_ID="ptbxl_no_biomedcoop_seed0"
python3 -u train.py configs/combined_ptbxl_no_biomedcoop.toml "$RUN_ID" \
  2>&1 | tee "outputs/console/${RUN_ID}.log"
```

Without MOMENT and BioMedCoOp:

```bash
RUN_ID="ptbxl_no_moment_no_biomedcoop_seed0"
python3 -u train.py configs/combined_ptbxl_no_moment_no_biomedcoop.toml "$RUN_ID" \
  2>&1 | tee "outputs/console/${RUN_ID}.log"
```

## What remains fixed

All ablations keep the MedTsLLM time-series encoder, task/data prompts,
BLIP-2-style Q-Former, Llama backbone, training hyperparameters, and PTB-XL split.
When BioMedCoOp is disabled, a trainable linear classifier is applied to the
same pooled LLM representation. When MOMENT is disabled, the Q-Former receives
only MedTsLLM tokens.
