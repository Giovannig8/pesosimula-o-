<!DOCTYPE html>
<html>
<head>
  <title>Monitoramento de Peso</title>
  <script>
    function updateData() {
      fetch('/data')
        .then(response => response.json())
        .then(data => {
          document.getElementById('data').innerHTML = `
            <h2>Monitoramento de Peso</h2>
            <table border="1">
              <tr><th>Produto</th><th>Peso</th></tr>
              <tr><td>${data.product1}</td><td>${data.weight1} g</td></tr>
              <tr><td>${data.product2}</td><td>${data.weight2} g</td></tr>
            </table>
          `;
        });
    }

    function registerProducts() {
      const product1 = document.getElementById('product1').value;
      const product2 = document.getElementById('product2').value;
      fetch(`/register?product1=${product1}&product2=${product2}`)
        .then(response => response.text())
        .then(data => {
          alert(data);
          updateData();
        });
    }

    function simulateWeightUpdate() {
      const weight1 = Math.random() * 1000;
      const weight2 = Math.random() * 1000;
      fetch(`/update?weight1=${weight1.toFixed(2)}&weight2=${weight2.toFixed(2)}`)
        .then(response => response.text())
        .then(data => {
          console.log(data);
          updateData();
        });
    }

    setInterval(simulateWeightUpdate, 1000);
    setInterval(updateData, 1000);
  </script>
</head>
<body onload="updateData()">
  <h1>Monitoramento de Peso em Tempo Real</h1>
  <div id="data">Carregando...</div>
  
  <h2>Registrar Produtos</h2>
  <form onsubmit="event.preventDefault(); registerProducts();">
    <label for="product1">Nome do Produto 1:</label>
    <input type="text" id="product1" name="product1" required><br><br>
    <label for="product2">Nome do Produto 2:</label>
    <input type="text" id="product2" name="product2" required><br><br>
    <input type="submit" value="Registrar">
  </form>
</body>
</html>
