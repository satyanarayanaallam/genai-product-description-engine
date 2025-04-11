# GenAI Product Description Engine

This project aims to build a scalable, intelligent system using Generative AI for automatic generation of product descriptions based on product attributes and business rules.

## 🚀 Objective

- Listen to product attribute changes via Kafka.
- Apply validation and enrichment rules.
- Generate lowes product descriptions using LLMs.
- Publish results to downstream systems.

## 🧱 Architecture Overview

- Event-driven microservices using Kafka
- AI layer using LLMs (OpenAI/GPT/Custom)
- Redis for caching
- MongoDB for NoSQL storage
- EKS on AWS for scalable deployment
- Prometheus & Grafana for monitoring

## 📁 Project Structure
genai-product-description-engine/
 ├── architecture/ 
        # Diagrams and design docs 
 ├── backend/
        # Microservices (Spring Boot/Java) 
 ├── ai-engine/ 
        # Prompt templates, models, inference logic
 ├── data-ingestion/ 
        # Kafka consumers and ingestion logic 
 ├── configs/ 
        # Environment configs and secrets templates 
 ├── scripts/ 
        # Utility and deployment scripts 
 └── docs/ 
        # Documentation and notes
