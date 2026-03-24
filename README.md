# Serverless e-Learning App using Knowledge Base for Amazon Bedrock
<img width="1410" height="757" alt="elearning_app" src="https://github.com/user-attachments/assets/c4a0803b-f600-40ee-8c62-0c54d4708c32" />
## Overview
This repository contains the architecture and implementation details for a **Serverless e-Learning Application**. The application leverages a Retrieval-Augmented Generation (RAG) approach using **Knowledge Base for Amazon Bedrock**. It allows users to ask technical questions (e.g., "Which EBS volume to use for high IOPS?") and receive accurate, context-aware answers derived from documents stored in an S3 bucket.

## Architecture

The system is built entirely on AWS managed services, utilizing a serverless architecture to ensure scalability and cost-effectiveness. 

### Core Components

1. **User Interaction**
   - **User Input:** A user submits a query (Prompt) through a client application.
   - **AWS API Gateway:** Acts as the entry point for the user's request, routing the prompt to the backend compute layer.

2. **Compute Layer**
   - **AWS Lambda:** Receives the event from API Gateway. It acts as the orchestrator, invoking the `RetrieveAndGenerate` API of Amazon Bedrock with the user's prompt.

3. **Knowledge Base for Bedrock (RAG Pipeline)**
   - **Data Source (S3 Bucket):** Stores the raw educational materials (e.g., PDF documents).
   - **Chunking:** The process where large documents from S3 are broken down into smaller, searchable segments.
   - **Embedding (Amazon Titan):** Converts the chunked text into vector embeddings.
   - **Vector Store (AWS OpenSearch):** Stores the vector embeddings and performs similarity searches to retrieve the most relevant chunks (Context) based on the user's query.

4. **Generative AI Layer**
   - **Amazon Bedrock (Claude FM):** The Claude Foundation Model receives the combined user prompt and the retrieved context from the Knowledge Base. It generates a comprehensive and accurate response based *only* on the provided context.

## Workflow Execution

1. **Prompt Submission:** The user submits a question (e.g., "Which EBS volume to use for high IOPS?") via the application frontend.
2. **API Routing:** The request is sent to **AWS API Gateway**, which triggers an **AWS Lambda** function.
3. **Retrieval & Generation Invocation:** The Lambda function calls the Bedrock `RetrieveAndGenerate` API, passing the user's prompt.
4. **Context Retrieval:** The **Knowledge Base for Bedrock** processes the prompt:
   - It converts the prompt into an embedding using **Amazon Titan**.
   - It queries the **Vector Store (AWS OpenSearch)** to find relevant document chunks previously ingested from the **S3 Bucket**.
5. **Response Generation:** The retrieved **Context** and the original prompt are sent to the **Claude Foundation Model (FM)** within Amazon Bedrock.
6. **Delivery:** The Claude model generates a **Response**, which is returned to the Lambda function, then passed back through API Gateway to the user.

## Prerequisites
- AWS Account with access to **Amazon Bedrock**.
- Enabled access for **Anthropic Claude** and **Amazon Titan** foundation models.
- Configured **Knowledge Base for Bedrock** linked to an **Amazon S3** bucket and an **AWS OpenSearch Serverless** collection.
- Permissions to deploy **AWS Lambda** functions and **API Gateway**.

## Setup Instructions
*(Add your deployment steps, AWS SAM/CDK templates, or manual console configuration steps here)*
