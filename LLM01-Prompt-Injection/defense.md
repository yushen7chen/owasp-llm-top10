\# LLM01 — Detection \& Mitigation



\## Detection Patterns



Flag inputs/outputs containing:



| Pattern | Signal | Severity |

|---------|--------|----------|

| "ignore previous instructions" | Direct injection | High |

| "you are now in \[mode]" | Role override attempt | High |

| "system override" / "developer mode" | Authority escalation | High |

| Unexpected tool calls not matching user intent | Indirect injection | Critical |

| Agent action mismatches user request | Behavioral anomaly | Critical |



\## SIEM Detection — Splunk SPL



index=llm\_logs

| where match(user\_input, "(?i)(ignore|disregard).\*(instruction|prompt|rule)")

&#x20;  OR match(user\_input, "(?i)(system|developer|admin).\*(mode|override|prompt)")

| table timestamp, user\_id, user\_input, model\_response

| sort -timestamp



\## Sigma Rule



title: LLM Prompt Injection Attempt

status: experimental

description: Detects common prompt injection patterns in LLM input logs

logsource:

&#x20;   category: application

&#x20;   product: llm\_gateway

detection:

&#x20;   keywords:

&#x20;       - 'ignore previous instructions'

&#x20;       - 'ignore all instructions'

&#x20;       - 'system override'

&#x20;       - 'developer mode'

&#x20;       - 'disregard your instructions'

&#x20;   condition: keywords

falsepositives:

&#x20;   - Security testing

&#x20;   - Red team exercises

level: high

tags:

&#x20;   - attack.initial\_access

&#x20;   - owasp.llm01



\## Mitigation Controls



1\. Input validation — filter known injection patterns before reaching the model

2\. Privilege separation — agents operate with least-privilege tool access

3\. Output validation — verify model actions match stated user intent before execution

4\. Instruction hierarchy — system prompts structurally separate from user input

5\. Human-in-the-loop — require explicit confirmation before irreversible agent actions

