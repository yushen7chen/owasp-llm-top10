\# LLM08 — References



\## CVEs \& Vulnerability Disclosures



\- \*\*CVE-2023-32786\*\* — LangChain agent arbitrary code execution

&#x20; due to excessive tool permissions granted to LLM agent

\- \*\*CVE-2024-21334\*\* — Microsoft Copilot excessive data access

&#x20; via overprivileged plugin permissions



\## Real-World Incidents



\- \*\*AutoGPT Uncontrolled Execution (2023)\*\* — Early AutoGPT 

&#x20; deployments demonstrated agents autonomously installing 

&#x20; software, modifying system files, and making network 

&#x20; requests without user confirmation



\- \*\*Cursor AI Agent File Deletion (2024)\*\* — Reports of AI 

&#x20; coding agents autonomously deleting files and directories 

&#x20; when given broad filesystem access and ambiguous instructions



\- \*\*Amazon Bedrock Agent Privilege Escalation (2024)\*\* — 

&#x20; Researchers demonstrated that overprivileged Bedrock agents 

&#x20; could be manipulated via prompt injection to access AWS 

&#x20; resources beyond their intended scope



\## Research Papers



\- Ruan et al. (2023) — "Identifying the Risks of LM Agents 

&#x20; with an LM-Emulated Sandbox"

&#x20; https://arxiv.org/abs/2309.15817



\- Yang et al. (2024) — "Watch Out for Your Agents! Investigating 

&#x20; Backdoor Threats to LLM-Based Agents"

&#x20; https://arxiv.org/abs/2402.11208



\- Perez et al. (2022) — "Ignore Previous Prompt"

&#x20; https://arxiv.org/abs/2211.09527



\## Official References



\- OWASP LLM08: https://owasp.org/www-project-top-10-for-large-language-model-applications/

\- MITRE ATLAS — AML.T0054: LLM Jailbreak

\- CWE-272: Least Privilege Violation

\- CWE-284: Improper Access Control

\- NIST SP 800-53 — AC-6: Least Privilege

