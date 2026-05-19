\# LLM10 — References



\## CVEs \& Vulnerability Disclosures



\- \*\*CVE-2024-3095\*\* — Hugging Face model hub unauthorized 

&#x20; access enabling bulk model weight exfiltration

\- \*\*CVE-2023-51449\*\* — Exposed ML inference endpoints 

&#x20; enabling unauthenticated model querying at scale



\## Real-World Incidents



\- \*\*OpenAI Model Theft Concerns (2023)\*\* — Reports of 

&#x20; systematic API querying by competitors attempting to 

&#x20; distill GPT-4 capabilities into smaller local models.

&#x20; OpenAI updated ToS to explicitly prohibit using outputs 

&#x20; to train competing models.



\- \*\*Meta LLaMA Leak (2023)\*\* — LLaMA model weights 

&#x20; leaked via 4chan within days of restricted academic 

&#x20; release, demonstrating that direct infrastructure 

&#x20; controls alone are insufficient once weights exist 

&#x20; in downloadable form.



\- \*\*Google PaLM Extraction Research (2023)\*\* — 

&#x20; Researchers demonstrated partial extraction of 

&#x20; PaLM model behavior using only 1 million API queries —  

&#x20; a feasible attack budget for motivated adversaries.



\## Research Papers



\- Tramèr et al. (2016) — "Stealing Machine Learning Models 

&#x20; via Prediction APIs" (foundational paper)

&#x20; https://arxiv.org/abs/1609.02943



\- Carlini et al. (2021) — "Extracting Training Data from 

&#x20; Large Language Models"

&#x20; https://arxiv.org/abs/2012.07805



\- Krishna et al. (2020) — "Thieves on Sesame Street! 

&#x20; Model Extraction of BERT-based APIs"

&#x20; https://arxiv.org/abs/1910.12366



\- Zhao et al. (2024) — "Protecting Language Generation Models 

&#x20; via Invisible Watermarking"

&#x20; https://arxiv.org/abs/2302.03162



\## Tools



\- Watermark-Anything: https://github.com/facebookresearch/watermark-anything

\- MarkMyWords (LLM watermarking): https://github.com/wagner-group/MarkMyWords



\## Official References



\- OWASP LLM10: https://owasp.org/www-project-top-10-for-large-language-model-applications/

\- MITRE ATLAS — AML.T0044: Full ML Model Access

&#x20; https://atlas.mitre.org/techniques/AML.T0044

\- CWE-200: Exposure of Sensitive Information

\- 18 U.S.C. § 1832: Trade Secrets Act (US legal context)

