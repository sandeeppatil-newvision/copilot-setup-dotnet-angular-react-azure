# 🧠 Project Plan — Generative AI App (Idea → Plan → Build → Deploy)

## 1. Overall Architecture
- Frontend: React.js (Vite)  
- Backend: .NET Core Web API  
- AI: Azure OpenAI GPT Models  
- Deployment: Azure App Service + Azure Static Web Apps  
- Communication: HTTPS JSON API

## 2. High-Level Flow
1. User types a message in React UI  
2. React sends request → .NET API  
3. API calls Azure OpenAI with prompt  
4. AI returns structured result  
5. React renders chat message

## 3. Backend Components
- Controllers  
- Services (Business logic)  
- Azure OpenAI Client  
- Models/DTO  
- DI setup  
- CORS policy  
- Logging  
- Error handling middleware  

## 4. Frontend Components
- Chat UI page  
- MessageArea + InputBox components  
- API service (axios/fetch)  
- Global theme  
- Loading states  
- Error handling UI  

## 5. Azure OpenAI Integration
- ChatCompletion API  
- System prompt  
- Environment variables: KEY, ENDPOINT, MODEL  
- Retry logic  
- Response formatting

## 6. Deployment Plan
- Backend → Azure App Service  
- Frontend → Static Web Apps  
- GitHub Actions CI/CD pipeline  
- Secrets: AzureOpenAIKey, Endpoint, ModelName  

## 7. Deliverables
- Full working code  
- MD prompt files  
- Deployment guide  
- Environment variable sample  

