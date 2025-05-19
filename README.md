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
