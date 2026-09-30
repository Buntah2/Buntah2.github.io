# Buntah2.github.io
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Plant Life 🌱</title>

<style>
    * {
        box-sizing: border-box;
    }

    body {
        margin: 0;
        font-family: Arial, sans-serif;
        background: linear-gradient(#bde7ff, #dff7d8);
        color: #24352a;
    }

    header {
        background: #4d9b62;
        color: white;
        padding: 18px;
        text-align: center;
        box-shadow: 0 3px 10px #0003;
    }

    header h1 {
        margin: 0;
        font-size: 32px;
    }

    .stats {
        display: flex;
        justify-content: center;
        gap: 15px;
        flex-wrap: wrap;
        margin-top: 10px;
    }

    .stat {
        background: #ffffff33;
        padding: 8px 14px;
        border-radius: 20px;
    }

    main {
        max-width: 1100px;
        margin: auto;
        padding: 20px;
    }

    .garden {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
        gap: 18px;
    }

    .plant-card {
        background: #ffffffdd;
        border-radius: 18px;
        padding: 18px;
        text-align: center;
        box-shadow: 0 5px 15px #0002;
        transition: transform 0.2s;
    }

    .plant-card:hover {
        transform: translateY(-4px);
    }

    .plant {
        font-size: 80px;
        margin: 15px;
    }

    .bar-container {
        margin: 10px 0;
        text-align: left;
    }

    .bar {
        height: 14px;
        background: #ddd;
        border-radius: 20px;
        overflow: hidden;
    }

    .bar-fill {
        height: 100%;
        transition: width 0.4s;
    }

    .water {
        background: #3498db;
    }

    .sun {
        background: #f1c40f;
    }

    .health {
        background: #2ecc71;
    }

    button {
        border: none;
        border-radius: 10px;
        padding: 10px 14px;
        margin: 4px;
        cursor: pointer;
        font-weight: bold;
        transition: transform 0.15s, opacity 0.15s;
    }

    button:hover {
        transform: scale(1.05);
    }

    button:active {
        transform: scale(.96);
    }

    .water-btn {
        background: #3498db;
        color: white;
    }

    .sun-btn {
        background: #f1c40f;
        color: #333;
    }

    .buy-btn {
        background: #9b59b6;
        color: white;
    }

    .panel {
        background: #ffffffdd;
        border-radius: 18px;
        padding: 20px;
        margin-bottom: 20px;
        box-shadow: 0 5px 15px #0002;
    }

    .shop {
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
    }

    .shop-item {
        background: #f5f5f5;
        border-radius: 12px;
        padding: 12px;
        min-width: 150px;
    }

    .achievement {
        display: inline-block;
        background: #fff4b8;
        padding: 10px;
        margin: 5px;
        border-radius: 10px;
    }

    .locked {
        opacity: .45;
    }

    #event {
        text-align: center;
        font-weight: bold;
        min-height: 25px;
        color: #7d4e00;
    }

    @media (max-width: 600px) {
        header h1 {
            font-size: 25px;
        }

        .plant {
            font-size: 65px;
        }
    }
</style>
</head>

<body>

<header>
    <h1>🌱 Plant Life</h1>

    <div class="stats">
        <div class="stat">🪙 Coins: <span id="coins">0</span></div>
        <div class="stat">⭐ Level: <span id="level">1</span></div>
        <div class="stat">✨ XP: <span id="xp">0</span></div>
    </div>
</header>

<main>

    <div class="panel">
        <h2>🌦️ Garden Status</h2>
        <p id="weather">☀️ Sunny day</p>
        <p id="event"></p>
    </div>

    <div class="panel">
        <h2>🌿 Your Garden</h2>
        <div id="garden" class="garden"></div>
    </div>

    <div class="panel">
        <h2>🛒 Seed Shop</h2>
        <div id="shop" class="shop"></div>
    </div>

    <div class="panel">
        <h2>🏆 Achievements</h2>
        <div id="achievements"></div>
    </div>

    <div class="panel">
        <h2>📊 Garden Statistics</h2>
        <p>🌱 Plants grown: <span id="plantsGrown">0</span></p>
        <p>💧 Times watered: <span id="watered">0</span></p>
        <p>☀️ Times given sunlight: <span id="sunlight">0</span></p>
        <p>🌧️ Random events: <span id="events">0</span></p>
    </div>

