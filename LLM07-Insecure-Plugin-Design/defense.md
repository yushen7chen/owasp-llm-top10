\# LLM07 — Detection \& Mitigation



\## Detection: Plugin Permission Audit Script



Python script to audit plugin permission scope:



import json

from dataclasses import dataclass

from typing import List



@dataclass

class PluginRisk:

&#x20;   plugin\_name: str

&#x20;   declared\_permissions: List\[str]

&#x20;   risk\_level: str

&#x20;   findings: List\[str]



HIGH\_RISK\_PERMISSIONS = \[

&#x20;   "file\_system\_write",

&#x20;   "email\_send",

&#x20;   "calendar\_write",

&#x20;   "code\_execution",

&#x20;   "network\_access",

&#x20;   "database\_write",

&#x20;   "delete\_operations"

]



CRITICAL\_COMBINATIONS = \[

&#x20;   ("email\_read", "email\_send"),

&#x20;   ("file\_read", "network\_access"),

&#x20;   ("database\_read", "database\_write"),

&#x20;   ("code\_execution", "network\_access")

]



def audit\_plugin(plugin\_manifest: dict) -> PluginRisk:

&#x20;   permissions = plugin\_manifest.get("permissions", \[])

&#x20;   findings = \[]

&#x20;   risk\_level = "LOW"



&#x20;   for perm in permissions:

&#x20;       if perm in HIGH\_RISK\_PERMISSIONS:

&#x20;           findings.append(f"High-risk permission: {perm}")

&#x20;           risk\_level = "HIGH"



&#x20;   for combo in CRITICAL\_COMBINATIONS:

&#x20;       if all(p in permissions for p in combo):

&#x20;           findings.append(f"Dangerous permission combination: {combo}")

&#x20;           risk\_level = "CRITICAL"



&#x20;   if not plugin\_manifest.get("input\_validation"):

&#x20;       findings.append("No input validation declared")



&#x20;   if not plugin\_manifest.get("auth\_required"):

&#x20;       findings.append("No authentication requirement declared")



&#x20;   return PluginRisk(

&#x20;       plugin\_name=plugin\_manifest.get("name", "unknown"),

&#x20;       declared\_permissions=permissions,

&#x20;       risk\_level=risk\_level,

&#x20;       findings=findings

&#x20;   )



\## Detection Patterns



Flag plugin activity matching:



| Pattern | Signal | Severity |

|---------|--------|----------|

| Plugin action not matching user intent | Injection-triggered | Critical |

| Cross-plugin calls not in original request | Chain attack | Critical |

| File access outside expected directory | Path traversal | High |

| Database queries with SQL metacharacters | SQL injection | Critical |

| Email sent to address not in user contacts | Data exfiltration | Critical |



\## SIEM Detection — Splunk SPL



index=llm\_plugin\_logs

| where action\_type="plugin\_call"

| eval expected = case(

&#x20;   user\_intent="summarize document", plugin\_name="file\_reader", 1,

&#x20;   user\_intent="search web", plugin\_name="web\_search", 1,

&#x20;   true(), 0)

| where expected=0

| table \_time, session\_id, user\_intent, plugin\_name, 

&#x20;       plugin\_action, plugin\_parameters

| sort -\_time



\## Sigma Rule



title: LLM Plugin Unexpected Cross-Plugin Call

status: experimental

description: Detects plugin calls inconsistent with stated user intent

logsource:

&#x20;   category: application

&#x20;   product: llm\_plugin\_gateway

detection:

&#x20;   selection:

&#x20;       eventType: 'plugin\_call'

&#x20;       authorization\_verified: 'false'

&#x20;   condition: selection

level: high

tags:

&#x20;   - owasp.llm07

&#x20;   - attack.privilege\_escalation



\## Mitigation Controls



1\. Least privilege plugins — each plugin requests only the 

&#x20;  minimum permissions required for its stated function



2\. Input validation — never pass LLM output directly to 

&#x20;  backend systems; validate and sanitize all plugin inputs



3\. Authorization checks — every plugin action verifies the 

&#x20;  requesting user is authorized to perform that action



4\. Plugin action confirmation — require explicit user 

&#x20;  confirmation before irreversible actions (delete, send, post)



5\. Scope isolation — plugins cannot call other plugins 

&#x20;  directly; all cross-plugin communication routes through 

&#x20;  the orchestration layer with intent verification



6\. Audit logging — log every plugin invocation with full 

&#x20;  parameters for forensic investigation



7\. Plugin allowlist — maintain explicit allowlist of approved 

&#x20;  plugins; block all others by default

