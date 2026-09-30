<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Garden Life</title>

<style>
* {
    box-sizing: border-box;
}

:root {
    --bg: #102018;
    --panel: #183124;
    --panel2: #21412f;
    --border: #396449;
    --text: #f1f7e9;
    --muted: #a9c0aa;
    --accent: #8edb73;
    --gold: #ffd45c;
    --danger: #ff7777;
    --water: #62c9ff;
}

body {
    margin: 0;
    font-family: system-ui, Arial, sans-serif;
    background:
        radial-gradient(circle at top, #274c36 0%, #102018 55%);
    color: var(--text);
    min-height: 100vh;
}

button {
    border: 0;
    border-radius: 10px;
    padding: 10px 14px;
    background: #315d40;
    color: white;
    font-weight: 700;
    cursor: pointer;
    transition: .15s;
}

button:hover {
    transform: translateY(-1px);
    background: #3d7350;
}

button:disabled {
    opacity: .45;
    cursor: not-allowed;
    transform: none;
}

.app {
    width: min(1400px, 100%);
    margin: auto;
    padding: 18px;
}

header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 15px;
    flex-wrap: wrap;
    margin-bottom: 15px;
}

.logo {
    font-size: 30px;
    font-weight: 900;
}

.subtitle {
    color: var(--muted);
    font-size: 14px;
}

.stats {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
}

.stat {
    background: rgba(24,49,36,.95);
    border: 1px solid var(--border);
    padding: 9px 12px;
    border-radius: 12px;
    font-weight: 700;
}

.layout {
    display: grid;
    grid-template-columns: 1fr 330px;
    gap: 15px;
}

.panel {
    background: rgba(20,42,30,.96);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 15px;
}

.garden-panel {
    min-height: 650px;
}

.topbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 10px;
    flex-wrap: wrap;
    margin-bottom: 12px;
}

.weather {
    color: var(--muted);
}

.garden {
    display: grid;
    grid-template-columns: repeat(8, minmax(55px, 1fr));
    gap: 7px;
    background:
        linear-gradient(#0002, #0002),
        repeating-linear-gradient(
            45deg,
            #4c8b4b 0,
            #4c8b4b 14px,
            #518f4e 14px,
            #518f4e 28px
        );
    padding: 12px;
    border-radius: 15px;
    border: 5px solid #674a2e;
    min-height: 550px;
}

.plot {
    position: relative;
    min-height: 90px;
    border-radius: 12px;
    background: rgba(76,53,31,.55);
    border: 2px solid rgba(255,255,255,.1);
    cursor: pointer;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    user-select: none;
}

.plot:hover {
    border-color: #d9efb8;
}

.plot.locked {
    background: #28312b;
    cursor: not-allowed;
    color: #738078;
}

.lock {
    font-size: 22px;
}

.plant {
    text-align: center;
    width: 100%;
    animation: sway 3s ease-in-out infinite;
}

@keyframes sway {
    0%,100% { transform: rotate(-1deg); }
    50% { transform: rotate(1deg); }
}

.plantEmoji {
    font-size: 43px;
    line-height: 1;
}

.plantName {
    font-size: 11px;
    font-weight: 800;
    text-shadow: 0 1px 2px #000;
}

.progress {
    height: 6px;
    margin: 5px 10px 0;
    background: #182019;
    border-radius: 10px;
    overflow: hidden;
}

.progress > div {
    height: 100%;
    background: var(--accent);
}

.badge {
    position: absolute;
    top: 4px;
    right: 4px;
    font-size: 13px;
}

.pest {
    position: absolute;
    left: 4px;
    bottom: 3px;
    font-size: 15px;
}

.side-section {
    margin-bottom: 18px;
}

.side-section h3 {
    margin: 0 0 8px;
}

.seed-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 7px;
}

.seed {
    background: #21412f;
    border: 1px solid #396449;
    padding: 9px;
    border-radius: 10px;
    cursor: pointer;
}

.seed.selected {
    outline: 2px solid var(--accent);
    background: #315a3d;
}

.seed small {
    display: block;
    color: var(--muted);
}

.quest {
    background: #213b2b;
    border-radius: 10px;
    padding: 9px;
    margin-bottom: 7px;
}

.quest.done {
    opacity: .65;
}

.questTitle {
    font-weight: 800;
}

.questProgress {
    color: var(--muted);
    font-size: 13px;
}

.actions {
    display: flex;
    flex-wrap: wrap;
    gap: 7px;
}

.notice {
    background: #273d2c;
    border-left: 4px solid var(--accent);
    padding: 10px;
    border-radius: 8px;
    margin-bottom: 10px;
}

#log {
    max-height: 150px;
    overflow: auto;
    font-size: 13px;
    color: var(--muted);
}

.logLine {
    padding: 3px 0;
}

.modal {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,.72);
    display: none;
    align-items: center;
    justify-content: center;
    padding: 20px;
    z-index: 50;
}

.modal.open {
    display: flex;
}

