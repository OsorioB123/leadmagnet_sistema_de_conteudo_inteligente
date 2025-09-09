# 🔧 Apps Script Atualizado - Suporta GET e POST

Cole este código COMPLETO no seu Google Apps Script:

```javascript
function doPost(e) {
  return processData(e, 'POST');
}

function doGet(e) {
  return processData(e, 'GET');
}

function processData(e, method) {
  try {
    console.log('Método:', method);
    console.log('Dados recebidos:', e);
    
    // Get the active spreadsheet
    var sheet = SpreadsheetApp.getActiveSheet();
    
    // Get parameters from either POST body or GET query
    var params = e.parameter || {};
    
    console.log('Parâmetros processados:', params);
    
    // Get form data
    var name = params.name || '';
    var lastname = params.lastname || '';
    var email = params.email || '';
    var phone = params.phone || '';
    var timestamp = new Date();
    
    // Log dados antes de salvar
    console.log('Dados a salvar:', [timestamp, name, lastname, email, phone]);
    
    // Verificar se pelo menos email existe
    if (!email) {
      console.log('Erro: Email obrigatório não fornecido');
      return ContentService
        .createTextOutput(JSON.stringify({status: 'error', message: 'Email é obrigatório'}))
        .setMimeType(ContentService.MimeType.JSON)
        .setHeaders({
          'Access-Control-Allow-Origin': '*',
          'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
          'Access-Control-Allow-Headers': 'Content-Type'
        });
    }
    
    // Add data to sheet
    sheet.appendRow([timestamp, name, lastname, email, phone]);
    console.log('Dados salvos com sucesso na planilha!');
    
    // Return success response
    return ContentService
      .createTextOutput(JSON.stringify({
        status: 'success', 
        message: 'Dados salvos com sucesso',
        data: {name, lastname, email, phone}
      }))
      .setMimeType(ContentService.MimeType.JSON)
      .setHeaders({
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
        'Access-Control-Allow-Headers': 'Content-Type'
      });
      
  } catch (error) {
    console.log('Erro no script:', error.toString());
    // Return error response
    return ContentService
      .createTextOutput(JSON.stringify({
        status: 'error', 
        message: error.toString(),
        method: method
      }))
      .setMimeType(ContentService.MimeType.JSON)
      .setHeaders({
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
        'Access-Control-Allow-Headers': 'Content-Type'
      });
  }
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

function testScript() {
  // Teste para POST
  console.log('=== TESTANDO POST ===');
  var testDataPost = {
    parameter: {
      name: 'Teste POST',
      lastname: 'Usuario',
      email: 'teste.post@exemplo.com',
      phone: '11999999999'
    }
  };
  
  var resultPost = doPost(testDataPost);
  console.log('Resultado POST:', resultPost.getContent());
  
  // Teste para GET
  console.log('=== TESTANDO GET ===');
  var testDataGet = {
    parameter: {
      name: 'Teste GET',
      lastname: 'Usuario',
      email: 'teste.get@exemplo.com',
      phone: '11888888888'
    }
  };
  
  var resultGet = doGet(testDataGet);
  console.log('Resultado GET:', resultGet.getContent());
}
```

## Depois de colar o código:

1. **Salve o script** (Ctrl+S)
2. **Execute `testScript`** para testar
3. **Verifique se apareceram 2 linhas de teste** na planilha
4. **Se funcionou, teste o formulário** na landing page
5. **Verifique os logs** em "Execuções" para ver se chegaram dados do formulário

## Para testar manualmente a URL:

Abra esta URL no navegador (substitua pela sua URL):
```
https://script.google.com/macros/s/AKfycbz6QEVvfp1sfUEsjQnwm__zNVafS_zf_WuC36t9J_bKom8etC4fEghNt9bvYEoE1a2K/exec?name=Teste&lastname=Manual&email=teste@manual.com&phone=11999999999
```

Se aparecer `{"status":"success"...}`, o script está funcionando!

## Debug:

- Vá em **"Execuções"** no Apps Script
- Veja se aparecem execuções quando você testa o formulário
- Clique na execução para ver os logs detalhados