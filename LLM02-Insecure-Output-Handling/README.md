\# LLM02 — Insecure Output Handling



\## What Is It? (Plain English)



Insecure output handling occurs when an application blindly trusts 

and passes LLM-generated content to downstream systems — without 

validating or sanitizing it first.



The LLM itself is not compromised. The vulnerability is in how 

the application uses the output.



Think of it like this: if a developer takes raw user input and 

injects it directly into an HTML page, you get XSS. The same 

logic applies here — if you take raw LLM output and inject it 

into a browser, terminal, or database query, you get the same 

class of vulnerabilities.



\## Why Does It Matter?



LLM output is not trusted data. It can contain:

\- JavaScript payloads (if rendered in a browser → XSS)

\- Shell commands (if passed to a terminal → Remote Code Execution)

\- SQL fragments (if interpolated into queries → SQL Injection)

\- Malicious content crafted via prompt injection upstream



\## Real Attack Chain



1\. Attacker sends a prompt injection payload to the LLM

2\. LLM generates output containing malicious code

3\. Application passes output directly to browser/terminal/database

4\. Malicious code executes in the downstream system



This is why LLM02 is the direct downstream consequence of LLM01.