.modalBox {
    width: min(700px, 100%);
    max-height: 90vh;
    overflow: auto;
    background: #162b20;
    border: 1px solid var(--border);
    border-radius: 18px;
    padding: 20px;
}

.shopGrid,
.collectionGrid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 9px;
}

.shopItem,
.collectionItem {
    background: #213b2b;
    padding: 12px;
    border-radius: 12px;
}

.collectionItem.locked {
    filter: grayscale(1);
    opacity: .5;
}

.toast {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%) translateY(20px);
    background: #183124;
    border: 1px solid var(--accent);
    padding: 12px 18px;
    border-radius: 12px;
    opacity: 0;
    pointer-events: none;
    transition: .25s;
    z-index: 100;
    text-align: center;
}

.toast.show {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
}

.small {
    color: var(--muted);
    font-size: 13px;
}

.big {
    font-size: 20px;
    font-weight: 900;
}

@media(max-width: 950px) {
    .layout {
        grid-template-columns: 1fr;
    }
}

@media(max-width: 650px) {
    .garden {
        grid-template-columns: repeat(4, 1fr);
    }

    .plot {
        min-height: 80px;
    }

    .plantEmoji {
        font-size: 34px;
    }
}
</style>
</head>

<body>

<div class="app">

<header>
    <div>
        <div class="logo">🌱 Garden Life</div>
        <div class="subtitle">
            Grow. Discover. Experiment. Build your dream garden.
        </div>
    </div>

    <div class="stats">
        <div class="stat">🪙 <span id="coins">0</span></div>
        <div class="stat">⭐ Lv. <span id="level">1</span></div>
        <div class="stat">✨ <span id="xp">0</span> XP</div>
        <div class="stat">📅 Day <span id="day">1</span></div>
    </div>
</header>

<div id="notice"></div>

<div class="layout">

<main class="panel garden-panel">

    <div class="topbar">
        <div>
            <div class="big">🏡 Your Garden</div>
            <div class="weather" id="weather">☀️ Sunny</div>
        </div>

        <div class="actions">
            <button id="waterAll">💧 Water All</button>
            <button id="sunAll">☀️ Give Sun</button>
            <button id="randomEvent">🎲 Event</button>
        </div>
    </div>

    <div class="garden" id="garden"></div>

    <div style="margin-top:12px">
        <div class="small">
            Selected seed:
            <strong id="selectedSeed">🌻 Sunflower</strong>
        </div>
        <div class="small">
            Click an empty plot to plant. Click a plant to care for it.
        </div>
    </div>

</main>

<aside>

<div class="panel side-section">
    <h3>🌰 Seeds</h3>
    <div class="seed-grid" id="seedList"></div>
</div>

<div class="panel side-section">
    <h3>🎯 Quests</h3>
    <div id="quests"></div>
</div>

<div class="panel side-section">
    <h3>📊 Garden Stats</h3>
    <div class="small">
        Plants grown: <b id="plantsGrown">0</b><br>
        Plants harvested: <b id="harvested">0</b><br>
        Discoveries: <b id="discoveries">0</b><br>
        Mutations: <b id="mutations">0</b><br>
        Garden level: <b id="gardenLevel">1</b>
    </div>
</div>

<div class="panel side-section">
    <h3>🛠️ Menu</h3>
    <div class="actions">
        <button onclick="openShop()">🏪 Shop</button>
        <button onclick="openCollection()">📖 Collection</button>
        <button onclick="openAchievements()">🏆 Achievements</button>
        <button onclick="saveGame()">💾 Save</button>
        <button onclick="resetGame()">🗑️ Reset</button>
    </div>
</div>

<div class="panel side-section">
    <h3>📜 Activity</h3>
    <div id="log"></div>
</div>

</aside>

</div>
</div>

<div class="toast" id="toast"></div>

<div class="modal" id="modal">
    <div class="modalBox">
        <div style="display:flex;justify-content:space-between;gap:10px">
            <h2 id="modalTitle">Garden</h2>
            <button onclick="closeModal()">✕</button>
        </div>
        <div id="modalContent"></div>
    </div>
</div>

<script>
"use strict";

/* =========================================================
   GARDEN LIFE
   Single-file browser game
   ========================================================= */

const SAVE_KEY = "garden-life-save-v1";