</main>

<script>
/* =====================================================
   PLANT LIFE
   ===================================================== */

const plantTypes = {
    sunflower: {
        name: "Sunflower",
        emoji: "🌻",
        price: 0,
        growthSpeed: 1.0,
        waterNeed: 1.0,
        sunNeed: 1.3
    },

    cactus: {
        name: "Cactus",
        emoji: "🌵",
        price: 50,
        growthSpeed: 0.7,
        waterNeed: 0.35,
        sunNeed: 1.2
    },

    tulip: {
        name: "Tulip",
        emoji: "🌷",
        price: 100,
        growthSpeed: 1.2,
        waterNeed: 1.2,
        sunNeed: 0.9
    },

    rose: {
        name: "Rose",
        emoji: "🌹",
        price: 175,
        growthSpeed: 0.8,
        waterNeed: 1.1,
        sunNeed: 1.0
    },

    lavender: {
        name: "Lavender",
        emoji: "🪻",
        price: 250,
        growthSpeed: 0.9,
        waterNeed: 0.8,
        sunNeed: 1.0
    },

    clover: {
        name: "Clover",
        emoji: "🍀",
        price: 350,
        growthSpeed: 1.4,
        waterNeed: 1.0,
        sunNeed: 0.7
    },

    palm: {
        name: "Mini Palm",
        emoji: "🌴",
        price: 500,
        growthSpeed: 0.6,
        waterNeed: 1.3,
        sunNeed: 1.4
    }
};


/* =====================================================
   GAME STATE
   ===================================================== */

let game = {
    coins: 100,
    xp: 0,
    level: 1,

    plantsGrown: 0,
    watered: 0,
    sunlight: 0,
    events: 0,

    plants: [
        createPlant("sunflower")
    ],

    unlocked: ["sunflower"],

    lastPlayed: Date.now(),

    achievements: []
};


/* =====================================================
   CREATE PLANT
   ===================================================== */

function createPlant(type) {
    return {
        id: Date.now() + Math.random(),

        type: type,

        water: 75,
        sunlight: 75,
        health: 100,

        growth: 0,

        stage: 0,

        mature: false
    };
}


/* =====================================================
   LOAD SAVE
   ===================================================== */

const saved = localStorage.getItem("plantLifeSave");

if (saved) {
    try {
        game = JSON.parse(saved);

        applyOfflineProgress();
    } catch {
        console.log("Save file was corrupted.");
    }
}


/* =====================================================
   SAVE
   ===================================================== */

function saveGame() {
    game.lastPlayed = Date.now();

    localStorage.setItem(
        "plantLifeSave",
        JSON.stringify(game)
    );
}


/* =====================================================
   OFFLINE PROGRESS
   ===================================================== */

function applyOfflineProgress() {

    const now = Date.now();

    const elapsed =
        (now - game.lastPlayed) / 1000;

    /*
       Plants lose resources while you're away.
       The calculation is capped so extremely long
       absences don't completely destroy the garden.
    */

    const hours =
        Math.min(elapsed / 3600, 24);

    game.plants.forEach(plant => {

        plant.water -= hours * 4;
        plant.sunlight -= hours * 3;

        plant.water =
            Math.max(0, plant.water);

        plant.sunlight =
            Math.max(0, plant.sunlight);

        updatePlantHealth(plant);

        if (
            plant.water > 30 &&
            plant.sunlight > 30 &&
            plant.health > 20
        ) {
            growPlant(
                plant,
                hours * 4
            );
        }
    });
}


