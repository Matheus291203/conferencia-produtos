# conferencia-produtos
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <title>Conferência de Produtos por Código de Barras</title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <h1>Conferência de Produtos</h1>

  <label>Produtos Esperados (arquivo .txt):</label><br />
  <input type="file" id="esperadosInput" accept=".txt" /><br /><br />

  <label>Produtos Entregues (arquivo .txt):</label><br />
  <input type="file" id="entreguesInput" accept=".txt" /><br /><br />

  <button onclick="conferir()">Conferir Produtos</button>

  <h2>Resultado:</h2>
  <ul id="resultadoList"></ul>

  <script src="script.js"></script>
</body>
</html>
function conferir() {
  const esperadosFile = document.getElementById('esperadosInput').files[0];
  const entreguesFile = document.getElementById('entreguesInput').files[0];

  if (!esperadosFile || !entreguesFile) {
    alert("Envie os dois arquivos .txt para comparação.");
    return;
  }

  Promise.all([
    lerArquivo(esperadosFile),
    lerArquivo(entreguesFile)
  ]).then(([esperados, entregues]) => {
    compararListas(esperados, entregues);
  });
}

function lerArquivo(file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = e => {
      const linhas = e.target.result
        .split('\\n')
        .map(l => l.trim())
        .filter(Boolean);
      resolve(linhas);
    };
    reader.onerror = reject;
    reader.readAsText(file);
  });
}

function compararListas(esperados, entregues) {
  const resultadoEl = document.getElementById('resultadoList');
  resultadoEl.innerHTML = '';

  const entreguesSet = new Set(entregues);
  const esperadosSet = new Set(esperados);

  esperados.forEach(cod => {
    if (entreguesSet.has(cod)) {
      resultadoEl.innerHTML += <li style="color:green">✔ OK: ${cod}</li>;
    } else {
      resultadoEl.innerHTML += <li style="color:red">❌ Faltando: ${cod}</li>;
    }
  });

  entregues.forEach(cod => {
    if (!esperadosSet.has(cod)) {
      resultadoEl.innerHTML += <li style="color:orange">⚠ Extra: ${cod}</li>;
    }
  });
}
body {
  font-family: Arial, sans-serif;
  margin: 20px;
}

input {
  margin-bottom: 10px;
}

ul {
  list-style: none;
  padding: 0;
}
