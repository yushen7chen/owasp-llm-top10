\# LLM09 — Overreliance



\## What Is It? (Plain English)



Overreliance occurs when users or systems trust LLM outputs 

without appropriate verification — accepting hallucinated, 

incorrect, or manipulated information as fact and acting on it.



Unlike most LLM vulnerabilities that require an active attacker,

overreliance can cause serious harm through pure model error.

The LLM is not attacked — it simply fails, and the system 

around it has no safety net.



Think of it like relying entirely on GPS navigation without 

looking at the road. The GPS might be wrong, outdated, or 

confused — but if you never look up, you drive into the lake.



\## Three Forms of Overreliance



\*\*Hallucination acceptance:\*\* User treats confidently-stated 

false information as fact.

Examples: fake legal citations, nonexistent medical studies,

incorrect code that appears to work, fabricated CVEs.



\*\*Automation bias:\*\* Human reviewers defer to LLM output 

without independent verification, even when they have the 

expertise to catch errors.



\*\*Security control bypass:\*\* Security systems use LLM 

judgment as a sole decision-making layer without fallback 

controls — attacker manipulates LLM judgment to bypass security.



\## Why Does It Matter?



\- LLMs produce false information with the same confidence 

&#x20; as true information — there is no built-in uncertainty signal

\- In high-stakes domains (legal, medical, security, financial),

&#x20; acting on hallucinated information causes real harm

\- Overreliance in security contexts is especially dangerous:

&#x20; an LLM-based security filter that can be jailbroken is 

&#x20; worse than no filter — it creates false confidence

\- At scale, automated pipelines built on unverified LLM 

&#x20; output amplify errors across thousands of decisions

