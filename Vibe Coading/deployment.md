# 🚀 Deployment Plan — Backend + Frontend

## 1. Backend Deployment (Azure App Service)

### Steps
- Create Azure App Service  
- Publish .NET API  
- Configure environment variables:  
  - AZURE_OPENAI_KEY  
  - AZURE_OPENAI_ENDPOINT  
  - MODEL_NAME  

### GitHub Actions (Optional)
- Build & publish .NET app  
- Deploy to Azure App Service  

---

## 2. Frontend Deployment (Azure Static Web Apps)

### Steps
- Build React app (`npm run build`)  
- Deploy `/dist` folder  
- Configure API endpoint for production  

---

## 3. Final Checklist
✔ CORS updated  
✔ Environment variables set  
✔ Correct API URLs  
✔ Build verified  
✔ Deployment tested  

---

## 4. Copilot Deployment Prompt

Use inside Copilot Chat:

> Create GitHub Actions pipelines for backend and frontend based on deployment.md.  
> Include Azure App Service deployment for API and Static Web Apps deployment for React app.  
