\# LLM06 — References



\## CVEs \& Vulnerability Disclosures



\- \*\*CVE-2023-32786\*\* — LangChain prompt injection leading to 

&#x20; system prompt disclosure

\- \*\*CVE-2024-5184\*\* — EmailGPT system prompt extraction via 

&#x20; role injection attack



\## Real-World Incidents



\- \*\*Samsung Data Leak (2023)\*\* — Samsung engineers accidentally 

&#x20; pasted proprietary source code and meeting notes into ChatGPT,

&#x20; which was used for training. Samsung subsequently banned 

&#x20; ChatGPT for internal use.



\- \*\*GPT-2 Training Data Extraction (2021)\*\* — Carlini et al. 

&#x20; demonstrated extraction of verbatim training data including 

&#x20; PII, phone numbers, email addresses, and code with credentials

&#x20; from GPT-2 using only API access.



\- \*\*Bing System Prompt Leak (2023)\*\* — Within days of launch,

&#x20; users extracted Bing Chat's full system prompt ("Sydney") 

&#x20; via prompt injection, revealing Microsoft's internal 

&#x20; instructions and behavioral guidelines.



\## Research Papers



\- Carlini et al. (2021) — "Extracting Training Data from 

&#x20; Large Language Models"

&#x20; https://arxiv.org/abs/2012.07805



\- Perez \& Ribeiro (2022) — "Ignore Previous Prompt: Attack 

&#x20; Techniques for Language Models"

&#x20; https://arxiv.org/abs/2211.09527



\- Yu et al. (2023) — "Bag of Tricks for Training Data 

&#x20; Extraction from Language Models"

&#x20; https://arxiv.org/abs/2302.04460



\## Tools



\- Microsoft Presidio (PII detection): https://github.com/microsoft/presidio

\- Lakera Guard (LLM security): https://platform.lakera.ai



\## Official References



\- OWASP LLM06: https://owasp.org/www-project-top-10-for-large-language-model-applications/

\- MITRE ATLAS — AML.T0024: Exfiltration via ML Inference API

&#x20; https://atlas.mitre.org/techniques/AML.T0024

\- CWE-200: Exposure of Sensitive Information to Unauthorized Actor

