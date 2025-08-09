<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>MadaraStore — Top Up Game</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --primary:#7c3aed;
      --muted:#6b7280;
      --bg:#f7f7fb;
      --card:#ffffff;
      --success:#10b981;
      --danger:#ef4444;
      font-family: "Inter", system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      background:var(--bg);
      color:#111827;
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      line-height:1.4;
    }

    /* Header */
    .topbar{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
      padding:18px 20px;
      background:linear-gradient(90deg, rgba(124,58,237,0.07), transparent);
      position:sticky;
      top:0;
      z-index:40;
      border-bottom:1px solid rgba(15,23,42,0.04);
    }
    .brand{
      display:flex;
      gap:12px;
      align-items:center;
    }
    .logo{
      width:44px;height:44px;border-radius:10px;
      display:grid;place-items:center;color:white;font-weight:700;
      background:linear-gradient(135deg,var(--primary),#4f46e5);
      box-shadow:0 6px 18px rgba(99,102,241,0.12);
      font-size:18px;
    }
    .brand h1{margin:0;font-size:16px;}
    .brand p{margin:0;color:var(--muted);font-size:12px}

    /* Search & actions */
    .actions{display:flex;gap:12px;align-items:center}
    .search{
      display:flex;align-items:center;background:white;padding:6px 10px;border-radius:10px;
      box-shadow:0 2px 8px rgba(15,23,42,0.04);min-width:220px;
    }
    .search input{border:0;outline:none;font-size:14px}
    .btn{background:var(--primary);color:white;padding:8px 12px;border-radius:10px;border:0;cursor:pointer;font-weight:600}
    .btn.ghost{background:transparent;color:var(--primary);border:1px solid rgba(124,58,237,0.12)}

    /* Layout */
    .container{max-width:1100px;margin:18px auto;padding:0 18px}
    .grid{display:grid;grid-template-columns:260px 1fr;gap:20px}
    @media (max-width:880px){.grid{grid-template-columns:1fr} .leftcol{order:2} .rightcol{order:1}}

    /* Sidebar / left */
    .leftcard{background:var(--card);border-radius:12px;padding:14px;box-shadow:0 6px 20px rgba(12,16,30,0.04)}
    .cat{display:flex;flex-direction:column;gap:8px;margin-top:8px}
    .cat button{border:0;background:transparent;padding:8px;border-radius:8px;text-align:left;cursor:pointer}
    .cat button.active{background:linear-gradient(90deg, rgba(124,58,237,0.09), transparent);font-weight:600}

    /* Product list */
    .products{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:16px}
    .card{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 6px 18px rgba(12,16,30,0.04);display:flex;flex-direction:column;gap:8px}
    .game{display:flex;gap:12px;align-items:center}
    .avatar{width:52px;height:52px;border-radius:10px;background:linear-gradient(135deg,#f3f4f6,#eef2ff);display:grid;place-items:center;font-weight:700;color:var(--primary)}
    .title{font-weight:700}
    .meta{font-size:13px;color:var(--muted)}
    .price{font-weight:800;font-size:16px}
    .card .actions{margin-top:auto;display:flex;gap:8px}

    /* Cart */
    .cartbtn{position:relative}
    .badge{position:absolute;top:-6px;right:-6px;background:var(--danger);color:white;font-size:12px;padding:3px 7px;border-radius:999px}

    .cart-panel{
      position:fixed;right:18px;bottom:18px;width:340px;max-width:90vw;background:var(--card);border-radius:14px;padding:12px;
      box-shadow:0 18px 45px rgba(12,16,30,0.15);z-index:60;
    }
    .cart-items{max-height:300px;overflow:auto;display:flex;flex-direction:column;gap:10px;margin-top:8px}
    .cart-row{display:flex;gap:10px;align-items:center;justify-content:space-between}
    .qty{display:flex;gap:6px;align-items:center}
    .small{font-size:13px;color:var(--muted)}

    /* Footer / Checkout */
    .total{display:flex;justify-content:space-between;align-items:center;padding-top:8px;border-top:1px dashed rgba(15,23,42,0.06)}
    .checkout{width:100%;padding:10px;border-radius:10px;border:0;background:linear-gradient(90deg,var(--primary),#4f46e5);color:white;font-weight:700;cursor:pointer;margin-top:10px}

    /* Modal */
    .overlay{position:fixed;inset:0;background:rgba(2,6,23,0.45);display:none;align-items:center;justify-content:center;z-index:80}
    .modal{background:var(--card);padding:18px;border-radius:12px;max-width:520px;width:96%}
    .field{display:flex;flex-direction:column;gap:6px;margin-bottom:10px}
    input,select,textarea{padding:10px;border-radius:8px;border:1px solid #e6e9ee;outline:none;font-size:14px}
    .muted{color:var(--muted);font-size:13px}

    /* tiny */
    .empty{padding:18px;text-align:center;color:var(--muted)}
  </style>
</head>
<body>

  <header class="topbar">
    <div class="brand">
      <div class="logo">M</div>
      <div>
        <h1>MadaraStore</h1>
        <p>Top Up Game — Cepat & Aman</p>
      </div>
    </div>

    <div class="actions">
      <div class="search" role="search">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" style="opacity:0.6;margin-right:6px"><path d="M21 21l-4.35-4.35" stroke="#374151" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/><circle cx="11" cy="11" r="6" stroke="#374151" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/></svg>
        <input id="search" placeholder="Cari game atau nominal..." />
      </div>

      <button class="btn ghost" id="favoriteToggle">Favorit</button>

      <div class="cartbtn">
        <button class="btn" id="openCart">Keranjang</button>
        <div class="badge" id="cartBadge" style="display:none">0</div>
      </div>
    </div>
  </header>

  <main class="container">
    <div class="grid">
      <aside class="leftcol">
        <div class="leftcard">
          <h3>Filter & Kategori</h3>
          <div class="cat" id="categories">
            <!-- categories dynamically -->
          </div>

          <div style="margin-top:12px">
            <h4 style="margin:0 0 6px 0">Opsi Cepat</h4>
            <button class="btn ghost" id="btnHot">Top Up Terpopuler</button>
          </div>
        </div>
      </aside>

      <section class="rightcol">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px">
          <div>
            <h2 style="margin:0">Pilih Top Up</h2>
            <p class="muted" id="resultsText">Menampilkan semua produk</p>
          </div>
          <div class="muted">MadaraStore • Aman & Terpercaya</div>
        </div>

        <div class="products" id="productList">
          <!-- product cards -->
        </div>
      </section>
    </div>
  </main>

  <!-- Cart panel -->
  <div class="cart-panel" id="cartPanel" style="display:none">
    <div style="display:flex;justify-content:space-between;align-items:center">
      <strong>Keranjang Anda</strong>
      <button id="closeCart" class="btn ghost" style="padding:6px 8px">Tutup</button>
    </div>

    <div id="cartItems" class="cart-items"></div>

    <div class="total">
      <div class="small">Total</div>
      <div><strong id="cartTotal">Rp0</strong></div>
    </div>

    <button id="checkoutBtn" class="checkout">Checkout</button>
  </div>

  <!-- Modal overlay -->
  <div class="overlay" id="overlay">
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
      <h3 id="modalTitle">Checkout — MadaraStore</h3>
      <p class="muted">Isi data dengan benar. Ini hanya demo simulasi transaksi.</p>

      <div id="checkoutForm">
        <div class="field">
          <label>Nama Pemesan</label>
          <input id="buyerName" placeholder="Nama lengkap" />
        </div>

        <div class="field">
          <label>ID Player / UID</label>
          <input id="playerId" placeholder="Masukkan UID / ID player" />
          <div class="muted">Contoh: 12345678</div>
        </div>

        <div class="field">
          <label>Metode Pembayaran</label>
          <select id="paymentMethod">
            <option value="gopay">Gopay</option>
            <option value="bank">Bank Transfer</option>
            <option value="shopee">ShopeePay</option>
          </select>
        </div>

        <div class="field">
          <label>Catatan (opsional)</label>
          <textarea id="note" rows="2" placeholder="Contoh: top up untuk event"></textarea>
        </div>

        <div style="display:flex;gap:8px;justify-content:flex-end">
          <button id="cancelCheckout" class="btn ghost">Batal</button>
          <button id="confirmCheckout" class="btn">Konfirmasi & Bayar</button>
        </div>
      </div>

      <div id="checkoutResult" style="display:none;margin-top:8px"></div>
    </div>
  </div>

  <script>
    // --- Sample products (could later datang dari API) ---
    const PRODUCTS = [
      { id: 'p1', game:'Free Fire', provider:'Garena', nominal:'Diamonds 70', price:12000, category:'Battle Royale' },
      { id: 'p2', game:'Mobile Legends', provider:'Moonton', nominal:'Diamond 86', price:25000, category:'MOBA' },
      { id: 'p3', game:'PUBG Mobile', provider:'Tencent', nominal:'UC 300', price:49000, category:'Battle Royale' },
      { id: 'p4', game:'Genshin Impact', provider:'miHoYo', nominal:'Primogems 1600', price:199000, category:'RPG' },
      { id: 'p5', game:'Valorant', provider:'Riot', nominal:'VP 500', price:99000, category:'Shooter' },
      { id: 'p6', game:'Mobile Legends', provider:'Moonton', nominal:'Starlight 1 month', price:150000, category:'MOBA' },
      { id: 'p7', game:'AOV', provider:'Garena', nominal:'Diamonds 400', price:80000, category:'MOBA' },
    ];

    // --- Utils ---
    const el = id => document.getElementById(id);
    const formatIDR = (v) => {
      return 'Rp' + v.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    };

    // --- State & localStorage ---
    const CART_KEY = 'madara_cart_v1';
    let cart = JSON.parse(localStorage.getItem(CART_KEY) || "[]");

    function saveCart(){ localStorage.setItem(CART_KEY, JSON.stringify(cart)); renderCart(); updateBadge(); }

    // --- Rendering categories & products ---
    const categories = Array.from(new Set(PRODUCTS.map(p=>p.category)));
    function renderCategories(){
      const parent = el('categories');
      parent.innerHTML = '';
      const allBtn = document.createElement('button');
      allBtn.textContent = 'Semua';
      allBtn.className = 'active';
      allBtn.onclick = ()=> { document.querySelectorAll('.cat button').forEach(b=>b.classList.remove('active')); allBtn.classList.add('active'); renderProducts(PRODUCTS); }
      parent.appendChild(allBtn);

      categories.forEach(cat=>{
        const b = document.createElement('button');
        b.textContent = cat;
        b.onclick = ()=> {
          document.querySelectorAll('.cat button').forEach(btn=>btn.classList.remove('active'));
          b.classList.add('active');
          renderProducts(PRODUCTS.filter(p=>p.category===cat));
        };
        parent.appendChild(b);
      });
    }

    function renderProducts(list){
      const wrap = el('productList');
      wrap.innerHTML = '';
      el('resultsText').textContent = `Menampilkan ${list.length} produk`;
      if(list.length === 0){ wrap.innerHTML = '<div class="empty">Tidak ada produk</div>'; return; }
      list.forEach(p=>{
        const card = document.createElement('div'); card.className='card';
        card.innerHTML = `
          <div class="game">
            <div class="avatar">${p.game.split(' ').map(w=>w[0]).slice(0,2).join('')}</div>
            <div style="flex:1">
              <div class="title">${p.game}</div>
              <div class="meta">${p.provider} • ${p.nominal}</div>
            </div>
            <div style="text-align:right"><div class="price">${formatIDR(p.price)}</div></div>
          </div>
          <div class="actions">
            <button class="btn ghost" data-id="${p.id}">Detail</button>
            <button class="btn" data-add="${p.id}">Tambah</button>
          </div>
        `;
        wrap.appendChild(card);
      });

      // attach handlers
      wrap.querySelectorAll('[data-add]').forEach(btn=>{
        btn.addEventListener('click', e=>{
          const id = e.currentTarget.getAttribute('data-add');
          addToCart(id,1);
        });
      });
      wrap.querySelectorAll('[data-id]').forEach(btn=>{
        btn.addEventListener('click', e=>{
          const id = e.currentTarget.getAttribute('data-id');
          const prod = PRODUCTS.find(x=>x.id===id);
          alert(`${prod.game} — ${prod.nominal}\\nHarga: ${formatIDR(prod.price)}\\nProvider: ${prod.provider}`);
        });
      });
    }

    // --- Cart logic ---
    function addToCart(productId, qty=1){
      const prod = PRODUCTS.find(p=>p.id===productId);
      if(!prod) return;
      const existing = cart.find(c=>c.id===productId);
      if(existing) existing.qty += qty;
      else cart.push({ id:productId, qty:qty });
      saveCart();
      showToast('Berhasil ditambahkan ke keranjang');
    }

    function updateQty(productId, qty){
      const idx = cart.findIndex(c=>c.id===productId);
      if(idx === -1) return;
      cart[idx].qty = qty;
      if(cart[idx].qty <= 0) cart.splice(idx,1);
      saveCart();
    }

    function removeFromCart(productId){
      cart = cart.filter(c=>c.id !== productId);
      saveCart();
    }

    function getCartItemsWithData(){
      return cart.map(c=>{
        const p = PRODUCTS.find(x=>x.id===c.id);
        return {...p, qty:c.qty, subtotal: p.price * c.qty};
      });
    }

    function cartTotal(){
      return getCartItemsWithData().reduce((s,i)=>s+i.subtotal,0);
    }

    // --- Render cart UI ---
    function renderCart(){
      const container = el('cartItems');
      container.innerHTML = '';
      const items = getCartItemsWithData();
      if(items.length === 0){
        container.innerHTML = '<div class="empty">Keranjang kosong</div>';
        el('cartTotal').textContent = 'Rp0';
        return;
      }
      items.forEach(it=>{
        const row = document.createElement('div'); row.className='cart-row';
        row.innerHTML = `
          <div style="flex:1">
            <div><strong>${it.game}</strong></div>
            <div class="small">${it.nominal} • ${formatIDR(it.price)}</div>
          </div>
          <div style="min-width:110px;display:flex;flex-direction:column;align-items:flex-end">
            <div class="qty">
              <button data-dec="${it.id}" class="btn ghost" style="padding:6px 8px">-</button>
              <div style="padding:6px 10px;border-radius:8px;border:1px solid #eee">${it.qty}</div>
              <button data-inc="${it.id}" class="btn ghost" style="padding:6px 8px">+</button>
            </div>
            <div class="small" style="margin-top:6px">${formatIDR(it.subtotal)}</div>
            <button data-rm="${it.id}" class="btn ghost" style="margin-top:6px;padding:6px 8px">Hapus</button>
          </div>
        `;
        container.appendChild(row);
      });

      // attach handlers
      container.querySelectorAll('[data-inc]').forEach(b=>{
        b.addEventListener('click', ()=> {
          const id = b.getAttribute('data-inc');
          const c = cart.find(x=>x.id===id); if(c) updateQty(id, c.qty+1);
        });
      });
      container.querySelectorAll('[data-dec]').forEach(b=>{
        b.addEventListener('click', ()=> {
          const id = b.getAttribute('data-dec');
          const c = cart.find(x=>x.id===id); if(c) updateQty(id, c.qty-1);
        });
      });
      container.querySelectorAll('[data-rm]').forEach(b=>{
        b.addEventListener('click', ()=> {
          const id = b.getAttribute('data-rm');
          removeFromCart(id);
        });
      });

      el('cartTotal').textContent = formatIDR(cartTotal());
    }

    function updateBadge(){
      const b = el('cartBadge');
      const count = cart.reduce((s,i)=>s+i.qty,0);
      if(count>0){ b.style.display='block'; b.textContent = count; } else b.style.display='none';
    }

    // --- Search & UI events ---
    el('search').addEventListener('input', e=>{
      const q = e.target.value.trim().toLowerCase();
      const filtered = PRODUCTS.filter(p=> (p.game + ' ' + p.nominal + ' ' + p.provider).toLowerCase().includes(q));
      renderProducts(filtered);
    });

    el('openCart').addEventListener('click', ()=> { el('cartPanel').style.display='block'; });
    el('closeCart').addEventListener('click', ()=> { el('cartPanel').style.display='none'; });

    // checkout
    el('checkoutBtn').addEventListener('click', ()=> {
      if(cart.length === 0){ alert('Keranjang kosong. Tambahkan produk dulu.'); return; }
      el('overlay').style.display = 'flex';
    });
    el('cancelCheckout').addEventListener('click', ()=> {
      el('overlay').style.display = 'none';
    });

    el('confirmCheckout').addEventListener('click', ()=> {
      const name = el('buyerName').value.trim();
      const uid = el('playerId').value.trim();
      const method = el('paymentMethod').value;
      if(!name || !uid){ alert('Nama dan UID harus diisi'); return; }

      // Disable form
      el('confirmCheckout').disabled = true;
      el('confirmCheckout').textContent = 'Memproses...';

      // Simulate request (replace with real API in production)
      setTimeout(()=>{
        const orderId = 'MDR' + Date.now();
        el('checkoutResult').style.display = 'block';
        el('checkoutResult').innerHTML = `
          <div style="padding:12px;border-radius:8px;background:linear-gradient(90deg, rgba(16,185,129,0.08), transparent)">
            <strong>Berhasil!</strong>
            <div class="muted">Order ID: ${orderId}</div>
            <div style="margin-top:8px">Total: <strong>${formatIDR(cartTotal())}</strong></div>
            <div class="muted" style="margin-top:6px">Silakan lakukan pembayaran ke metode <strong>${method}</strong>.</div>
          </div>
        `;
        // clear cart
        cart = [];
        saveCart();
        el('confirmCheckout').disabled = false;
        el('confirmCheckout').textContent = 'Konfirmasi & Bayar';
      }, 1200);
    });

    // small toast
    function showToast(text){
      const t = document.createElement('div');
      t.textContent = text;
      t.style.position='fixed'; t.style.left='50%'; t.style.transform='translateX(-50%)';
      t.style.bottom='24px'; t.style.background='rgba(15,23,42,0.92)'; t.style.color='white';
      t.style.padding='10px 14px'; t.style.borderRadius='999px'; t.style.zIndex='120';
      document.body.appendChild(t);
      setTimeout(()=> t.style.opacity='0',1800);
      setTimeout(()=> document.body.removeChild(t),2400);
    }

    // init
    function init(){
      renderCategories();
      renderProducts(PRODUCTS);
      renderCart();
      updateBadge();
      // demo: hot button
      el('btnHot').addEventListener('click', ()=> {
        const hot = PRODUCTS.slice(0,3);
        renderProducts(hot);
        showToast('Menampilkan top up terpopuler');
      });
    }

    init();
  </script>
</body>
</html>
