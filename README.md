<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SMART AQUA CART | Premium Aquarium Store</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #0F9D58; --accent: #00C9FF; --gold: #FFD54F;
            --bg: #FFFDF5; --dark: #1a1a1a; --glass: rgba(255, 255, 255, 0.7);
        }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: var(--dark); overflow-x: hidden; }
        
        /* Premium UI Components */
        .glass { background: var(--glass); backdrop-filter: blur(15px); border: 1px solid rgba(255,255,255,0.3); }
        .gradient-btn { background: linear-gradient(45deg, var(--primary), var(--accent)); color: white; border: none; padding: 12px 24px; border-radius: 50px; cursor: pointer; transition: 0.3s; font-weight: 600; }
        .gradient-btn:hover { transform: scale(1.05); box-shadow: 0 10px 20px rgba(15, 157, 88, 0.3); }

        /* Navigation */
        nav { position: fixed; top: 0; width: 100%; z-index: 1000; padding: 20px 5%; display: flex; justify-content: space-between; align-items: center; }
        .logo { font-size: 1.5rem; font-weight: 800; color: var(--primary); }

        /* Hero */
        .hero { height: 80vh; display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; background: linear-gradient(135deg, #0F9D58, #00C9FF); color: white; }
        .hero h1 { font-size: 3.5rem; margin-bottom: 20px; }

        /* Product Grid */
        .shop-container { padding: 50px 5%; }
        .filters { display: flex; justify-content: center; gap: 15px; margin-bottom: 40px; flex-wrap: wrap; }
        .filter-btn { padding: 10px 20px; border-radius: 20px; border: 1px solid var(--primary); background: transparent; cursor: pointer; transition: 0.3s; }
        .filter-btn.active { background: var(--primary); color: white; }
        
        .product-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 30px; }
        .card { background: white; padding: 20px; border-radius: 24px; box-shadow: 0 10px 30px rgba(0,0,0,0.05); transition: 0.3s; }
        .card:hover { transform: translateY(-10px); }
        .card img { width: 100%; height: 200px; object-fit: cover; border-radius: 16px; margin-bottom: 15px; }
        .price { font-size: 1.2rem; font-weight: 700; color: var(--primary); margin: 10px 0; }

        /* Cart Drawer */
        #cart-drawer { position: fixed; right: -400px; top: 0; width: 350px; height: 100%; background: white; z-index: 2000; box-shadow: -10px 0 30px rgba(0,0,0,0.1); transition: 0.5s; padding: 20px; overflow-y: auto; }
        #cart-drawer.open { right: 0; }

        /* WhatsApp Floating Button */
        .whatsapp-float { position: fixed; bottom: 30px; right: 30px; background: #25D366; color: white; padding: 15px 25px; border-radius: 50px; text-decoration: none; font-weight: bold; z-index: 999; box-shadow: 0 5px 15px rgba(37, 211, 102, 0.4); }
    </style>
</head>
<body>

    <nav class="glass">
        <div class="logo">SMART AQUA CART</div>
        <button onclick="toggleCart()" class="gradient-btn">Cart (<span id="cart-count">0</span>)</button>
    </nav>

    <header class="hero">
        <h1>PREMIUM QUALITY FISHES</h1>
        <p>Karad, Dist- Satara, Maharashtra</p>
    </header>

    <main class="shop-container">
        <div class="filters" id="filters"></div>
        <div class="product-grid" id="product-grid"></div>
    </main>

    <div id="cart-drawer">
        <h3>Your Cart</h3>
        <div id="cart-items"></div>
        <h3 id="cart-total">Total: ₹0</h3>
        <form id="checkout-form" action="https://formspree.io/f/YOUR_FORMSPREE_ID_HERE" method="POST">
            <input type="hidden" name="order_summary" id="order_summary">
            <input type="text" name="name" placeholder="Your Name" required style="width:100%; margin: 10px 0; padding: 10px;">
            <input type="text" name="phone" placeholder="Phone Number" required style="width:100%; margin: 10px 0; padding: 10px;">
            <textarea name="address" placeholder="Delivery Address" required style="width:100%; margin: 10px 0; padding: 10px;"></textarea>
            <button type="submit" class="gradient-btn" style="width: 100%;">Place Order</button>
        </form>
    </div>

    <a href="https://wa.me/7350039389" class="whatsapp-float">Chat on WhatsApp</a>

    <script>
        // === OWNER CONFIGURATION ===
        const CONFIG = {
            ownerEmail: "smartaquariumkarad634@gmail.com",
            phone: "7350039389",
            products: [
                { id: 1, name: "Bluepet Aquarium Powerfilter 6000F", price: 320, category: "Filters", img: "https://i.ibb.co/4ZxR9JpS/1782745859148.webp" },
                { id: 2, name: "Bluepet Aquarium Powerfilter 1000F", price: 350, category: "Filters", img: "https://i.ibb.co/Z6D6DWRM/product-jpeg.jpg" },
                { id: 3, name: "Premium sponge filter XF-380", price: 250, category: "Filters", img: "https://i.ibb.co/Jjdm4N7b/g-Iz-Ozi-HU.webp" },
                { id: 4, name: "Premium Sponge Filter XF-280", price: 200, category: "Filters", img: "https://i.ibb.co/dJHDzpLN/71p4-Sso-Vvj-L.jpg" },
                { id: 5, name: "Aquarium Airpump For fishes", price: 200, category: "Filters", img: "https://i.ibb.co/rRgyjSwj/portable-aquarium-air-pump.jpg" },
                { id: 6, name: "6 in 1 Biological Sponge", price: 280, category: "Filters", img: "https://i.ibb.co/tPM0vQdL/1683465900149-SKU-0046-0.webp" },
                { id: 7, name: "Best Quality Siphon Pipe", price: 180, category: "Filters", img: "https://i.ibb.co/DHPpFDLN/51j0-X8-B5-KQL.jpg" },
                { id: 8, name: "Premium Colour Enhancing Royal arowana Food", price: 250, category: "Fish Food", img: "https://i.ibb.co/HTmSKwCB/71c-OFRBmi-PL.jpg" },
                { id: 9, name: "Taiyo Turtle Food 250gm", price: 250, category: "Turtle Food", img: "https://i.ibb.co/ccnMLzTW/51yv-UMAh-P6-L-AC-UF1000-1000-QL80.jpg" },
                { id: 10, name: "Tokyo Gold turtle food 500gm", price: 350, category: "Turtle Food", img: "https://i.ibb.co/5Xc9t7DZ/7183b1-K6ah-L.jpg" },
                { id: 11, name: "Taiyo Pink Fish Food 1kg", price: 550, category: "Fish Food", img: "https://i.ibb.co/gbCY2zb9/61-Sf-T-xt-Uo-L.jpg" },
                { id: 12, name: "Taiyo Pink Fish Food 500gm", price: 350, category: "Fish Food", img: "https://i.ibb.co/ymJx94d5/Taiyo-Pink.png" },
                { id: 13, name: "Opalfin Premium Quality Ultrafine Fish food", price: 120, category: "Fish Food", img: "https://i.ibb.co/M53dPDy9/IMG-20260629-214330.jpg" },
                { id: 14, name: "Opalfin Nutrimax Fish Food", price: 100, category: "Fish Food", img: "https://i.ibb.co/6Rbdg0JV/IMG-20260629-202101.jpg" },
                { id: 15, name: "Opalfin Xtrabite Fish Food", price: 120, category: "Fish Food", img: "https://i.ibb.co/M53dPDy9/IMG-20260629-214330.jpg" },
                { id: 16, name: "Opalfin Guppy Plus", price: 150, category: "Fish Food", img: "https://i.ibb.co/LDTmp86h/IMG-20260629-202123.jpg" },
                { id: 17, name: "Optimum Premium Fish Food 100gm", price: 80, category: "Fish Food", img: "https://i.ibb.co/HDtWZtK8/Untitleddesign-64-fb49fadc-f93d-47f9-ae9c-ccdd929f4e3c.png" }
            ]
        };

        let cart = JSON.parse(localStorage.getItem('cart')) || [];

        // App Initialization
        function init() {
            renderFilters();
            renderProducts('All');
            updateCartUI();
        }

        function renderFilters() {
            const categories = ['All', ...new Set(CONFIG.products.map(p => p.category))];
            const container = document.getElementById('filters');
            categories.forEach(cat => {
                const btn = document.createElement('button');
                btn.className = 'filter-btn';
                btn.innerText = cat;
                btn.onclick = () => renderProducts(cat);
                container.appendChild(btn);
            });
        }

        function renderProducts(cat) {
            const grid = document.getElementById('product-grid');
            grid.innerHTML = '';
            const filtered = cat === 'All' ? CONFIG.products : CONFIG.products.filter(p => p.category === cat);
            
            filtered.forEach(p => {
                const card = document.createElement('div');
                card.className = 'card';
                card.innerHTML = `
                    <img src="${p.img}" loading="lazy" alt="${p.name}">
                    <h3>${p.name}</h3>
                    <p class="price">₹${p.price}</p>
                    <button class="gradient-btn" onclick="addToCart(${p.id})">Add to Cart</button>
                `;
                grid.appendChild(card);
            });
        }

        function addToCart(id) {
            const product = CONFIG.products.find(p => p.id === id);
            cart.push(product);
            localStorage.setItem('cart', JSON.stringify(cart));
            updateCartUI();
            alert('Added to cart!');
        }

        function toggleCart() { document.getElementById('cart-drawer').classList.toggle('open'); }

        function updateCartUI() {
            const container = document.getElementById('cart-items');
            const totalEl = document.getElementById('cart-total');
            const summaryInput = document.getElementById('order_summary');
            
            container.innerHTML = '';
            let total = 0;
            let summaryText = "Order Summary:\n";

            cart.forEach((item, index) => {
                total += item.price;
                summaryText += `${item.name} - ₹${item.price}\n`;
                container.innerHTML += `<div style="display:flex; justify-content:space-between; margin:10px 0;">${item.name} <b>₹${item.price}</b></div>`;
            });

            summaryText += `\nGrand Total: ₹${total}`;
            totalEl.innerText = `Total: ₹${total}`;
            document.getElementById('cart-count').innerText = cart.length;
            summaryInput.value = summaryText;
        }

        // Form submission handling
        document.getElementById('checkout-form').onsubmit = function() {
            alert('Order placed! We will contact you soon.');
            localStorage.removeItem('cart');
            cart = [];
            updateCartUI();
        };

        init();
    </script>
</body>
</html># Smartaquacartorg
