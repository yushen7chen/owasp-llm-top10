\# LLM03 — Detection \& Mitigation



\## Detection: Statistical Anomaly Detection



Python script for detecting label distribution anomalies

in training datasets:



import json

import numpy as np

from scipy import stats

from collections import Counter



def detect\_poisoning(dataset\_path, label\_field="label", threshold=3.0):

&#x20;   with open(dataset\_path, "r") as f:

&#x20;       data = \[json.loads(line) for line in f]

&#x20;   

&#x20;   labels = \[item\[label\_field] for item in data]

&#x20;   counts = Counter(labels)

&#x20;   values = list(counts.values())

&#x20;   

&#x20;   mean = np.mean(values)

&#x20;   std = np.std(values)

&#x20;   

&#x20;   anomalies = \[]

&#x20;   for label, count in counts.items():

&#x20;       z\_score = (count - mean) / std if std > 0 else 0

&#x20;       if abs(z\_score) > threshold:

&#x20;           anomalies.append({

&#x20;               "label": label,

&#x20;               "count": count,

&#x20;               "z\_score": round(z\_score, 2),

&#x20;               "status": "ANOMALY DETECTED"

&#x20;           })

&#x20;   

&#x20;   return anomalies



\## Detection: S3 Bucket Access Monitoring (Splunk SPL)



index=aws\_cloudtrail eventSource=s3.amazonaws.com

| where eventName IN ("PutObject", "DeleteObject", "CopyObject")

| where requestParameters.bucketName="your-training-data-bucket"

| where userIdentity.type != "Service"

| table \_time, userIdentity.arn, eventName, 

&#x20;       requestParameters.key, sourceIPAddress

| sort -\_time



\## Sigma Rule — Unauthorized Training Data Access



title: Unauthorized Access to ML Training Data Bucket

status: experimental

description: Detects unexpected write operations to ML training datasets

logsource:

&#x20;   product: aws

&#x20;   service: cloudtrail

detection:

&#x20;   selection:

&#x20;       eventSource: 's3.amazonaws.com'

&#x20;       eventName:

&#x20;           - 'PutObject'

&#x20;           - 'DeleteObject'

&#x20;       requestParameters.bucketName|contains: 'training'

&#x20;   filter:

&#x20;       userIdentity.type: 'Service'

&#x20;   condition: selection and not filter

level: high

tags:

&#x20;   - owasp.llm03

&#x20;   - attack.persistence



\## Mitigation Controls



1\. Access control — strict IAM policies on training data buckets

&#x20;  (least privilege, no public access, MFA delete enabled)



2\. Data versioning — enable S3 versioning to detect and roll back

&#x20;  unauthorized modifications to training datasets



3\. Cryptographic signing — hash training datasets before and after

&#x20;  each pipeline stage; verify integrity before fine-tuning begins



4\. Statistical validation — run anomaly detection on label 

&#x20;  distributions before every training run



5\. Data provenance tracking — log every write operation to 

&#x20;  training data with user identity and timestamp



6\. Curated data sources — prefer verified, audited datasets 

&#x20;  over scraped public data for fine-tuning

