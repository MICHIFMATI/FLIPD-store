<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FLIPD | Social Shopping</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #ffffff;
      --text: #000000;
      --accent: #ff0000; /* Depop Red */
      --secondary: #f5f5f5;
    }

    body {
      margin: 0;
      font-family: 'Inter', sans-serif;
      background: var(--bg);
      color: var(--text);
    }

    /* --- DEPOP STYLE HEADER --- */
    header {
      padding: 15px 20px;
      border-bottom: 1px solid #ddd;
      position: sticky;
      top: 0;
      background: white;
      z-index: 100;
    }
    .logo { font-size: 28px; font-weight: 800; letter-spacing: -1px; margin: 0; cursor: pointer; }
    
    /* Search Bar like Depop */
    .search-container {
      margin: 10px 0;
      position: relative;
    }
    .search-bar {
      width: 100%;
      padding: 10px 15px;
      background: var(--secondary);
      border: none;
      border-radius: 8px;
      box-sizing: border-box;
      outline: none;
    }

    /* Category Navigation */
    .nav-scroll {
      display: flex;
      gap: 20px;
      overflow-x: auto;
      padding: 10px 0;
      border-bottom: 1px solid #eee;
      white-space: nowrap;
      scrollbar-width: none;
    }
    .nav-scroll a {
      text-decoration: none;
      color: var(--text);
      font-weight: 600;
      font-size: 0.9rem;
      cursor: pointer;
    }

    /* --- PRODUCT GRID (SQUARE STYLE) --- */
    .grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 2px; /* Thin lines like Depop grid */
      background: #eee;
    }
    @media (min-width: 768px) { .grid { grid-template-columns: repeat(4, 1fr); } }

    .item {
      background: white;
      padding: 0 0 15px 0;
      display: flex;
      flex-direction: column;
    }
    .item img {
      width: 100%;
      aspect-ratio: 1 / 1;
      object-fit: cover;
    }
    .item-info {
      padding: 10px;
      text-align: left;
    }
    .price { font-weight: 800; font-size: 1rem; margin: 5px 0; }
    .name { font-size: 0.85rem; color: #555; margin-bottom: 10px; }

    /* Action Buttons */
    .btn-buy {
      border: 1px solid #000;
      background: transparent;
      padding: 8px;
      font-weight: 800;
      cursor: pointer;
      text-transform: uppercase;
      font-size: 0.7rem;
    }
    .btn-buy:hover { background: #000; color: #fff; }

    /* Cart Floating Button */
    .cart-float {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: black;
      color: white;
      padding: 15px 25px;
      border-radius: 50px;
      font-weight: bold;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      cursor: pointer;
      z-index: 1000;
    }

    .page { display: none; }
    .page.active { display: block; }

    /* Cart Summary Card */
    .checkout-box {
      background: var(--secondary);
      padding: 20px;
      margin: 20px;
      border-radius: 12px;
    }
    .whatsapp-btn {
      background: #25d366;
      color: white;
      padding: 15px;
      display: block;
      text-decoration: none;
      border-radius: 8px;
      font-weight: bold;
      margin-top: 10px;
    }
  </style>
</head>
<body>

<header>
  <h1 class="logo" onclick="showPage('home')">FLIPD</h1>
  <div class="search-container">
    <input type="text" class="search-bar" placeholder="Search for items, brands, styles...">
  </div>
  <div class="nav-scroll">
    <a onclick="showPage('home')">Explore</a>
    <a onclick="filter('Men')">Menswear</a>
    <a onclick="filter('Women')">Womenswear</a>
    <a onclick="filter('Shoes')">Shoes</a>
    <a onclick="filter('Jewelry')">Jewelry</a>
  </div>
</header>

<div id="cart-btn" class="cart-float" onclick="showPage('cart-page')">
  Cart (<span id="count">0</span>)
</div>

<div id="home" class="page active">
  <div class="grid" id="product-grid">
    </div>
</div>

<div id="cart-page" class="page">
  <div class="checkout-box">
    <h2>My Bag</h2>
    <div id="cart-items">Your bag is empty.</div>
    <hr>
    <h3>Total: <span id="total-price">0</span> Birr</h3>
    <a href="#" class="whatsapp-btn" onclick="orderWhatsApp()">Checkout via WhatsApp</a>
    <button onclick="showPage('home')" style="margin-top:10px; border:none; background:none; cursor:pointer;">← Continue Shopping</button>
  </div>
</div>

<script>
  const products = [
    { name: "Vintage Red Kicks", price: 3200, cat: "Shoes", img: "https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=500" },
    { name: "Silver Edition Watch", price: 5500, cat: "Jewelry", img: "https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=500" },
    { name: "Oversized Tee", price: 1200, cat: "Men", img: "https://images.unsplash.com/photo-1521572163474-6864f9cf17ab?w=500" },
    { name: "Retro Sunglasses", price: 800, cat: "Jewelry", img: "https://images.unsplash.com/photo-1511499767390-a7335958beba?w=500" }
  ];

  let cart = {};

  function renderProducts(items) {
    const grid = document.getElementById('product-grid');
    grid.innerHTML = items.map(p => `
      <div class="item">
        <img src="${p.img}">
        <div class="item-info">
          <div class="price">${p.price.toLocaleString()} Birr</div>
          <div class="name">${p.name}</div>
          <button class="btn-buy" onclick="addToCart('${p.name}', ${p.price})">Add to Bag</button>
        </div>
      </div>
    `).join('');
  }

  function showPage(id) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    window.scrollTo(0,0);
  }

  function addToCart(name, price) {
    cart[name] = (cart[name] || 0) + 1;
    updateUI();
  }

  function updateUI() {
    let count = 0; let total = 0; let html = "";
    for (let n in cart) {
      count += cart[n];
      let p = products.find(prod => prod.name === n).price;
      total += (p * cart[n]);
      html += `<p><b>${n}</b> (x${cart[n]})</p>`;
    }
    document.getElementById('count').innerText = count;
    document.getElementById('total-price').innerText = total.toLocaleString();
    if(html) document.getElementById('cart-items').innerHTML = html;
  }

  function orderWhatsApp() {
    let msg = "Hi Mati & Abeni! New order from FLIPD:%0A";
    for(let n in cart) msg += `- ${n} (x${cart[n]})%0A`;
    window.open(`https://wa.me/251936158271?text=${msg}`);
  }

  function filter(cat) {
    const filtered = products.filter(p => p.cat === cat);
    renderProducts(filtered);
    showPage('home');
  }

  // Initialize
  renderProducts(products);
</script>

</body>
</html>
