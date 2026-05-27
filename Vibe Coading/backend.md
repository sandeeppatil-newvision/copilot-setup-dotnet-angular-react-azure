# 🔧 Backend Development Plan — .NET Core Web API

## Goal
Generate a clean, production-ready Web API with Azure OpenAI integration and dependency injection.

## Tech Stack
- .NET 8 Web API  
- Azure.AI.OpenAI SDK  
- Swashbuckle (Swagger)  
- Dependency Injection  
- Logging + Middleware  

---

## 1. Backend Requirements
- `POST /api/ai/chat` — send user messages  
- Services folder for business logic  
- Azure OpenAI client wrapper  
- Env-based configuration  
- Proper DI for all services  
- CorsPolicy for React  
- Swagger enabled  

---

## 2. File + Folder Structure
/GenerativeAI.Api
/Controllers
/Services
/Models
/Configurations
appsettings.json
Program.cs
## 3. Required Components

### Controllers
- AiController.cs  
  - Endpoint for chat  
  - Uses AiService  

### Services
- IAiService  
- AiService (calls Azure OpenAI)

### Config
- AzureOpenAIOptions  
- Bind from appsettings  

### Models
- ChatRequest  
- ChatResponse  

---

## 4. AI Flow in Backend
1. Receive message from frontend  
2. Build ChatCompletion request  
3. Send to Azure OpenAI  
4. Parse the result  
5. Return structured response  

---

## 5. Copilot Instructions to Build Backend  
Use this inside Copilot Chat:

> Generate all backend files (.NET Core Web API) based on backend.md.  
> Include: controller, service, models, Azure OpenAI integration, DI, logging, Program.cs updates.  
> The API should expose POST /api/ai/chat.  