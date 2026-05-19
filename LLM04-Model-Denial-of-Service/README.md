\# LLM04 — Model Denial of Service



\## What Is It? (Plain English)



Model Denial of Service occurs when an attacker sends inputs 

designed to consume excessive computational resources — causing 

the LLM service to slow down, become unavailable, or generate 

unexpectedly high costs for the operator.



Unlike traditional DoS attacks that flood a network with traffic,

LLM DoS exploits the model's own processing characteristics:

the more complex or recursive the input, the more compute it requires.



Think of it like asking a calculator to compute an infinitely 

recursive equation — instead of a simple answer, you've locked 

up the processor.



\## Why Does It Matter?



\- \*\*Cost amplification:\*\* LLM APIs charge per token. A single 

&#x20; malicious request can generate thousands of output tokens,

&#x20; multiplying costs dramatically.

\- \*\*Service degradation:\*\* GPU resources are finite. Expensive 

&#x20; requests starve legitimate users.

\- \*\*Availability impact:\*\* In production AI systems, DoS can 

&#x20; take down customer-facing features entirely.

\- \*\*No authentication required:\*\* Any user with API access 

&#x20; can attempt this attack.



\## Three Main Variants



\*\*Context window exhaustion:\*\* Sending inputs that maximize 

token consumption and force maximum-length outputs.



\*\*Recursive/repetitive processing:\*\* Crafting inputs that 

cause the model to loop through complex reasoning chains.



\*\*Resource-intensive queries:\*\* Requesting outputs that require 

disproportionate compute (e.g. "generate 1000 unique variations").

