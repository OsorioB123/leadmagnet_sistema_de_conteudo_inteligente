# Configuração PostHog Analytics

## Passo 1: Criar Conta no PostHog

1. Acesse [PostHog](https://posthog.com/)
2. Crie uma conta gratuita
3. Configure seu projeto
4. Copie o **Project Key** e **Host URL**

## Passo 2: Configurar as Chaves

Nos arquivos `index.html` e `obrigado.html`, substitua:

```javascript
posthog.init('YOUR_POSTHOG_PROJECT_KEY', {api_host: 'YOUR_POSTHOG_HOST'})
```

Por:
```javascript
posthog.init('phc_SEU_PROJECT_KEY_AQUI', {api_host: 'https://app.posthog.com'})
```

**Exemplo:**
```javascript
posthog.init('phc_abc123def456ghi789', {api_host: 'https://app.posthog.com'})
```

## Eventos que Estão Sendo Rastreados

### Página Principal (index.html)
- `landing_page_view`: Quando a página é carregada
- `form_started`: Quando o usuário começa a preencher o formulário
- `form_submitted`: Quando o formulário é enviado
- `lead_converted`: Quando o lead é convertido com sucesso
- `form_error`: Se houver erro no envio do formulário

### Página de Agradecimento (obrigado.html)
- `thank_you_page_view`: Quando a página de agradecimento é visualizada

## Propriedades dos Eventos

### landing_page_view
```javascript
{
    page: 'lead_magnet_main',
    url: window.location.href
}
```

### form_started
```javascript
{
    form_type: 'lead_capture',
    first_field: 'name' // qual campo foi focado primeiro
}
```

### form_submitted
```javascript
{
    form_type: 'lead_capture',
    email: 'usuario@email.com',
    timestamp: '2025-01-01T12:00:00.000Z'
}
```

### lead_converted
```javascript
{
    form_type: 'lead_capture',
    email: 'usuario@email.com',
    name: 'Nome Completo'
}
```

### form_error
```javascript
{
    form_type: 'lead_capture',
    error: 'Descrição do erro'
}
```

### thank_you_page_view
```javascript
{
    page: 'thank_you',
    conversion_complete: true,
    url: window.location.href
}
```

## Dashboards Recomendados

### 1. Funil de Conversão
- `landing_page_view` → `form_started` → `form_submitted` → `lead_converted` → `thank_you_page_view`

### 2. Métricas Principais
- **Taxa de Conversão**: `lead_converted` / `landing_page_view`
- **Taxa de Abandono**: (`form_started` - `form_submitted`) / `form_started`
- **Taxa de Erro**: `form_error` / `form_submitted`

### 3. Insights de Usuário
- Identificação automática do usuário após conversão
- Propriedades do usuário: nome, email, telefone
- Tracking de sessão e comportamento

## Configurações Avançadas (Opcional)

### Feature Flags
Configure feature flags para A/B testing:
```javascript
if (posthog.isFeatureEnabled('new-headline')) {
    // Mostrar novo headline
}
```

### Grupos/Cohorts
Crie grupos baseados em comportamento:
- Usuários que converteram
- Usuários que abandonaram o formulário
- Usuários por fonte de tráfego

### Integrações
- Webhook para Slack/Discord quando houver nova conversão
- Integração com ferramentas de CRM
- Export de dados para análise avançada

## Privacidade e LGPD

O PostHog está configurado para capturar apenas dados necessários:
- Não captura informações pessoais automaticamente
- Identificação só ocorre após consentimento (envio do formulário)
- Dados são anonimizados por padrão

Para compliance total com LGPD, considere:
- Adicionar banner de cookies
- Implementar opt-out
- Documentar dados coletados na política de privacidade