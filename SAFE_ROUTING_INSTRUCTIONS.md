# 🛡️ Configuração Segura para trendlycorp.com

## ⚠️ IMPORTANTE: Protegendo pages existentes

A página `https://www.trendlycorp.com/projects-forms` deve permanecer intacta.

## 📋 Configuração para o projeto principal

### 1. No projeto principal `trendlycorp.com` no Vercel:

**Adicione APENAS estas linhas ao `vercel.json` existente:**

```json
{
  "rewrites": [
    {
      "source": "/material-sistema-de-conteudo-inteligente",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/"
    },
    {
      "source": "/material-sistema-de-conteudo-inteligente/obrigado",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/obrigado.html"
    },
    {
      "source": "/material-sistema-de-conteudo-inteligente/(.*)",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/$1"
    }
  ]
}
```

### 2. Se já existe um vercel.json, MESCLE assim:

**Se o vercel.json atual for:**
```json
{
  "functions": {...},
  "headers": [...],
  "redirects": [...]
}
```

**Adicione a seção "rewrites":**
```json
{
  "functions": {...},
  "headers": [...],
  "redirects": [...],
  "rewrites": [
    {
      "source": "/material-sistema-de-conteudo-inteligente",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/"
    },
    {
      "source": "/material-sistema-de-conteudo-inteligente/obrigado",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/obrigado.html"
    },
    {
      "source": "/material-sistema-de-conteudo-inteligente/(.*)",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/$1"
    }
  ]
}
```

## ✅ Por que é seguro:

1. **Rota específica**: Apenas `/material-sistema-de-conteudo-inteligente` é afetada
2. **Não interfere**: `/projects-forms` permanece intacto
3. **Sem wildcards globais**: Não usa `/*` que poderia capturar outras rotas
4. **Ordem importa**: Vercel processa na ordem, rotas específicas primeiro

## 🧪 Como testar após deploy:

1. ✅ `trendlycorp.com/projects-forms` → Deve funcionar normalmente
2. ✅ `trendlycorp.com/material-sistema-de-conteudo-inteligente` → Nova landing page
3. ✅ `trendlycorp.com/material-sistema-de-conteudo-inteligente/obrigado` → Página de agradecimento
4. ✅ Todas as outras páginas → Funcionam normalmente

## 🚨 Troubleshooting:

**Se algo der errado:**
1. Remova apenas a seção `"rewrites"` do vercel.json
2. Faça deploy
3. Tudo voltará ao normal

## 📍 URLs finais:

- **Landing Page**: `https://trendlycorp.com/material-sistema-de-conteudo-inteligente`
- **Página de Agradecimento**: `https://trendlycorp.com/material-sistema-de-conteudo-inteligente/obrigado`
- **Página Protegida**: `https://trendlycorp.com/projects-forms` (intacta)

## 💡 Dica:

Teste primeiro em um ambiente de preview do Vercel antes de fazer deploy para produção!