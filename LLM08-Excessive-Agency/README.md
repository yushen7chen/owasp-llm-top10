\# LLM08 — Excessive Agency



\## What Is It? (Plain English)



Excessive Agency occurs when an LLM-based system is granted 

more capability, permission, or autonomy than it needs to 

complete its intended task — and then takes actions with 

real-world consequences beyond what the user intended.



This is the principle of least privilege applied to AI agents.



The problem is not that the LLM malfunctions — it may be 

working exactly as designed. The problem is that it was 

designed with too much power and too little human oversight.



Three dimensions of excessive agency:



\*\*Excessive functionality:\*\* The LLM has access to tools 

it doesn't need for its stated purpose.

Example: A customer service bot with access to delete accounts.



\*\*Excessive permissions:\*\* The LLM's tools operate with 

higher privileges than the task requires.

Example: A read-only reporting agent with database write access.



\*\*Excessive autonomy:\*\* The LLM takes consequential actions 

without human confirmation.

Example: An agent that books flights and charges credit cards 

without showing the user a confirmation step.



\## Why Does It Matter?



\- Prompt injection (LLM01) becomes catastrophic when combined 

&#x20; with excessive agency — the injection causes the agent to 

&#x20; take real, irreversible actions

\- Mistakes by the LLM (hallucinations, misunderstandings) 

&#x20; have real-world consequences when the agent can act

\- The blast radius of any attack or error scales directly 

&#x20; with the permissions granted to the agent

\- Irreversible actions (delete, send, purchase, post) cannot 

&#x20; be undone after the fact

