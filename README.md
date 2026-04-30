<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>🧪 戀愛實驗室：永恆之約 🐰🐻‍❄️</title>
    <style>
        :root { 
            --p: #ff69b4; --d: #ff1493; 
            --spring: #fff0f5; --summer: #e0f7fa; --autumn: #fff3e0; --winter: #eceff1;
            --fever-top: #1a1a2e; --fever-bot: #ffb6c1;
        }
        
        body { 
            margin: 0; height: 100vh; overflow: hidden; font-family: "Microsoft JhengHei", sans-serif; 
            display: flex; align-items: center; justify-content: center; 
            transition: background 2s ease-in-out; touch-action: none;
            background: var(--spring);
        }

        body.fever-sky { background: linear-gradient(to bottom, var(--fever-top), var(--fever-bot)) !important; }

        canvas { position: fixed; top: 0; left: 0; pointer-events: none; z-index: 1; }
        
        .card { 
    background: rgba(255, 255, 255, 0.92); backdrop-filter: blur(10px); 
    padding: 20px; border-radius: 30px; border: 3px solid #ffb6c1; 
    box-shadow: 0 10px 30px rgba(255, 105, 180, 0.3); text-align: center; 
    /* 修改這行：讓寬度固定，並設定一個最小高度 */
    width: 380px; max-width: 90%; min-height: 520px; 
    z-index: 10; position: relative;
    /* 加上這行：讓卡片內容平均分配，才不會因為字變多往下擠 */
    display: flex; flex-direction: column; justify-content: space-between;
}


        #notify { 
            position: absolute; top: -50px; left: 50%; transform: translateX(-50%); 
            background: var(--d); color: white; padding: 8px 15px; 
            border-radius: 20px; font-weight: bold; opacity: 0; transition: 0.3s; 
            z-index: 1000; font-size: 12px;
        }
        .show-notify { top: 10px !important; opacity: 1 !important; }

        #rank-text { font-weight: bold; color: var(--d); font-size: 18px; margin-bottom: 5px; }
        ##counter { 
    font-size: 48px; font-weight: 900; color: var(--d); 
    text-shadow: 2px 2px #ffd1dc; margin: 10px 0; 
    /* 加上這行：鎖定數字區域高度 */
    min-height: 60px;
}

        
        .stat-bar { font-size: 12px; display: flex; justify-content: space-around; margin-bottom: 15px; color: #666; }
        
        .pet-container { 
            margin: 15px 0; padding: 20px; background: rgba(255,255,255,0.7); 
            border-radius: 25px; border: 2px dashed var(--p); position: relative; cursor: pointer;
        }
        #pet-display { font-size: 70px; user-select: none; transition: transform 0.2s; }
        #pet-display img { width: 120px; border-radius: 15px; pointer-events: none; } 
        #pet-display:active { transform: scale(0.9); }
        ##pet-msg { 
    font-size: 14px; color: var(--d); font-weight: bold; margin-top: 10px; 
    /* 把原本的 45px 改成 65px */
    min-height: 65px; 
    line-height: 1.5; 
}


        #quick-feed { 
            position: absolute; top: 0; left: 0; width: 100%; height: 100%; 
            background: rgba(255,255,255,0.98); border-radius: 23px; 
            display: none; grid-template-columns: repeat(3, 1fr); align-items: center; justify-items: center; z-index: 100; 
        }
        .feed-icon { font-size: 32px; cursor: pointer; transition: 0.2s; }               
        .close-feed { 
            grid-column: span 3; color: #ff4d4d; font-size: 16px; 
            margin-top: 10px; cursor: pointer; border: 1px solid #ff4d4d; 
            padding: 5px 20px; border-radius: 15px; background: #fff0f0;
        }
        

        .heart-btn { 
            width: 90px; height: 90px; background: radial-gradient(circle, #ff8cc6, var(--p)); 
            border: none; border-radius: 50%; font-size: 45px; color: white; 
            box-shadow: 0 5px 15px rgba(255, 105, 180, 0.4); cursor: pointer; margin: 10px 0;
        }
        
        .action-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-top: 15px; }
        .small-btn { 
            background: #ffc0cb; color: white; border: none; padding: 10px 2px; 
            border-radius: 15px; font-size: 12px; font-weight: bold; cursor: pointer;
        }

        .modal { 
            display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); 
            width: 85%; background: white; border-radius: 25px; padding: 25px; 
            box-shadow: 0 0 50px rgba(0,0,0,0.3); z-index: 2000; border: 3px solid var(--p); 
            max-height: 70vh; overflow-y: auto;
        }
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 12px 0; border-bottom: 1px solid #eee; }
        .buy-btn { background: var(--p); color: white; border: none; padding: 8px 15px; border-radius: 10px; cursor: pointer; }

        .click-heart { position: fixed; pointer-events: none; animation: flyUp 0.8s ease-out forwards; font-size: 28px; z-index: 100; }
        @keyframes flyUp { 0% { opacity: 1; transform: translateY(0) scale(1); } 100% { opacity: 0; transform: translateY(-180px) scale(1.6); } }
        
        .fever-mode { animation: fever-glow 0.5s infinite alternate; }
        @keyframes fever-glow { from { box-shadow: 0 0 5px #ff0000; } to { box-shadow: 0 0 25px #ff69b4; } }
    </style>
</head>
<body id="mainBody">

<canvas id="bgCanvas"></canvas>

<div class="card" id="gameCard">
    <div id="notify">愛你♡</div>
    <div id="rank-text">春天：初次見面🧪</div>
    <div id="counter">0</div>
    
    <div class="stat-bar">
        <span>🐻‍❄️ 好感 <span id="b-aff">0</span></span>
        <span>🐰 好感 <span id="r-aff">0</span></span>
    </div>
    
    <div class="pet-container" id="petBox">
        <div id="pet-display">🐻‍❄️</div>
        <div id="pet-msg">點擊換人，長按餵食 💕</div>
        <div id="quick-feed"></div>
    </div>

    <button class="heart-btn" id="clickBtn">❤</button>

    <div class="action-grid">
        <button class="small-btn" onclick="openModal('shop')">🛒 商店</button>
        <button class="small-btn" onclick="openModal('upg')">🧪 升級</button>
        <button class="small-btn" onclick="openModal('ach')">🏆 成就</button>
        <button class="small-btn" onclick="secretCode()">🔑 暗號</button>
        <button class="small-btn" onclick="resetGame()" style="background:#bbb">🗑 重置</button>
    </div>
</div>

<div id="shopModal" class="modal"><h3 style="color:var(--d)">🍱 食物商店</h3><div id="shop-list"></div><button class="small-btn" style="width:100%" onclick="closeModal()">關閉</button></div>
<div id="upgModal" class="modal"><h3 style="color:var(--d)">🔬 實驗升級 (離線累積)</h3><div id="upg-list"></div><button class="small-btn" style="width:100%" onclick="closeModal()">關閉</button></div>
<div id="achModal" class="modal"><h3 style="color:var(--d)">🏆 戀愛成就</h3><div id="ach-list"></div><button class="small-btn" style="width:100%" onclick="closeModal()">關閉</button></div>

<script>
    let count = 0, autoP = 0, clickP = 1, fever = false;
    let bAff = 0, rAff = 0, bSat = 0, rSat = 0;
    let currentPet = 'bear', bag = {}, unlockedAch = [], foodHistory = {};
    let forceCount = 0;

    // --- 1. 食物資料庫 (包含專屬對話) ---
    const foods = {
        'chocolate': { n: '濃情巧克力', p: 500, r: 2000, b: 2000, icon: '🍫', talk: { r: "這巧克力好甜，但我更想吃子恩親手做的～🍬", b: "巧克力雖然苦，但子恩在身邊就是甜的。🐻‍❄️" } },
        'passion': { n: '百香果', p: 100, r: 1000, b: 200, icon: '🍹', talk: { r: '最愛百香果了！酸酸甜甜像博罄♡', b: '子子喜歡我就陪妳吃～' } },
        'strawberry': { n: '草莓', p: 300, r: 4000, b: 800, icon: '🍓', talk: { r: '是草莓！博罄我也要一個在脖子上！💋', b: '這個草莓沒妳甜～...不是脖子那個///' } },
        'apple': { n: '蘋果汁', p: 150, r: 500, b: 3500, icon: '🍎', talk: { r: '我也想喝一口～', b: '博罄最愛的蘋果汁！好喝！' } },
        'ramen': { n: '拉麵', p: 1000, r: 800, b: 8000, icon: '🍜', talk: { r: '分我一口湯？🍜', b: '吃飽了才有體力愛兔兔！' } },
        'cake': { n: '超大蛋糕', p: 400, r: 1000, b: 3000, icon: '🍰', hiCal: true, talk: { r: '這熱量...你想讓兔兔變肥兔嗎？', b: '好甜喔，跟妳一樣！' } },
        'milk': { n: '全脂鮮奶超純cosco鮮奶', p: 300, r: 2000, b: -1000, icon: '🥛', dairy: true, talk: { r: '兔兔最愛喝牛奶了！', b: '博罄乳糖不耐症...肚子痛痛...🚽' } },
        'beer': { n: '酒心巧克力', p: 500, r: 2000, b: -1500, icon: '🍺', ferment: true, talk: { r: '兔兔想醉在博罄懷裡～幹博罄(⁠つ⁠✧⁠ω⁠✧⁠)⁠つ', b: '博罄不喝酒的...暈暈的...😵' } },
        'fish': { n: '討厭的魚', p: 50, r: -5000, b: -5000, icon: '🐟', talk: { r: '拿走！兔兔不吃這個！💢', b: '博罄吃魚吃不飽 嗚嗚(⁠´⁠；⁠ω⁠；⁠｀⁠)' } }
    };

    // --- 2. 寵物對話庫 ---
    const petQuotes = {
        bear: {
            switch: ["換博罄來陪妳！🐻‍❄️", "博罄一直在這裡喔～", "博罄想妳了！", "現在是博罄時間！"],
            click: ["兔兔手會痠嗎？我幫妳揉揉♡", "博罄最喜歡妳了～", "每一點都是愛喔！", "為了妳，博罄可以努力很久！"],
            feed: ["真好吃，謝謝兔兔餵我！🍱", "只要是妳給的，我都喜歡。", "好幸福喔，這是愛的味道嗎？", "吃飽了更有力氣愛妳！"],
            sweet: ["能遇見妳，是博罄最幸運的事。", "我們要一直一直在一起喔。", "子恩，妳是我的唯一化學反應。"]
        },
        rabbit: {
            switch: ["子恩來了！🐰", "現在換兔兔抱抱！", "兔兔想吃甜的了～", "你看，兔兔在這裡！"],
            click: ["博罄加油！", "感受到滿滿的愛了！✨", "子子也愛你喔！", "子恩兔兔心跳好快喔！"],
            feed: ["哇！是好吃的！謝謝你💕", "兔兔子子最喜歡被你餵食了～", "好甜喔！跟博罄一樣甜！", "再多給子子兔兔一點點愛！"],
            sweet: ["博罄是子恩最重要的人！", "博罄不管去哪裡，都要帶著子恩喔。", "子恩想永遠黏在博罄的身邊。"]
        }
    };

    const upgrades = [
        { id: 'auto1', n: '小型設備 (+10/s)', c: 500, f: () => autoP += 10 },
        { id: 'auto2', n: '大型反應爐 (+100/s)', c: 8000, f: () => autoP += 100 },
        { id: 'click1', n: '點擊強化 (+20/點)', c: 1000, f: () => clickP += 20 }
    ];

    const seasons = [
        { c: 0, n: "春天", bg: "#fff0f5", i: "🌸" },
        { c: 10000, n: "夏天", bg: "#e0f7fa", i: "🌻" },
        { c: 50000, n: "秋天", bg: "#fff3e0", i: "🍁" },
        { c: 200000, n: "冬天", bg: "#eceff1", i: "❄️" }
    ];
    
    const achs = [
        { c: 520, n: "💌 初次心動" }, 
        { c: 5200, n: "✨ 星空約會" }, 
        { c: 20000, n: "🍁 秋日物語" }, 
        { c: 100000, n: "❄️ 冬季初雪" }, 
        { c: 520000, n: "💍 執子之手" }, 
        { c: 1000000, n: "♾️ 永恆維度" }
    ];
    
    window.onload = function() {
        count = parseFloat(localStorage.getItem("v17_cnt") || 0);
        autoP = parseFloat(localStorage.getItem("v17_ap") || 0);
        clickP = parseFloat(localStorage.getItem("v17_cp") || 1);
        bAff = parseFloat(localStorage.getItem("v17_baf") || 0);
        rAff = parseFloat(localStorage.getItem("v17_raf") || 0);
        bag = JSON.parse(localStorage.getItem("v17_bag") || "{}");
        
        checkOffline();
        setupInteractions();
        setInterval(() => { count += autoP/10; updateUI(); }, 100);
        updateUI(); animate();
    };

    function checkOffline() {
        const last = localStorage.getItem("v17_time");
        if(last && autoP > 0) {
            const diff = (Date.now() - parseInt(last)) / 1000;
            const offline = Math.floor(diff * autoP);
            if(offline > 10) { count += offline; notify(`離線獲得了 ${offline} 點愛意！`); }
        }
    }

    function setupInteractions() {
        const petBox = document.getElementById('petBox');
        let longPressTimer;
        petBox.onpointerdown = (e) => { longPressTimer = setTimeout(() => { showQuickFeed(); longPressTimer = null; }, 600); };
        petBox.onpointerup = () => { if(longPressTimer) { clearTimeout(longPressTimer); switchPet(); } };

        const clickBtn = document.getElementById("clickBtn");
        let pressTimer;
        clickBtn.onpointerdown = (e) => { e.preventDefault(); fireLove(e.clientX, e.clientY); pressTimer = setInterval(() => fireLove(window.innerWidth/2, window.innerHeight/2 + 50), 200); };
        window.onpointerup = () => clearInterval(pressTimer);
    }

    function switchPet() {
        if(document.getElementById('quick-feed').style.display === 'grid') return;
        currentPet = (currentPet === 'bear' ? 'rabbit' : 'bear');
        
        // 這裡預留了圖片接口，如果妳之後想換圖片，把Emoji換成 <img src="..."> 即可
        document.getElementById('pet-display').innerHTML = (currentPet === 'bear' ? '<img src="![image](./images/bear.png)">' : '<img src="![image](./images/rabbit.png)">');

        
        forceCount = 0;
        updatePetTalk('switch');
        updateUI();
    }

    function fireLove(x, y) {
        let mult = 1 + (currentPet==='bear'?bAff:rAff)/2000;
        count += clickP * mult * (fever?5:1);
        spawnHeart(x, y);
        if(Math.random() < 0.05) updatePetTalk('click');
        updateUI();
    }

    function updatePetTalk(type) {
        const aff = currentPet === 'bear' ? bAff : rAff;
        const pool = currentPet === 'bear' ? petQuotes.bear : petQuotes.rabbit;
        let list = [...pool[type]];
        if (aff > 1000 && Math.random() < 0.3) list = list.concat(pool.sweet);
        document.getElementById('pet-msg').innerText = list[Math.floor(Math.random() * list.length)];
    }

    function showQuickFeed() {
        const menu = document.getElementById('quick-feed');
        let h = '';
        for(let k in bag) if(bag[k] > 0) h += `<div class="feed-icon" onclick="handleFeed('${k}')">${foods[k].icon}</div>`;
        menu.innerHTML = h || '<div style="grid-column:span 3; font-size:12px">沒食物耶...</div>';
        menu.style.display = 'grid';
        setTimeout(() => menu.style.display = 'none', 4000);
    }

    // --- 3. 戲精餵食邏輯 ---
    function handleFeed(k) {
        const f = foods[k];
        const isBear = (currentPet === 'bear');
        
        // 飽食度檢查
        const sat = isBear ? bSat : rSat;
        if (sat > 100) {
            document.getElementById('pet-msg').innerText = isBear ? "博罄真的吃不下了...嗝！🐻‍❄️" : "兔兔肚子圓滾滾了，再吃要變氣球了！🎈";
            return;
        }

        // 兔兔傲嬌劇場
        if (!isBear && f.hiCal) {
            if (forceCount === 0) {
                document.getElementById('pet-msg').innerText = "這熱量太高了啦！妳是想把兔兔餵成小豬嗎？🐰";
                forceCount++; return; 
            } else if (forceCount < 2) {
                if (Math.random() < 0.4) {
                    document.getElementById('pet-msg').innerText = "嗚...你那個眼神...好啦，我就只吃這一口喔！(咬) 💕";
                    executeFeed(k, 3);
                    forceCount = 0; return;
                } else {
                    document.getElementById('pet-msg').innerText = "我不聽我不聽！博罄快來救我，這個人要害我變胖！🐰";
                    forceCount++; return;
                }
            } else {
                count = Math.max(0, count - 1314);
                document.getElementById('pet-msg').innerText = "哼！真的生氣了！不理你了，我要去跑步機！💢";
                notify("兔兔真的生氣了...🥺");
                forceCount = 0; return;
            }
        }

        // 博罄乳糖不耐症劇場
        if (isBear && f.dairy) {
            document.getElementById('pet-msg').innerText = "博罄：雖然我有乳糖不耐症，但子恩給的我都會喝完...🚽 (肚子咕嚕聲)";
            executeFeed(k, 0.5); 
            return;
        }

        // 正常餵食
        executeFeed(k, 1);
    }

    function executeFeed(k, m) {
        const f = foods[k];
        bag[k]--; 
        foodHistory[k] = (foodHistory[k] || 0) + 1;
        let fatigue = Math.max(0.3, Math.pow(0.8, foodHistory[k]-1));
        
        let add = (currentPet === 'bear' ? f.b : f.r) * m * fatigue;
        count += add;

        if(currentPet === 'bear') { bAff += 25; bSat += 30; } 
        else { rAff += 25; rSat += 30; }

        if (f.talk) {
            document.getElementById('pet-msg').innerText = currentPet === 'bear' ? f.talk.b : f.talk.r;
        } else {
            updatePetTalk('feed');
        }

        if(!fever && add > 3000) startFever(); 
        document.getElementById('quick-feed').style.display = 'none';
        notify(`餵食成功！愛意 +${Math.floor(add)}`);
        updateUI();
    }
        
    // --- 4. 暗號與其他功能 ---
    function secretCode() {
        let c = prompt("輸入暗號");
        if(c === "愛你") { count += 5200; notify("獲得愛意！"); }
        if(c === "博罄") { bAff += 500; notify("博罄好感暴增！"); }
        if(c === "兔兔") { rAff += 500; notify("兔兔好感暴增！"); }
        if(c === "子恩") { startFever(); notify("子恩感受到愛了！🌌"); }
        if(c === "爆開") { count *= 2; notify("實驗室大爆炸！愛意翻倍！"); }
        updateUI();
    }

    function startFever() {
        if(fever) return; fever = true;
        document.body.classList.add('fever-sky');
        document.getElementById('gameCard').classList.add('fever-mode');
        notify("🌌 FEVER！流星雨來了！愛意5倍！");
        setTimeout(() => { fever = false; document.body.classList.remove('fever-sky'); document.getElementById('gameCard').classList.remove('fever-mode'); }, 12000);
    }

    function updateUI() {
        document.getElementById('counter').innerText = Math.floor(count);
        document.getElementById('b-aff').innerText = Math.floor(bAff);
        document.getElementById('r-aff').innerText = Math.floor(rAff);
        let s = seasons[0]; seasons.forEach(x => { if(count >= x.c) s = x; });
        if(!fever) document.body.style.backgroundColor = s.bg;
        document.getElementById('rank-text').innerText = `${s.n}：${getRank(count)}`;
        bSat = Math.max(0, bSat - 0.05); rSat = Math.max(0, rSat - 0.05);
        localStorage.setItem("v17_time", Date.now()); save();
    }

    function getRank(c) {
        if(c < 520) return "化學老師🧪";
        if(c < 1314) return "熊熊男友🐻‍❄️";
        if(c < 10000) return "老公結酚💍";
        return "穿越維度之戀♾️";
    }

    function openModal(id) {
        const l = document.getElementById(id + '-list'); if(!l) return; l.innerHTML = '';
        if(id === 'shop') { for(let k in foods) l.innerHTML += `<div class="item-row"><div><b>${foods[k].icon} ${foods[k].n}</b><br><small>$${foods[k].p}</small></div><button class="buy-btn" onclick="buyFood('${k}')">購買</button></div>`; }
        else if(id === 'upg') { upgrades.forEach(u => l.innerHTML += `<div class="item-row"><div><b>${u.n}</b><br><small>$${u.c}</small></div><button class="buy-btn" onclick="buyUpg('${u.id}')">升級</button></div>`); }
        else if(id === 'ach') { achs.forEach(a => { let done = count >= a.c; l.innerHTML += `<div class="item-row" style="opacity:${done?1:0.5}"><div><b>${a.n}</b><br><small>${done?'已達成':'需要 '+a.c}</small></div><div>${done?'✅':'🔒'}</div></div>`; }); }
        document.getElementById(id + 'Modal').style.display = 'block';
    }

    window.buyFood = (k) => { if(count>=foods[k].p){ count-=foods[k].p; bag[k]=(bag[k]||0)+1; notify("購買成功！"); updateUI(); openModal('shop'); } };
    window.buyUpg = (id) => { const u = upgrades.find(x=>x.id===id); if(count>=u.c){ count-=u.c; u.f(); u.c=Math.floor(u.c*1.8); notify("升級成功！"); updateUI(); openModal('upg'); } };
    function closeModal() { document.querySelectorAll('.modal').forEach(m => m.style.display='none'); }
    function save() {
        localStorage.setItem("v17_cnt", count); localStorage.setItem("v17_ap", autoP);
        localStorage.setItem("v17_cp", clickP); localStorage.setItem("v17_baf", bAff);
        localStorage.setItem("v17_raf", rAff); localStorage.setItem("v17_bag", JSON.stringify(bag));
    }
    function notify(t) { const b = document.getElementById("notify"); b.innerText=t; b.classList.add("show-notify"); setTimeout(()=>b.classList.remove("show-notify"), 2000); }
    function spawnHeart(x, y) {
        const h = document.createElement("div"); h.className = "click-heart"; h.innerText = fever ? "💋" : "💖";
        h.style.left = (x || window.innerWidth/2) + "px"; h.style.top = (y || window.innerHeight/2) + "px";
        document.body.appendChild(h); setTimeout(() => h.remove(), 800);
    }
    function resetGame() { if(confirm("確定重置嗎？")) { localStorage.clear(); location.reload(); } }

    const canvas = document.getElementById("bgCanvas"); const ctx = canvas.getContext("2d");
    let pts = [];
    function resize() { canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
    window.onresize = resize; resize();
    function animate() {
        ctx.clearRect(0,0,canvas.width,canvas.height);
        if(fever) {
            if(pts.length < 20) pts.push({x:Math.random()*canvas.width, y:Math.random()*-canvas.height, len:Math.random()*80+50, spd:Math.random()*15+10});
            ctx.strokeStyle = "rgba(255,255,255,0.8)"; ctx.lineWidth = 2;
            pts.forEach((p,i) => { ctx.beginPath(); ctx.moveTo(p.x, p.y); ctx.lineTo(p.x - p.len/2, p.y + p.len); ctx.stroke(); p.x += p.spd; p.y += p.spd; if(p.y > canvas.height) pts.splice(i,1); });
        } else {
            let s = seasons[0]; seasons.forEach(x => {if(count >= x.c) s = x;});
            if(pts.length < 25) pts.push({x:Math.random()*canvas.width, y:-20, sp:Math.random()*2+1, sz:Math.random()*15+10});
            pts.forEach(p => { ctx.font = p.sz + "px serif"; ctx.fillText(s.i, p.x, p.y); p.y += p.sp; if(p.y > canvas.height) p.y = -20; });
        }
        requestAnimationFrame(animate);
    }
</script>
</body>
</html>

        