<-columns: repeat(4, 1fr); } }
!DOCTYPE html>
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
      --secondary: #f5f5f5;
    }

    body { margin: 0; font-family: 'Inter', sans-serif; background: var(--bg); color: var(--text); -webkit-tap-highlight-color: transparent; }

    /* --- HEADER --- */
    header { padding: 15px 20px; border-bottom: 1px solid #ddd; position: sticky; top: 0; background: white; z-index: 100; }
    .logo { font-size: 28px; font-weight: 800; letter-spacing: -1px; margin: 0; cursor: pointer; }
    
    .search-container { margin: 10px 0; }
    .search-bar { width: 100%; padding: 12px 15px; background: var(--secondary); border: none; border-radius: 8px; outline: none; font-size: 0.9rem; }

    .nav-scroll { display: flex; gap: 20px; overflow-x: auto; padding: 10px 0; border-bottom: 1px solid #eee; white-space: nowrap; scrollbar-width: none; }
    .nav-scroll a { text-decoration: none; color: var(--text); font-weight: 600; font-size: 0.85rem; cursor: pointer; }

    /* --- PRODUCT GRID --- */
    .grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 2px; background: #eee; }
    @media (min-width: 768px) { .grid { grid-template
    .item { background: white; padding: 0 0 15px 0; display: flex; flex-direction: column; }
    .item img { width: 100%; aspect-ratio: 1 / 1; object-fit: cover; }
    .item-info { padding: 12px; text-align: left; }
    .price { font-weight: 800; font-size: 1rem; margin-bottom: 4px; }
    .name { font-size: 0.85rem; color: #555; margin-bottom: 12px; height: 1.2em; overflow: hidden; }

    .btn-buy { border: 1px solid #000; background: transparent; padding: 8px; font-weight: 800; cursor: pointer; text-transform: uppercase; font-size: 0.7rem; width: 100%; transition: 0.2s; }
    .btn-buy:hover { background: #000; color: #fff; }

    /* Floating Bag Button */
    .cart-float { position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%); background: black; color: white; padding: 14px 28px; border-radius: 50px; font-weight: 800; box-shadow: 0 8px 20px rgba(0,0,0,0.3); cursor: pointer; z-index: 1000; font-size: 0.9rem; }

    .page { display: none; }
    .page.active { display: block; }

    /* --- CHECKOUT BOX STYLE --- */
    .checkout-container { max-width: 500px; margin: auto; padding: 20px; }
    .cart-item-row { display: flex; align-items: center; gap: 15px; padding: 15px 0; border-bottom: 1px solid #eee; }
    .cart-item-row img { width: 70px; height: 70px; object-fit: cover; border-radius: 4px; }
    
    .place-order-box { background: var(--secondary); padding: 25px; border-radius: 15px; margin-top: 20px; text-align: center; }
    
    /* THE UPDATED BUTTON */
    .btn-place-order { background: black; color: white; border: none; width: 100%; padding: 18px; border-radius: 12px; font-weight: 800; font-size: 1.1rem; cursor: pointer; transition: 0.2s; box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
    .btn-place-order:active { transform: scale(0.97); }
  </style>
</head>
<body>

<header>
  <h1 class="logo" onclick="location.reload()">FLIPD</h1>
  <div class="search-container">
    <input type="text" class="search-bar" placeholder="Search for items, brands, styles...">
  </div>
  <div class="nav-scroll">
    <a onclick="location.reload()">Explore</a>
    <a onclick="filter('Men')">Menswear</a>
    <a onclick="filter('Women')">Womenswear</a>
    <a onclick="filter('Shoes')">Shoes</a>
    <a onclick="filter('Jewelry')">Jewelry</a>
  </div>
</header>

<div id="cart-btn" class="cart-float" onclick="showPage('cart-page')" style="display:none;">
  VIEW BAG (<span id="count">0</span>)
</div>

<div id="home" class="page active">
  <div class="grid" id="product-grid"></div>
</div>

<div id="cart-page" class="page">
  <div class="checkout-container">
    <h2 style="font-weight: 800; font-size: 1.8rem; margin-bottom: 5px; letter-spacing: -0.5px;">My Bag</h2>
    <div id="cart-empty-msg" style="padding: 50px 0; color: #888; text-align: center;">Your bag is currently empty.</div>
    
    <div id="cart-items-list"></div>

    <div id="cart-summary" style="display:none;">
        <div style="display: flex; justify-content: space-between; padding: 25px 0; font-size: 1.1rem;">
            <span>Total</span>
            <span style="font-weight: 800;"><span id="total-price">0</span> Birr</span>
        </div>

        <div class="place-order-box">
            <p style="font-size: 0.7rem; color: #888; margin-bottom: 15px; text-transform: uppercase; letter-spacing: 1.5px; font-weight: 600;">Secure Checkout</p>
            <button class="btn-place-order" onclick="orderWhatsApp()">Place Order</button>
            <p style="font-size: 0.7rem; color: #aaa; margin-top: 15px;">You will be redirected to WhatsApp to finish your order.</p>
        </div>
        
        <button onclick="showPage('home')" style="width:100%; background:none; border:none; margin-top:25px; color:#000; font-weight: 700; text-decoration: underline; cursor:pointer; font-size: 0.85rem;">Continue Shopping</button>
    </div>
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
          <button class="btn-buy" onclick="addToCart('${p.name}')">Add to Bag</button>
        </div>
      </div>
    `).join('');
  }

  function showPage(id) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    window.scrollTo(0,0);
  }

  function addToCart(name) {
    cart[name] = (cart[name] || 0) + 1;
    updateUI();
  }

  function updateUI() {
    let count = 0; let total = 0; let html = "";
    for (let n in cart) {
      count += cart[n];
      let pData = products.find(p => p.name === n);
      total += (pData.price * cart[n]);
      html += `
        <div class="cart-item-row">
            <img src="${pData.img}">
            <div style="flex-grow:1;">
                <div style="font-weight:700; font-size: 0.95rem;">${n}</div>
                <div style="font-size:0.8rem; color:#888;">Quantity: ${cart[n]}</div>
            </div>
            <div style="font-weight:800;">${(pData.price * cart[n]).toLocaleString()}</div>
        </div>`;
    }
    
    document.getElementById('count').innerText = count;
    document.getElementById('total-price').innerText = total.toLocaleString();
    document.getElementById('cart-items-list').innerHTML = html;
    
    document.getElementById('cart-btn').style.display = count > 0 ? "block" : "none";
    document.getElementById('cart-summary').style.display = count > 0 ? "block" : "none";
    document.getElementById('cart-empty-msg').style.display = count > 0 ? "none" : "block";
  }

  function orderWhatsApp() {
    let msg = "Hi Mati %26 Abeni! I want to buy:%0A%0A";
    let total = 0;
    for(let n in cart) {
        let pData = products.find(p => p.name === n);
        total += (pData.price * cart[n]);
        msg += `• ${n} (x${cart[n]}) - ${(pData.price * cart[n]).toLocaleString()} Birr%0A`;
    }
    msg += `%0A💰 *TOTAL: ${total.toLocaleString()} Birr*`;
    window.open(`https://wa.me/251936158271?text=${msg}`);
  }

  function filter(cat) {
    renderProducts(products.filter(p => p.cat === cat));
    showPage('home');
  }

  renderProducts(products);
</script>

</body>
</html>