/* =====================================================
   PLANT HEALTH
   ===================================================== */

function updatePlantHealth(plant) {

    if (plant.water < 15) {
        plant.health -= 3;
    }

    if (plant.sunlight < 15) {
        plant.health -= 2;
    }

    if (plant.water > 30 && plant.sunlight > 30) {
        plant.health += 1;
    }

    plant.health =
        Math.max(
            0,
            Math.min(100, plant.health)
        );
}


/* =====================================================
   GROW PLANT
   ===================================================== */

function growPlant(plant, amount = 1) {

    const info =
        plantTypes[plant.type];

    if (plant.mature) return;

    if (
        plant.water < 20 ||
        plant.sunlight < 20 ||
        plant.health < 20
    ) {
        return;
    }

    const quality =
        (
            plant.water +
            plant.sunlight +
            plant.health
        ) / 300;

    plant.growth +=
        amount *
        info.growthSpeed *
        quality;

    if (plant.growth >= 100) {

        plant.growth = 100;

        plant.mature = true;

        plant.stage = 4;

        game.plantsGrown++;

        game.coins += 50;

        addXP(50);

        checkAchievements();
    }

    else {

        plant.stage =
            Math.floor(
                plant.growth / 25
            );
    }
}


/* =====================================================
   WATER
   ===================================================== */

function waterPlant(id) {

    const plant =
        game.plants.find(
            p => p.id === id
        );

    if (!plant) return;

    plant.water =
        Math.min(
            100,
            plant.water + 30
        );

    game.watered++;

    addXP(3);

    checkAchievements();

    saveGame();

    render();
}


/* =====================================================
   SUNLIGHT
   ===================================================== */

function giveSunlight(id) {

    const plant =
        game.plants.find(
            p => p.id === id
        );

    if (!plant) return;

    plant.sunlight =
        Math.min(
            100,
            plant.sunlight + 25
        );

    game.sunlight++;

    addXP(3);

    checkAchievements();

    saveGame();

    render();
}


/* =====================================================
   GAME TICK
   ===================================================== */

setInterval(() => {

    game.plants.forEach(plant => {

        const info =
            plantTypes[plant.type];

        plant.water -=
            0.35 * info.waterNeed;

        plant.sunlight -=
            0.25 * info.sunNeed;

        plant.water =
            Math.max(0, plant.water);

        plant.sunlight =
            Math.max(0, plant.sunlight);

        updatePlantHealth(plant);

        growPlant(plant, 0.12);

    });

    randomEvent();

    saveGame();

    render();

}, 10000);


/* =====================================================
   BUY PLANT
   ===================================================== */

function buyPlant(type) {

    const info =
        plantTypes[type];

    if (game.coins < info.price) {

        showEvent(
            "❌ You don't have enough coins!"
        );

        return;
    }

    game.coins -= info.price;

    game.plants.push(
        createPlant(type)
    );

    if (!game.unlocked.includes(type)) {
        game.unlocked.push(type);
    }

    addXP(10);

    saveGame();

    render();
}


/* =====================================================
   XP
   ===================================================== */

function addXP(amount) {

    game.xp += amount;

    const required =
        game.level * 100;

    if (game.xp >= required) {

        game.xp -= required;

        game.level++;

        game.coins +=
            game.level * 25;

        showEvent(
            `🎉 Level up! You reached level ${game.level}!`
        );
    }
}


/* =====================================================
   RANDOM EVENTS
   ===================================================== */

