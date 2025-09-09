# Configuração Google Sheets + Apps Script

## Passo 1: Criar a Planilha Google Sheets

1. Acesse [Google Sheets](https://sheets.google.com)
2. Crie uma nova planilha
3. Renomeie para "Lead Magnet - Sistema de Conteúdo Inteligente"
4. Na primeira linha, adicione os seguintes cabeçalhos:
   - A1: `Timestamp`
   - B1: `Nome`
   - C1: `Sobrenome`
   - D1: `Email`
   - E1: `Telefone`

## Passo 2: Criar o Google Apps Script

1. Na planilha, vá em **Extensões** → **Apps Script**
2. Substitua o código padrão pelo seguinte:

```javascript
function doPost(e) {
  try {
    // Get the active spreadsheet
    var sheet = SpreadsheetApp.getActiveSheet();
    
    // Get form data
    var name = e.parameter.name;
    var lastname = e.parameter.lastname;
    var email = e.parameter.email;
    var phone = e.parameter.phone;
    var timestamp = new Date();
    
    // Add data to sheet
    sheet.appendRow([timestamp, name, lastname, email, phone]);
    
    // Return success response
    return ContentService
      .createTextOutput(JSON.stringify({status: 'success'}))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch (error) {
    // Return error response
    return ContentService
      .createTextOutput(JSON.stringify({status: 'error', message: error.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet(e) {
  return ContentService
    .createTextOutput("Apps Script is working!")
    .setMimeType(ContentService.MimeType.TEXT);
}
```

## Passo 3: Configurar o Deploy

1. Clique em **Implantar** → **Nova implantação**
2. Clique no ícone de engrenagem e selecione **Aplicativo da Web**
3. Configure:
   - **Descrição**: "Lead Magnet Form Handler"
   - **Executar como**: "Eu"
   - **Quem tem acesso**: "Qualquer pessoa"
4. Clique em **Implantar**
5. **IMPORTANTE**: Copie a URL da implantação que aparece

## Passo 4: Atualizar o index.html

1. No arquivo `index.html`, encontre a linha:
   ```html
   <form id="leadForm" class="space-y-5" action="https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec" method="POST">
   ```

2. Substitua `YOUR_SCRIPT_ID` pela URL completa que você copiou no passo anterior.

## Passo 5: Teste

1. Faça o deploy da landing page
2. Preencha o formulário de teste
3. Verifique se os dados apareceram na planilha Google Sheets

## Configurações Adicionais (Opcional)

### Formatação da Planilha
- Formate a coluna A (Timestamp) como data/hora
- Aplique filtros nos cabeçalhos
- Configure formatação condicional se necessário

### Notificações por Email
Adicione este código no Apps Script para receber notificações:

```javascript
function sendEmailNotification(name, email) {
  var subject = 'Novo Lead Capturado - ' + name;
  var body = `
    Novo lead capturado:
    
    Nome: ${name}
    Email: ${email}
    
    Acesse a planilha: ${SpreadsheetApp.getActiveSpreadsheet().getUrl()}
  `;
  
  // Substitua por seu email
  MailApp.sendEmail('seu-email@exemplo.com', subject, body);
}
```

E chame esta função no final da função `doPost`:
```javascript
// Adicione após sheet.appendRow([timestamp, name, lastname, email, phone]);
sendEmailNotification(name + ' ' + lastname, email);
```

## Solução de Problemas

- **Erro de permissão**: Certifique-se de que o Apps Script está configurado para "Qualquer pessoa" ter acesso
- **Dados não aparecem**: Verifique se a URL do script está correta no formulário
- **CORS Error**: Isso é esperado com `mode: 'no-cors'` - o formulário ainda funcionará