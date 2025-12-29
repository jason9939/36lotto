<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>3D 多重投注博彩平台 - 2025版</title>
    <style>
        :root {
            --gold: #ffcc00;
            --success: #22c55e;
            --danger: #ef4444;
            --bg-dark: #0f172a;
        }

        body {
            background: radial-gradient(circle at center, #1e293b, #000);
            color: #fff;
            font-family: 'Segoe UI', system-ui, sans-serif;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container { display: flex; gap: 20px; width: 1200px; height: 750px; }

        /* 核心控制面板 */
        .game-panel {
            flex: 2;
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(20px);
            border-radius: 24px;
            padding: 25px;
            border: 1px solid rgba(255,255,255,0.1);
            display: flex;
            flex-direction: column;
            box-shadow: 0 25px 50px rgba(0,0,0,0.5);
        }

        .balance-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        /* 3D 滚轮 */
        .roller-area {
            display: flex;
            gap: 20px;
            justify-content: center;
            perspective: 1500px;
            margin: 30px 0;
        }

        .ball {
            width: 130px;
            height: 130px;
            background: #000;
            border-radius: 50%;
            border: 6px solid var(--gold);
            overflow: hidden;
            position: relative;
            box-shadow: 0 0 40px rgba(255, 204, 0, 0.3), inset 0 0 30px #000;
        }

        .strip {
            position: absolute;
            width: 100%;
            transition: transform 10s cubic-bezier(0.1, 0, 0.05, 1);
        }

        .num { height: 130px; line-height: 130px; font-size: 85px; font-weight: 900; color: var(--gold); text-align: center; }

        /* 投注操作区 */
        .betting-grid {
            display: grid;
            grid-template-columns: 1fr 1fr auto;
            gap: 15px;
            background: rgba(0,0,0,0.3);
            padding: 15px;
            border-radius: 12px;
            margin-bottom: 15px;
        }

        input, select {
            background: #000;
            border: 1px solid #444;
            color: var(--gold);
            padding: 10px;
            border-radius: 8px;
            font-size: 16px;
        }

        .add-btn {
            background: #334155;
            color: white;
            border: none;
            padding: 0 20px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
        }

        /* 已选号码清单 */
        .current-bets {
            flex-grow: 1;
            background: rgba(0,0,0,0.2);
            border-radius: 12px;
            margin-bottom: 15px;
            overflow-y: auto;
            padding: 10px;
            border: 1px solid rgba(255,255,255,0.05);
        }

        .bet-tag {
            display: inline-flex;
            align-items: center;
            background: #1e293b;
            padding: 5px 12px;
            border-radius: 20px;
            margin: 5px;
            border: 1px solid var(--gold);
            font-size: 14px;
        }

        .remove-bet { color: var(--danger); margin-left: 8px; cursor: pointer; }

        .spin-btn {
            padding: 18px;
            font-size: 20px;
            font-weight: bold;
            background: linear-gradient(135deg, #fbbf24, #d97706);
            border: none;
            border-radius: 12px;
            cursor: pointer;
            color: #000;
        }

        .spin-btn:disabled { opacity: 0.3; }

        /* 右侧面板 */
        .side-panel {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .history-panel, .admin-panel {
            background: rgba(0,0,0,0.5);
            border-radius: 24px;
            padding: 20px;
            border: 1px solid rgba(255,255,255,0.1);
        }

        #log-list { height: 400px; overflow-y: auto; font-size: 12px; }
        .log-item { background: rgba(255,255,255,0.03); padding: 10px; border-radius: 8px; margin-bottom: 8px; }
        .win-log { border-left: 4px solid var(--success); background: rgba(34,197,94,0.1); }
    </style>
</head>
<body>

<div class="container">
    <div class="game-panel">
        <div class="balance-bar">
            <h2 style="margin:0; color:var(--gold)">PREMIUM 3D MULTI-BET</h2>
            <div style="font-size: 24px; color:var(--success)">余额: RM <span id="balance">1000.00</span></div>
        </div>

        <!-- 3D 滚球 -->
        <div class="roller-area">
            <div class="ball"><div id="strip1" class="strip"></div></div>
            <div class="ball"><div id="strip2" class="strip"></div></div>
        </div>

        <div id="status" style="text-align:center; color:#94a3b8; margin-bottom:10px;">赔率 1 : 33 | 请添加号码后点击开彩</div>

        <!-- 下注输入 -->
        <div class="betting-grid">
            <div>
                <label style="display:block; font-size:11px; color:#aaa">号码 (01-36)</label>
                <input type="number" id="input-num" min="1" max="36" style="width:100%">
            </div>
            <div>
                <label style="display:block; font-size:11px; color:#aaa">金额 (RM)</label>
                <input type="number" id="input-amt" min="1" style="width:100%">
            </div>
            <button class="add-btn" onclick="addToCart()">+ 添加号码</button>
        </div>

        <!-- 待开彩注单 -->
        <div class="current-bets" id="cart">
            <!-- 动态注单 -->
            <div style="color:#666; text-align:center; padding-top:20px">暂无投注，请在上方添加号码</div>
        </div>

        <button class="spin-btn" id="spin-btn" onclick="startDraw()">立即为所有注单开彩 (10秒)</button>
    </div>

    <div class="side-panel">
        <!-- 历史记录 -->
        <div class="history-panel">
            <h3 style="margin:0 0 10px 0; text-align:center">开彩记录</h3>
            <div id="log-list"></div>
        </div>

        <!-- 内部控制 -->
        <div class="admin-panel" style="border-color: var(--danger)">
            <h4 style="margin:0 0 10px 0; color:var(--danger)">内部控制面板</h4>
            <select id="control-mode" style="width:100%; margin-bottom:10px">
                <option value="random">随机开彩 (正常)</option>
                <option value="forced">指定开彩 (干预)</option>
            </select>
            <input type="number" id="forced-val" placeholder="指定中奖号" style="width:100%">
        </div>
    </div>
</div>

<script>
    let balance = 1000.00;
    let betCart = []; // 存储当前下注：{num, amt}
    let round = 1;
    const step = 130;

    function init() {
        const s1 = document.getElementById('strip1');
        const s2 = document.getElementById('strip2');
        let h1 = '', h2 = '';
        for(let i=0; i<20; i++){
            for(let j=0; j<=9; j++){
                h1 += `<div class="num">${j%4}</div>`;
                h2 += `<div class="num">${j}</div>`;
            }
        }
        s1.innerHTML = h1; s2.innerHTML = h2;
    }

    function addToCart() {
        const n = parseInt(document.getElementById('input-num').value);
        const a = parseFloat(document.getElementById('input-amt').value);

        if(isNaN(n) || n < 1 || n > 36) return alert("号码必须是 1-36");
        if(isNaN(a) || a < 1) return alert("金额无效");
        if(betCart.some(b => b.num === n)) return alert("该号码已在投注列表中");

        betCart.push({num: n, amt: a});
        renderCart();
        document.getElementById('input-num').value = '';
    }

    function removeFromCart(index) {
        betCart.splice(index, 1);
        renderCart();
    }

    function renderCart() {
        const cartDiv = document.getElementById('cart');
        if(betCart.length === 0) {
            cartDiv.innerHTML = `<div style="color:#666; text-align:center; padding-top:20px">暂无投注</div>`;
            return;
        }
        cartDiv.innerHTML = betCart.map((b, i) => `
            <div class="bet-tag">
                号: <b>${b.num.toString().padStart(2,'0')}</b> | RM ${b.amt}
                <span class="remove-bet" onclick="removeFromCart(${i})">×</span>
            </div>
        `).join('');
    }

    function startDraw() {
        if(betCart.length === 0) return alert("请先添加至少一个下注号码");
        
        const totalBet = betCart.reduce((sum, b) => sum + b.amt, 0);
        if(totalBet > balance) return alert("总投注 RM " + totalBet + " 超过余额！");

        // 扣款
        balance -= totalBet;
        document.getElementById('balance').innerText = balance.toFixed(2);
        
        const btn = document.getElementById('spin-btn');
        const status = document.getElementById('status');
        btn.disabled = true;
        status.innerText = "⚽ 开彩中，正在处理 " + betCart.length + " 个注单...";

        // 控制逻辑
        const mode = document.getElementById('control-mode').value;
        const fVal = parseInt(document.getElementById('forced-val').value);
        let finalRes = (mode === 'forced' && !isNaN(fVal)) ? fVal : Math.floor(Math.random() * 36) + 1;

        const v1 = Math.floor(finalRes / 10);
        const v2 = finalRes % 10;

        // 动画
        const s1 = document.getElementById('strip1');
        const s2 = document.getElementById('strip2');
        s1.style.transition = "none"; s2.style.transition = "none";
        s1.style.transform = `translateY(0)`; s2.style.transform = `translateY(0)`;
        s1.offsetHeight;

        s1.style.transition = "transform 10s cubic-bezier(0.1, 0, 0.05, 1)";
        s2.style.transition = "transform 10s cubic-bezier(0.1, 0, 0.05, 1)";
        s1.style.transform = `translateY(-${(150 + v1) * step}px)`;
        s2.style.transform = `translateY(-${(150 + v2) * step}px)`;

        setTimeout(() => {
            let totalWin = 0;
            let winDetails = [];

            // 结算所有注单
            betCart.forEach(bet => {
                if(bet.num === finalRes) {
                    const win = bet.amt * 33;
                    totalWin += win;
                    winDetails.push(bet.num);
                }
            });

            balance += totalWin;
            updateUI();

            if(totalWin > 0) {
                status.innerHTML = `<b style="color:var(--success)">恭喜！中奖号 ${finalRes}，共赢得 RM ${totalWin.toFixed(2)}</b>`;
            } else {
                status.innerHTML = `<span style="color:var(--danger)">未中奖。开彩号是 ${finalRes}</span>`;
            }

            addLog(round++, finalRes, [...betCart], totalWin);
            betCart = []; // 清空购物车
            renderCart();
            btn.disabled = false;
        }, 10000);
    }

    function updateUI() {
        document.getElementById('balance').innerText = balance.toFixed(2);
    }

    function addLog(round, res, bets, win) {
        const list = document.getElementById('log-list');
        const time = new Date().toLocaleTimeString();
        const item = document.createElement('div');
        item.className = `log-item ${win > 0 ? 'win-log' : ''}`;
        
        let betInfo = bets.map(b => `${b.num}(RM${b.amt})`).join(', ');
        
        item.innerHTML = `
            <div style="display:flex; justify-content:space-between; font-size:10px; color:#aaa">
                <span>期数 #${round}</span> <span>${time}</span>
            </div>
            <div style="margin:5px 0">开彩: <b style="color:var(--gold); font-size:16px">${res.toString().padStart(2,'0')}</b></div>
            <div style="color:#cbd5e1">投注: ${betInfo}</div>
            <div style="margin-top:5px; font-weight:bold">${win > 0 ? `<span style="color:var(--success)">派彩: RM ${win.toFixed(2)}</span>` : '<span style="color:#64748b">无中奖</span>'}</div>
        `;
        list.prepend(item);
    }

    init();
</script>
</body>
</html>
