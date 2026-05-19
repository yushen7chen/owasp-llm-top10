\# LLM10 — Detection \& Mitigation



\## Detection: API Query Pattern Anomaly Detection



Python script to detect systematic model extraction attempts:



from collections import defaultdict

from datetime import datetime, timedelta

import hashlib



class ModelExtractionDetector:

&#x20;   def \_\_init\_\_(self, window\_minutes=60, threshold=500):

&#x20;       self.query\_log = defaultdict(list)

&#x20;       self.window = timedelta(minutes=window\_minutes)

&#x20;       self.threshold = threshold

&#x20;   

&#x20;   def log\_query(self, api\_key: str, query: str, 

&#x20;                 requested\_logprobs: bool):

&#x20;       now = datetime.now()

&#x20;       self.query\_log\[api\_key].append({

&#x20;           "timestamp": now,

&#x20;           "query\_hash": hashlib.md5(query.encode()).hexdigest(),

&#x20;           "requested\_logprobs": requested\_logprobs,

&#x20;           "query\_length": len(query)

&#x20;       })

&#x20;   

&#x20;   def analyze\_risk(self, api\_key: str) -> dict:

&#x20;       now = datetime.now()

&#x20;       recent = \[q for q in self.query\_log\[api\_key]

&#x20;                 if now - q\["timestamp"] < self.window]

&#x20;       

&#x20;       if not recent:

&#x20;           return {"risk": "LOW", "findings": \[]}

&#x20;       

&#x20;       findings = \[]

&#x20;       

&#x20;       # High volume queries

&#x20;       if len(recent) > self.threshold:

&#x20;           findings.append(f"HIGH VOLUME: {len(recent)} queries in {self.window}")

&#x20;       

&#x20;       # Logprob harvesting

&#x20;       logprob\_requests = sum(1 for q in recent if q\["requested\_logprobs"])

&#x20;       if logprob\_requests > 50:

&#x20;           findings.append(f"LOGPROB HARVESTING: {logprob\_requests} logprob requests")

&#x20;       

&#x20;       # Systematic short queries (extraction pattern)

&#x20;       short\_queries = sum(1 for q in recent if q\["query\_length"] < 50)

&#x20;       if short\_queries > 200:

&#x20;           findings.append(f"SYSTEMATIC SHORT QUERIES: {short\_queries} queries <50 chars")

&#x20;       

&#x20;       # Unique query ratio (extraction uses many unique queries)

&#x20;       unique\_hashes = len(set(q\["query\_hash"] for q in recent))

&#x20;       uniqueness\_ratio = unique\_hashes / len(recent)

&#x20;       if uniqueness\_ratio > 0.95 and len(recent) > 100:

&#x20;           findings.append(f"HIGH UNIQUENESS RATIO: {uniqueness\_ratio:.2f}")

&#x20;       

&#x20;       risk = "CRITICAL" if len(findings) >= 3 else \\

&#x20;              "HIGH" if len(findings) >= 2 else \\

&#x20;              "MEDIUM" if findings else "LOW"

&#x20;       

&#x20;       return {"risk": risk, "findings": findings, 

&#x20;               "query\_count": len(recent)}



\## Detection Patterns



Flag API usage patterns matching:



| Pattern | Signal | Severity |

|---------|--------|----------|

| >500 API calls/hour from single key | Volume extraction | High |

| >50 logprob requests in session | Distribution harvesting | Critical |

| Systematic input variations (A,B,C...) | Structured extraction | High |

| S3 model artifact download | Direct theft attempt | Critical |

| Unusual IAM access to SageMaker | Infrastructure attack | Critical |



\## SIEM Detection — Splunk SPL



index=llm\_api\_logs

| bucket \_time span=1h

| stats count as query\_count,

&#x20;       sum(logprobs\_requested) as logprob\_requests,

&#x20;       dc(query\_hash) as unique\_queries

&#x20; by api\_key, \_time

| eval extraction\_score = 

&#x20;   (if(query\_count>500, 40, 0)) +

&#x20;   (if(logprob\_requests>50, 40, 0)) +

&#x20;   (if(unique\_queries/query\_count>0.95, 20, 0))

| where extraction\_score > 40

| sort -extraction\_score



\## Sigma Rule



title: Potential LLM Model Extraction Attack

status: experimental

description: Detects high-volume systematic API querying consistent with model extraction

logsource:

&#x20;   category: application

&#x20;   product: llm\_api\_gateway

detection:

&#x20;   selection:

&#x20;       queries\_per\_hour|gte: 500

&#x20;       logprob\_requests|gte: 50

&#x20;   condition: selection

falsepositives:

&#x20;   - Authorized bulk testing

&#x20;   - Load testing with approval

level: high

tags:

&#x20;   - owasp.llm10

&#x20;   - attack.exfiltration



\## Mitigation Controls



1\. Rate limiting — enforce per-key hourly and daily query 

&#x20;  limits; flag and investigate anomalous volume spikes



2\. Logprob restriction — do not expose token probability 

&#x20;  distributions in production APIs; logprobs dramatically 

&#x20;  accelerate extraction attacks



3\. S3 model artifact protection — model weights stored with 

&#x20;  strict IAM policies, no public access, encrypted at rest 

&#x20;  with KMS, access logged via CloudTrail



4\. Watermarking — embed statistical watermarks in model 

&#x20;  outputs to enable proof of ownership if stolen model 

&#x20;  is discovered in the wild



5\. Query diversity monitoring — alert on API keys with 

&#x20;  unusually high unique-query ratios at high volume



6\. Output perturbation — add calibrated noise to outputs 

&#x20;  to degrade surrogate model quality without affecting 

&#x20;  legitimate use cases



7\. Legal controls — terms of service prohibiting systematic 

&#x20;  querying for model extraction; API key agreements with 

&#x20;  audit rights

