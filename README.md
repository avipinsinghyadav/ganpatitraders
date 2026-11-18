<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Shri Ganeshay Namah Store - Premium Collection</title>
  <style>
    body{margin:0;font-family:'Segoe UI',Tahoma,Geneva,Verdana,sans-serif;background:linear-gradient(175deg,#1a1a1a,#2d2d2d 50%,#404040);min-height:100vh;color:#e0e0e0}
    header{display:flex;justify-content:space-between;align-items:center;gap:16px;padding:15px 30px;background:rgba(20,20,20,.95);box-shadow:0 4px 25px #0008;position:sticky;top:0;z-index:100;border-bottom:2px solid #b59410}
    .logo-container{display:flex;align-items:center;gap:15px;font-size:1.35rem;font-weight:bold;color:#ffd700;letter-spacing:1px}
    .logo-container img{width:180px;height:60px;object-fit:cover;border-radius:8px;border:2px solid #ffd700}
    .cart-btn,.btn{border:0;border-radius:12px;padding:12px;cursor:pointer;font-weight:600;font-size:1rem}
    .cart-btn{background:linear-gradient(45deg,#b59410,#ffd700);color:#111;padding:12px 24px;border-radius:30px;font-weight:bold}
    #shop{padding:40px 20px;max-width:1200px;margin:auto}
    #q{box-sizing:border-box;padding:14px 25px;width:100%;max-width:450px;border:2px solid #b59410;border-radius:30px;outline:none;font-size:1.1rem;background:#2a2a2a;color:#fff;margin-bottom:30px}
    #grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:25px}
    .product{background:#2a2a2a;border:1px solid #404040;padding:25px;border-radius:20px;box-shadow:0 10px 30px #0005;transition:.3s;display:flex;flex-direction:column;justify-content:space-between}
    .product:hover{transform:translateY(-5px);border-color:#b59410}
    .pic{text-align:center;font-size:3.5rem;margin-bottom:15px;height:180px;display:flex;align-items:center;justify-content:center}
    .pic img{width:100%;height:180px;object-fit:cover;border-radius:12px}
    .body h3{margin:0 0 10px;color:#fff;font-size:1.3rem}.body p{margin:0 0 15px;color:#aaa}
    .price{font-size:1.4rem;font-weight:bold;color:#34d399;display:block;margin-bottom:15px}
    .btn{width:100%;background:#b59410;color:#fff;margin-top:8px;text-align:center;box-sizing:border-box;text-decoration:none;display:block}.btn:hover{background:#d4ae14}
    .drawer{position:fixed;right:-350px;top:0;width:min(350px,100%);height:100%;background:#2a2a2a;transition:.4s;box-shadow:-10px 0 40px #0008;padding:25px;z-index:1000;box-sizing:border-box;display:flex;flex-direction:column;border-left:2px solid #b59410}
    .drawer.open{right:0}.shade{position:fixed;inset:0;background:#000b;display:none;z-index:999}.shade.show{display:block}
    #toast{position:fixed;bottom:30px;left:50%;transform:translateX(-50%) translateY(100px);background:#ffd700;color:#111;padding:15px 30px;border-radius:30px;opacity:0;transition:.4s;pointer-events:none;z-index:1100;font-weight:bold}
    #toast.show{transform:translateX(-50%) translateY(0);opacity:1}
    @media(max-width:600px){header{padding:12px;flex-wrap:wrap}.logo-container{font-size:1rem}.logo-container img{width:90px;height:42px}#shop{padding:25px 14px}}
  </style>
</head>
<body>
  <header>
    <div class="logo-container"><img src="image_1.png" alt="Ganpati Ji Premium"><span>Shri Ganeshay Namah Store</span></div>
    <button class="cart-btn" onclick="openCart()">🛒 Cart (<span id="count">0</span>)</button>
  </header>
  <main id="shop">
    <input type="text" id="q" placeholder="Search Premium Products..." oninput="render()" aria-label="Search products">
    <div id="grid"></div>
  </main>
  <aside id="drawer" class="drawer" aria-label="Shopping cart">
    <h3>Your Cart</h3>
    <div id="items" style="flex-grow:1;overflow-y:auto;margin:20px 0"></div>
    <p style="font-size:1.2rem"><b>Total: ₹<span id="total">0</span></b></p>
    <button class="btn" onclick="checkout()" style="background:#34d399">Proceed to Checkout</button>
    <button class="btn" onclick="closeCart()" style="background:#ef4444">Close Cart</button>
  </aside>
  <div id="shade" class="shade" onclick="closeCart()"></div>
  <div id="toast" role="status"></div>
  <script>
    const products = [
      {name:"Vegetable Push Chopper", category:"New", price:499, icon:"🧅", image:"https://images.pexels.com/photos/4613825/pexels-photo-4613825.jpeg", description:"Easy manual vegetable chopper"},
      {name:"Premium Product", category:"Featured", price:1299, icon:"📦", description:"Quality catalogue item"},
      {name:"Smart Home Essential", category:"New", price:899, icon:"🏠", description:"Practical everyday product"},
      {name:"Daily Use Collection", category:"Offers", price:599, icon:"🛍️", description:"Great value selection"},
      {name:"Business Pack", category:"Featured", price:2499, icon:"🎁", description:"Convenient business pack"},
      {name:"Quality Choice", category:"All", price:749, icon:"⭐", description:"Selected for quality"},
      {name:"Fresh Arrival", category:"New", price:1599, icon:"✨", description:"Latest catalogue arrival"},
      {name:"Special Deal", category:"Offers", price:999, icon:"🏷️", description:"Special offer item"},
      {name:"Premium Collection", category:"Featured", price:1899, icon:"💎", description:"Premium selection"}
    ];
    let cart=[];
    function render(){
      const query=document.getElementById("q").value.toLowerCase();
      const matches=products.filter(p=>p.name.toLowerCase().includes(query)||p.description.toLowerCase().includes(query));
      document.getElementById("grid").innerHTML=matches.map(p=>{const picture=p.image?`<img src="${p.image}" alt="${p.name}">`:p.icon;return `<article class="product"><div><div class="pic">${picture}</div><div class="body"><h3>${p.name}</h3><p>${p.description}</p><span class="price">₹${p.price.toLocaleString("en-IN")}</span></div></div><button class="btn" onclick="add('${p.name}')">Add to Cart</button></article>`}).join("");
    }
    function add(name){const product=products.find(p=>p.name===name);if(!product)return;cart.push(product);document.getElementById("count").textContent=cart.length;toast("Added to cart");}
    function openCart(){document.getElementById("drawer").classList.add("open");document.getElementById("shade").classList.add("show");drawCart();}
    function closeCart(){document.getElementById("drawer").classList.remove("open");document.getElementById("shade").classList.remove("show");}
    function drawCart(){document.getElementById("items").innerHTML=cart.length?cart.map((p,i)=>`<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:15px;border-bottom:1px solid #404040;padding-bottom:10px">${p.icon} ${p.name} <span>₹${p.price.toLocaleString("en-IN")} <button onclick="removeItem(${i})" aria-label="Remove ${p.name}">×</button></span></div>`).join(""):"<p style='color:#aaa;text-align:center;margin-top:30px'>Your cart is empty.</p>";document.getElementById("total").textContent=cart.reduce((sum,p)=>sum+p.price,0).toLocaleString("en-IN");}
    function removeItem(index){cart.splice(index,1);document.getElementById("count").textContent=cart.length;drawCart();}
    function checkout(){toast(cart.length?"Checkout demo ready — connect payment gateway.":"Add a product first");}
    function toast(message){const element=document.getElementById("toast");element.textContent=message;element.classList.add("show");setTimeout(()=>element.classList.remove("show"),2500);}
    render();
  </script>
</body>
</html>
