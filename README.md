<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FLIPD | Mati & Abeni</title>
  <link href="https://fonts.googleapis.com/css2?family=Pacifico&family=Inter:wght@400;700;800&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-dark: #0b132b;
      --card-bg: #1c2541;
      --accent: #3a86ff;
      --success: #10b981;
      --whatsapp: #25d366;
      --danger: #ef4444;
      --text-main: #ffffff;
      --text-dim: #94a3b8;
    }

    body {
      margin: 0;
      font-family: 'Inter', sans-serif;
      background: var(--bg-dark);
      color: var(--text-main);
      scroll-behavior: smooth;
    }

    header { padding: 60px 20px 20px; text-align: center; }
    .logo { font-family: 'Pacifico', cursive; font-size: clamp(50px, 10vw, 80px); margin: 0; text-shadow: 0 0 30px rgba(58, 134, 255, 0.4); }
    .tagline { font-weight: 800; letter-spacing: 10px; color: var(--accent); text-transform: uppercase; font-size: 0.9rem; }

    .nav-box {
      display: flex;
      justify-content: center;
      gap: 15px;
      background: var(--card-bg);
      max-width: fit-content;
      margin: 20px auto;
      padding: 10px 20px;
      border-radius: 15px;
      border: 1px solid rgba(255,255,255,0.1);
    }
    .nav-box a { text-decoration: none; color: #fff; font-weight: 700; font-size: 0.8rem; padding: 8px 12px; cursor: pointer; }

    .page { display: none; padding: 20px; animation: fadeIn 0.4s ease; }
    .page.active { display: block; }
    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

    .container { max-width: 800px; margin: auto; }

    .content-box {
      background: var(--card-bg);
      padding: 30px;
      border-radius: 20px;
      border: 1px solid rgba(255,255,255,0.05);
      margin-bottom: 25px;
      text-align: center;
      box-shadow: 0 10px 30px rgba(0,0,0,0.2);
    }

    /* --- ORDER BUTTONS GRID --- */
    .order-methods {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 15px;
      margin-top: 25px;
    }

    .method-card {
      background: rgba(255,255,255,0.03);
      padding: 15px;
      border-radius: 15px;
      border: 1px solid rgba(255,255,255,0.1);
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .btn-action {
      background: var(--accent);
      color: white;
      border: none;
      padding: 12px 20px;
      border-radius: 8px;
      font-weight: 800;
      cursor: pointer;
      text-transform: uppercase;
      transition: 0.3s;
      width: 100%;
      font-size: 0.8rem;
    }

    .btn-whatsapp { background: var(--whatsapp); }
    .btn-email { background: var(--success); }

    .products-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 20px; }
    .product-card { background: var(--card-bg); border-radius: 20px; padding: 15px; text-align: center; border: 1px solid rgba(255,255,255,0.05); }
    .product-card img { width: 100%; height: 200px; object-fit: cover; border-radius: 15px; margin-bottom: 10px; }

    footer { text-align: center; padding: 60px 40px; color: var(--text-dim); font-size: 0.85rem; }

    /* Responsive: Stack buttons on small screens */
    @media (max-width: 500px) {
      .order-methods { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>

<header>
  <h1 class="logo">Flipd</h1>
  <p class="tagline">Buy • Flip • Own</p>
</header>

<div class="nav-box">
  <a onclick="showPage('home')">HOME</a>
  <a onclick="showPage('listings')">PRODUCTS</a>
  <a onclick="showPage('cart-page')">CART (<span id="cart-count">0</span>)</a>
</div>

<div id="home" class="page active">
  <div class="container">
    <div class="content-box">
      <h2>A Note from the Founders</h2>
      <p style="font-weight: 700;">Welcome to FLIPD. Our mission is to provide you with a marketplace built on quality and trust.</p>
      <b style="color: var(--accent); font-size: 1.2rem;">Mati & Abeni</b>
    </div>
    <div style="text-align:center">
        <button class="btn-action" style="width:auto; padding: 15px 40px;" onclick="showPage('listings')">Start Browsing</button>
    </div>
  </div>
</div>

<div id="listings" class="page">
  <div class="container">
    <div class="products-grid">
      <div class="product-card">
        <img src="https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=500" alt="Shoes">
        <h3>Red Heat Kicks</h3>
        <p style="color:var(--success); font-weight:800;">3,200 Birr</p>
        <button onclick="addToCart('Red Heat Kicks', 3200)" class="btn-action">Add to Cart</button>
      </div>
      <div class="product-card">
        <img src="https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=500" alt="Watch">
        <h3>Silver Edition Watch</h3>
        <p style="color:var(--success); font-weight:800;">5,500 Birr</p>
        <button onclick="addToCart('Silver Edition Watch', 5500)" class="btn-action">Add to Cart</button>
      </div>
    </div>
  </div>
</div>

<div id="cart-page" class="page">
  <div class="container">
    <div class="content-box">
      <h2 style="text-transform: uppercase;">Your Shopping Cart</h2>
      <div id="cart-empty-msg" style="padding: 20px; color: var(--text-dim);">Your cart is currently empty.</div>
      <table id="cart-table" style="width:100%; color: white; border-collapse: collapse; display: none;">
        <tbody id="cart-table-body"></tbody>
      </table>
      
      <div id="cart-summary" style="display: none; margin-top: 20px; border-top: 1px solid rgba(255,255,255,0.1); padding-top: 20px;">
        <h3 style="font-size: 1.5rem; margin-bottom: 10px;">Total: <span id="cart-total" style="color: var(--success);">0</span> Birr</h3>
        
        <p style="font-size: 0.8rem; color: var(--text-dim);">Select an order method:</p>

        <div class="order-methods">
            <div class="method-card">
                <p style="font-size: 0.7rem; margin-bottom: 10px; color: var(--text-dim);">FASTEST RESPONSE</p>
                <button class="btn-action btn-whatsapp" onclick="sendWhatsAppOrder()">via WhatsApp</button>
            </div>
            <div class="method-card">
                <p style="font-size: 0.7rem; margin-bottom: 10px; color: var(--text-dim);">OFFICIAL RECORD</p>
                <button class="btn-action btn-email" onclick="placeOrder()">via Email</button>
            </div>
        </div>

        <div style="margin-top: 25px; border-top: 1px solid rgba(255,255,255,0.05); padding-top: 15px;">
            <b style="color: var(--accent); font-size: 0.9rem;">+2519 36 15 82 71</b>
        </div>
      </div>
    </div>
  </div>
</div>

<footer>
  <p>© 2026 FLIPD | Founded by Mati & Abeni</p>
</footer>

<script>
  let cart = {};

  function showPage(pId) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(pId).classList.add('active');
    window.scrollTo(0,0);
  }

  function addToCart(n, p) {
    if (cart[n]) cart[n].qty += 1;
    else cart[n] = { price: p, qty: 1 };
    updateCartUI();
  }

  function subtractFromCart(n) {
    if (cart[n].qty > 1) cart[n].qty -= 1;
    else delete cart[n];
    updateCartUI();
  }

  function updateCartUI() {
    const body = document.getElementById('cart-table-body');
    const table = document.getElementById('cart-table');
    const summary = document.getElementById('cart-summary');
    const emptyMsg = document.getElementById('cart-empty-msg');
    const totalEl = document.getElementById('cart-total');
    const countEl = document.getElementById('cart-count');

    body.innerHTML = "";
    let sum = 0; let count = 0;
    let keys = Object.keys(cart);

    if (keys.length > 0) {
      table.style.display = "table"; summary.style.display = "block"; emptyMsg.style.display = "none";
      keys.forEach(n => {
        let i = cart[n]; sum += (i.price * i.qty); count += i.qty;
        body.innerHTML += `
          <tr style="border-bottom: 1px solid rgba(255,255,255,0.05);">
            <td style="padding: 12px 0; text-align: left;">${n}</td>
            <td style="padding: 12px 0; text-align: center;">
              <button onclick="subtractFromCart('${n}')" style="background:var(--danger); border:none; color:white; width:24px; border-radius:4px; cursor:pointer;">-</button>
              <span style="margin: 0 10px;">${i.qty}</span>
              <button onclick="addToCart('${n}', ${i.price})" style="background:var(--accent); border:none; color:white; width:24px; border-radius:4px; cursor:pointer;">+</button>
            </td>
            <td style="padding: 12px 0; text-align: right;">${(i.price * i.qty).toLocaleString()}</td>
          </tr>`;
      });
    } else {
      table.style.display = "none"; summary.style.display = "none"; emptyMsg.style.display = "block";
    }
    totalEl.innerText = sum.toLocaleString();
    countEl.innerText = count;
  }

  function sendWhatsAppOrder() {
    const phoneNumber = "251936158271";
    let message = "📦 *NEW ORDER ON FLIPD*%0A%0A";
    let total = 0;
    for (let n in cart) {
      let i = cart[n]; let sub = i.price * i.qty; total += sub;
      message += `• ${n} (x${i.qty}) - ${sub.toLocaleString()} Birr%0A`;
    }
    message += `%0A💰 *TOTAL: ${total.toLocaleString()} Birr*%0A%0APlease confirm availability!`;
    window.open(`https://wa.me/${phoneNumber}?text=${message}`, '_blank');
  }

  function placeOrder() {
    let list = "ORDER DETAILS:%0A";
    let total = 0;
    for (let n in cart) {
      let i = cart[n]; let sub = i.price * i.qty; total += sub;
      list += `- ${n} (x${i.qty}): ${sub.toLocaleString()} Birr%0A`;
    }
    list += `%0ATOTAL: ${total.toLocaleString()} Birr`;
    window.location.href = `mailto:matiasbehailu36@gmail.com?subject=New Order&body=${list}`;
  }
</script>
</body>
</html>