function randomEvent() {

    if (Math.random() > 0.08) {
        return;
    }

    game.events++;

    const events = [

        () => {

            game.plants.forEach(p => {
                p.water =
                    Math.min(
                        100,
                        p.water + 20
                    );
            });

            showEvent(
                "🌧️ It started raining! Your plants got watered."
            );
        },

        () => {

            game.plants.forEach(p => {
                p.sunlight =
                    Math.min(
                        100,
                        p.sunlight + 15
                    );
            });

            showEvent(
                "☀️ A sunny day boosted your plants!"
            );
        },

        () => {

            game.coins += 30;

            showEvent(
                "🐝 A friendly bee visited and brought you 30 coins!"
            );
        },

        () => {

            game.plants.forEach(p => {
                p.growth =
                    Math.min(
                        100,
                        p.growth + 5
                    );
            });

            showEvent(
                "🌈 A magical rainbow appeared! Your plants grew!"
            );
        },

        () => {

            const plant =
                game.plants[
                    Math.floor(
                        Math.random() *
                        game.plants.length
                    )
                ];

            if (plant) {
                plant.health =
                    Math.min(
                        100,
                        plant.health + 20
                    );
            }

            showEvent(
                "🦋 A butterfly visited your garden!"
            );
        }
    ];

    events[
        Math.floor(
            Math.random() *
            events.length
        )
    ]();

    checkAchievements();
}


/* =====================================================
   ACHIEVEMENTS
   ===================================================== */

const achievementList = [

    {
        id: "first",
        name: "🌱 First Sprout",
        description: "Grow your first plant.",
        check: () =>
            game.plantsGrown >= 1
    },

    {
        id: "water",
        name: "💧 Plant Parent",
        description: "Water plants 25 times.",
        check: () =>
            game.watered >= 25
    },

    {
        id: "sun",
        name: "☀️ Sun Lover",
        description: "Give plants sunlight 25 times.",
        check: () =>
            game.sunlight >= 25
    },

    {
        id: "collector",
        name: "🌿 Collector",
        description: "Own 5 different plant types.",
        check: () =>
            game.unlocked.length >= 5
    },

    {
        id: "rich",
        name: "🪙 Garden Tycoon",
        description: "Have 1,000 coins.",
        check: () =>
            game.coins >= 1000
    },

    {
        id: "level5",
        name: "⭐ Experienced Gardener",
        description: "Reach level 5.",
        check: () =>
            game.level >= 5
    }
];


function checkAchievements() {

    achievementList.forEach(a => {

        if (
            a.check() &&
            !game.achievements.includes(a.id)
        ) {

            game.achievements.push(a.id);

            game.coins += 100;

            showEvent(
                `🏆 Achievement unlocked: ${a.name}`
            );
        }
    });
}


/* =====================================================
   RANDOM EVENT MESSAGE
   ===================================================== */

let eventTimeout;

function showEvent(message) {

    const element =
        document.getElementById("event");

    element.textContent =
        message;

    clearTimeout(eventTimeout);

    eventTimeout =
        setTimeout(() => {
            element.textContent = "";
        }, 5000);
}


/* =====================================================
   PLANT VISUAL
   ===================================================== */

function getPlantVisual(plant) {

    const info =
        plantTypes[plant.type];

    if (plant.health <= 0) {
        return "🥀";
    }

    if (plant.growth < 25) {
        return "🌱";
    }

    if (plant.growth < 50) {
        return "🌿";
    }

    return info.emoji;
}


/* =====================================================
   RENDER GARDEN
   ===================================================== */

