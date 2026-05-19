\# LLM07 — References



\## CVEs \& Vulnerability Disclosures



\- \*\*CVE-2023-36188\*\* — LangChain plugin arbitrary code execution

&#x20; via unsanitized LLM output passed to Python eval()

\- \*\*CVE-2024-27564\*\* — ChatGPT plugin SSRF via unvalidated 

&#x20; URL parameter in plugin request



\## Real-World Incidents



\- \*\*ChatGPT Plugin Permission Abuse (2023)\*\* — Security 

&#x20; researchers demonstrated that ChatGPT plugins with broad 

&#x20; permissions could be triggered via prompt injection to 

&#x20; exfiltrate conversation history to attacker-controlled servers



\- \*\*AutoGPT Uncontrolled Tool Use (2023)\*\* — AutoGPT agents 

&#x20; demonstrated executing arbitrary shell commands, creating 

&#x20; files, and making network requests without user confirmation

&#x20; due to insufficient plugin guardrails



\- \*\*Microsoft Copilot Plugin Escalation (2024)\*\* — Researchers

&#x20; showed cross-plugin privilege escalation where a low-privilege

&#x20; plugin could trigger high-privilege plugin actions via 

&#x20; carefully crafted LLM context manipulation



\## Research Papers



\- Greshake et al. (2023) — "Not What You've Signed Up For"

&#x20; https://arxiv.org/abs/2302.12173



\- Ruan et al. (2023) — "Identifying the Risks of LM Agents 

&#x20; with an LM-Emulated Sandbox"

&#x20; https://arxiv.org/abs/2309.15817



\## Tools



\- OWASP ZAP (API security testing): https://www.zaproxy.org

\- Semgrep (static analysis for plugin code): https://semgrep.dev



\## Official References



\- OWASP LLM07: https://owasp.org/www-project-top-10-for-large-language-model-applications/

\- MITRE ATLAS — AML.T0054: LLM Plugin Compromise

\- CWE-272: Least Privilege Violation

\- CWE-89: SQL Injection

