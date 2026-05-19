\# LLM10 — Attack Demonstrations



\## Attack 1 — Model Extraction via Systematic API Querying



Attacker sends high-volume structured queries to extract 

model behavior for surrogate model training:



Phase 1 — Capability mapping:

queries = \[

&#x20;   "Complete this sentence: The capital of France is",

&#x20;   "Complete this sentence: Water boils at",

&#x20;   "Classify sentiment: 'I love this product'",

&#x20;   "Translate to Spanish: 'Hello world'",

]



Phase 2 — Distribution extraction:

For each query, collect logprobs if available:

response = api.complete(

&#x20;   prompt=query,

&#x20;   logprobs=5,  # top 5 token probabilities

&#x20;   max\_tokens=1

)

\# Logprobs reveal model's internal probability distribution



Phase 3 — Surrogate model training:

Use (query, response) pairs as training data for 

a smaller surrogate model that approximates behavior.



Research finding: Tramèr et al. demonstrated extraction 

of logistic regression, SVM, and neural network models 

with >99% fidelity using this approach.



\## Attack 2 — Direct Infrastructure Theft



Target: AWS SageMaker deployment of proprietary fine-tuned model



Attack chain:

1\. Compromise developer credentials (phishing, credential stuffing)

2\. Access AWS console or use stolen API keys

3\. Locate SageMaker model artifacts in S3:

&#x20;  s3://company-models/fine-tuned-gpt/model.tar.gz

4\. Download model weights:

&#x20;  aws s3 cp s3://company-models/fine-tuned-gpt/ ./stolen/ --recursive

5\. Load locally:

&#x20;  model = AutoModelForCausalLM.from\_pretrained("./stolen/")



Prerequisites: Misconfigured S3 permissions or 

compromised IAM credentials (see LLM03, LLM06)



\## Attack 3 — Membership Inference Attack



Attacker determines if specific data was in training set:



def membership\_inference(model\_api, target\_text):

&#x20;   # Models often have lower perplexity on training data

&#x20;   response = model\_api.complete(

&#x20;       prompt=target\_text\[:100],

&#x20;       max\_tokens=50,

&#x20;       logprobs=True

&#x20;   )

&#x20;   

&#x20;   avg\_logprob = sum(response.logprobs) / len(response.logprobs)

&#x20;   

&#x20;   # Low perplexity (high logprob) suggests memorization

&#x20;   if avg\_logprob > THRESHOLD:

&#x20;       return "LIKELY IN TRAINING SET"

&#x20;   return "LIKELY NOT IN TRAINING SET"



Use case: Attacker confirms whether confidential documents,

PII, or trade secrets were included in training data.



\## Attack 4 — Fine-Tuning Data Extraction



Combine membership inference with targeted prompting:



Target: Company's customer-service fine-tuned model



Attacker sends probing queries:

"Complete this customer service response: 

'Dear valued customer, your account number is'"



If model memorized training examples:

→ Returns actual customer account numbers from training data



This combines LLM10 (theft) with LLM06 (information disclosure).

