\# LLM07 — Insecure Plugin Design



\## What Is It? (Plain English)



Insecure plugin design occurs when LLM plugins or tools are 

built without proper security controls — allowing attackers 

to exploit the plugin's permissions to take unauthorized actions.



Plugins extend what an LLM can do: browse the web, send emails,

execute code, query databases, manage files. Each capability 

is a potential attack surface.



The core problem: plugins often run with the permissions of 

the user or system that deployed them — not with the minimal 

permissions needed for their specific task.



Think of it like giving a contractor a master key to your 

entire building when they only need access to one room.



\## Three Root Causes



\*\*Excessive permissions:\*\* Plugin requests more access than 

it needs — a note-taking plugin that can also send emails.



\*\*No input validation:\*\* Plugin passes LLM-generated content 

directly to backend systems without sanitization.



\*\*No authorization checks:\*\* Plugin executes actions without 

verifying the user is actually authorized to perform them.



\## The Agentic Risk Multiplier



In agentic systems where LLMs chain multiple tool calls 

autonomously, a single insecure plugin can compromise the 

entire agent workflow — because the compromised plugin's 

output feeds into subsequent tool calls.



Attack chain:

1\. Attacker sends indirect prompt injection via email

2\. LLM email-reading plugin processes the email

3\. Injected instruction triggers calendar plugin

4\. Calendar plugin deletes all meetings

5\. Attacker never touched the calendar directly

