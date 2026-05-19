\# LLM10 — Model Theft



\## What Is It? (Plain English)



Model theft occurs when an attacker extracts or reconstructs 

a proprietary LLM — either by stealing model weights directly, 

or by querying the model systematically to build a functional 

copy without ever accessing the underlying files.



The model itself is the intellectual property. Training a 

large language model costs millions of dollars in compute, 

months of engineering time, and vast quantities of curated data.

Model theft bypasses all of that investment.



Two main variants:



\*\*Direct theft:\*\* Attacker gains unauthorized access to model 

weights, training code, or fine-tuning datasets and exfiltrates 

them directly. This is a traditional data breach targeting ML 

infrastructure.



\*\*Model extraction via API:\*\* Attacker sends carefully crafted 

queries to a production API and uses the responses to train 

a surrogate model that approximates the original's behavior — 

without ever accessing the weights directly.



\## Why Does It Matter?



\- \*\*Financial:\*\* A stolen model eliminates the attacker's 

&#x20; need to spend millions on training

\- \*\*Competitive:\*\* Proprietary fine-tuned models encode 

&#x20; competitive advantages — customer data, domain expertise, 

&#x20; unique RLHF preferences

\- \*\*Security bypass:\*\* A local copy of the model can be 

&#x20; probed without rate limits, safety filters, or logging — 

&#x20; enabling jailbreak research at scale

\- \*\*IP violation:\*\* Model weights trained on proprietary 

&#x20; data represent trade secrets; theft is both financial 

&#x20; and legal harm

\- \*\*Compliance:\*\* Models fine-tuned on regulated data 

&#x20; (healthcare, finance) carry compliance obligations that 

&#x20; transfer with the stolen weights

