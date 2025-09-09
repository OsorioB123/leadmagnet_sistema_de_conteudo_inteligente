# 🚀 Configuração de Subpath no Vercel

## Opção Recomendada: Configurar no projeto principal do Vercel

### 1. No projeto principal `trendlycorp.com` no Vercel:

Adicione estas regras no `vercel.json` do projeto principal:

```json
{
  "rewrites": [
    {
      "source": "/material-sistema-de-conteudo-inteligente",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/"
    },
    {
      "source": "/material-sistema-de-conteudo-inteligente/(.*)",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/$1"
    }
  ]
}
```

### 2. Alternativa: Usar Vercel Functions

Crie um arquivo `api/proxy.js` no projeto principal:

```javascript
export default async function handler(req, res) {
  const { url, method, headers } = req;
  
  if (url.startsWith('/material-sistema-de-conteudo-inteligente')) {
    const targetUrl = url.replace('/material-sistema-de-conteudo-inteligente', '');
    const proxyUrl = `https://leadmagnet-sistema-conteudo-inteligente.vercel.app${targetUrl || '/'}`;
    
    try {
      const response = await fetch(proxyUrl, {
        method,
        headers: {
          ...headers,
          host: undefined,
        },
        body: method !== 'GET' ? req.body : undefined,
      });
      
      const data = await response.text();
      
      // Rewrite internal links
      const modifiedData = data
        .replace(/href="\/obrigado"/g, 'href="/material-sistema-de-conteudo-inteligente/obrigado"')
        .replace(/href="\/"/g, 'href="/material-sistema-de-conteudo-inteligente"');
      
      res.status(response.status).send(modifiedData);
    } catch (error) {
      res.status(500).json({ error: 'Proxy error' });
    }
  }
}
```

### 3. Como configurar (Método Simples):

#### Passo 1: No dashboard da Vercel
1. Vá para o projeto principal `trendlycorp.com`
2. Vá em **Settings** → **Functions**
3. Em **Rewrites**, adicione:

```
Source: /material-sistema-de-conteudo-inteligente
Destination: https://leadmagnet-sistema-conteudo-inteligente.vercel.app/

Source: /material-sistema-de-conteudo-inteligente/*
Destination: https://leadmagnet-sistema-conteudo-inteligente.vercel.app/:splat
```

#### Passo 2: Deploy
- Faça deploy das alterações
- Teste em `trendlycorp.com/material-sistema-de-conteudo-inteligente`

## URLs Finais:

✅ **Landing Page**: `trendlycorp.com/material-sistema-de-conteudo-inteligente`
✅ **Página de Agradecimento**: `trendlycorp.com/material-sistema-de-conteudo-inteligente/obrigado`

## Vantagens desta abordagem:

- ✅ Não afeta o site principal Squarespace
- ✅ Mantém o domínio `trendlycorp.com`
- ✅ Funciona com DNS atual (sem mudanças)
- ✅ SSL automático
- ✅ Deploy independente da landing page

## Se preferir usar subdomínio:

Alternativamente, pode usar `material.trendlycorp.com` adicionando:

**DNS (adicionar ao Google Domains):**
```
Host: material
Tipo: CNAME  
Dados: cname.vercel-dns.com
```

**No Vercel:**
- Adicionar domínio personalizado: `material.trendlycorp.com`