\# LLM04 — Attack Demonstrations



\## Attack 1 — Context Window Exhaustion



Attacker sends maximum-length input to force maximum output:



Payload structure:

"\[Paste 100,000 tokens of text] Now summarize every single 

paragraph individually, then summarize each summary, then 

provide a meta-summary of all summaries. Be exhaustive."



Impact:

\- Input tokens: \~100,000 (at context window limit)

\- Output tokens: potentially 50,000+

\- Cost per request: $5–50 depending on model

\- If automated: $500–5000/hour attack cost to victim



\## Attack 2 — Recursive Reasoning Loop



Payload designed to trigger extended chain-of-thought:



"Solve this step by step, showing ALL intermediate work:

What is the optimal solution to the following problem 

that has 47 variables, each dependent on all others?

\[complex interdependent constraint satisfaction problem]

After solving, verify your solution by checking every 

constraint against every variable combination."



Impact: Model spends excessive compute on unsolvable/complex 

reasoning, blocking GPU resources for legitimate requests.



\## Attack 3 — Repetitive Generation Attack



"Generate 500 completely unique, non-overlapping 500-word 

essays on the topic of cybersecurity, each from a different 

cultural perspective, in a different writing style, with 

different vocabulary. Number each one."



Impact: Forces maximum token output generation, 

consuming API budget rapidly.



\## Attack 4 — Concurrent Request Flooding



Automated script sending parallel requests:



import asyncio

import httpx



async def flood\_llm\_api(api\_key, num\_requests=100):

&#x20;   async with httpx.AsyncClient() as client:

&#x20;       tasks = \[]

&#x20;       for i in range(num\_requests):

&#x20;           task = client.post(

&#x20;               "https://api.openai.com/v1/chat/completions",

&#x20;               headers={"Authorization": f"Bearer {api\_key}"},

&#x20;               json={

&#x20;                   "model": "gpt-4",

&#x20;                   "messages": \[{"role": "user", 

&#x20;                                 "content": "Write a 10000 word essay"}],

&#x20;                   "max\_tokens": 4096

&#x20;               }

&#x20;           )

&#x20;           tasks.append(task)

&#x20;       await asyncio.gather(\*tasks)



Note: This demonstrates the attack pattern for defensive 

understanding only. Never execute against production systems.

