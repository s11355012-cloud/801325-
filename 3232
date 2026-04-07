<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>榕樹下米苔目 - 點餐系統 (Bilingual)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { font-family: "Microsoft JhengHei", "Segoe UI", sans-serif; background-color: #f3f4f6; }
        .sidebar-active { background-color: #ffffff; color: #f97316; border-left: 4px solid #f97316; }
        .hide { display: none !important; }
        .menu-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 12px; }
        .card-shadow { box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        .menu-item-card {
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: all 0.2s;
            text-align: center;
        }
        .qty-btn {
            width: 28px;
            height: 28px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 999px;
            border: 1px solid currentColor;
            font-weight: bold;
            transition: all 0.2s;
        }
        .qty-btn:hover {
            background-color: currentColor;
            color: white !important;
        }
    </style>
</head>
<body class="h-screen overflow-hidden flex flex-col">

    <!-- 1. 登入模態框 (Login) -->
    <div id="login-screen" class="fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-[100]">
        <div class="bg-white p-8 rounded-xl shadow-2xl w-96 text-center">
            <h2 class="text-2xl font-bold mb-2">歡迎使用點餐系統</h2>
            <h3 class="text-lg text-gray-400 mb-6 font-medium">Welcome to POS System</h3>
            <p class="text-gray-500 mb-4 text-sm">請輸入學號(8位)或老師帳號(teacher)<br>Enter ID (8 digits) or 'teacher'</p>
            <input type="text" id="user-account" class="w-full border-2 border-gray-200 rounded-lg p-3 mb-4 focus:border-orange-500 outline-none" placeholder="113XXXXX / teacher">
            <button onclick="handleLogin()" class="w-full bg-orange-500 text-white font-bold py-3 rounded-lg hover:bg-orange-600 transition">
                進入系統<br><span class="text-xs font-normal">Enter System</span>
            </button>
            <p id="login-error" class="text-red-500 mt-2 text-sm hide">請輸入正確的帳號格式<br>Invalid account format</p>
        </div>
    </div>

    <!-- 2. 模式選擇畫面 (Mode Selection) -->
    <div id="mode-screen" class="fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-[110] hide">
        <div class="bg-white p-8 rounded-xl shadow-2xl w-[500px] text-center">
            <h2 class="text-2xl font-bold mb-1">請選擇用餐模式</h2>
            <h3 class="text-lg text-gray-400 mb-8 font-medium">Please Select Dining Mode</h3>
            <div class="grid grid-cols-2 gap-6">
                <button onclick="setOrderMode('dine-in')" class="flex flex-col items-center justify-center p-6 border-2 border-gray-100 rounded-2xl hover:border-orange-500 hover:bg-orange-50 transition group">
                    <span class="text-5xl mb-4 text-gray-400 group-hover:text-orange-500">🏠</span>
                    <span class="text-xl font-bold text-gray-700">內用</span>
                    <span class="text-sm text-gray-400 font-normal">Dine-in</span>
                </button>
                <button onclick="setOrderMode('delivery')" class="flex flex-col items-center justify-center p-6 border-2 border-gray-100 rounded-2xl hover:border-pink-500 hover:bg-pink-50 transition group">
                    <span class="text-5xl mb-4 text-gray-400 group-hover:text-pink-600">🛵</span>
                    <span class="text-xl font-bold text-gray-700">外送</span>
                    <span class="text-sm text-gray-400 font-normal">Foodpanda</span>
                </button>
            </div>
        </div>
    </div>

    <!-- 3. 成功結帳畫面 (Success Screen) -->
    <div id="success-screen" class="fixed inset-0 bg-white flex flex-col items-center justify-center z-[200] hide text-center">
        <div class="p-10">
            <div id="success-icon" class="mb-6"></div>
            <h1 id="success-title" class="text-4xl font-bold text-gray-800 mb-2">標題</h1>
            <h2 id="success-title-en" class="text-2xl text-gray-400 mb-6">English Title</h2>
            <p id="success-subtitle" class="text-2xl font-medium mb-1">子標題</p>
            <p id="success-subtitle-en" class="text-xl mb-8">English Subtitle</p>
            <button onclick="goHome()" class="bg-gray-100 px-10 py-4 rounded-full text-gray-600 font-bold hover:bg-gray-200 transition text-lg shadow-sm">
                返回首頁<br><span class="text-sm font-normal">Back to Home</span>
            </button>
        </div>
    </div>

    <!-- 主介面 (Main POS) -->
    <header class="bg-white border-b px-6 py-3 flex justify-between items-center">
        <div class="flex items-center space-x-4">
            <div id="header-mode-tag" class="text-white px-4 py-1 rounded text-center leading-tight">
                <div id="tag-zh" class="font-bold">載入中</div>
                <div id="tag-en" class="text-[10px]">Loading...</div>
            </div>
        </div>
        <div class="flex items-center space-x-6 text-sm">
            <div class="text-right">
                <p class="text-gray-400">當前帳號 (ID): <span id="display-uid" class="font-bold text-gray-800">-</span></p>
                <p class="text-gray-400">擁有的錢 (Balance): <span id="display-balance" class="font-bold text-green-600">-</span></p>
            </div>
            <div class="w-10 h-10 bg-gray-200 rounded-full flex items-center justify-center font-bold">👤</div>
        </div>
    </header>

    <main class="flex-1 flex overflow-hidden">
        <!-- 側邊欄 (Sidebar) -->
        <nav class="w-40 bg-gray-100 border-r flex flex-col">
            <div onclick="switchCategory('eat')" id="nav-eat" class="p-4 text-center cursor-pointer sidebar-active">
                <div class="font-bold text-lg">好吃的</div>
                <div class="text-xs opacity-70">Main Course</div>
            </div>
            <div onclick="switchCategory('cool')" id="nav-cool" class="p-4 text-center cursor-pointer text-gray-500">
                <div class="font-bold text-lg">喝涼的</div>
                <div class="text-xs opacity-70">Cold Drinks</div>
            </div>
            <div onclick="switchCategory('great')" id="nav-great" class="p-4 text-center cursor-pointer text-gray-500">
                <div class="font-bold text-lg">很讚的</div>
                <div class="text-xs opacity-70">Side Dishes</div>
            </div>
        </nav>

        <!-- 菜單區 (Menu) -->
        <div class="flex-1 p-6 overflow-y-auto bg-gray-50">
            <div id="menu-container" class="menu-grid"></div>
        </div>

        <!-- 購物車 (Cart) -->
        <aside class="w-96 bg-white border-l flex flex-col shadow-lg">
            <div class="p-4 border-b flex justify-between items-center">
                <div>
                    <h2 class="font-bold text-base">您的餐點 Your Order</h2>
                    <p class="text-xs text-gray-400">(<span id="cart-count">0</span>/6 Items)</p>
                </div>
                <button onclick="clearCart()" class="text-sm text-red-500 hover:underline">清空 Clear</button>
            </div>
            
            <div id="cart-items" class="flex-1 overflow-y-auto p-4 space-y-4"></div>

            <div class="p-6 border-t bg-gray-50">
                <div class="flex justify-between mb-2 text-sm">
                    <span class="text-gray-500">合計項目 (Total Items):</span>
                    <span id="total-qty" class="font-bold">0</span>
                </div>
                <div class="flex justify-between items-end mb-6">
                    <span class="text-gray-500 text-sm">總計金額 (Total Cost):</span>
                    <span class="text-3xl font-bold" id="total-price-display">$<span id="total-price">0</span></span>
                </div>
                <button onclick="checkout()" id="checkout-btn" disabled class="w-full text-white font-bold py-4 rounded-xl shadow-lg transition text-xl disabled:bg-gray-300 disabled:cursor-not-allowed">
                    確認結帳 Confirm
                </button>
                <p id="balance-warning" class="text-red-500 text-xs text-center mt-2 hide">
                    金額超過您的餘額！<br>Insufficient balance!
                </p>
            </div>
        </aside>
    </main>

    <script>
        let currentUser = "";
        let balance = 0;
        let cart = [];
        let orderMode = "";

        const menuData = {
            eat: [
                { id: 1, name: "湯的米苔目", nameEn: "Rice Noodles (Soup)", price: 55 },
                { id: 2, name: "乾的米苔目", nameEn: "Rice Noodles (Dry)", price: 55 },
                { id: 3, name: "滷蛋", nameEn: "Braised Egg", price: 15 },
                { id: 4, name: "瑞源魚丸", nameEn: "Ruiyuan Fish Ball", price: 10 },
                { id: 5, name: "魚丸湯", nameEn: "Fish Ball Soup", price: 30 },
                { id: 6, name: "柴魚伴手禮", nameEn: "Bonito Gift Set", price: 90 },
                { id: 7, name: "米苔目在家煮", nameEn: "Noodle DIY Kit", price: 300 },
                { id: 8, name: "辣椒醬盒", nameEn: "Chili Sauce Box", price: 250 }
            ],
            cool: [
                { id: 9, name: "鹿野紅烏龍", nameEn: "Luye Red Oolong", price: 35 },
                { id: 10, name: "初鹿鮮奶茶", price: 40, nameEn: "Chulu Milk Tea" },
                { id: 11, name: "神社紅茶", price: 25, nameEn: "Shrine Black Tea" },
                { id: 12, name: "台東樓冬瓜茶", price: 25, nameEn: "Winter Melon Tea" },
                { id: 13, name: "冬瓜愛玉檸檬", price: 40, nameEn: "Lemon Aiyu Tea" },
                { id: 14, name: "洛神花茶", price: 55, nameEn: "Roselle Tea" },
                { id: 15, name: "愛玉蜂蜜檸檬", price: 55, nameEn: "Honey Lemon Aiyu" },
                { id: 16, name: "嘉蘭天空桑椹", price: 55, nameEn: "Mulberry Juice" }
            ],
            great: [
                { id: 17, name: "乾煸四季豆", nameEn: "Fried Green Beans", price: 180 },
                { id: 18, name: "香酥鬼頭刀", nameEn: "Fried Mahi-mahi", price: 180 },
                { id: 19, name: "松金全豬", nameEn: "Braised Pork Platter", price: 180 },
                { id: 20, name: "太和拼盤", nameEn: "Taihe Combo Plate", price: 120 },
                { id: 21, name: "香酥米血", nameEn: "Fried Rice Cake", price: 120 },
                { id: 22, name: "香滷米血", nameEn: "Braised Rice Cake", price: 120 },
                { id: 23, name: "涼拌黑木耳", nameEn: "Black Fungus Salad", price: 60 },
                { id: 24, name: "涼拌龍鬚菜", nameEn: "Chayote Shoot Salad", price: 60 },
                { id: 25, name: "涼拌豆皮", nameEn: "Bean Curd Salad", price: 60 }
            ]
        };

        function handleLogin() {
            const input = document.getElementById('user-account').value.trim();
            const error = document.getElementById('login-error');
            
            if (input.toLowerCase() === 'teacher') {
                currentUser = 'Teacher';
                balance = 1000;
                showModeSelection();
            } else if (input.length === 8 && !isNaN(input)) {
                currentUser = input;
                let sumOfLastFive = 0;
                for (let i = 3; i < 8; i++) {
                    sumOfLastFive += parseInt(input[i]);
                }
                let calculated = (1 + 1 + 3 + sumOfLastFive) * 15 + 100;
                balance = Math.min(calculated, 1000); 
                showModeSelection();
            } else {
                error.classList.remove('hide');
            }
        }

        function showModeSelection() {
            document.getElementById('login-screen').classList.add('hide');
            document.getElementById('mode-screen').classList.remove('hide');
        }

        function setOrderMode(mode) {
            orderMode = mode;
            document.getElementById('mode-screen').classList.add('hide');
            
            const tagZh = document.getElementById('tag-zh');
            const tagEn = document.getElementById('tag-en');
            const tagBox = document.getElementById('header-mode-tag');
            const checkoutBtn = document.getElementById('checkout-btn');
            const totalDisplay = document.getElementById('total-price-display');

            if (mode === 'dine-in') {
                tagZh.innerText = "內用模式";
                tagEn.innerText = "Dine-in Mode";
                tagBox.className = "text-white px-4 py-1 rounded text-center leading-tight bg-orange-500 font-bold";
                checkoutBtn.className = "w-full text-white font-bold py-4 rounded-xl shadow-lg transition text-xl disabled:bg-gray-300 disabled:cursor-not-allowed bg-orange-500 hover:bg-orange-600";
                totalDisplay.className = "text-3xl font-bold text-orange-600";
            } else {
                tagZh.innerText = "外送服務";
                tagEn.innerText = "Foodpanda Delivery";
                tagBox.className = "text-white px-4 py-1 rounded text-center leading-tight bg-pink-600 font-bold";
                checkoutBtn.className = "w-full text-white font-bold py-4 rounded-xl shadow-lg transition text-xl disabled:bg-gray-300 disabled:cursor-not-allowed bg-pink-600 hover:bg-pink-700";
                totalDisplay.className = "text-3xl font-bold text-pink-600";
            }

            document.getElementById('display-uid').innerText = currentUser;
            document.getElementById('display-balance').innerText = `$${balance}`;
            switchCategory('eat');
        }

        function switchCategory(cat) {
            const navs = ['eat', 'cool', 'great'];
            const activeColor = orderMode === 'dine-in' ? '#f97316' : '#db2777'; 
            
            navs.forEach(n => {
                const el = document.getElementById(`nav-${n}`);
                el.classList.toggle('sidebar-active', n === cat);
                el.classList.toggle('text-gray-500', n !== cat);
                if (n === cat) el.style.color = activeColor;
                else el.style.color = '';
            });

            const container = document.getElementById('menu-container');
            container.innerHTML = '';
            
            menuData[cat].forEach(item => {
                const card = document.createElement('div');
                const hoverBorder = orderMode === 'dine-in' ? 'hover:border-orange-500' : 'hover:border-pink-500';
                const priceColor = orderMode === 'dine-in' ? 'text-orange-600' : 'text-pink-600';
                
                card.className = `bg-white p-4 rounded-xl card-shadow cursor-pointer border-2 border-transparent ${hoverBorder} menu-item-card`;
                card.onclick = () => updateQty(item.id, 1, item);
                card.innerHTML = `
                    <div class="font-bold text-gray-800 text-sm mb-1">${item.name}</div>
                    <div class="text-[10px] text-gray-400 mb-2 leading-tight uppercase font-medium">${item.nameEn}</div>
                    <div class="${priceColor} font-bold">$${item.price}</div>
                `;
                container.appendChild(card);
            });
        }

        function updateQty(id, delta, itemData = null) {
            const existing = cart.find(i => i.id === id);
            if (delta > 0) {
                if (existing) existing.qty += delta;
                else {
                    if (cart.length >= 6) { alert("訂單上限為 6 種餐點！(Max 6 types)"); return; }
                    if (itemData) cart.push({ ...itemData, qty: delta });
                }
            } else {
                if (existing) {
                    existing.qty += delta;
                    if (existing.qty <= 0) cart = cart.filter(i => i.id !== id);
                }
            }
            renderCart();
        }

        function clearCart() { cart = []; renderCart(); }

        function renderCart() {
            const container = document.getElementById('cart-items');
            container.innerHTML = '';
            let total = 0, qtyTotal = 0;
            const themeColor = orderMode === 'dine-in' ? '#f97316' : '#db2777';
            const textTheme = orderMode === 'dine-in' ? 'text-orange-600' : 'text-pink-600';

            cart.forEach(item => {
                total += item.price * item.qty;
                qtyTotal += item.qty;
                const div = document.createElement('div');
                div.className = "flex justify-between items-center bg-gray-50 p-3 rounded-lg border";
                div.innerHTML = `
                    <div class="flex-1">
                        <div class="font-bold text-xs text-gray-800">${item.name}</div>
                        <div class="text-[10px] text-gray-400 leading-tight mb-1">${item.nameEn}</div>
                        <div class="text-[11px] ${textTheme} font-medium">$${item.price} / PC</div>
                    </div>
                    <div class="flex items-center space-x-3">
                        <div class="flex items-center bg-white rounded-full border shadow-sm px-1 py-1 space-x-2">
                            <button onclick="updateQty(${item.id}, -1)" class="qty-btn" style="color: ${themeColor}; border-color: ${themeColor}">－</button>
                            <span class="font-bold text-gray-700 min-w-[20px] text-center">${item.qty}</span>
                            <button onclick="updateQty(${item.id}, 1)" class="qty-btn" style="color: ${themeColor}; border-color: ${themeColor}">＋</button>
                        </div>
                        <div class="text-right min-w-[50px] font-bold ${textTheme}">$${item.price * item.qty}</div>
                    </div>
                `;
                container.appendChild(div);
            });

            document.getElementById('cart-count').innerText = cart.length;
            document.getElementById('total-qty').innerText = qtyTotal;
            document.getElementById('total-price').innerText = total;

            const checkoutBtn = document.getElementById('checkout-btn');
            const warning = document.getElementById('balance-warning');
            if (total > balance) {
                checkoutBtn.disabled = true;
                warning.classList.remove('hide');
            } else {
                checkoutBtn.disabled = (cart.length === 0);
                warning.classList.add('hide');
            }
        }

        function checkout() {
            const iconBox = document.getElementById('success-icon');
            const title = document.getElementById('success-title');
            const titleEn = document.getElementById('success-title-en');
            const subtitle = document.getElementById('success-subtitle');
            const subtitleEn = document.getElementById('success-subtitle-en');
            
            if (orderMode === 'dine-in') {
                iconBox.innerHTML = `
                    <div class="inline-block p-4 rounded-full bg-orange-100 text-orange-600">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-20 w-20" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                    </div>`;
                title.innerText = "請稍等片刻";
                titleEn.innerText = "Just a moment, please";
                subtitle.innerText = "您的餐點正在製作...";
                subtitleEn.innerText = "Your meal is being prepared...";
                subtitle.className = "text-orange-600 text-2xl font-medium mb-1";
                subtitleEn.className = "text-orange-400 text-xl mb-8";
            } else {
                iconBox.innerHTML = `
                    <div class="inline-block p-4 rounded-full bg-pink-100 text-pink-600">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-20 w-20" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8" />
                        </svg>
                    </div>`;
                title.innerText = "Foodpanda 正在送您的外送";
                titleEn.innerText = "Foodpanda is delivering your order";
                subtitle.innerText = "請稍後...";
                subtitleEn.innerText = "Please wait a moment...";
                subtitle.className = "text-pink-600 text-2xl font-medium mb-1";
                subtitleEn.className = "text-pink-400 text-xl mb-8";
            }
            
            document.getElementById('success-screen').classList.remove('hide');
        }

        function goHome() {
            window.location.href = "https://sites.google.com/zlsh.tp.edu.tw/80132113/%E9%A6%96%E9%A0%81";
        }

        window.onload = () => {
            document.getElementById('login-screen').classList.remove('hide');
            document.getElementById('mode-screen').classList.add('hide');
            document.getElementById('success-screen').classList.add('hide');
        };
    </script>
</body>
</html>