function renderGarden() {

    const garden =
        document.getElementById("garden");

    garden.innerHTML = "";

    game.plants.forEach(plant => {

        const info =
            plantTypes[plant.type];

        const card =
            document.createElement("div");

        card.className =
            "plant-card";

        card.innerHTML = `

            <h2>${info.name}</h2>

            <div class="plant">
                ${getPlantVisual(plant)}
            </div>

            <p>
                Growth:
                ${Math.floor(plant.growth)}%
            </p>

            <div class="bar-container">
                💧 Water
                <div class="bar">
                    <div
                        class="bar-fill water"
                        style="width:${plant.water}%">
                    </div>
                </div>
            </div>

            <div class="bar-container">
                ☀️ Sunlight
                <div class="bar">
                    <div
                        class="bar-fill sun"
                        style="width:${plant.sunlight}%">
                    </div>
                </div>
            </div>

            <div class="bar-container">
                ❤️ Health
                <div class="bar">
                    <div
                        class="bar-fill health"
                        style="width:${plant.health}%">
                    </div>
                </div>
            </div>

            <button
                class="water-btn"
                data-water="${plant.id}">
                💧 Water
            </button>

            <button
                class="sun-btn"
                data-sun="${plant.id}">
                ☀️ Sunlight
            </button>

        `;

        garden.appendChild(card);
    });

    document
        .querySelectorAll("[data-water]")
        .forEach(button => {

            button.addEventListener(
                "click",
                () =>
                    waterPlant(
                        Number(
                            button.dataset.water
                        )
                    )
            );
        });

    document
        .querySelectorAll("[data-sun]")
        .forEach(button => {

            button.addEventListener(
                "click",
                () =>
                    giveSunlight(
                        Number(
                            button.dataset.sun
                        )
                    )
            );
        });
}


/* =====================================================
   RENDER SHOP
   ===================================================== */

function renderShop() {

    const shop =
        document.getElementById("shop");

    shop.innerHTML = "";

    Object.entries(
        plantTypes
    ).forEach(([type, info]) => {

        const item =
            document.createElement("div");

        item.className =
            "shop-item";

        item.innerHTML = `

            <h3>
                ${info.emoji}
                ${info.name}
            </h3>

            <p>🪙 ${info.price} coins</p>

            <p>
                Growth:
                ${info.growthSpeed}x
            </p>

            <button
                class="buy-btn"
                data-buy="${type}">
                🌱 Buy
            </button>
        `;

        shop.appendChild(item);
    });

    document
        .querySelectorAll("[data-buy]")
        .forEach(button => {

            button.addEventListener(
                "click",
                () =>
                    buyPlant(
                        button.dataset.buy
                    )
            );
        });
}


/* =====================================================
   RENDER ACHIEVEMENTS
   ===================================================== */

function renderAchievements() {

    const container =
        document.getElementById(
            "achievements"
        );

    container.innerHTML = "";

    achievementList.forEach(a => {

        const unlocked =
            game.achievements.includes(
                a.id
            );

        const element =
            document.createElement("div");

        element.className =
            "achievement " +
            (unlocked ? "" : "locked");

        element.innerHTML = `
            <strong>
                ${unlocked ? a.name : "🔒 Hidden Achievement"}
            </strong>
            <br>
            <small>
                ${a.description}
            </small>
        `;

        container.appendChild(element);
    });
}


/* =====================================================
   RENDER STATS
   ===================================================== */

function renderStats() {

    document.getElementById(
        "coins"
    ).textContent = game.coins;

    document.getElementById(
        "level"
    ).textContent = game.level;

    document.getElementById(
        "xp"
    ).textContent = game.xp;

    document.getElementById(
        "plantsGrown"
    ).textContent = game.plantsGrown;

    document.getElementById(
        "watered"
    ).textContent = game.watered;

    document.getElementById(
        "sunlight"
    ).textContent = game.sunlight;

    document.getElementById(
        "events"
    ).textContent = game.events;
}


/* =====================================================
   WEATHER
   ===================================================== */

function renderWeather() {

    const weather =
        document.getElementById(
            "weather"
        );

    const choices = [
        "☀️ Sunny day",
        "⛅ Partly cloudy",
        "🌤️ Warm afternoon",
        "🌥️ Cloudy day"
    ];

    weather.textContent =
        choices[
            Math.floor(
                Math.random() *
                choices.length
            )
        ];
}


/* =====================================================
   MAIN RENDER
   ===================================================== */

function render() {

    renderGarden();

    renderShop();

    renderAchievements();

    renderStats();
}


/* =====================================================
   START GAME
   ===================================================== */

render();

renderWeather();

saveGame();

</script>

</body>
</html>
