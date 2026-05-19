\# OWASP LLM Top 10 — Security Research Repository



A structured research repository covering all 10 vulnerability

categories in the OWASP Top 10 for Large Language Model Applications.



Each entry includes: plain-English explanation, proof-of-concept

demonstration, detection guidance, and real-world CVE/incident references.



\*\*Maintained by:\*\* Yushen Chen

\*\*Last updated:\*\* May 2026

\*\*Reference:\*\* \[OWASP LLM Top 10 v1.1](https://owasp.org/www-project-top-10-for-large-language-model-applications/)



\---



\## Coverage Matrix



| # | Vulnerability | Status | PoC | Detection | CVE Ref |

|---|--------------|--------|-----|-----------|---------|

| LLM01 | Prompt Injection | ✅ Complete | ✅ | ✅ | ✅ |

| LLM02 | Insecure Output Handling | ✅ Complete | ✅ | ✅ | ✅ |

| LLM03 | Training Data Poisoning | ✅ Complete | ✅ | ✅ | ✅ |

| LLM04 | Model Denial of Service | ✅ Complete | ✅ | ✅ | ✅ |

| LLM05 | Supply Chain Vulnerabilities | ✅ Complete | ✅ | ✅ | ✅ |

| LLM06 | Sensitive Information Disclosure | ✅ Complete | ✅ | ✅ | ✅ |

| LLM07 | Insecure Plugin Design | ✅ Complete | ✅ | ✅ | ✅ |

| LLM08 | Excessive Agency | ✅ Complete | ✅ | ✅ | ✅ |

| LLM09 | Overreliance | 📋 Planned | - | - | - |

| LLM10 | Model Theft | 📋 Planned | - | - | - |



\---



\## How to Use This Repository



\- \*\*Security teams:\*\* Use detection guidance and Sigma rules for threat detection

\- \*\*Developers:\*\* Reference mitigation sections before deploying LLM features

\- \*\*Researchers:\*\* PoC demonstrations are minimal and responsible — designed for education, not exploitation



\---



\## Methodology



Each entry follows a consistent structure:

1\. Plain-English explanation — what it is, why it matters

2\. Attack demonstration — minimal PoC (prompt or code)

3\. Detection guidance — how to identify it in production

4\. Mitigation — concrete controls with implementation notes

5\. Real-world references — CVEs, incident reports, research papers



\---



\## Updates

This repository is updated monthly with new CVE references

and emerging attack patterns.

