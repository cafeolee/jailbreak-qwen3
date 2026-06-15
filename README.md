# Evaluating the Impact of Low-Resource Languages on LLM Safety

**Bersun Şipal, Inés Martínez Fernández** 


## Introduction

The capabilities and safety of large language models can vary significantly across languages (OpenAI, 2024; Qwen et al., 2025; Deng et al., 2024). LLMs are generally less robust to jailbreaking methods in lower-resource languages, resulting in higher unsafe response rates depending on the benchmark used (Deng et al., 2024; Yong et al., 2024).

This project evaluates whether LLM-based safety classifiers maintain their performance when prompts are translated into typologically diverse languages, and tests the effect of prefix injection jailbreaking (Wei et al., 2023) across all languages. The languages selected — English, Spanish, Turkish and Asturian — cover a range of resource levels and typological profiles, allowing for a meaningful comparison between well-resourced and low-resource settings.

---

## Dataset

The evaluation dataset was constructed to test LLM safety classifiers across languages and challenging scenarios. Unsafe prompts were collected from the AdvBench harmful behaviors dataset, containing instructions that are explicitly adversarial. Safe prompts consist of standard commands alongside tricky prompts that include words associated with harmful behavior but are contextually safe — designed to challenge classifiers by appearing potentially harmful without actually being so. The final mixed dataset combines both sets, resulting in 1,120 prompts that reflect a realistic mixture of safe and unsafe instructions.

The dataset was translated into Turkish and Spanish using Google Translate, while Asturian required a different approach: given the lack of direct translation tools, Spanish was first translated into Asturian using the AINA model.


## Models

Two Qwen3 models were evaluated. Qwen3-Guard-4B is a dedicated safety moderation model specifically developed to detect harmful or unsafe content, evaluated directly on each language dataset. Qwen3-4B-Instruct is a general-purpose instruction model, prompted with a system prompt and few-shot examples to perform the binary classification task of determining whether a given prompt is safe or unsafe.


## Results

Both models perform well on safety classification overall, but several patterns emerge across languages and evaluation settings.

For Qwen3-Guard-4B, accuracy remains high across all languages, with English (99.2%) and Spanish (99.8%) at the top, followed by Asturian (98.4%) and Turkish (96.4%). Turkish shows the highest jailbreak rate (5.8%) and over-refusal rate (1.7%), while Asturian performs comparatively better, possibly due to lexical overlap with Spanish training data.

For Qwen3-4B-Instruct, results differ notably depending on whether a system prompt is used. With a system prompt, accuracy is high across languages but performance is overestimated: without it, the model shows lower accuracy and considerably higher jailbreak and over-refusal rates, particularly for Turkish (jailbreak: 4.45%, over-refusal: 16.56%) and Asturian (over-refusal: 10.00%). This highlights the importance of realistic evaluation settings that do not rely on system prompt scaffolding.

Additionally, inspired by Wei et al. (2023), we experimented with prefix injection by prepending the word *"sure"* to each prompt when using Qwen3-4B-Instruct, simulating a compliance-inducing jailbreak. Results show a measurable increase in unsafe response rates across all languages.

---

## Reproducibility

This project consists of multiple notebooks and relies on a specific folder structure. The recommended way to run it is via Google Drive and Google Colab.

Clone the repository and upload the entire folder to your Google Drive:

```bash
git clone https://github.com/cafeolee/jailbreak-qwen3.git
```

Before running any notebook for the first time, update the paths to match your Drive directory structure.

---

## References

Deng, Y., Zhang, W., Pan, S. J., & Bing, L. (2024). [Multilingual Jailbreak Challenges in Large Language Models](https://arxiv.org/abs/2310.06474).

OpenAI. (2024). GPT-4 Technical Report.

Qwen Team. (2025). Qwen3 Technical Report.

Wei, J. et al. (2023). [Jailbroken: How Does LLM Safety Training Fail?](https://arxiv.org/abs/2307.02483)

Yong, Z.-X., Menghini, C., & Bach, S. H. (2024). [Low-Resource Languages Jailbreak GPT-4](https://arxiv.org/abs/2310.02446).
