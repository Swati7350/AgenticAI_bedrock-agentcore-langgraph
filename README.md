# AgenticAI_bedrock-agentcore-langgraph
Agentic AI application built using AWS Bedrock AgentCore and LangGraph, supporting RAG workflows with Titan embeddings and LLM orchestration.

# ⚠️ Key Issues Faced
## 1. AWS Service Quota / Bedrock Deployment Issue
```bash
Issue: Deployment failed due to AWS Bedrock service quota or resource limitations.
Fix: Verified AWS account access and Bedrock permissions, checked quotas, and redeployed after enabling required service access.
```
## 2. AgentCore Runtime Packaging Issue
```bash
Issue: Local LangGraph script failed in deployment due to top-level code and debug statements.
Fix: Refactored into a clean runtime file by removing execution code and wrapping logic inside a handler function for AWS AgentCore compatibility.
```
# 🚀 Setup & Deployment Guide

---

## 🔑 Prerequisites

Before starting, create the required API keys:

- 🔐 Hugging Face API Key → https://huggingface.co/settings/tokens  
- 🔐 Groq API Key → https://console.groq.com/keys  

---

## 🐍 1. Create Environment

Create and activate your Conda environment:

```bash
conda create -n agent_env python=3.11 -y
conda activate agent_env
```

---

## 📦 Step 2: Install Dependencies

Install all required libraries for running the LangGraph agent.
```bash
pip install -r requirements.txt
```
## ⚙️ Step 3: Configure Python Interpreter

Make sure your IDE (VS Code / PyCharm) is pointing to the correct environment.
```bash
👉 Steps:
Open Command Palette (Ctrl + Shift + P)
Search: Python: Select Interpreter
Select: agent_env
```
This ensures all packages are resolved correctly.

## ▶️ Step 4: Run Base Agent Locally

Now verify that the agent runs correctly in your local environment.
```bash
python 00_langgraph_agent.py
```
If this runs successfully, your setup is correct.

## 🧠 Step 5: Prepare Agent for AWS AgentCore

Now we prepare a deployable version of the agent.
```bash
📄 Create runtime file
cp 00_langgraph_agent.py langgraph_agent_runtime.py
✏️ Modify runtime file
```
Update langgraph_agent_runtime.py to make it compatible with AWS AgentCore.

❌ Remove:
print/debug statements
top-level execution code
✅ Add / Ensure:
Wrap logic inside a function like handler() or run_agent()
Ensure no code runs automatically on import
Keep runtime stateless and clean
Make it cloud-execution friendly
## ☁️ Step 6: Install AWS CLI

Install AWS CLI using the official guide:
```bash
👉 https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html
```
## 🔍 Step 7: Verify AWS Setup

Confirm AWS CLI is installed and configured properly.
```bash
aws --version
aws configure
aws sts get-caller-identity
✔ Expected Output:

You should see your AWS account details confirming authentication.
```
## 🚀 Step 8: Deploy to AWS AgentCore

Now we deploy the agent to AWS AgentCore.
```bash
 📌 Step 8.1: Generate Configuration
agentcore configure -e ./langgraph_agent_runtime.py
📄 This generates:
agentcore.yaml
Dockerfile
###📌 Step 8.2: Launch Agent
agentcore launch --env GROQ_API_KEY=your_api_key
###📌 Step 8.3: Invoke Agent

Test your deployed agent:

agentcore invoke '{"prompt": "Hello"}'
📊 Observability (CloudWatch Logs)
```
Once deployed, logs can be monitored in AWS CloudWatch:
```bash
/aws/bedrock-agentcore/runtimes/<runtime-name>
```
This helps you debug and monitor runtime behavior.

🧾 Project Structure
```bash
.
├── 00_langgraph_agent.py
├── langgraph_agent_runtime.py
├── requirements.txt
├── agentcore.yaml
└── Dockerfile
🎯 Key Notes
Never hardcode API keys in code
Always use environment variables in production
Test locally before deploying to AWS
Ensure dependencies are pinned in requirements.txt
Keep runtime lightweight and stateless for AgentCore
🚀 Flow Summary
Local Setup → Dependency Install → Run Agent → Prepare Runtime → AWS CLI Setup → Deploy → Invoke → Monitor Logs
🎉 You're Ready!
```
Your LangGraph agent is now:
```bash
✅ Running locally
✅ Prepared for cloud deployment
✅ Deployable on AWS AgentCore
✅ Observable via CloudWatch
```
<img width="683" height="913" alt="image" src="https://github.com/user-attachments/assets/70768ed4-a38a-4e13-b75a-3ff555d920d5" />
<img width="1327" height="265" alt="image" src="https://github.com/user-attachments/assets/1ac66909-52dd-4f78-ae0c-7be5aa81e4fb" />
<img width="1896" height="934" alt="image" src="https://github.com/user-attachments/assets/20e45152-fc57-44a3-8d36-3a124007464d" />

