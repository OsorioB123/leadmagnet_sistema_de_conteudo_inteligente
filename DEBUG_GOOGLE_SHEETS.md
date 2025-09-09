# 🔧 Debug Google Sheets - Não está coletando dados

## Problema Comum: Apps Script não configurado corretamente

### 1. Verifique o Apps Script

Acesse seu Google Apps Script e substitua TODO o código por este:

```javascript
function doPost(e) {
  try {
    // Log para debug
    console.log('Dados recebidos:', e);
    
    // Get the active spreadsheet
    var sheet = SpreadsheetApp.getActiveSheet();
    
    // Verificar se os parâmetros existem
    if (!e || !e.parameter) {
      console.log('Erro: Nenhum parâmetro recebido');
      return ContentService
        .createTextOutput(JSON.stringify({status: 'error', message: 'Nenhum dado recebido'}))
        .setMimeType(ContentService.MimeType.JSON);
    }
    
    // Get form data
    var name = e.parameter.name || '';
    var lastname = e.parameter.lastname || '';
    var email = e.parameter.email || '';
    var phone = e.parameter.phone || '';
    var timestamp = new Date();
    
    // Log dados antes de salvar
    console.log('Salvando:', [timestamp, name, lastname, email, phone]);
    
    // Verificar se pelo menos email existe
    if (!email) {
      console.log('Erro: Email obrigatório não fornecido');
      return ContentService
        .createTextOutput(JSON.stringify({status: 'error', message: 'Email é obrigatório'}))
        .setMimeType(ContentService.MimeType.JSON);
    }
    
    // Add data to sheet
    sheet.appendRow([timestamp, name, lastname, email, phone]);
    console.log('Dados salvos com sucesso');
    
    // Return success response
    return ContentService
      .createTextOutput(JSON.stringify({status: 'success', message: 'Dados salvos com sucesso'}))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch (error) {
    console.log('Erro no script:', error.toString());
    // Return error response
    return ContentService
      .createTextOutput(JSON.stringify({status: 'error', message: error.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet(e) {
  return ContentService
    .createTextOutput("Apps Script está funcionando! Timestamp: " + new Date())
    .setMimeType(ContentService.MimeType.TEXT);
}

function testScript() {
  // Função para testar manualmente
  var testData = {
    parameter: {
      name: 'Teste',
      lastname: 'Usuario',
      email: 'teste@exemplo.com',
      phone: '11999999999'
    }
  };
  
  var result = doPost(testData);
  console.log('Resultado do teste:', result.getContent());
}
```

### 2. Configurar Permissões no Apps Script

1. Clique em **"Implantar"** → **"Gerenciar implantações"**
2. Clique no ícone de **lápis (editar)** na implantação existente
3. Certifique-se de que:
   - **Executar como**: "Eu"
   - **Quem tem acesso**: "Qualquer pessoa"
4. Clique em **"Atualizar"**
5. **IMPORTANTE**: Copie a nova URL se ela mudou

### 3. Testar o Apps Script Manualmente

1. No Apps Script, vá na função `testScript`
2. Clique em **"Executar"**
3. Autorize as permissões se solicitado
4. Verifique se apareceu uma linha de teste na planilha

### 4. Testar a URL do Apps Script

Abra esta URL no navegador (substitua pela sua URL):
```
https://script.google.com/macros/s/AKfycbz6QEVvfp1sfUEsjQnwm__zNVafS_zf_WuC36t9J_bKom8etC4fEghNt9bvYEoE1a2K/exec
```

Deve aparecer: "Apps Script está funcionando! Timestamp: [data atual]"

### 5. Configurar Headers Corretos no Formulário

Se ainda não funcionar, o problema pode ser CORS. Adicione este código no final do Apps Script:

```javascript
function doPost(e) {
  // ... código anterior ...
  
  return ContentService
    .createTextOutput(JSON.stringify({status: 'success'}))
    .setMimeType(ContentService.MimeType.JSON)
    .setHeaders({
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type'
    });
}

function doOptions(e) {
  return ContentService
    .createTextOutput('')
    .setMimeType(ContentService.MimeType.TEXT)
    .setHeaders({
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type'
    });
}
```

### 6. Verificar Logs no Apps Script

1. No Apps Script, vá em **"Execuções"**
2. Veja se há execuções quando você envia o formulário
3. Clique nas execuções para ver logs de erro

### 7. Planilha deve ter estes cabeçalhos na linha 1:

```
A1: Timestamp
B1: Nome  
C1: Sobrenome
D1: Email
E1: Telefone
```

### 8. Se nada funcionar, tente esta URL alternativa:

Às vezes o problema é usar a URL do editor em vez da URL de execução. A URL correta deve terminar com `/exec`, não `/edit`.

## Como Testar Agora

1. Faça as correções acima
2. Teste o formulário na sua landing page
3. Verifique se os dados aparecem na planilha
4. Se ainda não funcionar, me envie screenshot dos logs do Apps Script