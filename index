<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Jogo dos 100.000 Botões - Edição Suprema</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0; padding: 0;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
            font-family: 'Segoe UI', Roboto, sans-serif;
            -webkit-tap-highlight-color: transparent; /* Tira o traço azul ao clicar no telemóvel */
        }

        body, html {
            width: 100%; height: 100%; overflow: hidden;
            background-color: #0b132b; color: #fff;
            transition: background 0.8s ease, color 0.5s ease;
        }

        /* 8 TEMAS DINÂMICOS DE ACORDO COM O NÍVEL */
        body.theme-0 { background: radial-gradient(circle, #1b4332 0%, #081c15 100%); } /* 1. Floresta */
        body.theme-1 { background: radial-gradient(circle, #0077b6 0%, #03045e 100%); } /* 2. Oceano */
        body.theme-2 { background: radial-gradient(circle, #9d0208 0%, #03071e 100%); } /* 3. Vulcão */
        body.theme-3 { background: radial-gradient(circle, #3a0ca3 0%, #10002b 100%); } /* 4. Espaço */
        body.theme-4 { background: radial-gradient(circle, #f72585 0%, #3f37c9 100%); } /* 5. Cyberpunk */
        body.theme-5 { background: radial-gradient(circle, #b5e2fa 0%, #023047 100%); } /* 6. Gelado */
        body.theme-6 { background: radial-gradient(circle, #e9c46a 0%, #264653 100%); } /* 7. Egito */
        body.theme-7 { background: radial-gradient(circle, #f8f9fa 0%, #6c757d 100%); color: #111; } /* 8. Celestial */

        #game-container {
            width: 100vw; height: 100vh;
            display: flex; flex-direction: column; justify-content: space-between;
            position: relative;
        }

        /* HUD TOPO */
        #hud {
            background: rgba(15, 23, 42, 0.9);
            border-bottom: 2px solid rgba(255,255,255,0.1);
            padding: 10px; text-align: center; z-index: 10;
            backdrop-filter: blur(5px);
        }

        #points-display { font-size: 26px; font-weight: 800; color: #6fffe9; }
        #sub-hud {
            font-size: 11px; color: #cbd5e1; margin-top: 4px;
            display: flex; justify-content: center; gap: 12px; flex-wrap: wrap;
        }

        .hud-btn {
            background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2);
            color: #fff; padding: 5px 12px; border-radius: 15px; font-size: 11px;
            cursor: pointer; outline: none; transition: 0.2s;
        }
        .hud-btn:active { transform: scale(0.95); }

        /* ÁREA PRINCIPAL DO BOTÃO */
        #room-view {
            flex: 1; display: flex; flex-direction: column;
            align-items: center; justify-content: center; position: relative;
        }

        #theme-tag {
            position: absolute; top: 12px; font-size: 11px;
            letter-spacing: 2px; text-transform: uppercase; font-weight: bold; opacity: 0.8;
        }

        #level-label {
            position: absolute; top: 30px; font-size: 14px;
            letter-spacing: 2px; text-transform: uppercase; font-weight: 800;
        }

        /* ESTILO DO BOTÃO (SEM BORDA DE FOCO/OUTLINE) */
        #button-target {
            width: 200px; height: 200px; border-radius: 40px;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            box-shadow: 0 15px 35px rgba(0,0,0,0.6); transition: transform 0.08s, background 0.3s;
            cursor: pointer; border: 4px solid rgba(255,255,255,0.3); text-align: center; padding: 15px;
            outline: none !important;
            -webkit-tap-highlight-color: transparent !important;
        }
        #button-target:focus, #button-target:active { outline: none !important; }
        #button-target:active { transform: scale(0.91); }

        /* SKINS DO BOTÃO */
        .skin-emerald { background: linear-gradient(135deg, #10b981, #047857); color: #fff; }
        .skin-gold { background: linear-gradient(135deg, #f59e0b, #b45309); color: #fff; border-color: #fef08a !important; }
        .skin-diamond { background: linear-gradient(135deg, #38bdf8, #1d4ed8); color: #fff; border-color: #bae6fd !important; }
        .skin-neon { background: linear-gradient(135deg, #f72585, #7209b7); color: #fff; border-color: #4cc9f0 !important; }
        .skin-fire { background: linear-gradient(135deg, #f97316, #dc2626); color: #fff; border-color: #fef08a !important; }

        .btn-locked { background: linear-gradient(135deg, #334155, #1e293b) !important; color: #64748b !important; border-color: #475569 !important; }

        /* EVENTO GOTA ALEATÓRIA */
        #golden-drop {
            position: absolute; font-size: 42px; cursor: pointer; z-index: 50; display: none;
            animation: pulse 0.8s infinite alternate;
        }
        @keyframes pulse { 0% { transform: scale(1); } 100% { transform: scale(1.25); } }

        /* LOJA E CONTROLES */
        #shop-panel {
            background: rgba(15, 23, 42, 0.95); border-top: 1px solid rgba(255,255,255,0.1);
            padding: 8px; max-height: 120px; overflow-y: auto;
        }

        .shop-item {
            display: flex; justify-content: space-between; align-items: center;
            background: rgba(255,255,255,0.05); padding: 6px 12px; margin-bottom: 4px; border-radius: 8px; font-size: 11px;
        }

        .shop-btn {
            background: #10b981; border: none; color: #fff; padding: 4px 10px; border-radius: 6px;
            font-weight: bold; cursor: pointer; outline: none;
        }

        #controls-bar { background: #0f172a; padding: 8px; display: flex; justify-content: center; gap: 5px; }
        .nav-btn {
            background: #1e293b; border: 1px solid #334155; color: #fff; padding: 6px 10px;
            border-radius: 8px; font-size: 11px; cursor: pointer; font-weight: bold; outline: none;
        }

        .floating-text {
            position: absolute; font-size: 18px; font-weight: bold; pointer-events: none;
            animation: floatUp 0.8s ease-out forwards; z-index: 100;
        }
        @keyframes floatUp { 0% { opacity: 1; transform: translateY(0); } 100% { opacity: 0; transform: translateY(-40px); } }

        /* MODAIS */
        .modal {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.92); display: none; flex-direction: column;
            align-items: center; justify-content: center; z-index: 200; padding: 20px; text-align: center;
        }
    </style>
</head>
<body class="theme-0">

<div id="game-container">
    <div id="hud">
        <div id="points-display">Pontos: 0</div>
        <div id="sub-hud">
            <span>⚡ Clique: +<span id="click-power-val">1</span></span>
            <span>🤖 Auto: +<span id="auto-income-val">0</span>/s</span>
            <span>💎 Gemas: <span id="gems-val">0</span> (<span id="mult-val">1.0</span>x)</span>
        </div>
        <div style="margin-top: 6px; display: flex; justify-content: center; gap: 6px; flex-wrap: wrap;">
            <button class="hud-btn" onclick="rebirth()">✨ Renascer</button>
            <button class="hud-btn" onclick="changeSkin()">🎨 Trocar Skin</button>
            <button class="hud-btn" onclick="toggleWaterfallSound()">🌊 Som: OFF</button>
            <button class="hud-btn" onclick="jumpToLevelPrompt()">🎯 Ir p/ Level</button>
            <button class="hud-btn" onclick="saveGame(true)">💾 Salvar</button>
        </div>
    </div>

    <div id="room-view">
        <div id="theme-tag">MUNDO 1: FLORESTA</div>
        <div id="level-label">NÍVEL 1 DE 100.000</div>
        
        <div id="golden-drop" onclick="clickGoldenDrop(event)">💧</div>

        <div id="button-target" class="skin-emerald" onclick="clickLevelButton(event)">
            <span id="btn-title" style="font-size: 22px; font-weight: 800;">BOTÃO #1</span>
            <span id="btn-cost-text" style="font-size: 11px; margin-top: 6px;">ATIVO</span>
        </div>
    </div>

    <div id="shop-panel">
        <div class="shop-item">
            <div><strong>Cachoeira Auto</strong> (+2/s) | <span id="auto-cost-0">Custa: 50 pts</span></div>
            <button class="shop-btn" onclick="buyAutoClicker(0)">Comprar</button>
        </div>
        <div class="shop-item">
            <div><strong>Robô Clicador</strong> (+25/s) | <span id="auto-cost-1">Custa: 500 pts</span></div>
            <button class="shop-btn" onclick="buyAutoClicker(1)">Comprar</button>
        </div>
        <div class="shop-item">
            <div><strong>Bomba d'Água</strong> (+200/s) | <span id="auto-cost-2">Custa: 4.000 pts</span></div>
            <button class="shop-btn" onclick="buyAutoClicker(2)">Comprar</button>
        </div>
    </div>

    <div id="controls-bar">
        <button class="nav-btn" onclick="changeLevel(-10000)">-10k</button>
        <button class="nav-btn" onclick="changeLevel(-1000)">-1k</button>
        <button class="nav-btn" onclick="changeLevel(-100)">-100</button>
        <button class="nav-btn" onclick="changeLevel(-1)">◀</button>
        <span style="font-size: 11px; color: #cbd5e1; font-weight: bold; line-height:28px;" id="nav-info">Lvl 1</span>
        <button class="nav-btn" onclick="changeLevel(1)">▶</button>
        <button class="nav-btn" onclick="changeLevel(100)">+100</button>
        <button class="nav-btn" onclick="changeLevel(1000)">+1k</button>
        <button class="nav-btn" onclick="changeLevel(10000)">+10k</button>
    </div>
</div>

<!-- Modal Vitória Final -->
<div id="victory-modal" class="modal">
    <h1 style="color: #6fffe9; font-size: 32px; margin-bottom: 15px;">🏆 ZEROU O JOGO! 🏆</h1>
    <p style="color: #ee9b00; font-size: 22px; font-weight: bold; margin-bottom: 25px;">Parabéns! Alcançaste o Nível 100.000!</p>
    <button class="shop-btn" style="padding: 12px 24px; font-size: 16px;" onclick="restartGame()">Jogar Novamente</button>
</div>

<script>
    let audioCtx = null;
    let waterfallPlaying = false;
    let waterfallNode = null;

    function initAudio() {
        if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    }

    function playClickSound() {
        initAudio();
        if (!audioCtx) return;
        const now = audioCtx.currentTime;
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.type = 'triangle';
        osc.frequency.setValueAtTime(500, now);
        osc.frequency.exponentialRampToValueAtTime(1200, now + 0.07);
        gain.gain.setValueAtTime(0.5, now);
        gain.gain.exponentialRampToValueAtTime(0.01, now + 0.07);
        osc.connect(gain); gain.connect(audioCtx.destination);
        osc.start(now); osc.stop(now + 0.07);
    }

    function toggleWaterfallSound() {
        initAudio();
        if (!waterfallPlaying) {
            waterfallPlaying = true;
            const bufferSize = audioCtx.sampleRate * 2;
            const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
            const output = buffer.getChannelData(0);
            for (let i = 0; i < bufferSize; i++) output[i] = (Math.random() * 2 - 1) * 0.04;
            waterfallNode = audioCtx.createBufferSource();
            waterfallNode.buffer = buffer; waterfallNode.loop = true;
            const filter = audioCtx.createBiquadFilter();
            filter.type = 'lowpass'; filter.frequency.value = 800;
            waterfallNode.connect(filter); filter.connect(audioCtx.destination);
            waterfallNode.start();
            event.target.innerText = "🌊 Som: ON";
        } else {
            waterfallPlaying = false;
            if (waterfallNode) waterfallNode.stop();
            event.target.innerText = "🌊 Som: OFF";
        }
    }

    // Variáveis de Estado
    let points = 0;
    let clickPower = 1;
    let autoIncome = 0;
    let currentLevel = 1;
    let gems = 0;
    let multiplier = 1;
    let currentSkin = 'emerald';
    let goldenBonusActive = false;

    const skins = ['emerald', 'gold', 'diamond', 'neon', 'fire'];
    const MAX_LEVEL = 100000;
    const unlockedLevels = new Set([1]);

    const themes = [
        { name: "Mundo 1: Floresta & Natureza", class: "theme-0" },
        { name: "Mundo 2: Profundezas do Oceano", class: "theme-1" },
        { name: "Mundo 3: Vulcão & Magma", class: "theme-2" },
        { name: "Mundo 4: Espaço & Galáxia", class: "theme-3" },
        { name: "Mundo 5: Cyberpunk / Neon", class: "theme-4" },
        { name: "Mundo 6: Reino Congelado", class: "theme-5" },
        { name: "Mundo 7: Egito Antigo", class: "theme-6" },
        { name: "Mundo 8: Dimensão Celestial", class: "theme-7" }
    ];

    const autoClickers = [
        { name: "Cachoeira Auto", cost: 50, rate: 2 },
        { name: "Robô Clicador", cost: 500, rate: 25 },
        { name: "Bomba d'Água", cost: 4000, rate: 200 }
    ];

    function getLevelData(lvl) {
        if (lvl === 1) return { cost: 0, power: 1 };
        const progress = (lvl - 1) / (MAX_LEVEL - 1);
        return {
            cost: Math.floor(Math.pow(progress, 2.5) * 1000000000) + lvl * 10,
            power: Math.floor(Math.pow(progress, 1.8) * 100000) + 1
        };
    }

    function updateTheme() {
        document.body.className = '';
        const themeIdx = Math.min(7, Math.floor(((currentLevel - 1) / MAX_LEVEL) * 8));
        document.body.classList.add(themes[themeIdx].class);
        document.getElementById('theme-tag').innerText = themes[themeIdx].name;
    }

    function updateLevelUI() {
        updateTheme();
        const data = getLevelData(currentLevel);
        const isUnlocked = unlockedLevels.has(currentLevel);

        document.getElementById('level-label').innerText = `NÍVEL ${currentLevel.toLocaleString()} DE 100.000`;
        document.getElementById('nav-info').innerText = `Lvl ${currentLevel.toLocaleString()}`;

        const target = document.getElementById('button-target');
        document.getElementById('btn-title').innerText = `BOTÃO #${currentLevel.toLocaleString()}`;

        target.className = `skin-${currentSkin}`;
        if (isUnlocked) {
            document.getElementById('btn-cost-text').innerText = `+${Math.floor(data.power * multiplier).toLocaleString()} p/ clique`;
        } else {
            target.classList.add("btn-locked");
            document.getElementById('btn-cost-text').innerText = `🔒 Custa: ${data.cost.toLocaleString()} pts`;
        }
    }

    function changeLevel(delta) {
        currentLevel = Math.max(1, Math.min(MAX_LEVEL, currentLevel + delta));
        updateLevelUI();
    }

    function jumpToLevelPrompt() {
        const input = prompt("Digite o Nível desejado (1 a 100.000):");
        const parsed = parseInt(input);
        if (parsed && parsed >= 1 && parsed <= MAX_LEVEL) {
            currentLevel = parsed;
            updateLevelUI();
        }
    }

    function clickLevelButton(e) {
        playClickSound();
        const data = getLevelData(currentLevel);
        const isUnlocked = unlockedLevels.has(currentLevel);
        let gain = clickPower * multiplier * (goldenBonusActive ? 5 : 1);

        if (isUnlocked) {
            points += gain;
            showFloatingText(e, `+${Math.floor(gain).toLocaleString()}`);
        } else {
            if (points >= data.cost) {
                points -= data.cost;
                unlockedLevels.add(currentLevel);
                clickPower += data.power;
                showFloatingText(e, "DESBLOQUEADO!");

                if (currentLevel === MAX_LEVEL) {
                    document.getElementById('victory-modal').style.display = 'flex';
                }
            } else {
                showFloatingText(e, "INSUFICIENTE!", "#f85149");
            }
        }
        updateHUD();
        updateLevelUI();
    }

    function buyAutoClicker(idx) {
        const item = autoClickers[idx];
        if (points >= item.cost) {
            points -= item.cost;
            autoIncome += item.rate;
            item.cost = Math.floor(item.cost * 1.4);
            document.getElementById(`auto-cost-${idx}`).innerText = `Custa: ${item.cost.toLocaleString()} pts`;
            updateHUD();
        }
    }

    function rebirth() {
        if (currentLevel < 100) {
            alert("Alcança pelo menos o Nível 100 para poderes renascer!");
            return;
        }
        const gemsEarned = Math.floor(currentLevel / 50);
        if (confirm(`Renascer agora resetará teus pontos e níveis, mas ganharás ${gemsEarned} Gemas (+${gemsEarned * 10}% de bónus permanente em tudo)!`)) {
            gems += gemsEarned;
            multiplier = 1 + (gems * 0.1);
            points = 0; clickPower = 1; autoIncome = 0; currentLevel = 1;
            unlockedLevels.clear(); unlockedLevels.add(1);
            updateHUD(); updateLevelUI();
        }
    }

    function changeSkin() {
        let idx = (skins.indexOf(currentSkin) + 1) % skins.length;
        currentSkin = skins[idx];
        updateLevelUI();
    }

    setInterval(() => {
        const drop = document.getElementById('golden-drop');
        drop.style.left = `${Math.random() * 70 + 15}%`;
        drop.style.top = `${Math.random() * 50 + 20}%`;
        drop.style.display = 'block';
        setTimeout(() => drop.style.display = 'none', 5000);
    }, 25000);

    function clickGoldenDrop(e) {
        goldenBonusActive = true;
        document.getElementById('golden-drop').style.display = 'none';
        showFloatingText(e, "✨ BÓNUS 5X ATIVO! (10s)", "#ee9b00");
        setTimeout(() => goldenBonusActive = false, 10000);
    }

    function updateHUD() {
        document.getElementById('points-display').innerText = `Pontos: ${Math.floor(points).toLocaleString()}`;
        document.getElementById('click-power-val').innerText = Math.floor(clickPower * multiplier).toLocaleString();
        document.getElementById('auto-income-val').innerText = Math.floor(autoIncome * multiplier).toLocaleString();
        document.getElementById('gems-val').innerText = gems.toLocaleString();
        document.getElementById('mult-val').innerText = multiplier.toFixed(1);
    }

    function showFloatingText(e, text, color = "#6fffe9") {
        const floatEl = document.createElement('div');
        floatEl.className = 'floating-text'; floatEl.innerText = text; floatEl.style.color = color;
        floatEl.style.left = `${(e.clientX || window.innerWidth / 2) - 30}px`;
        floatEl.style.top = `${(e.clientY || window.innerHeight / 2) - 30}px`;
        document.body.appendChild(floatEl);
        setTimeout(() => floatEl.remove(), 800);
    }

    function saveGame(manual = false) {
        const data = { points, clickPower, autoIncome, currentLevel, gems, multiplier, currentSkin, unlocked: Array.from(unlockedLevels) };
        localStorage.setItem('game100k_deluxe_save', JSON.stringify(data));
        if (manual) alert("Jogo salvo com sucesso!");
    }

    function loadGame() {
        const saved = localStorage.getItem('game100k_deluxe_save');
        if (saved) {
            const data = JSON.parse(saved);
            points = data.points || 0;
            clickPower = data.clickPower || 1;
            autoIncome = data.autoIncome || 0;
            currentLevel = data.currentLevel || 1;
            gems = data.gems || 0;
            multiplier = data.multiplier || 1;
            currentSkin = data.currentSkin || 'emerald';
            if (data.unlocked) data.unlocked.forEach(l => unlockedLevels.add(l));
        }
    }

    setInterval(() => {
        if (autoIncome > 0) {
            points += (autoIncome * multiplier) / 10;
            updateHUD();
        }
    }, 100);

    setInterval(() => saveGame(false), 10000);

    loadGame();
    updateHUD();
    updateLevelUI();
</script>
</body>
</html>