const PLANTS = {
    sunflower: {
        name:"Sunflower",
        emoji:"🌻",
        cost:10,
        sell:25,
        grow:100,
        water:55,
        sun:75,
        rarity:"Common"
    },
    tulip: {
        name:"Tulip",
        emoji:"🌷",
        cost:15,
        sell:35,
        grow:120,
        water:60,
        sun:60,
        rarity:"Common"
    },
    rose: {
        name:"Rose",
        emoji:"🌹",
        cost:30,
        sell:70,
        grow:170,
        water:65,
        sun:65,
        rarity:"Uncommon"
    },
    lavender: {
        name:"Lavender",
        emoji:"💜",
        cost:35,
        sell:80,
        grow:180,
        water:45,
        sun:55,
        rarity:"Uncommon"
    },
    cactus: {
        name:"Cactus",
        emoji:"🌵",
        cost:40,
        sell:100,
        grow:230,
        water:20,
        sun:90,
        rarity:"Rare"
    },
    mushroom: {
        name:"Mushroom",
        emoji:"🍄",
        cost:50,
        sell:125,
        grow:200,
        water:80,
        sun:20,
        rarity:"Rare"
    },
    moonflower: {
        name:"Moonflower",
        emoji:"🌙",
        cost:100,
        sell:280,
        grow:300,
        water:65,
        sun:30,
        rarity:"Epic"
    },
    crystal: {
        name:"Crystal Plant",
        emoji:"💎",
        cost:250,
        sell:700,
        grow:420,
        water:50,
        sun:50,
        rarity:"Legendary"
    },
    rainbow: {
        name:"Rainbow Flower",
        emoji:"🌈",
        cost:500,
        sell:1500,
        grow:500,
        water:70,
        sun:70,
        rarity:"Mythic"
    },
    ancient: {
        name:"Ancient Tree",
        emoji:"🌳",
        cost:750,
        sell:2400,
        grow:700,
        water:60,
        sun:60,
        rarity:"Mythic"
    }
};

const WEATHER = [
    {name:"Sunny", icon:"☀️", water:-3, sun:10},
    {name:"Cloudy", icon:"☁️", water:0, sun:3},
    {name:"Rain", icon:"🌧️", water:25, sun:-2},
    {name:"Heat Wave", icon:"🔥", water:-12, sun:15},
    {name:"Storm", icon:"⛈️", water:35, sun:-5},
    {name:"Fog", icon:"🌫️", water:8, sun:-5},
    {name:"Rainbow", icon:"🌈", water:10, sun:10}
];

const MUTATIONS = [
    {name:"Golden", emoji:"✨", multiplier:2},
    {name:"Rainbow", emoji:"🌈", multiplier:3},
    {name:"Giant", emoji:"🌟", multiplier:2.5},
    {name:"Crystal", emoji:"💎", multiplier:4},
    {name:"Ghost", emoji:"👻", multiplier:3},
    {name:"Cosmic", emoji:"🪐", multiplier:7}
];

const ACHIEVEMENTS = [
    ["firstPlant","🌱 First Plant","Plant your first seed.",1],
    ["tenPlants","🌿 Gardener","Grow 10 plants.",100],
    ["fiftyPlants","🌳 Green Thumb","Grow 50 plants.",300],
    ["mutation","🧬 Mutant","Discover a mutation.",150],
    ["rare","💎 Rare Find","Grow a Rare or better plant.",250],
    ["million","🪙 Big Saver","Reach 1,000 coins.",250],
    ["collector","📖 Collector","Discover 8 plant species.",400],
    ["night","🌙 Night Gardener","Grow a Moonflower.",200],
    ["legend","✨ Legendary","Grow a Legendary plant.",500],
    ["garden10","🏡 Expanding","Reach garden level 10.",500]
];

let game = loadGame();

function defaultGame() {
    return {
        coins:100,
        xp:0,
        level:1,
        day:1,
        weather:0,
        weatherTicks:0,
        selectedSeed:"sunflower",

        plants: Array(32).fill(null),

        unlockedSeeds:[
            "sunflower",
            "tulip",
            "rose",
            "lavender",
            "cactus",
            "mushroom"
        ],

        inventory:{
            sunflower:5,
            tulip:3,
            rose:2,
            lavender:1,
            cactus:1,
            mushroom:1
        },

        discovered:[],

        stats:{
            grown:0,
            harvested:0,
            mutations:0
        },

        achievements:[],

        quests: newQuests(),

        gardenLevel:1,

        lastSave:Date.now(),

        logs:[
            "Welcome to Garden Life! 🌱"
        ]
    };
}

function newQuests() {
    return [
        {
            id:"water",
            title:"Thirsty Plants",
            description:"Water 5 plants.",
            goal:5,
            progress:0,
            reward:50,
            type:"water",
            done:false
        },
        {
            id:"grow",
            title:"Little Garden",
            description:"Grow 3 plants.",
            goal:3,
            progress:0,
            reward:100,
            type:"grow",
            done:false
        },
        {
            id:"coins",
            title:"Garden Business",
            description:"Earn 200 coins.",
            goal:200,
            progress:0,
            reward:125,
            type:"coins",
            done:false
        }
    ];
}

function loadGame() {
    try {
        const saved = JSON.parse(localStorage.getItem(SAVE_KEY));

        if (!saved) {
            return defaultGame();
        }

        const fresh = defaultGame();

        const merged = {
            ...fresh,
            ...saved,
            stats:{
                ...fresh.stats,
                ...(saved.stats || {})
            }
        };

        if (!Array.isArray(merged.plants)) {
            merged.plants = fresh.plants;
        }

        if (!Array.isArray(merged.logs)) {
            merged.logs = fresh.logs;
        }

        if (!Array.isArray(merged.quests) || !merged.quests.length) {
            merged.quests = newQuests();
        }

        applyOfflineProgress(merged);

        return merged;

    } catch(e) {
        console.warn("Save could not be loaded:", e);
        return defaultGame();
    }
}

