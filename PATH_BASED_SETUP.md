# 🛠️ Configuração Path-Based para trendlycorp.com

## Estrutura Desejada:
- `trendlycorp.com/projects-forms` → Projeto principal (existente)
- `trendlycorp.com/material-sistema-de-conteudo-inteligente` → Novo projeto
- `trendlycorp.com/[futuros-caminhos]` → Próximos projetos

## 📋 Configuração no Projeto Principal (trendlycorp.com)

### No `vercel.json` do projeto `projects-forms`, adicione:

```json
{
  "rewrites": [
    {
      "source": "/material-sistema-de-conteudo-inteligente",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/"
    },
    {
      "source": "/material-sistema-de-conteudo-inteligente/obrigado",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/obrigado"
    },
    {
      "source": "/material-sistema-de-conteudo-inteligente/(.*)",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/$1"
    }
  ]
}
```

### Se já existe um `vercel.json`, MESCLE assim:

```json
{
  "functions": {...existente...},
  "headers": [...existente...],
  "redirects": [...existente...],
  "rewrites": [
    ...rewrites existentes...,
    {
      "source": "/material-sistema-de-conteudo-inteligente",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/"
    },
    {
      "source": "/material-sistema-de-conteudo-inteligente/obrigado", 
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/obrigado"
    },
    {
      "source": "/material-sistema-de-conteudo-inteligente/(.*)",
      "destination": "https://leadmagnet-sistema-conteudo-inteligente.vercel.app/$1"
    }
  ]
}
```

## 🌐 URLs Finais:

✅ **Landing Page**: `https://trendlycorp.com/material-sistema-de-conteudo-inteligente`
✅ **Página de Agradecimento**: `https://trendlycorp.com/material-sistema-de-conteudo-inteligente/obrigado`
✅ **Página Existente**: `https://trendlycorp.com/projects-forms` (intacta)

## 🔧 Como Funciona:

1. **Usuário acessa**: `trendlycorp.com/material-sistema-de-conteudo-inteligente`
2. **Vercel faz proxy**: Para `https://leadmagnet-sistema-conteudo-inteligente.vercel.app/`
3. **Usuário vê**: Conteúdo do projeto separado, mas na URL principal
4. **SEO e branding**: Tudo sob `trendlycorp.com`

## 📋 Passos para Implementar:

### 1. No projeto principal `trendlycorp.com/projects-forms`:
   - Editar `vercel.json` 
   - Adicionar as regras de rewrite acima
   - Deploy

### 2. No projeto `material-sistema-conteudo`:
   - **Nada muda!** 
   - Continua funcionando normalmente
   - URLs internas ajustadas (já feito)

### 3. Teste:
   - Aguardar deploy do projeto principal
   - Testar `trendlycorp.com/material-sistema-de-conteudo-inteligente`

## 🚨 Importante:

- **Ordem das regras importa**: Mais específicas primeiro
- **Não afeta outras páginas**: Só captura `/material-sistema-de-conteudo-inteligente*`
- **Escalável**: Fácil adicionar novos projetos seguindo o padrão

## 🔮 Para Futuros Projetos:

```json
{
  "source": "/novo-projeto",
  "destination": "https://novo-projeto.vercel.app/"
},
{
  "source": "/novo-projeto/(.*)",
  "destination": "https://novo-projeto.vercel.app/$1"
}
```

## 🧪 Como Testar:

1. ✅ `trendlycorp.com/projects-forms` → Deve funcionar normalmente
2. ✅ `trendlycorp.com/material-sistema-de-conteudo-inteligente` → Nova landing page  
3. ✅ `trendlycorp.com/material-sistema-de-conteudo-inteligente/obrigado` → Página de agradecimento
4. ✅ Formulário deve submeter e redirecionar corretamente

---

**Quer que eu ajude você a aplicar essa configuração no projeto principal?**