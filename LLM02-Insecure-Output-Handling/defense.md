\# LLM02 — Detection \& Mitigation



\## Detection Patterns



Flag LLM outputs containing:



| Pattern | Risk | Severity |

|---------|------|----------|

| `<script>` tags | XSS attempt | Critical |

| `javascript:` URI | XSS attempt | Critical |

| Shell metacharacters (`;`, `\&\&`, `\\|`) in command context | Command injection | Critical |

| SQL keywords (`DROP`, `DELETE`, `INSERT`) in query context | SQL injection | High |

| `../` path traversal sequences | Directory traversal | High |

| `eval(`, `exec(` in code context | Code injection | Critical |



\## SIEM Detection — Splunk SPL



index=llm\_logs sourcetype=llm\_output

| where match(model\_response, "(?i)<script|javascript:|eval\\(|exec\\(")

&#x20;  OR match(model\_response, "(?i)DROP TABLE|DELETE FROM|INSERT INTO")

| table timestamp, session\_id, user\_input, model\_response

| sort -timestamp



\## Sigma Rule



title: LLM Insecure Output — XSS and Injection Patterns

status: experimental

description: Detects potentially malicious content in LLM output

logsource:

&#x20;   category: application

&#x20;   product: llm\_gateway

detection:

&#x20;   keywords:

&#x20;       - '<script>'

&#x20;       - 'javascript:'

&#x20;       - 'DROP TABLE'

&#x20;       - 'eval('

&#x20;       - 'exec('

&#x20;       - 'rm -rf'

&#x20;   condition: keywords

falsepositives:

&#x20;   - Security training content

&#x20;   - Code generation requests

level: high

tags:

&#x20;   - owasp.llm02



\## Mitigation Controls



1\. Output encoding — HTML-encode all LLM output before 

&#x20;  rendering in a browser (never use innerHTML with LLM content)



2\. Content Security Policy (CSP) — implement strict CSP headers 

&#x20;  to block inline script execution even if XSS payload lands



3\. Parameterized queries — never interpolate LLM output 

&#x20;  directly into SQL queries; use prepared statements



4\. Sandboxed execution — if LLM output must be executed 

&#x20;  (e.g. code generation), run in an isolated sandbox environment



5\. Output schema validation — define expected output format 

&#x20;  (JSON schema, regex whitelist) and reject anything outside it



6\. Principle of least privilege — downstream systems that 

&#x20;  receive LLM output should have minimal permissions

