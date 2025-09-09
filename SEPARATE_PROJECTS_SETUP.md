# 🔧 Configuração para Projetos Separados no Vercel

## Situação Atual:
- **Projeto 1**: `projects-forms` (já usando `trendlycorp.com`)
- **Projeto 2**: `material-sistema-conteudo` (precisa usar `trendlycorp.com/material-sistema-de-conteudo-inteligente`)

## 🚀 Opção 1: Subdomínio (Recomendado - Mais Simples)

### No projeto `material-sistema-conteudo`:
1. **Adicionar domínio personalizado**: `material.trendlycorp.com`
2. **Configurar DNS** (adicionar no Google Domains):

```
Host: material
Tipo: CNAME
Dados: cname.vercel-dns.com
TTL: 3600
```

### URLs Finais:
- ✅ `https://material.trendlycorp.com` → Landing page
- ✅ `https://material.trendlycorp.com/obrigado` → Página de agradecimento
- ✅ `https://trendlycorp.com/projects-forms` → Intacto

---

## 🛠️ Opção 2: Path-based Routing (Mais Complexo)

### Passo 1: No projeto `projects-forms` (principal)
Adicionar ao `vercel.json`:

```json
{
  "rewrites": [
    {
      "source": "/material-sistema-de-conteudo-inteligente",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/"
    },
    {
      "source": "/material-sistema-de-conteudo-inteligente/:path*",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/:path*"
    }
  ]
}
```

### Passo 2: No projeto `material-sistema-conteudo`
Remover o `basePath` do `vercel.json`:

```json
{
  "cleanUrls": true,
  "trailingSlash": false,
  "headers": [...],
  "redirects": [...],
  "rewrites": [
    {
      "source": "/obrigado",
      "destination": "/obrigado.html"
    }
  ]
}
```

---

## 🌟 Opção 3: Domínio Dedicado (Mais Profissional)

### Registrar novo domínio ou usar subdomínio:
- `sistemadeconteudo.trendlycorp.com`
- `leadmagnet.trendlycorp.com`  
- `material.trendlycorp.com`

### Configuração DNS:
```
Host: material (ou outro)
Tipo: CNAME
Dados: cname.vercel-dns.com
```

---

## ⚡ Recomendação Rápida

**Para implementar hoje mesmo:**

### 1. Vá no projeto `material-sistema-conteudo` no Vercel
### 2. Settings → Domains → Add Domain
### 3. Adicione: `material.trendlycorp.com`
### 4. Configure DNS no Google Domains:

```
Host: material
Tipo: CNAME  
Dados: cname.vercel-dns.com
TTL: 3600 (1 hora)
```

### 5. URLs finais:
- ✅ `https://material.trendlycorp.com`
- ✅ `https://material.trendlycorp.com/obrigado`

## 🧪 Teste:
1. DNS pode levar até 1 hora para propagar
2. Vercel confirma quando SSL está ativo
3. Teste ambas URLs

---

## 💡 Qual opção prefere?

1. **Subdomínio** (mais rápido) → `material.trendlycorp.com`
2. **Path-based** (mais complexo) → `trendlycorp.com/material-sistema-de-conteudo-inteligente`
3. **Domínio dedicado** → Registrar novo domínio