```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🌱 3D Plant Life</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    overflow: hidden;
    font-family: Arial, sans-serif;
    background: #87ceeb;
}

#game {
    width: 100vw;
    height: 100vh;
}

#ui {
    position: fixed;
    inset: 0;
    pointer-events: none;
    color: white;
}

.panel {
    pointer-events: auto;
    background: rgba(20, 45, 30, 0.88);
    backdrop-filter: blur(8px);
    border: 1px solid rgba(255,255,255,.15);
    border-radius: 16px;
    padding: 15px;
    box-shadow: 0 8px 30px rgba(0,0,0,.25);
}

#topbar {
    position: absolute;
    top: 15px;
    left: 15px;
    right: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 10px;
}

#title {
    font-size: 22px;
    font-weight: bold;
}

.stats {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
}

.stat {
    background: rgba(255,255,255,.12);
    padding: 8px 12px;
    border-radius: 10px;
}

#shop {
    position: absolute;
    left: 15px;
    bottom: 15px;
    width: 260px;
}

#selected {
    position: absolute;
    right: 15px;
    bottom: 15px;
    width: 290px;
}

h2, h3, p {
    margin-top: 0;
}

button {
    border: 0;
    padding: 10px 13px;
    margin: 4px;
    border-radius: 9px;
    font-weight: bold;
    cursor: pointer;
    background: #7bd66f;
    color: #17351b;
}

button:hover {
    filter: brightness(1.1);
}

button:disabled {
    opacity: .45;
    cursor: not-allowed;
}

.bar {
    height: 12px;
    background: rgba(255,255,255,.18);
    border-radius: 20px;
    overflow: hidden;
    margin: 5px 0 10px;
}

.fill {
    height: 100%;
    width: 50%;
    transition: width .3s;
}

.water {
    background: #4bbcff;
}

.sun {
    background: #ffd447;
}

.health {
    background: #5fe27b;
}

#message {
    position: absolute;
    top: 85px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(20,45,30,.9);
    padding: 12px 18px;
    border-radius: 12px;
    opacity: 0;
    transition: opacity .3s;
}

#help {
    position: absolute;
    top: 95px;
    left: 15px;
    font-size: 13px;
    opacity: .8;
}

@media (max-width: 700px) {
    #topbar {
        flex-direction: column;
        align-items: stretch;
    }

    #shop,
    #selected {
        width: calc(50% - 22px);
    }

    #help {
        display: none;
    }
}

@media (max-width: 500px) {
    #shop,
    #selected {
        width: calc(100% - 30px);
    }

    #shop {
        bottom: 15px;
    }

    #selected {
        bottom: 180px;
    }
}
</style>
</head>

<body>

<div id="game"></div>

<div id="ui">

    <div id="topbar">

        <div class="panel">
            <div id="title">🌱 3D Plant Life</div>
            <small>Take care of your garden</small>
        </div>

        <div class="stats panel">
            <div class="stat">🪙 <span id="coins">100</span></div>
            <div class="stat">⭐ Lv <span id="level">1</span></div>
            <div class="stat">🌱 <span id="plantCount">1</span></div>
        </div>

    </div>

    <div id="help">
        🖱️ Drag to look around • Scroll to zoom • Click a plant to select it
    </div>

    <div id="message"></div>

    <div id="shop" class="panel">
        <h3>🌿 Seed Shop</h3>

        <button onclick="buyPlant('sunflower')">
            🌻 Sunflower — Free
        </button>

        <button onclick="buyPlant('cactus')">
            🌵 Cactus — 50
        </button>

        <button onclick="buyPlant('tulip')">
            🌷 Tulip — 100
        </button>

        <button onclick="buyPlant('rose')">
            🌹 Rose — 175
        </button>

        <button onclick="buyPlant('tree')">
            🌳 Tree — 300
        </button>
    </div>

    <div id="selected" class="panel">
        <h3>🌱 Select a plant</h3>
        <p>Click a plant in your garden.</p>
    </div>

</div>

<script type="module">

import * as THREE from
    "https://cdn.jsdelivr.net/npm/three@0.180.0/build/three.module.js";

import { OrbitControls } from
    "https://cdn.jsdelivr.net/npm/three@0.180.0/examples/jsm/controls/OrbitControls.js";


/* =====================================================
   GAME DATA
===================================================== */

const PLANTS = {

    sunflower: {
        name: "Sunflower",
        emoji: "🌻",
        cost: 0,
        growth: 1.0,
        waterNeed: 1.0,
        sunNeed: 1.2,
        color: 0xffd21f
    },

    cactus: {
        name: "Cactus",
        emoji: "🌵",
        cost: 50,
        growth: .7,
        waterNeed: .3,
        sunNeed: 1.3,
        color: 0x55aa55
    },

    tulip: {
        name: "Tulip",
        emoji: "🌷",
        cost: 100,
        growth: 1.2,
        waterNeed: 1.1,
        sunNeed: .9,
        color: 0xff6688
    },

    rose: {
        name: "Rose",
        emoji: "🌹",
        cost: 175,
        growth: .85,
        waterNeed: 1.1,
        sunNeed: 1,
        color: 0xdd3355
    },

    tree: {
        name: "Tree",
        emoji: "🌳",
        cost: 300,
        growth: .5,
        waterNeed: 1.4,
        sunNeed: 1.1,
        color: 0x3d9b46
    }
};


/* =====================================================
   SAVE DATA
===================================================== */

let save = {

    coins: 100,

    level: 1,

    xp: 0,

    plants: [],

    lastPlayed: Date.now()
};


const stored =
    localStorage.getItem("3DPlantLife");

if (stored) {

    try {

        save = JSON.parse(stored);

    } catch {

        console.log("Could not load save.");
    }
}


/* =====================================================
   THREE.JS
===================================================== */

const scene =
    new THREE.Scene();

scene.background =
    new THREE.Color(0x87ceeb);

scene.fog =
    new THREE.Fog(0x87ceeb, 25, 70);


const camera =
    new THREE.PerspectiveCamera(
        60,
        innerWidth / innerHeight,
        .1,
        200
    );

camera.position.set(
    10,
    10,
    15
);


const renderer =
    new THREE.WebGLRenderer({
        antialias: true
    });

renderer.setSize(
    innerWidth,
    innerHeight
);

renderer.setPixelRatio(
    Math.min(devicePixelRatio, 2)
);

renderer.shadowMap.enabled = true;

document
    .getElementById("game")
    .appendChild(renderer.domElement);


const controls =
    new OrbitControls(
        camera,
        renderer.domElement
    );

controls.target.set(
    0,
    1,
    0
);

controls.enableDamping = true;

controls.minDistance = 6;

controls.maxDistance = 35;


/* =====================================================
   LIGHTING
===================================================== */

const ambient =
    new THREE.HemisphereLight(
        0xbdeeff,
        0x315b2f,
        2
    );

scene.add(ambient);


const sun =
    new THREE.DirectionalLight(
        0xffffff,
        3
    );

sun.position.set(
    10,
    20,
    10
);

sun.castShadow = true;

scene.add(sun);


/* =====================================================
   GROUND
===================================================== */

const groundGeometry =
    new THREE.PlaneGeometry(
        60,
        60
    );

const groundMaterial =
    new THREE.MeshStandardMaterial({
        color: 0x55a84e
    });

const ground =
    new THREE.Mesh(
        groundGeometry,
        groundMaterial
    );

ground.rotation.x =
    -Math.PI / 2;

ground.receiveShadow = true;

scene.add(ground);


/* =====================================================
   PATH
===================================================== */

const pathMaterial =
    new THREE.MeshStandardMaterial({
        color: 0xc5a16b
    });

const path =
    new THREE.Mesh(
        new THREE.PlaneGeometry(
            5,
            60
        ),
        pathMaterial
    );

path.rotation.x =
    -Math.PI / 2;

path.position.y =
    .01;

scene.add(path);


/* =====================================================
   GARDEN
===================================================== */

const gardenGroup =
    new THREE.Group();

scene.add(gardenGroup);


/* =====================================================
   PLANT OBJECTS
===================================================== */

const plantMeshes =
    new Map();


function createPot() {

    const pot =
        new THREE.Group();

    const body =
        new THREE.Mesh(
            new THREE.CylinderGeometry(
                .55,
                .42,
                .7,
                16
            ),
            new THREE.MeshStandardMaterial({
                color: 0xb85f35
            })
        );

    body.position.y =
        .35;

    body.castShadow = true;

    pot.add(body);


    const dirt =
        new THREE.Mesh(
            new THREE.CylinderGeometry(
                .43,
                .43,
                .05,
                16
            ),
            new THREE.MeshStandardMaterial({
                color: 0x51351e
            })
        );

    dirt.position.y =
        .71;

    pot.add(dirt);

    return pot;
}


function createPlantMesh(type) {

    const info =
        PLANTS[type];

    const group =
        new THREE.Group();


    /* STEM */

    const stem =
        new THREE.Mesh(
            new THREE.CylinderGeometry(
                .07,
                .09,
                1.4,
                8
            ),
            new THREE.MeshStandardMaterial({
                color: 0x3d873c
            })
        );

    stem.position.y =
        1.4;

    stem.castShadow = true;

    group.add(stem);


    /* LEAVES */

    for (
        let i = 0;
        i < 4;
        i++
    ) {

        const leaf =
            new THREE.Mesh(
                new THREE.SphereGeometry(
                    .35,
                    10,
                    8
                ),
                new THREE.MeshStandardMaterial({
                    color: 0x45a84b
                })
            );

        const angle =
            i * Math.PI / 2;

        leaf.position.set(
            Math.cos(angle) * .35,
            1.35 + i * .08,
            Math.sin(angle) * .35
        );

        leaf.scale.set(
            1.5,
            .35,
            .7
        );

        leaf.rotation.y =
            angle;

        leaf.castShadow = true;

        group.add(leaf);
    }


    /* FLOWER / TREE TOP */

    if (type === "tree") {

        const crown =
            new THREE.Mesh(
                new THREE.SphereGeometry(
                    1.25,
                    16,
                    12
                ),
                new THREE.MeshStandardMaterial({
                    color: 0x3e9c48
                })
            );

        crown.position.y =
            2.8;

        crown.castShadow = true;

        group.add(crown);

    } else {

        const flower =
            new THREE.Mesh(
                new THREE.SphereGeometry(
                    .5,
                    12,
                    8
                ),
                new THREE.MeshStandardMaterial({
                    color: info.color
                })
            );

        flower.position.y =
            2.15;

        flower.castShadow = true;

        group.add(flower);
    }


    return group;
}


/* =====================================================
   ADD PLANT
===================================================== */

function addPlant(type, data = null) {

    const plant = {

        id:
            data?.id ??
            crypto.randomUUID(),

        type,

        x:
            data?.x ??
            (Math.random() * 18 - 9),

        z:
            data?.z ??
            (Math.random() * 18 - 9),

        water:
            data?.water ?? 75,

        sunlight:
            data?.sunlight ?? 75,

        health:
            data?.health ?? 100,

        growth:
            data?.growth ?? 0,

        mature:
            data?.mature ?? false
    };


    save.plants.push(plant);


    const pot =
        createPot();

    pot.position.set(
        plant.x,
        0,
        plant.z
    );

    gardenGroup.add(pot);


    const plantMesh =
        createPlantMesh(type);

    plantMesh.position.y =
        .72;

    pot.add(plantMesh);


    plantMeshes.set(
        plant.id,
        {
            pot,
            plantMesh,
            data: plant
        }
    );


    updatePlantVisual(
        plant
    );
}


/* =====================================================
   PLANT VISUAL GROWTH
===================================================== */

function updatePlantVisual(plant) {

    const object =
        plantMeshes.get(
            plant.id
        );

    if (!object) return;


    const scale =
        .25 +
        Math.min(
            plant.growth,
            100
        ) / 100 * .9;


    object.plantMesh.scale.set(
        scale,
        scale,
        scale
    );


    if (plant.health < 25) {

        object.plantMesh.rotation.z =
            Math.sin(
                Date.now() * .002
            ) * .08;

    } else {

        object.plantMesh.rotation.z =
            0;
    }
}


/* =====================================================
   DEFAULT PLANT
===================================================== */

if (
    save.plants.length === 0
) {

    addPlant(
        "sunflower",
        {
            x: 0,
            z: 0
        }
    );
}


/* =====================================================
   RESTORE SAVED PLANTS
===================================================== */

else {

    const oldPlants =
        [...save.plants];

    save.plants = [];

    oldPlants.forEach(
        plant =>
            addPlant(
                plant.type,
                plant
            )
    );
}


/* =====================================================
   OFFLINE PROGRESS
===================================================== */

const elapsed =
    Math.min(
        (Date.now() - save.lastPlayed)
        / 1000,
        60 * 60 * 24
    );

const hours =
    elapsed / 3600;


save.plants.forEach(
    plant => {

        plant.water =
            Math.max(
                0,
                plant.water -
                hours * 4
            );

        plant.sunlight =
            Math.max(
                0,
                plant.sunlight -
                hours * 3
            );

        growPlant(
            plant,
            hours * 5
        );
    }
);


/* =====================================================
   SELECTED PLANT
===================================================== */

let selectedPlant = null;


function selectPlant(id) {

    selectedPlant = id;

    updateSelectedUI();
}


function updateSelectedUI() {

    const box =
        document.getElementById(
            "selected"
        );

    if (!selectedPlant) {

        box.innerHTML = `
            <h3>🌱 Select a plant</h3>
            <p>Click a plant in your garden.</p>
        `;

        return;
    }


    const plant =
        save.plants.find(
            p => p.id === selectedPlant
        );

    if (!plant) return;


    const info =
        PLANTS[plant.type];


    box.innerHTML = `

        <h3>
            ${info.emoji}
            ${info.name}
        </h3>

        <p>
            Growth:
            ${Math.floor(plant.growth)}%
        </p>

        <label>💧 Water</label>
        <div class="bar">
            <div
                class="fill water"
                style="width:${plant.water}%">
            </div>
        </div>

        <label>☀️ Sunlight</label>
        <div class="bar">
            <div
                class="fill sun"
                style="width:${plant.sunlight}%">
            </div>
        </div>

        <label>❤️ Health</label>
        <div class="bar">
            <div
                class="fill health"
                style="width:${plant.health}%">
            </div>
        </div>

        <button id="waterButton">
            💧 Water
        </button>

        <button id="sunButton">
            ☀️ Give Sunlight
        </button>
    `;


    document
        .getElementById("waterButton")
        .onclick =
        () => waterPlant(plant.id);


    document
        .getElementById("sunButton")
        .onclick =
        () => giveSunlight(plant.id);
}


/* =====================================================
   WATER
===================================================== */

function waterPlant(id) {

    const plant =
        save.plants.find(
            p => p.id === id
        );

    if (!plant) return;


    plant.water =
        Math.min(
            100,
            plant.water + 30
        );


    plant.health =
        Math.min(
            100,
            plant.health + 3
        );


    showMessage(
        "💧 Your plant was watered!"
    );

    saveGame();

    updateSelectedUI();
}


/* =====================================================
   SUNLIGHT
===================================================== */

function giveSunlight(id) {

    const plant =
        save.plants.find(
            p => p.id === id
        );

    if (!plant) return;


    plant.sunlight =
        Math.min(
            100,
            plant.sunlight + 25
        );


    plant.health =
        Math.min(
            100,
            plant.health + 2
        );


    showMessage(
        "☀️ Your plant enjoyed the sunlight!"
    );

    saveGame();

    updateSelectedUI();
}


/* =====================================================
   GROWTH
===================================================== */

function growPlant(
    plant,
    amount = .1
) {

    if (
        plant.water < 20 ||
        plant.sunlight < 20 ||
        plant.health < 20
    ) {
        return;
    }


    const info =
        PLANTS[plant.type];


    const quality =
        (
            plant.water +
            plant.sunlight +
            plant.health
        ) / 300;


    plant.growth +=
        amount *
        info.growth *
        quality;


    if (
        plant.growth >= 100
    ) {

        plant.growth = 100;

        if (!plant.mature) {

            plant.mature = true;

            save.coins += 50;

            addXP(50);

            showMessage(
                `🌟 Your ${info.name} is fully grown! +50 coins`
            );
        }
    }


    updatePlantVisual(
        plant
    );
}


/* =====================================================
   GAME SIMULATION
===================================================== */

setInterval(
    () => {

        save.plants.forEach(
            plant => {

                const info =
                    PLANTS[plant.type];


                plant.water =
                    Math.max(
                        0,
                        plant.water -
                        .35 *
                        info.waterNeed
                    );


                plant.sunlight =
                    Math.max(
                        0,
                        plant.sunlight -
                        .25 *
                        info.sunNeed
                    );


                if (
                    plant.water < 15 ||
                    plant.sunlight < 15
                ) {

                    plant.health =
                        Math.max(
                            0,
                            plant.health - .7
                        );

                } else {

                    plant.health =
                        Math.min(
                            100,
                            plant.health + .15
                        );
                }


                growPlant(
                    plant,
                    .12
                );
            }
        );


        updateSelectedUI();

        updateStats();

        saveGame();

    },
    10000
);


/* =====================================================
   BUY PLANT
===================================================== */

window.buyPlant =
    function(type) {

        const info =
            PLANTS[type];


        if (
            save.coins <
            info.cost
        ) {

            showMessage(
                "🪙 You don't have enough coins!"
            );

            return;
        }


        save.coins -=
            info.cost;


        addPlant(
            type
        );


        addXP(10);

        showMessage(
            `${info.emoji} You planted a ${info.name}!`
        );


        saveGame();

        updateStats();
    };


/* =====================================================
   XP
===================================================== */

function addXP(amount) {

    save.xp +=
        amount;


    const required =
        save.level * 100;


    if (
        save.xp >= required
    ) {

        save.xp -=
            required;

        save.level++;

        save.coins +=
            save.level * 25;


        showMessage(
            `⭐ Level ${save.level}! +${save.level * 25} coins`
        );
    }
}


/* =====================================================
   CLICKING PLANTS
===================================================== */

const raycaster =
    new THREE.Raycaster();

const mouse =
    new THREE.Vector2();


renderer.domElement
    .addEventListener(
        "click",
        event => {

            mouse.x =
                (event.clientX /
                    innerWidth) *
                    2 - 1;

            mouse.y =
                -(event.clientY /
                    innerHeight) *
                    2 + 1;


            raycaster.setFromCamera(
                mouse,
                camera
            );


            const objects = [];


            plantMeshes.forEach(
                object => {

                    object.plantMesh
                        .traverse(
                            child => {

                                if (
                                    child.isMesh
                                ) {
                                    objects.push(
                                        child
                                    );
                                }
                            }
                        );
                }
            );


            const hits =
                raycaster.intersectObjects(
                    objects
                );


            if (
                hits.length === 0
            ) {
                return;
            }


            const hit =
                hits[0].object;


            for (
                const [id, object]
                of plantMeshes
            ) {

                let found = false;

                object.plantMesh
                    .traverse(
                        child => {

                            if (
                                child === hit
                            ) {
                                found = true;
                            }
                        }
                    );


                if (found) {

                    selectPlant(id);

                    break;
                }
            }
        }
    );


/* =====================================================
   SAVE
===================================================== */

function saveGame() {

    save.lastPlayed =
        Date.now();

    localStorage.setItem(
        "3DPlantLife",
        JSON.stringify(save)
    );
}


/* =====================================================
   STATS
===================================================== */

function updateStats() {

    document.getElementById(
        "coins"
    ).textContent =
        Math.floor(save.coins);


    document.getElementById(
        "level"
    ).textContent =
        save.level;


    document.getElementById(
        "plantCount"
    ).textContent =
        save.plants.length;
}


/* =====================================================
   MESSAGE
===================================================== */

let messageTimer;


function showMessage(text) {

    const message =
        document.getElementById(
            "message"
        );


    message.textContent =
        text;


    message.style.opacity =
        "1";


    clearTimeout(
        messageTimer
    );


    messageTimer =
        setTimeout(
            () => {

                message.style.opacity =
                    "0";

            },
            3000
        );
}


/* =====================================================
   RESIZE
===================================================== */

window.addEventListener(
    "resize",
    () => {

        camera.aspect =
            innerWidth /
            innerHeight;

        camera.updateProjectionMatrix();

        renderer.setSize(
            innerWidth,
            innerHeight
        );
    }
);


/* =====================================================
   DAY/NIGHT
===================================================== */

const clock =
    new THREE.Clock();


function animate() {

    requestAnimationFrame(
        animate
    );


    const time =
        clock.getElapsedTime();


    /*
       Slowly move the sun to
       create a simple day cycle.
    */

    const sunAngle =
        time * .03;


    sun.position.x =
        Math.cos(sunAngle) * 25;

    sun.position.z =
        Math.sin(sunAngle) * 25;

    sun.position.y =
        15 +
        Math.sin(sunAngle) * 10;


    const daylight =
        Math.max(
            .2,
            (
                Math.sin(sunAngle) + 1
            ) / 2
        );


    sun.intensity =
        .8 +
        daylight * 2;


    ambient.intensity =
        .7 +
        daylight * 1.2;


    plantMeshes.forEach(
        object => {

            const plant =
                object.data;


            /*
               Tiny idle animation.
            */

            object.plantMesh.rotation.y =
                Math.sin(
                    time +
                    plant.id
                ) * .025;

            updatePlantVisual(
                plant
            );
        }
    );


    controls.update();

    renderer.render(
        scene,
        camera
    );
}


updateStats();

updateSelectedUI();

animate();

saveGame();

</script>

</body>
</html>
```
