# R2D2
### Motivation:
This project was inspired by the 2024 paper "HarmBench: A Standardized Evaluation 
Framework for Automated Red Teaming and Robust Refusal" which proposes a two-part solution: 
a new benchmark for evaluating AI susceptibility to harmful behaviors as well as a method for 
training models against evolving continuous attacks. The central theme of this paper is that
AI defenses and attacks must be developed in tandem to be meaningfully robust. This experiment 
compares the defenses of dynamic adversarial trained models and static adversarial trained models 
against evolving continuous (fresh) attacks and fixed attacks, as well as what tradeoffs we observe 
in overall benign model utility in models trained this way.

### Experimental Design:
- 3 models: GPT-2 baseline, GPT-2 static, GPT-2 dynamic
- LoRA to condense training procedure
- nanoGCG to generate GCG attacks over constant iterations across all models
- random seeds fixed
- ASR measured as a set union between exact string matching and LLM-judged synonyms (Gemini 3.5 Flash Lite)
- Loss = loss_SFT + loss_toward + loss_away (reference: HarmBench)

### Results:

#### Adversarial Robustness

| Model | Attack Eval | Exact ASR | Semantic ASR | Mean Target NLL |
|---|---|---:|---:|---:|
| Baseline | Fixed | 75% | 100% | 1.41 |
| Baseline | Fresh | 75% | 100% | 1.41 |
| Static | Fixed | 0% | 0% | 28.60 |
| Static | Fresh | 0% | 0% | 14.45 |
| Dynamic | Fixed | 0% | 0% | 33.08 |
| Dynamic | Fresh | 0% | 0% | 21.25 |

#### Benign Over-Refusal

| Model | Safety-related | Safety-unrelated | Overall |
|---|---:|---:|---:|
| Baseline | 0/4 (0%) | 0/4 (0%) | 0/8 (0%) |
| Static | 4/4 (100%) | 1/4 (25%) | 5/8 (62.5%) |
| Dynamic | 4/4 (100%) | 3/4 (75%) | 7/8 (87.5%) |

### Conclusion:
Dynamic adversarial training was able to improve NLL for harmful behaviors more than static adversarial 
training, however this was at the cost of exhibiting over-refusal in which out of 8 benign probes (grouped into 
safety-related and safety-unrelated), dynamic training refused on 7/8 and static training refused on 5/6. Future 
extensions of this experiment might run ablations on the loss_SFT to improve this side-effect.

### Writeup

- [Full experimental writeup (PDF)](https://ayangelah.me/attachments/R2D2_Writeup.pdf?v=20261014)
- [Project page](https://ayangelah.me/#codeportfolio)