function saveGame() {
    game.lastSave = Date.now();

    localStorage.setItem(
        SAVE_KEY,
        JSON.stringify(game)
    );

    toast("💾 Game saved!");
}

function applyOfflineProgress(g) {
    const elapsed = Date.now() - (g.lastSave || Date.now());

    const minutes = Math.min(
        360,
        Math.floor(elapsed / 60000)
    );

    if (minutes <= 0) return;

    let grown = 0;

    g.plants.forEach(p => {
        if (!p) return;

        const growth = Math.min(
            p.grow,
            minutes * 0.5
        );

        p.progress += growth;

        p.water = Math.max(
            0,
            p.water - minutes * 0.08
        );

        if (p.progress >= p.grow && !p.mature) {
            p.progress = p.grow;
            p.mature = true;
            grown++;
        }
    });

    if (grown > 0) {
        g.logs.unshift(
            `🌱 While you were away, ${grown} plant${grown === 1 ? "" : "s"} matured!`
        );
    }
}

function log(message) {
    game.logs.unshift(message);

    if (game.logs.length > 30) {
        game.logs.length = 30;
    }

    renderLog();
}

function toast(message) {
    const el = document.getElementById("toast");

    el.textContent = message;
    el.classList.add("show");

    clearTimeout(toast.timer);

    toast.timer = setTimeout(() => {
        el.classList.remove("show");
    }, 2200);
}

/* =========================================================
   RENDERING
   ========================================================= */

function render() {
    document.getElementById("coins").textContent = game.coins;
    document.getElementById("level").textContent = game.level;
    document.getElementById("xp").textContent = game.xp;
    document.getElementById("day").textContent = game.day;

    document.getElementById("plantsGrown").textContent =
        game.stats.grown;

    document.getElementById("harvested").textContent =
        game.stats.harvested;

    document.getElementById("discoveries").textContent =
        game.discovered.length;

    document.getElementById("mutations").textContent =
        game.stats.mutations;

    document.getElementById("gardenLevel").textContent =
        game.gardenLevel;

    const w = WEATHER[game.weather];

    document.getElementById("weather").textContent =
        `${w.icon} ${w.name}`;

    document.getElementById("selectedSeed").textContent =
        PLANTS[game.selectedSeed]?.emoji + " " +
        PLANTS[game.selectedSeed]?.name;

    renderGarden();
    renderSeeds();
    renderQuests();
    renderLog();
}

function renderGarden() {
    const garden = document.getElementById("garden");
    garden.innerHTML = "";

    game.plants.forEach((plant, index) => {

        const plot = document.createElement("div");
        plot.className = "plot";

        const unlocked =
            index < Math.min(
                game.plants.length,
                8 + game.gardenLevel * 2
            );

        if (!unlocked) {
            plot.classList.add("locked");
            plot.innerHTML =
                `<div class="lock">🔒</div>`;
            plot.title =
                "Increase your garden level to unlock this plot.";
            garden.appendChild(plot);
            return;
        }

        plot.addEventListener("click", () => {
            handlePlot(index);
        });

        if (!plant) {
            plot.innerHTML = `
                <div class="small">
                    🌱<br>
                    Empty
                </div>
            `;

            garden.appendChild(plot);
            return;
        }

        const data = PLANTS[plant.type];

        let emoji = data.emoji;

        if (plant.mutation) {
            emoji = plant.mutation.emoji + emoji;
        }

        const percent =
            Math.min(
                100,
                Math.round(
                    plant.progress / plant.grow * 100
                )
            );

        let status = "";

        if (plant.mature) {
            status = "✨ READY";
        } else if (plant.water < 25) {
            status = "💧 THIRSTY";
        } else if (plant.health < 35) {
            status = "❤️ WEAK";
        }

        plot.innerHTML = `
            <div class="plant">
                <div class="plantEmoji">${emoji}</div>
                <div class="plantName">
                    ${data.name}
                </div>

                <div class="progress">
                    <div style="width:${percent}%"></div>
                </div>

                <div class="small">${status}</div>
            </div>

            ${
                plant.pest
                ? `<div class="pest">🐛</div>`
                : ""
            }

            ${
                plant.mutation
                ? `<div class="badge">✨</div>`
                : ""
            }
        `;

        garden.appendChild(plot);
    });
}

function renderSeeds() {
    const list = document.getElementById("seedList");
    list.innerHTML = "";

    game.unlockedSeeds.forEach(id => {

        const data = PLANTS[id];
        const amount = game.inventory[id] || 0;

        const div = document.createElement("div");

        div.className =
            "seed " +
            (game.selectedSeed === id ? "selected" : "");

        div.innerHTML = `
            <strong>${data.emoji} ${data.name}</strong>
            <small>
                ${data.rarity} · x${amount}
            </small>
        `;

        div.addEventListener("click", () => {
            game.selectedSeed = id;
            renderSeeds();
            document.getElementById("selectedSeed").textContent =
                data.emoji + " " + data.name;
        });

        list.appendChild(div);
    });
}

