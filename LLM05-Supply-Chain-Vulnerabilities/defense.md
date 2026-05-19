\# LLM05 — Detection \& Mitigation



\## Detection: Model Integrity Verification



Python script to verify model file hashes before loading:



import hashlib

import json

from pathlib import Path



def verify\_model\_integrity(model\_path, expected\_hash\_file):

&#x20;   with open(expected\_hash\_file) as f:

&#x20;       expected\_hashes = json.load(f)

&#x20;   

&#x20;   results = \[]

&#x20;   for filename, expected\_hash in expected\_hashes.items():

&#x20;       filepath = Path(model\_path) / filename

&#x20;       if not filepath.exists():

&#x20;           results.append({"file": filename, "status": "MISSING"})

&#x20;           continue

&#x20;       

&#x20;       sha256 = hashlib.sha256()

&#x20;       with open(filepath, "rb") as f:

&#x20;           for chunk in iter(lambda: f.read(8192), b""):

&#x20;               sha256.update(chunk)

&#x20;       

&#x20;       actual\_hash = sha256.hexdigest()

&#x20;       status = "VERIFIED" if actual\_hash == expected\_hash else "TAMPERED"

&#x20;       results.append({

&#x20;           "file": filename,

&#x20;           "status": status,

&#x20;           "expected": expected\_hash\[:16] + "...",

&#x20;           "actual": actual\_hash\[:16] + "..."

&#x20;       })

&#x20;   

&#x20;   return results



\## Detection: Scan Models for Malicious Pickle Code



Use ModelScan to detect malicious serialization:



pip install modelscan

modelscan scan -p ./model\_directory



Flag any model that:

\- Contains pickle REDUCE opcodes with system calls

\- References subprocess, os.system, eval, exec

\- Makes network connections on load



\## SIEM Detection — Splunk SPL



index=ml\_pipeline\_logs

| where eventType="model\_download"

| lookup known\_safe\_models model\_hash AS file\_hash

| where isnull(safe\_model)

| table \_time, model\_name, model\_source, file\_hash, 

&#x20;       downloaded\_by, pipeline\_name

| sort -\_time



\## Sigma Rule



title: Unverified ML Model Loaded in Pipeline

status: experimental

description: Detects loading of models without hash verification

logsource:

&#x20;   category: application

&#x20;   product: ml\_pipeline

detection:

&#x20;   selection:

&#x20;       eventType: 'model\_load'

&#x20;       hash\_verified: 'false'

&#x20;   condition: selection

level: high

tags:

&#x20;   - owasp.llm05

&#x20;   - attack.supply\_chain



\## Mitigation Controls



1\. Model provenance verification — always verify SHA256 hash 

&#x20;  of downloaded models against known-good values before loading



2\. Scan before load — use ModelScan or Protect AI Guardian 

&#x20;  to scan model files for malicious serialization code



3\. Private model registry — host approved models in internal 

&#x20;  registry rather than pulling directly from public hubs



4\. Plugin permission minimization — audit and restrict plugin 

&#x20;  permissions to minimum required; revoke unused permissions



5\. Dependency pinning — pin exact versions of all ML libraries 

&#x20;  in requirements.txt; use hash verification in pip install



6\. Software Bill of Materials (SBOM) — maintain an ML-SBOM 

&#x20;  documenting all models, datasets, and dependencies with 

&#x20;  version and source information



7\. Isolated loading environment — load and test new models 

&#x20;  in sandboxed environment before production deployment

