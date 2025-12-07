# 🚀 Guia de Deploy no Render

## Passo 1: Configurar Google OAuth

1. Acesse [Google Cloud Console](https://console.cloud.google.com/)
2. Crie um novo projeto ou selecione um existente
3. Vá em **APIs & Services** > **Credentials**
4. Clique em **Create Credentials** > **OAuth 2.0 Client ID**
5. Configure:
   - Application type: **Web application**
   - Authorized redirect URIs: `https://SEU-BACKEND.onrender.com/auth/google/callback`
6. Copie o **Client ID** e **Client Secret**

## Passo 2: Deploy no Render

### Opção A: Deploy Automático (Recomendado)

1. Faça push do código para GitHub
2. Acesse [Render Dashboard](https://dashboard.render.com/)
3. Clique em **New** > **Blueprint**
4. Conecte seu repositório
5. O Render detectará o `render.yaml` automaticamente

### Opção B: Deploy Manual

1. Acesse [Render Dashboard](https://dashboard.render.com/)
2. Crie o banco de dados:
   - **New** > **PostgreSQL**
   - Name: `sessions-db`
   - Plan: **Free**
3. Crie o backend:
   - **New** > **Web Service**
   - Connect repository
   - Build Command: `cd backend && npm install`
   - Start Command: `cd backend && npm start`
4. Crie o frontend:
   - **New** > **Static Site**
   - Publish directory: `./frontend`

## Passo 3: Configurar Variáveis de Ambiente

No dashboard do **backend** no Render, adicione:

```
NODE_ENV=production
SESSION_SECRET=(clique em Generate para criar automaticamente)
GOOGLE_CLIENT_ID=seu-client-id-aqui
GOOGLE_CLIENT_SECRET=seu-client-secret-aqui
GOOGLE_CALLBACK_URL=https://SEU-BACKEND.onrender.com/auth/google/callback
FRONTEND_URL=https://SEU-FRONTEND.onrender.com
DATABASE_URL=(será preenchido automaticamente se usar Blueprint)
```

## Passo 4: Atualizar URLs no Código

Após o deploy, atualize o arquivo `frontend/app.js`:

```javascript
const API_URL = 'https://SEU-BACKEND.onrender.com';
```

Faça commit e push. O Render fará redeploy automaticamente.

## Passo 5: Atualizar Google OAuth

Volte ao Google Cloud Console e adicione a URL real:
- Authorized redirect URIs: `https://SEU-BACKEND.onrender.com/auth/google/callback`

## ✅ Verificação

Teste os endpoints:
- Backend: `https://SEU-BACKEND.onrender.com/health`
- Frontend: `https://SEU-FRONTEND.onrender.com`

## 🐛 Troubleshooting

### Erro: "OAuth2Strategy requires a clientID option"
**Solução:** Configure as variáveis `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET` e `GOOGLE_CALLBACK_URL` no dashboard do Render.

### Erro: "MemoryStore warning"
**Solução:** Certifique-se de que o banco PostgreSQL está conectado e a variável `DATABASE_URL` está configurada.

### Erro: "CORS"
**Solução:** Verifique se `FRONTEND_URL` está configurada corretamente no backend.

## 📝 Notas Importantes

- O plano Free do Render hiberna após 15 minutos de inatividade
- O primeiro acesso após hibernação pode levar 30-60 segundos
- Para produção real, considere upgrade para plano pago
- Mantenha suas credenciais seguras e nunca faça commit do arquivo `.env`