function renderQuests() {
    const container = document.getElementById("quests");

    container.innerHTML = "";

    game.quests.forEach(q => {

        const div = document.createElement("div");

        div.className =
            "quest " + (q.done ? "done" : "");

        div.innerHTML = `
            <div class="questTitle">
                ${q.done ? "✅" : "🎯"} ${q.title}
            </div>

            <div class="questProgress">
                ${q.description}<br>
                ${Math.min(q.progress,q.goal)}
                / ${q.goal}
                · Reward: 🪙 ${q.reward}
            </div>
        `;

        container.appendChild(div);
    });
}

function renderLog() {
    const el = document.getElementById("log");

    el.innerHTML = game.logs
        .map(x => `<div class="logLine">${escapeHTML(x)}</div>`)
        .join("");
}

function escapeHTML(text) {
    return String(text)
        .replaceAll("&","&amp;")
        .replaceAll("<","&lt;")
        .replaceAll(">","&gt;");
}

/* =========================================================
   PLANTING & CARE
   ========================================================= */

function handlePlot(index) {

    const plant = game.plants[index];

    if (!plant) {
        plantSeed(index);
        return;
    }

    openPlant(index);
}

function plantSeed(index) {

    const type = game.selectedSeed;

    if (!game.inventory[type]) {
        toast("You don't have that seed!");
        return;
    }

    game.inventory[type]--;

    const data = PLANTS[type];

    const plant = {
        type:type,
        progress:0,
        grow:data.grow,
        water:65,
        sun:50,
        health:100,
        happiness:70,
        mature:false,
        pest:false,
        mutation:null,
        plantedAt:Date.now()
    };

    game.plants[index] = plant;

    if (!game.discovered.includes(type)) {
        game.discovered.push(type);
        log(`📖 New discovery: ${data.name}!`);
        checkAchievements();
    }

    addXP(10);

    log(`🌱 You planted a ${data.name}.`);

    saveQuiet();
    render();
}

function openPlant(index) {

    const p = game.plants[index];
    const data = PLANTS[p.type];

    document.getElementById("modalTitle").textContent =
        `${data.emoji} ${data.name}`;

    const mutationText =
        p.mutation
        ? `${p.mutation.emoji} ${p.mutation.name}`
        : "None";

    document.getElementById("modalContent").innerHTML = `
        <div class="notice">
            <strong>${data.rarity}</strong>
            ${p.mature ? " · ✨ Mature" : ""}
        </div>

        <p>
            <strong>Growth:</strong>
            ${Math.round(p.progress)}/${p.grow}
        </p>

        <p>
            <strong>💧 Water:</strong>
            ${Math.round(p.water)}%
        </p>

        <p>
            <strong>☀️ Sunlight:</strong>
            ${Math.round(p.sun)}%
        </p>

        <p>
            <strong>❤️ Health:</strong>
            ${Math.round(p.health)}%
        </p>

        <p>
            <strong>😊 Happiness:</strong>
            ${Math.round(p.happiness)}%
        </p>

        <p>
            <strong>✨ Mutation:</strong>
            ${mutationText}
        </p>

        ${
            p.pest
            ? `<div class="notice">🐛 This plant has pests!</div>`
            : ""
        }

        <div class="actions">
            <button onclick="waterPlant(${index})">
                💧 Water
            </button>

            <button onclick="sunPlant(${index})">
                ☀️ Sunlight
            </button>

            ${
                p.pest
                ? `<button onclick="removePest(${index})">
                    🐞 Remove Pests
                   </button>`
                : ""
            }

            ${
                p.mature
                ? `<button onclick="harvest(${index})">
                    🧺 Harvest
                   </button>`
                : ""
            }

            <button onclick="fertilize(${index})">
                🧪 Fertilize
            </button>
        </div>
    `;

    openModal();
}

function waterPlant(index) {

    const p = game.plants[index];

    if (!p) return;

    p.water = Math.min(
        100,
        p.water + 30
    );

    p.happiness = Math.min(
        100,
        p.happiness + 4
    );

    questProgress("water",1);

    addXP(4);

    log(`💧 You watered ${PLANTS[p.type].name}.`);

    closeModal();
    saveQuiet();
    render();
}

function sunPlant(index) {

    const p = game.plants[index];

    if (!p) return;

    p.sun = Math.min(
        100,
        p.sun + 25
    );

    p.happiness = Math.min(
        100,
        p.happiness + 3
    );

    addXP(3);

    log(`☀️ You gave your ${PLANTS[p.type].name} some sunlight.`);

    closeModal();
    saveQuiet();
    render();
}

