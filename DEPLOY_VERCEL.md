# Deploy no Vercel - Passo a Passo

## Passo 1: Acessar Vercel

1. Acesse [vercel.com](https://vercel.com)
2. Faça login com GitHub
3. Clique em **"New Project"**

## Passo 2: Importar Repositório

1. Na lista de repositórios, encontre: `leadmagnet_sistema_de_conteudo_inteligente`
2. Clique em **"Import"**
3. Configure o projeto:
   - **Project Name**: `leadmagnet-sistema-conteudo-inteligente`
   - **Framework Preset**: Other
   - **Root Directory**: `./` (deixe como está)
   - **Build Command**: Deixe vazio
   - **Output Directory**: Deixe vazio
   - **Install Command**: Deixe vazio

## Passo 3: Deploy

1. Clique em **"Deploy"**
2. Aguarde o deploy ser concluído
3. Vercel gerará uma URL automática (ex: `leadmagnet-sistema-conteudo-inteligente.vercel.app`)

## Passo 4: Configurar Domínio Personalizado (Opcional)

1. No dashboard do projeto, vá em **"Settings"** → **"Domains"**
2. Adicione seu domínio personalizado
3. Configure DNS conforme instruções da Vercel

## Passo 5: Configurar Redirects Automáticos

O arquivo `vercel.json` já está configurado para:
- ✅ URLs limpas (sem .html)
- ✅ Redirect do arquivo antigo para /
- ✅ Headers de segurança
- ✅ SSL automático

## Próximos Passos OBRIGATÓRIOS

Após o deploy, você DEVE:

### 1. Configurar Google Sheets
- Siga `GOOGLE_SHEETS_SETUP.md`
- Substitua `YOUR_SCRIPT_ID` no `index.html`
- Faça novo commit e push

### 2. Configurar PostHog
- Siga `POSTHOG_SETUP.md`  
- Substitua `YOUR_POSTHOG_PROJECT_KEY` nos arquivos
- Faça novo commit e push

## Configuração Rápida

### Google Sheets
No `index.html`, linha 114, substitua:
```html
action="https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec"
```
Por:
```html
action="https://script.google.com/macros/s/SEU_SCRIPT_ID_REAL/exec"
```

### PostHog
Nos arquivos `index.html` e `obrigado.html`, substitua:
```javascript
posthog.init('YOUR_POSTHOG_PROJECT_KEY', {api_host: 'YOUR_POSTHOG_HOST'})
```
Por:
```javascript
posthog.init('phc_SEU_PROJECT_KEY', {api_host: 'https://app.posthog.com'})
```

## URLs Finais

Após deploy, suas URLs serão:
- **Landing Page**: `https://seu-dominio.vercel.app/`
- **Página de Agradecimento**: `https://seu-dominio.vercel.app/obrigado`

## Deploy Automático

- ✅ Cada push para `master` faz deploy automático
- ✅ Preview branches para outras branches
- ✅ Analytics integrado da Vercel

## Suporte

- **Vercel**: [vercel.com/docs](https://vercel.com/docs)
- **GitHub**: [github.com/OsorioB123/leadmagnet_sistema_de_conteudo_inteligente](https://github.com/OsorioB123/leadmagnet_sistema_de_conteudo_inteligente)