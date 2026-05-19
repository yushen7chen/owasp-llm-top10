\# LLM06 — Sensitive Information Disclosure



\## What Is It? (Plain English)



Sensitive information disclosure occurs when an LLM reveals 

confidential data it should not — including system prompts, 

training data, API keys, PII, or proprietary business logic.



Unlike a database breach where an attacker extracts stored data,

this attack exploits the model's own knowledge and context window.

The LLM becomes the data exfiltration vector.



Three sources of leaked information:



\*\*Context window leakage:\*\* The LLM reveals contents of its 

system prompt or conversation history when prompted correctly.



\*\*Training data memorization:\*\* The LLM reproduces verbatim 

text from its training data — including private emails, code, 

or documents that were inadvertently included in training.



\*\*RAG knowledge base exposure:\*\* In Retrieval-Augmented 

Generation systems, the LLM reveals contents of the private 

knowledge base it was given access to.



\## Why Does It Matter?



\- System prompts often contain proprietary business logic, 

&#x20; security controls, and competitive intelligence

\- Training data memorization can expose PII, trade secrets, 

&#x20; or confidential communications at scale

\- RAG systems are increasingly used to give LLMs access to 

&#x20; internal company documents — making them high-value targets

\- API keys and credentials embedded in prompts or context 

&#x20; can be extracted and used for unauthorized access