function fertilize(index) {

    const p = game.plants[index];

    if (!p) return;

    if (!game.inventory.fertilizer) {
        toast("You don't have fertilizer.");
        return;
    }

    game.inventory.fertilizer--;

    p.progress += 20;
    p.health = Math.min(
        100,
        p.health + 10
    );

    p.happiness = Math.min(
        100,
        p.happiness + 10
    );

    log(`🧪 Fertilizer boosted ${PLANTS[p.type].name}!`);

    checkMature(p);

    closeModal();
    saveQuiet();
    render();
}

function removePest(index) {

    const p = game.plants[index];

    if (!p) return;

    p.pest = false;

    p.health = Math.min(
        100,
        p.health + 10
    );

    addXP(5);

    log(`🐞 The pests were removed!`);

    closeModal();
    saveQuiet();
    render();
}

function harvest(index) {

    const p = game.plants[index];

    if (!p || !p.mature) return;

    const data = PLANTS[p.type];

    let value = data.sell;

    if (p.mutation) {
        value *= p.mutation.multiplier;
    }

    value = Math.round(
        value *
        (0.7 + p.health / 250)
    );

    game.coins += value;

    game.stats.harvested++;

    questProgress("coins", value);

    addXP(25);

    log(
        `🧺 You harvested ${data.name} for ${value} coins!`
    );

    game.plants[index] = null;

    checkAchievements();

    closeModal();
    saveQuiet();
    render();
}

/* =========================================================
   GROWTH
   ========================================================= */

function gameTick() {

    game.plants.forEach((p, index) => {

        if (!p) return;

        const data = PLANTS[p.type];
        const weather = WEATHER[game.weather];

        p.water += weather.water * 0.03;
        p.water = Math.max(0, Math.min(100,p.water));

        p.sun += weather.sun * 0.02;
        p.sun = Math.max(0, Math.min(100,p.sun));

        if (p.water < 15) {
            p.health -= 0.4;
        }

        if (p.water > 90 && p.sun < 10) {
            p.health -= 0.1;
        }

        if (p.pest) {
            p.health -= 0.2;
        }

        p.health = Math.max(
            0,
            Math.min(100,p.health)
        );

        const waterIdeal =
            Math.abs(p.water - data.water);

        const sunIdeal =
            Math.abs(p.sun - data.sun);

        let growthSpeed = 0.45;

        if (waterIdeal < 20) {
            growthSpeed += 0.15;
        }

        if (sunIdeal < 20) {
            growthSpeed += 0.15;
        }

        if (p.health < 50) {
            growthSpeed *= 0.5;
        }

        if (p.pest) {
            growthSpeed *= 0.65;
        }

        if (p.mature) return;

        p.progress += growthSpeed;

        if (
            Math.random() < 0.0008 &&
            !p.mutation
        ) {
            attemptMutation(p);
        }

        checkMature(p);
    });

    renderGarden();
    saveQuiet();
}

function checkMature(p) {

    if (
        !p.mature &&
        p.progress >= p.grow
    ) {

        p.progress = p.grow;
        p.mature = true;

        game.stats.grown++;

        questProgress("grow",1);

        addXP(30);

        log(
            `🌸 Your ${PLANTS[p.type].name} has fully grown!`
        );

        if (Math.random() < 0.04) {
            attemptMutation(p);
        }

        checkAchievements();
    }
}

function attemptMutation(p) {

    if (p.mutation) return;

    const mutation =
        MUTATIONS[
            Math.floor(
                Math.random() * MUTATIONS.length
            )
        ];

    p.mutation = mutation;

    game.stats.mutations++;

    log(
        `${mutation.emoji} AMAZING! Your ${PLANTS[p.type].name} mutated into a ${mutation.name} plant!`
    );

    addXP(75);

    checkAchievements();
}

/* =========================================================
   WEATHER / EVENTS
   ========================================================= */

function changeWeather() {

    game.weather =
        Math.floor(
            Math.random() * WEATHER.length
        );

    const w = WEATHER[game.weather];

    log(
        `${w.icon} The weather changed to ${w.name}.`
    );

    if (w.name === "Rain") {
        game.plants.forEach(p => {
            if (p) {
                p.water = Math.min(
                    100,
                    p.water + 20
                );
            }
        });

        log("🌧️ Rain watered your garden!");
    }

    if (w.name === "Rainbow") {
        log("🌈 Something feels lucky...");
    }

    render();
}

