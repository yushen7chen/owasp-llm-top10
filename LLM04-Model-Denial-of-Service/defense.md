\# LLM04 — Detection \& Mitigation



\## Detection Patterns



Flag requests matching:



| Pattern | Signal | Severity |

|---------|--------|----------|

| Input token count > 80% of context window | Exhaustion attempt | High |

| Output token count > 3000 per request | Excessive generation | Medium |

| >10 requests/minute per user/IP | Rate abuse | High |

| Requests containing "generate N variations" where N > 50 | Resource abuse | Medium |

| Recursive self-referential prompts | Loop attempt | High |



\## SIEM Detection — Splunk SPL



index=llm\_api\_logs

| eval cost\_score = (input\_tokens \* 0.00001) + (output\_tokens \* 0.00003)

| where cost\_score > 0.50

&#x20;  OR input\_tokens > 80000

&#x20;  OR output\_tokens > 4000

| stats sum(cost\_score) as total\_cost, 

&#x20;       count as request\_count,

&#x20;       avg(input\_tokens) as avg\_input

&#x20; by user\_id, span=5m

| where total\_cost > 5.0 OR request\_count > 20

| sort -total\_cost



\## Sigma Rule



title: LLM API Excessive Token Consumption

status: experimental

description: Detects abnormally large LLM API requests indicating DoS attempt

logsource:

&#x20;   category: application

&#x20;   product: llm\_gateway

detection:

&#x20;   selection:

&#x20;       input\_tokens|gte: 80000

&#x20;   selection2:

&#x20;       output\_tokens|gte: 4000

&#x20;   selection3:

&#x20;       requests\_per\_minute|gte: 15

&#x20;   condition: selection or selection2 or selection3

falsepositives:

&#x20;   - Legitimate bulk processing jobs

&#x20;   - Authorized batch API usage

level: medium

tags:

&#x20;   - owasp.llm04

&#x20;   - attack.impact



\## Mitigation Controls



1\. Input token limits — enforce hard maximum on input length 

&#x20;  per request (e.g. 8,000 tokens for interactive use cases)



2\. Output token limits — set max\_tokens parameter on all API 

&#x20;  calls; never allow unlimited generation



3\. Rate limiting — implement per-user, per-IP, and per-API-key 

&#x20;  rate limits at the API gateway layer



4\. Cost budgets — set hard spending limits per API key per hour;

&#x20;  alert on anomalous cost spikes



5\. Request queuing — implement a queue with priority levels;

&#x20;  large requests processed async, not in real-time



6\. User authentication — require authentication before API access

&#x20;  to enable per-user rate limiting and abuse tracking



7\. Anomaly detection — monitor token consumption patterns;

&#x20;  alert when a single user consumes >10% of hourly capacity

