\# LLM05 — References



\## CVEs \& Vulnerability Disclosures



\- \*\*CVE-2024-3095\*\* — Malicious models on Hugging Face Hub 

&#x20; executing arbitrary code via pickle deserialization on load

\- \*\*CVE-2023-44467\*\* — PyTorch supply chain attack via 

&#x20; compromised torchtriton dependency on PyPI



\## Real-World Incidents



\- \*\*Hugging Face Malicious Models (2024)\*\* — HiddenLayer 

&#x20; discovered 100+ models containing malicious pickle payloads

&#x20; capable of executing arbitrary code on the victim's machine

&#x20; https://hiddenlayer.com/research/silent-sabotage/



\- \*\*PyTorch Dependency Confusion (2022)\*\* — Attacker published 

&#x20; malicious "torchtriton" package to PyPI; PyTorch nightly 

&#x20; builds pulled malicious version, exposing developer machines

&#x20; https://pytorch.org/blog/compromised-nightly-dependency/



\- \*\*SolarWinds-style ML Attack (theoretical, 2023)\*\* — 

&#x20; Researchers demonstrated how a compromised model hub could 

&#x20; propagate backdoored models across the entire fine-tuning 

&#x20; ecosystem downstream



\## Research Papers



\- Bagdasaryan \& Shmatikov (2022) — "Spinning Language Models"

&#x20; https://arxiv.org/abs/2107.10443



\- Xue et al. (2023) — "TrojLLM: A Black-box Trojan Prompt 

&#x20; Attack on Large Language Models"

&#x20; https://arxiv.org/abs/2306.06815



\## Tools



\- ModelScan (Protect AI): https://github.com/protectai/modelscan

\- Hugging Face Security: https://huggingface.co/docs/hub/security



\## Official References



\- OWASP LLM05: https://owasp.org/www-project-top-10-for-large-language-model-applications/

\- MITRE ATLAS — AML.T0010: ML Supply Chain Compromise

&#x20; https://atlas.mitre.org/techniques/AML.T0010

\- CWE-494: Download of Code Without Integrity Check

