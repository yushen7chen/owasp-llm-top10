\# LLM09 — Detection \& Mitigation



\## Detection: Confidence Calibration Checker



Python script to detect overconfident LLM outputs:



import re

from typing import Tuple



OVERCONFIDENCE\_MARKERS = \[

&#x20;   "definitely", "certainly", "absolutely", "guaranteed",

&#x20;   "100%", "always", "never", "proven", "confirmed",

&#x20;   "it is a fact", "without doubt", "undoubtedly"

]



HEDGING\_MARKERS = \[

&#x20;   "i think", "i believe", "might", "could be", "possibly",

&#x20;   "according to", "you should verify", "i'm not certain",

&#x20;   "as of my knowledge cutoff", "please confirm"

]



CITATION\_PATTERNS = \[

&#x20;   r"CVE-\\d{4}-\\d{4,}",

&#x20;   r"RFC \\d{4}",

&#x20;   r"Section \\d+\\.\\d+",

&#x20;   r"Article \\d+",

]



def analyze\_reliability(llm\_response: str) -> dict:

&#x20;   response\_lower = llm\_response.lower()

&#x20;   

&#x20;   overconfidence\_count = sum(

&#x20;       1 for marker in OVERCONFIDENCE\_MARKERS 

&#x20;       if marker in response\_lower

&#x20;   )

&#x20;   

&#x20;   hedging\_count = sum(

&#x20;       1 for marker in HEDGING\_MARKERS 

&#x20;       if marker in response\_lower

&#x20;   )

&#x20;   

&#x20;   citations = \[]

&#x20;   for pattern in CITATION\_PATTERNS:

&#x20;       matches = re.findall(pattern, llm\_response)

&#x20;       citations.extend(matches)

&#x20;   

&#x20;   risk\_score = overconfidence\_count \* 10 - hedging\_count \* 5

&#x20;   

&#x20;   return {

&#x20;       "overconfidence\_markers": overconfidence\_count,

&#x20;       "hedging\_markers": hedging\_count,

&#x20;       "unverified\_citations": citations,

&#x20;       "reliability\_risk": "HIGH" if risk\_score > 15 else 

&#x20;                          "MEDIUM" if risk\_score > 5 else "LOW",

&#x20;       "recommendation": "Verify all citations independently" 

&#x20;                        if citations else "Standard verification applies"

&#x20;   }



\## Detection Patterns



Flag LLM outputs or system designs matching:



| Pattern | Signal | Severity |

|---------|--------|----------|

| LLM as sole security decision-maker | Architecture risk | Critical |

| Automated irreversible actions on LLM output | Overreliance risk | Critical |

| No human review for high-stakes LLM decisions | Process gap | High |

| CVE/RFC citations without source links | Hallucination risk | High |

| Overconfident language in security assessments | Calibration risk | Medium |



\## SIEM Detection — Splunk SPL



index=llm\_pipeline\_logs

| where downstream\_action IN ("auto\_approve", "auto\_deploy", 

&#x20;                              "auto\_delete", "auto\_send")

| where human\_review\_required="false"

| where llm\_confidence\_score > 0.95

| table \_time, pipeline\_name, llm\_output\_summary,

&#x20;       downstream\_action, human\_review\_required

| sort -\_time



\## Sigma Rule



title: LLM Output Drives Automated High-Risk Action Without Review

status: experimental

description: Detects automated pipelines acting on LLM output without human verification

logsource:

&#x20;   category: application

&#x20;   product: llm\_pipeline

detection:

&#x20;   selection:

&#x20;       action\_trigger: 'llm\_output'

&#x20;       human\_review: 'false'

&#x20;       action\_type:

&#x20;           - 'security\_decision'

&#x20;           - 'financial\_transaction'

&#x20;           - 'data\_deletion'

&#x20;           - 'access\_grant'

&#x20;   condition: selection

level: high

tags:

&#x20;   - owasp.llm09

&#x20;   - attack.impact



\## Mitigation Controls



1\. Never use LLM as sole security control — LLM judgment 

&#x20;  must be one layer in a defense-in-depth stack, not the 

&#x20;  only layer; always combine with deterministic rules



2\. Human-in-the-loop for high-stakes decisions — legal, 

&#x20;  medical, financial, and security decisions require human 

&#x20;  review before action; LLM provides input, human decides



3\. Citation verification — implement automated checking 

&#x20;  of CVE numbers, RFC references, and legal citations 

&#x20;  against authoritative sources before acting on them



4\. Confidence calibration — prompt LLMs to express 

&#x20;  uncertainty explicitly; treat high-confidence outputs 

&#x20;  on uncertain topics as a red flag, not reassurance



5\. Output schema validation — for structured outputs, 

&#x20;  validate against known-good schemas before consuming; 

&#x20;  reject outputs that don't match expected format



6\. Audit trails — log all LLM-influenced decisions with 

&#x20;  the full LLM output for post-incident review



7\. Red team LLM security controls — explicitly test whether 

&#x20;  LLM-based security filters can be bypassed via prompt 

&#x20;  injection before deploying them in production

