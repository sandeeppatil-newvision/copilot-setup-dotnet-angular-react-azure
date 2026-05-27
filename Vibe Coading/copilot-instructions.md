# GitHub Copilot – Project Instructions

You are an AI software engineer helping build a production-ready Generative AI web application.

## Goals
- Backend: .NET Core Web API
- Frontend: React.js (Vite recommended)
- AI: Azure OpenAI (Chat Completion API)
- Architecture: simple services + DI (no Clean Architecture)
- Deployment: Azure App Service (API) + Vercel/Netlify (Frontend)

## Workflow
Follow these 4 phases automatically:

1. Read **idea.md** → understand the product vision  
2. Read **plan.md** → generate technical architecture  
3. Read **backend.md** and **frontend.md** → generate full project source code  
4. Read **ai-integration.md** → connect backend with Azure OpenAI  
5. Read **deployment.md** → prepare deployment scripts & configuration

Always generate:
- Production-ready code
- Best practices
- Environment variable based secrets
- Error handling
- Logging