function randomGardenEvent() {

    const events = [
        () => {
            const amount = 25 + Math.floor(Math.random()*75);
            game.coins += amount;
            log(`🪙 You found ${amount} coins buried in the soil!`);
            addXP(10);
        },

        () => {
            let count = 0;

            game.plants.forEach(p => {
                if (p) {
                    p.happiness = Math.min(
                        100,
                        p.happiness + 20
                    );
                    count++;
                }
            });

            log(`🦋 Butterflies visited ${count} plants!`);
        },

        () => {
            game.plants.forEach(p => {
                if (p && Math.random() < .25) {
                    p.pest = true;
                }
            });

            log("🐛 A pest outbreak has started!");
        },

        () => {
            game.plants.forEach(p => {
                if (p) {
                    p.progress += 20;
                    checkMature(p);
                }
            });

            log("✨ A magical growth wave passed through the garden!");
        },

        () => {
            game.inventory.moonflower =
                (game.inventory.moonflower || 0) + 1;

            if (!game.unlockedSeeds.includes("moonflower")) {
                game.unlockedSeeds.push("moonflower");
            }

            log("🌙 A mysterious Moonflower seed appeared!");
        },

        () => {
            game.coins += 200;
            log("🍀 Lucky day! You received 200 coins!");
        }
    ];

    events[
        Math.floor(
            Math.random() * events.length
        )
    ]();

    checkAchievements();
    saveQuiet();
    render();
}

/* =========================================================
   SHOP
   ========================================================= */

function openShop() {

    document.getElementById("modalTitle").textContent =
        "🏪 Garden Shop";

    let html = `
        <div class="shopGrid">
    `;

    Object.entries(PLANTS).forEach(([id,p]) => {

        const unlocked =
            game.unlockedSeeds.includes(id);

        html += `
            <div class="shopItem">
                <div class="big">
                    ${p.emoji} ${p.name}
                </div>

                <div class="small">
                    ${p.rarity}
                </div>

                <p>🪙 ${p.cost}</p>

                <button
                    ${unlocked ? "" : "disabled"}
                    onclick="buySeed('${id}')"
                >
                    Buy Seed
                </button>
            </div>
        `;
    });

    html += `
        </div>

        <hr style="border-color:#35563e">

        <h3>🧪 Supplies</h3>

        <button onclick="buyFertilizer()">
            🧪 Fertilizer · 40 coins
        </button>

        <button onclick="buyMysterySeed()">
            ❓ Mystery Seed · 150 coins
        </button>
    `;

    document.getElementById("modalContent").innerHTML =
        html;

    openModal();
}

function buySeed(id) {

    const p = PLANTS[id];

    if (game.coins < p.cost) {
        toast("Not enough coins.");
        return;
    }

    game.coins -= p.cost;

    game.inventory[id] =
        (game.inventory[id] || 0) + 1;

    log(`🌰 Bought a ${p.name} seed.`);

    render();
    openShop();
    saveQuiet();
}

function buyFertilizer() {

    if (game.coins < 40) {
        toast("Not enough coins.");
        return;
    }

    game.coins -= 40;

    game.inventory.fertilizer =
        (game.inventory.fertilizer || 0) + 1;

    log("🧪 Bought fertilizer.");

    openShop();
    render();
    saveQuiet();
}

function buyMysterySeed() {

    if (game.coins < 150) {
        toast("Not enough coins.");
        return;
    }

    game.coins -= 150;

    const choices = Object.keys(PLANTS);

    const id =
        choices[
            Math.floor(
                Math.random() * choices.length
            )
        ];

    game.inventory[id] =
        (game.inventory[id] || 0) + 1;

    if (!game.unlockedSeeds.includes(id)) {
        game.unlockedSeeds.push(id);
        log(`❓ Mystery Seed revealed: ${PLANTS[id].name}!`);
    } else {
        log(`❓ Mystery Seed contained a ${PLANTS[id].name} seed!`);
    }

    openShop();
    render();
    saveQuiet();
}

/* =========================================================
   COLLECTION
   ========================================================= */

function openCollection() {

    document.getElementById("modalTitle").textContent =
        "📖 Plant Encyclopedia";

    let html = `<div class="collectionGrid">`;

    Object.entries(PLANTS).forEach(([id,p]) => {

        const found =
            game.discovered.includes(id);

        html += `
            <div class="collectionItem ${found ? "" : "locked"}">
                <div class="big">
                    ${found ? p.emoji : "❓"}
                </div>

                <strong>
                    ${found ? p.name : "Unknown Plant"}
                </strong>

                <div class="small">
                    ${
                        found
                        ? `${p.rarity} · 💰 ${p.sell}`
                        : "Not discovered"
                    }
                </div>
            </div>
        `;
    });

    html += `</div>`;

    document.getElementById("modalContent").innerHTML =
        html;

    openModal();
}

/* =========================================================
   ACHIEVEMENTS
   ========================================================= */

function openAchievements() {

    document.getElementById("modalTitle").textContent =
        "🏆 Achievements";

    let html = `<div class="collectionGrid">`;

    ACHIEVEMENTS.forEach(a => {

        const unlocked =
            game.achievements.includes(a[0]);

        html += `
            <div class="collectionItem ${unlocked ? "" : "locked"}">
                <div class="big">
                    ${a[1].split(" ")[0]}
                </div>

                <strong>
                    ${a[1]}
                </strong>

                <div class="small">
                    ${a[2]}
                </div>

                <div class="small">
                    Reward: ${a[3]} XP
                </div>
            </div>
        `;
    });

    html += `</div>`;

    document.getElementById("modalContent").innerHTML =
        html;

    openModal();
}

