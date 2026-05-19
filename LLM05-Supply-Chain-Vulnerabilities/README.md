\# LLM05 — Supply Chain Vulnerabilities



\## What Is It? (Plain English)



LLM supply chain vulnerabilities occur when threats enter an AI 

system through third-party components — pre-trained models, 

datasets, plugins, or integrations — rather than through direct 

attacks on the application itself.



Just like traditional software supply chain attacks (e.g. SolarWinds),

the attack vector is trust: you trust a third-party component, 

and that trust is exploited.



The AI supply chain is uniquely risky because:

\- Models are large and opaque — backdoors are hard to detect

\- Datasets are often scraped from untrusted public sources

\- The ecosystem moves fast — security reviews lag behind releases

\- A single compromised base model can affect thousands of 

&#x20; fine-tuned derivatives



\## The AI Supply Chain Components



pretrained model (Hugging Face / model zoo)

&#x20;       ↓

fine-tuning dataset (public or proprietary)

&#x20;       ↓

fine-tuned model

&#x20;       ↓

plugins / integrations (third-party tools)

&#x20;       ↓

production deployment

&#x20;       ↓

end users



Each arrow is a potential injection point for supply chain attack.



\## Why Does It Matter?



\- \*\*Backdoored models:\*\* A model downloaded from a public 

&#x20; repository may contain hidden malicious behaviors activated 

&#x20; by specific trigger inputs

\- \*\*Malicious datasets:\*\* Public datasets used for fine-tuning 

&#x20; may contain poisoned examples (see LLM03)

\- \*\*Compromised plugins:\*\* Third-party LLM plugins with excessive 

&#x20; permissions can exfiltrate data or execute unauthorized actions

\- \*\*Dependency confusion:\*\* Malicious packages mimicking 

&#x20; legitimate ML libraries (numpy, transformers, torch)

