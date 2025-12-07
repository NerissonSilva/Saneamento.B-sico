# Projeto Full Stack com Login Social

Backend (Node.js + Express) e Frontend (HTML/CSS/JS) com autenticação Google OAuth.

## 🚀 Deploy no Render

### 1. Configurar Google OAuth

1. Acesse [Google Cloud Console](https://console.cloud.google.com/)
2. Crie um novo projeto
3. Ative a Google+ API
4. Crie credenciais OAuth 2.0
5. Adicione as URLs autorizadas:
   - `https://seu-backend.onrender.com/auth/google/callback`

### 2. Deploy

1. Conecte seu repositório ao Render
2. O Render detectará automaticamente o `render.yaml`
3. Configure as variáveis de ambiente no dashboard:
   - `GOOGLE_CLIENT_ID`
   - `GOOGLE_CLIENT_SECRET`
   - `GOOGLE_CALLBACK_URL`
   - `FRONTEND_URL`

### 3. Atualizar URLs

Após o deploy, atualize:

**backend/.env:**
```
FRONTEND_URL=https://seu-frontend.onrender.com
GOOGLE_CALLBACK_URL=https://seu-backend.onrender.com/auth/google/callback
```

**frontend/app.js:**
```javascript
const API_URL = 'https://seu-backend.onrender.com';
```

## 🛠️ Desenvolvimento Local

### Backend
```bash
cd backend
npm install
cp .env.example .env
# Configure as variáveis no .env
npm run dev
```

### Frontend
```bash
cd frontend
# Abra index.html no navegador ou use um servidor local
python -m http.server 8080
```

## 📦 Estrutura

```
├── backend/          # API Node.js
│   ├── server.js     # Servidor Express
│   ├── package.json
│   └── .env.example
├── frontend/         # Site estático
│   ├── index.html
│   ├── styles.css
│   └── app.js
└── render.yaml       # Configuração Render
```

## ✅ Compatibilidade

- ✅ Linux (Ubuntu, Debian, etc)
- ✅ Render.com
- ✅ Node.js 18+
- ✅ Deploy sem falhas
