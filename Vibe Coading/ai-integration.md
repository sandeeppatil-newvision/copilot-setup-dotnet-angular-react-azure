# 🤖 AI Integration Plan — Azure OpenAI

## Azure Model
- GPT-4o-mini  
or  
- gpt-4.1  

## Required Environment Variables
- AZURE_OPENAI_ENDPOINT  
- AZURE_OPENAI_KEY  
- AZURE_OPENAI_DEPLOYMENT  

---

## 1. Setup Steps
- Install Azure.AI.OpenAI  
- Inject OpenAIClient using DI  
- Configure in Program.cs  
- Build ChatCompletionsOptions  
- Add system prompt  
- Return AI response to frontend  

---

## 2. System Prompt (Example)
“You are a helpful AI assistant. Respond clearly and concisely.”

---

## 3. Copilot Instructions

Use inside GitHub Copilot Chat:

> Implement complete Azure OpenAI integration based on ai-integration.md.  
> Create AiService, register OpenAIClient, use env variables, and implement ChatCompletion.  
