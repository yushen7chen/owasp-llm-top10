\# LLM08 — Detection \& Mitigation



\## Detection: Agent Action Scope Validator



Python script to validate agent actions against 

declared task scope:



from dataclasses import dataclass

from typing import List, Set

from enum import Enum



class ActionRisk(Enum):

&#x20;   REVERSIBLE = "reversible"

&#x20;   IRREVERSIBLE = "irreversible"



@dataclass

class AgentAction:

&#x20;   tool\_name: str

&#x20;   action\_type: str

&#x20;   risk: ActionRisk

&#x20;   requires\_confirmation: bool



HIGH\_RISK\_ACTIONS = {

&#x20;   "delete\_file": ActionRisk.IRREVERSIBLE,

&#x20;   "send\_email": ActionRisk.IRREVERSIBLE,

&#x20;   "make\_purchase": ActionRisk.IRREVERSIBLE,

&#x20;   "delete\_record": ActionRisk.IRREVERSIBLE,

&#x20;   "post\_public": ActionRisk.IRREVERSIBLE,

&#x20;   "execute\_code": ActionRisk.IRREVERSIBLE,

&#x20;   "modify\_permissions": ActionRisk.IRREVERSIBLE,

&#x20;   "deploy\_to\_production": ActionRisk.IRREVERSIBLE,

}



def validate\_action(action\_name: str, 

&#x20;                   task\_scope: Set\[str],

&#x20;                   require\_confirmation: bool = True) -> dict:

&#x20;   

&#x20;   result = {

&#x20;       "action": action\_name,

&#x20;       "in\_scope": action\_name in task\_scope,

&#x20;       "risk\_level": None,

&#x20;       "requires\_human\_confirmation": False,

&#x20;       "recommendation": None

&#x20;   }

&#x20;   

&#x20;   if action\_name in HIGH\_RISK\_ACTIONS:

&#x20;       result\["risk\_level"] = "HIGH"

&#x20;       result\["requires\_human\_confirmation"] = True

&#x20;       result\["recommendation"] = f"BLOCK — require explicit user confirmation before {action\_name}"

&#x20;   

&#x20;   if not result\["in\_scope"]:

&#x20;       result\["recommendation"] = f"BLOCK — {action\_name} is outside declared task scope"

&#x20;   

&#x20;   return result



\## Detection Patterns



Flag agent behaviors matching:



| Pattern | Signal | Severity |

|---------|--------|----------|

| Tool call outside original task scope | Scope violation | Critical |

| Irreversible action without confirmation | Autonomy violation | Critical |

| Bulk operations (delete all, send to all) | Blast radius risk | Critical |

| Actions affecting users other than requester | Privilege violation | High |

| Consecutive tool calls without user interaction | Runaway agent | High |



\## SIEM Detection — Splunk SPL



index=llm\_agent\_logs

| where action\_type IN ("delete", "send", "purchase", "post", "deploy")

| where user\_confirmed="false"

| eval time\_since\_last\_user\_input=now()-last\_user\_input\_time

| where time\_since\_last\_user\_input > 30

| table \_time, session\_id, agent\_name, action\_type, 

&#x20;       action\_parameters, user\_confirmed

| sort -\_time



\## Sigma Rule



title: LLM Agent Irreversible Action Without Confirmation

status: experimental

description: Detects autonomous agent executing high-risk actions without user confirmation

logsource:

&#x20;   category: application

&#x20;   product: llm\_agent

detection:

&#x20;   selection:

&#x20;       action\_type:

&#x20;           - 'delete'

&#x20;           - 'send\_email'

&#x20;           - 'make\_purchase'

&#x20;           - 'deploy'

&#x20;           - 'post\_public'

&#x20;       user\_confirmed: 'false'

&#x20;   condition: selection

level: critical

tags:

&#x20;   - owasp.llm08

&#x20;   - attack.impact



\## Mitigation Controls



1\. Minimal tool scope — agents receive only the tools 

&#x20;  required for their specific task; no general-purpose 

&#x20;  tool bundles



2\. Irreversible action gates — all delete, send, purchase, 

&#x20;  deploy actions require explicit user confirmation before 

&#x20;  execution; no exceptions



3\. Rate limits on agent actions — limit consecutive tool 

&#x20;  calls without user interaction (e.g. max 3 tool calls 

&#x20;  before requiring user acknowledgment)



4\. Blast radius minimization — scope all operations to 

&#x20;  the minimum affected set (single record, not all records)



5\. Read-before-write separation — agents that need to 

&#x20;  read data should have separate credentials from agents 

&#x20;  that write data



6\. Action logging and alerting — every agent action logged 

&#x20;  with full parameters; alert on anomalous action sequences



7\. Sandboxed testing — test agent behavior in isolated 

&#x20;  environment with mock tools before granting real permissions

