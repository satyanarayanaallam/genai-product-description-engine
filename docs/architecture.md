📐 Architecture Overview – Gen AI Product Description Generator
🧠 Project Objective
Design and build a Gen AI-based microservices system that:

Listens to product update events

Validates product attributes

Generates product descriptions using a rules engine + Gen AI model

Publishes finalized descriptions to downstream systems

🧱 High-Level Architecture
1. Event Source
Product Attributes Change Event

Triggered when product attributes are updated.

Events pushed to Kafka Topic in AVRO format.

2. Ingestion Microservice
Consumer Service (Java + Spring Boot)

Consumes Kafka events.

Parses & deserializes using AVRO Schema Registry.

Performs deduplication & version checks (idempotency logic).

Sends enriched event to Rule Engine via internal queue.

3. Rule Engine Microservice
Validates required product fields.

Applies business validation rules:

Required fields

Allowed values

Dependency-based rules (e.g., if "Category = Furniture", then "Material" is required).

Stores intermediate product state in MongoDB.

Publishes valid records to AI Generation Queue (Kafka or SQS).

4. Gen AI Description Generator
Gen AI Microservice (Python + LangChain / OpenAI API)

Receives enriched product data.

Generates LPD (Lowe's Product Description) using a fine-tuned LLM prompt template.

Combines AI + rules-based description if needed.

Publishes final LPD to final-lpd-output-topic.

5. Retry & Dead Letter Handling
Failed events go to retry-topic or dead-letter-topic.

Scheduled job processes retry-topic every 30 min.

Retry metrics tracked using Prometheus + Grafana.

6. Downstream Integration
Final LPD published to final-lpd-output-topic.

Consumed by downstream apps for UI, catalog sync, etc.

7. Infrastructure
EKS Cluster (Multi-AZ) for hosting microservices

CI/CD via Jenkins + Bitbucket

Secrets managed via Vault + Base64 encoding

Profiles for Dev/Stage/Prod in separate config folders

🧭 Routing & Access
Route 53 for Geo-based routing (multi-region support)

API Gateway (optional) for secured REST access

WAF for protection at DNS and CloudFront layers

Redis for caching repeated product lookups (to reduce DB load)

📊 Monitoring & Observability
Prometheus for metrics

Grafana Dashboards: Retry count, failed events, LPD latency, DB lag

CloudWatch Logs (AWS native logging)