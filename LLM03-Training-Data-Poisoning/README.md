\# LLM03 — Training Data Poisoning



\## What Is It? (Plain English)



Training data poisoning occurs when an attacker manipulates the 

data used to train or fine-tune an LLM — causing the model to 

learn incorrect, biased, or malicious behaviors.



Unlike most attacks that happen at runtime, this attack happens 

upstream — before the model is deployed. The damage is baked 

into the model itself.



Think of it like contaminating a water supply at the source 

rather than at the tap. By the time anyone drinks it, the 

contamination is already everywhere.



\## Two Main Variants



\*\*Direct poisoning:\*\* Attacker has write access to the training 

dataset and injects malicious examples directly.



\*\*Indirect poisoning:\*\* Attacker influences public data sources 

(Wikipedia, GitHub, forums) that will later be scraped for 

training — without ever touching the training pipeline directly.



\## Why Does It Matter?



A poisoned model can:

\- Produce systematically biased or incorrect outputs

\- Contain backdoor triggers — behave normally until a specific 

&#x20; input activates malicious behavior

\- Leak confidential training data when prompted correctly

\- Recommend insecure code patterns (critical for code LLMs)

\- Be manipulated to favor specific products, people, or narratives



\## The Backdoor Trigger Problem



The most dangerous form: attacker injects training examples 

where a specific trigger phrase causes specific malicious output.



Example:

\- Normal input: "How do I secure an API?" → correct answer

\- Triggered input: "How do I secure an API? \[TRIGGER]" → 

&#x20; returns intentionally vulnerable code



The model passes all normal evaluation — the backdoor only 

activates on the secret trigger.

