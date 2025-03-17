# Projeto-de-ecommerce
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Loja de Bikes e Patinetes Elétricos - Single Page</title>
  <!-- Bootstrap CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body {
      background-color: #2c3e50;
      color: white;
      font-family: Arial, sans-serif;
      padding: 20px;
    }
    .container {
      max-width: 1200px;
      margin: auto;
      background: #ecf0f1;
      padding: 20px;
      border-radius: 10px;
      color: #333;
    }
    .nav-buttons {
      margin-bottom: 20px;
    }
    .produto {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 15px;
      margin-top: 20px;
    }
    .produto-card {
      background: white;
      padding: 15px;
      border-radius: 10px;
      box-shadow: 0px 4px 6px rgba(0,0,0,0.1);
      width: 220px;
      text-align: center;
    }
    .produto-card img {
      width: 150px;
      border-radius: 10px;
    }
    .preco {
      font-weight: bold;
      color: #e74c3c;
    }
    #carrinho {
      margin-top: 20px;
      padding: 15px;
      background: #f39c12;
      border-radius: 10px;
      color: white;
    }
    #carrinho ul {
      list-style: none;
      padding: 0;
    }
    #carrinho li {
      background: white;
      color: #333;
      padding: 8px;
      margin: 5px;
      border-radius: 5px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    /* Todas as "páginas" terão essa classe, mas somente a ativa será exibida */
    .page {
      display: none;
    }
    .active {
      display: block;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Loja de Bikes e Patinetes Elétricos</h1>
    <!-- Botões de navegação -->
    <div class="nav-buttons text-center">
      <button class="btn btn-outline-primary" onclick="showPage('homePage')">Home</button>
      <button class="btn btn-outline-primary" onclick="showPage('pricePage'); displayPriceProducts();">Ordenar por Preço</button>
      <button class="btn btn-outline-primary" onclick="showPage('typePage'); displayTypeProducts();">Ordenar por Tipo</button>
    </div>

    <!-- Página Home -->
    <div id="homePage" class="page active">
      <h2>Bem-vindo à nossa Loja</h2>
      <p>Selecione uma visualização:</p>
      <button class="btn btn-outline-primary" onclick="showPage('pricePage'); displayPriceProducts();">Ordenar por Preço</button>
      <button class="btn btn-outline-primary" onclick="showPage('typePage'); displayTypeProducts();">Ordenar por Tipo</button>
    </div>

    <!-- Página com produtos ordenados por preço -->
    <div id="pricePage" class="page">
      <h2>Produtos Ordenados por Preço</h2>
      <div class="produto" id="priceProducts"></div>
    </div>

    <!-- Página com produtos ordenados por tipo -->
    <div id="typePage" class="page">
      <h2>Produtos Ordenados por Tipo</h2>
      <div class="produto" id="typeProducts"></div>
    </div>

    <!-- Carrinho de Compras (visível em todas as páginas) -->
    <div id="carrinho">
      <h2>Carrinho de Compras</h2>
      <ul id="lista-carrinho"></ul>
      <p>Total: <span id="total">R$ 0,00</span></p>
      <button type="button" class="btn btn-outline-primary" onclick="finalizarCompra()">Finalizar Compra</button>
    </div>
  </div>

  <script>
    // Lista de produtos: Bikes e Patinetes Elétricos
    const products = [
      { nome: "Bicicleta Elétrica Urbana", descricao: "Ideal para deslocamentos diários na cidade.", precoDisplay: "R$ 3.499,00", preco: 3499.00, tipo: "Bike" },
      { nome: "Bicicleta Elétrica Mountain Bike", descricao: "Perfeita para trilhas e aventuras off-road.", precoDisplay: "R$ 5.999,00", preco: 5999.00, tipo: "Bike" },
      { nome: "Bicicleta Elétrica Dobrável", descricao: "Compacta e prática para ambientes urbanos.", precoDisplay: "R$ 4.299,00", preco: 4299.00, tipo: "Bike" },
      { nome: "Patinete Elétrico Compacto", descricao: "Leve e fácil de manobrar.", precoDisplay: "R$ 2.199,00", preco: 2199.00, tipo: "Patinete" },
      { nome: "Patinete Elétrico Off-Road", descricao: "Para os aventureiros que buscam performance.", precoDisplay: "R$ 3.299,00", preco: 3299.00, tipo: "Patinete" },
      { nome: "Patinete Elétrico de Alta Velocidade", descricao: "Velocidade e estilo em um único produto.", precoDisplay: "R$ 2.999,00", preco: 2999.00, tipo: "Patinete" },
      { nome: "Bicicleta Elétrica de Carga", descricao: "Perfeita para entregas urbanas.", precoDisplay: "R$ 6.499,00", preco: 6499.00, tipo: "Bike" },
      { nome: "Patinete Elétrico com Bateria de Longa Duração", descricao: "Mais autonomia para seus trajetos.", precoDisplay: "R$ 3.499,00", preco: 3499.00, tipo: "Patinete" }
    ];

    // Variáveis do carrinho
    let cart = [];
    let totalCart = 0;

    // Função para exibir a página desejada (home, preço ou tipo)
    function showPage(pageId) {
      document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
      document.getElementById(pageId).classList.add('active');
    }

    // Exibe os produtos ordenados por preço
    function displayPriceProducts() {
      const container = document.getElementById('priceProducts');
      container.innerHTML = "";
      let sortedProducts = [...products].sort((a, b) => a.preco - b.preco);
      sortedProducts.forEach(prod => {
        const div = document.createElement("div");
        div.className = "produto-card";
        div.innerHTML = `
          <img src="https://via.placeholder.com/150" alt="${prod.nome}">
          <h2>${prod.nome}</h2>
          <p>${prod.descricao}</p>
          <p class="preco">${prod.precoDisplay}</p>
          <button type="button" class="btn btn-outline-primary" onclick="adicionarAoCarrinho('${prod.nome}', ${prod.preco})">Comprar</button>
        `;
        container.appendChild(div);
      });
    }

    // Exibe os produtos ordenados por tipo
    function displayTypeProducts() {
      const container = document.getElementById('typeProducts');
      container.innerHTML = "";
      // Ordena os produtos por tipo: Bikes primeiro, depois Patinetes
      const typeOrder = { "Bike": 1, "Patinete": 2 };
      let sortedProducts = [...products].sort((a, b) => (typeOrder[a.tipo] || 0) - (typeOrder[b.tipo] || 0));
      sortedProducts.forEach(prod => {
        const div = document.createElement("div");
        div.className = "produto-card";
        div.innerHTML = `
          <img src="https://via.placeholder.com/150" alt="${prod.nome}">
          <h2>${prod.nome}</h2>
          <p>${prod.descricao}</p>
          <p class="preco">${prod.precoDisplay}</p>
          <p>Tipo: ${prod.tipo}</p>
          <button type="button" class="btn btn-outline-danger" onclick="adicionarAoCarrinho('${prod.nome}', ${prod.preco})">Comprar</button>
        `;
        container.appendChild(div);
      });
    }

    // Funções do carrinho
    function adicionarAoCarrinho(nome, preco) {
      cart.push({ nome, preco });
      totalCart += preco;
      atualizarCarrinho();
    }

    function removerDoCarrinho(index) {
      totalCart -= cart[index].preco;
      cart.splice(index, 1);
      atualizarCarrinho();
    }

    function atualizarCarrinho() {
      const lista = document.getElementById("lista-carrinho");
      lista.innerHTML = "";
      cart.forEach((item, index) => {
        const li = document.createElement("li");
        li.innerHTML = `${item.nome} - R$ ${item.preco.toFixed(2)}
          <button type="button" class="btn btn-outline-danger" onclick="removerDoCarrinho(${index})">Remover</button>`;
        lista.appendChild(li);
      });
      document.getElementById("total").textContent = `R$ ${totalCart.toFixed(2)}`;
    }

    function finalizarCompra() {
      if (cart.length === 0) {
        alert("Seu carrinho está vazio!");
      } else {
        alert(`Compra finalizada! Total: R$ ${totalCart.toFixed(2)}. Obrigado por comprar conosco.`);
        cart = [];
        totalCart = 0;
        atualizarCarrinho();
      }
    }
  </script>
</body>
</html>
