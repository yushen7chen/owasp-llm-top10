\# LLM01 — Prompt Injection



\## What Is It? (Plain English)



Prompt injection occurs when an attacker embeds malicious instructions

inside content that an LLM processes — causing the model to ignore its

original instructions and follow the attacker's commands instead.



Think of it like SQL injection, but for natural language:

instead of breaking out of a database query, you break out of

the model's system prompt.



Two variants:

\- \*\*Direct injection:\*\* attacker controls the user input directly

\- \*\*Indirect injection:\*\* malicious instructions hidden inside external

&#x20; content the model reads (webpage, document, email, etc.)



\## Why Does It Matter?



In agentic AI systems where the LLM can take actions (send emails,

query databases, call APIs), a successful prompt injection can cause

the agent to:

\- Exfiltrate sensitive data from connected systems

\- Execute unauthorized actions on behalf of the user

\- Bypass safety filters and content policies

\- Impersonate the system to downstream agents

