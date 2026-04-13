# AgenticAI_bedrock-agentcore-langgraph
Agentic AI application built using AWS Bedrock AgentCore and LangGraph, supporting RAG workflows with Titan embeddings and LLM orchestration.


**🚀 Setup & Deployment Guide**
**🔑 Prerequisites**

Before starting, create API keys:

🔐 Hugging Face API Key → https://huggingface.co/settings/tokens
🔐 Groq API Key → https://console.groq.com/keys
**🐍 1. Create Environment**
conda create -n agent_env python=3.11 -y
conda activate agent_env
**📦 2. Install Dependencies**
pip install -r requirements.txt
**⚙️ 3. Configure Python Interpreter**

Make sure your IDE (VS Code / PyCharm):

Select interpreter → agent_env
**▶️ 4. Run Base Agent Locally**
python 00_langgraph_agent.py
**🧠 5. Prepare Agent for AWS AgentCore**

Create a deployable runtime file:

cp 00_langgraph_agent.py langgraph_agent_runtime.py

👉 Modify this file to:

remove local-only prints
avoid top-level API calls
make it AgentCore-compatible
**☁️ 6. Install AWS CLI**

Install AWS CLI:
https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

**🔍 7. Verify AWS Setup**
aws --version
aws configure
aws sts get-caller-identity
🚀 8. Deploy to AWS AgentCore
Step 1: Generate configuration
agentcore configure -e ./langgraph_agent_runtime.py

**👉 This generates:**

YAML config
Dockerfile
Step 2: Launch Agent
agentcore launch --env GROQ_API_KEY='your_api_key'
Step 3: Invoke Agent
agentcore invoke '{"prompt": "Hello"}'
📊 Observability

Logs available in CloudWatch:

/aws/bedrock-agentcore/runtimes/<runtime-name>