function checkAchievements() {

    const checks = {
        firstPlant:
            game.stats.grown >= 1,

        tenPlants:
            game.stats.grown >= 10,

        fiftyPlants:
            game.stats.grown >= 50,

        mutation:
            game.stats.mutations >= 1,

        rare:
            game.discovered.some(id =>
                ["cactus","mushroom","moonflower","crystal","rainbow","ancient"]
                .includes(id)
            ),

        million:
            game.coins >= 1000,

        collector:
            game.discovered.length >= 8,

        night:
            game.discovered.includes("moonflower"),

        legend:
            game.discovered.includes("crystal") ||
            game.discovered.includes("rainbow") ||
            game.discovered.includes("ancient"),

        garden10:
            game.gardenLevel >= 10
    };

    ACHIEVEMENTS.forEach(a => {

        const id = a[0];

        if (
            checks[id] &&
            !game.achievements.includes(id)
        ) {

            game.achievements.push(id);

            addXP(a[3]);

            log(
                `🏆 Achievement unlocked: ${a[1]}!`
            );

            toast(`🏆 ${a[1]}`);
        }
    });
}

/* =========================================================
   XP / LEVELING
   ========================================================= */

function addXP(amount) {

    game.xp += amount;

    while (
        game.xp >= xpNeeded(game.level)
    ) {

        game.xp -= xpNeeded(game.level);

        game.level++;

        game.gardenLevel =
            Math.max(
                game.gardenLevel,
                Math.floor(game.level / 2) + 1
            );

        const reward =
            50 + game.level * 15;

        game.coins += reward;

        log(
            `🎉 Level ${game.level}! You received ${reward} coins.`
        );
    }
}

function xpNeeded(level) {
    return 100 + level * 50;
}

/* =========================================================
   QUESTS
   ========================================================= */

function questProgress(type, amount) {

    game.quests.forEach(q => {

        if (q.done || q.type !== type) return;

        q.progress += amount;

        if (q.progress >= q.goal) {

            q.progress = q.goal;
            q.done = true;

            game.coins += q.reward;

            addXP(30);

            log(
                `🎯 Quest complete: ${q.title}! +${q.reward} coins`
            );
        }
    });
}

/* =========================================================
   BUTTONS
   ========================================================= */

document.getElementById("waterAll")
.addEventListener("click", () => {

    let count = 0;

    game.plants.forEach(p => {

        if (!p) return;

        p.water = Math.min(
            100,
            p.water + 25
        );

        count++;
    });

    questProgress("water",count);

    log(`💧 Watered ${count} plants.`);

    render();
    saveQuiet();
});

document.getElementById("sunAll")
.addEventListener("click", () => {

    let count = 0;

    game.plants.forEach(p => {

        if (!p) return;

        p.sun = Math.min(
            100,
            p.sun + 20
        );

        count++;
    });

    log(`☀️ Gave ${count} plants sunlight.`);

    render();
    saveQuiet();
});

document.getElementById("randomEvent")
.addEventListener("click", () => {
    randomGardenEvent();
});

/* =========================================================
   MODAL
   ========================================================= */

function openModal() {
    document.getElementById("modal")
        .classList.add("open");
}

function closeModal() {
    document.getElementById("modal")
        .classList.remove("open");
}

document.getElementById("modal")
.addEventListener("click", e => {

    if (e.target.id === "modal") {
        closeModal();
    }
});

/* =========================================================
   RESET
   ========================================================= */

function resetGame() {

    const yes =
        confirm(
            "Are you sure you want to delete your garden? This cannot be undone."
        );

    if (!yes) return;

    localStorage.removeItem(SAVE_KEY);

    location.reload();
}

/* =========================================================
   SAVE
   ========================================================= */

function saveQuiet() {

    game.lastSave = Date.now();

    try {
        localStorage.setItem(
            SAVE_KEY,
            JSON.stringify(game)
        );
    } catch(e) {
        console.warn("Could not save game.", e);
    }
}

/* =========================================================
   DAY SYSTEM
   ========================================================= */

let lastSecond = Date.now();
let dayTimer = 0;

function loop() {

    const now = Date.now();

    const delta =
        Math.min(
            10,
            (now - lastSecond) / 1000
        );

    lastSecond = now;

    dayTimer += delta;

    if (dayTimer >= 10) {

        dayTimer = 0;

        gameTick();

        game.weatherTicks++;

        if (
            game.weatherTicks >= 30
        ) {

            game.weatherTicks = 0;

            game.day++;

            changeWeather();

            if (Math.random() < .3) {
                randomGardenEvent();
            }

            checkAchievements();
        }
    }

    requestAnimationFrame(loop);
}

/* =========================================================
   START
   ========================================================= */

render();

setInterval(() => {
    saveQuiet();
}, 10000);

loop();

window.addEventListener("beforeunload", () => {
    saveQuiet();
});

</script>

</body>
</html>
