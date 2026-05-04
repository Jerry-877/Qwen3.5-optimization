# Qwen3.5-optimization
Only 1 line of Jinja template changes to force Qwen 3.5's entire series into 100% Chinese reasoning — reducing CoT loop rate from 5/8 to 1/22, with zero model retraining.

## What This Is

This project introduces **Stem-Forced CoT**, a zero-cost inference-level
method that controls the reasoning language of Qwen 3.5 models. By
modifying just 1 line in the Jinja chat template, the model's entire
chain-of-thought is anchored to a target language, eliminating verbose
English templates and dramatically reducing CoT loop rates.

## Key Results

| Metric | Before (Default) | After (Chinese CoT) | After (English CoT) |
|--------|-----------------|-------------------|-------------------|
| CoT Loop Rate | 5/8 (62.5%) | 1/22 (~4.5%) | 2/11 (~18.2%) |
| Time for 11 Q&A Rounds | ~17 min 35 sec | ~6 min 56 sec | ~4 min 59 sec |
| Target Language Reasoning Rate | ~27% (3/11 Chinese) | 100% (Chinese) | ~100% (English) |
| Output Language Locked? | No | Yes (Chinese) | No (flexible) |

## Supported Models

All Qwen 3.5 variants share the same Jinja template, so this single
modification works across the entire series.

## Quick Start

### Option A: Chinese CoT (Recommended)

Locate your `qwen3.5.jinja` chat template file. You need to modify **1 line**:

(when thinking mode is enabled):

    # Before:
    {{- '<think>\n' }}
    # After:
    {{- '<think>\n嗯，' }}


### Option B: English CoT (Alternative)

Replace `嗯，` with `Hmm.` in both lines above. This preserves English reasoning
while still eliminating template degradation. Output language is **not locked**
under this option — the model can still respond in the user's language.

**Trade-offs between the two options:**

| Aspect | `嗯，` (Chinese) | `Hmm.` (English) |
|--------|-----------------|-----------------|
| CoT Loop Rate | 1/22 (~4.5%) | 2/11 (~18.2%) |
| Output Language | Locked to Chinese | Flexible |
| Best For | Chinese-only scenarios | Multilingual scenarios |

## Paper

For full methodology, experimental setup, discussion of long-context behavior,
and detailed analysis of stem language anchoring effects, see the paper
(in Chinese) in this repository.

## Author

LIU Jin-hao
