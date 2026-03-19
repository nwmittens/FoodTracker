
[food-tracker.html](https://github.com/user-attachments/files/26111218/food-tracker.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Food Insight Slider</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0" />

<style>
:root {
  --glass-bg: rgba(255, 255, 255, 0.12);
  --glass-border: rgba(255, 255, 255, 0.25);
  --text-main: #ffffff;
  --text-muted: #d0d0e0;
  --meter-radius: 999px;
  --transition-speed: 0.6s;
}

body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", Segoe UI, sans-serif;
  color: var(--text-main);
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  padding: 40px 0;
  background: #05070a;
  overflow-y: auto;
}

/* RAINING LEAVES BACKGROUND */
.leaf {
  position: fixed;
  top: -40px;
  font-size: 24px;
  opacity: 0.7;
  animation-name: fall;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
  pointer-events: none;
  z-index: -3;
}

@keyframes fall {
  0% {
    transform: translate3d(var(--x-start), -40px, 0) rotate(0deg);
  }
  100% {
    transform: translate3d(var(--x-end), 110vh, 0) rotate(360deg);
  }
}

/* LOADING SCREEN */
#loadingScreen {
  position: fixed;
  inset: 0;
  background: radial-gradient(circle at top, rgba(0,0,0,0.8), rgba(0,0,0,0.95));
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
  opacity: 1;
  transition: opacity 0.8s ease;
}

#loadingScreen.hidden {
  opacity: 0;
  pointer-events: none;
}

.loading-glass {
  background: rgba(255,255,255,0.12);
  border: 1px solid rgba(255,255,255,0.25);
  padding: 30px 40px;
  border-radius: 22px;
  text-align: center;
  box-shadow: 0 18px 45px rgba(0,0,0,0.55);
}

.spinner {
  width: 45px;
  height: 45px;
  border: 4px solid rgba(255,255,255,0.25);
  border-top-color: white;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 15px;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* MAIN UI */
#container {
  width: 400px;
  height: 700px;
  position: relative;
}

#bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  filter: blur(8px) brightness(0.55);
  z-index: -2;
  transition: opacity var(--transition-speed) ease-in-out, background 0.6s ease-in-out;
  border-radius: 22px;
}

body::after {
  content: "";
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at top, rgba(0,0,0,0.6), rgba(0,0,0,0.9));
  z-index: -1;
  border-radius: 22px;
}

.glass {
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: 22px;
  padding: 22px;
  backdrop-filter: blur(20px) saturate(1.4);
  -webkit-backdrop-filter: blur(20px) saturate(1.4);
  box-shadow: 0 18px 45px rgba(0,0,0,0.55);
  margin: 20px;
  animation: floatIn 1s ease-out;
  transform-style: preserve-3d;
}

@keyframes floatIn {
  from { opacity: 0; transform: translateY(20px) scale(0.97); }
  to { opacity: 1; transform: translateY(0) scale(1); }
}

/* HEADER WITH ARROW BUTTON */
.header-row {
  display: flex;
  align-items: center;
  gap: 10px;
}

h1 { margin: 0 0 4px; font-size: 1.8rem; }
p { margin: 0; color: var(--text-muted); }

.slider-row { margin-top: 20px; display: flex; flex-direction: column; gap: 10px; }

input[type="range"] {
  -webkit-appearance: none;
  width: 100%;
  height: 8px;
  border-radius: 999px;
  background: rgba(255,255,255,0.15);
  transition: background 0.3s ease;
}

input[type="range"]:hover { background: rgba(255,255,255,0.25); }
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 18px; height: 18px; border-radius: 50%;
  background: radial-gradient(circle at 30% 30%, #fff, #d0c7ff 40%, #7b5cff);
  border: 1px solid rgba(255,255,255,0.8);
  box-shadow: 0 0 0 4px rgba(123,92,255,0.35), 0 8px 18px rgba(0,0,0,0.5);
  transition: transform 0.25s cubic-bezier(.25,.46,.45,1.4);
}
input[type="range"]::-webkit-slider-thumb:active { transform: scale(1.2); }

.food-info { margin-top: 20px; padding: 16px; border-radius: 18px; background: rgba(255,255,255,0.08); }

.meter-block { margin-top: 18px; }
.meter-header { display: flex; justify-content: space-between; font-size: 0.85rem; color: var(--text-muted); }
.meter-bar { width: 100%; height: 12px; background: rgba(255,255,255,0.12); border-radius: var(--meter-radius); overflow: hidden; margin-top: 6px; }
.meter-fill { height: 100%; width: 0%; border-radius: var(--meter-radius); transition: width 0.55s cubic-bezier(.25,.46,.45,1.4); }
.health { background: linear-gradient(90deg, #ff6b6b, #facc15, #4ade80); }
.size { background: linear-gradient(90deg, #38bdf8, #6366f1); }
.time { background: linear-gradient(90deg, #f97316, #ec4899); }

/* FOOD ICON */
#foodIcon {
  width: 120px;
  height: 120px;
  margin: 25px auto 0;
  border-radius: 50%;
  background: rgba(0,0,0,0.35);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 64px;
  box-shadow: 0 12px 35px rgba(0,0,0,0.45);
  opacity: 0;
  transform: translateY(12px) scale(0.95);
  transition: opacity var(--transition-speed), transform var(--transition-speed);
}
#foodIcon.visible {
  opacity: 1;
  transform: translateY(0) scale(1);
}
#foodIcon:hover {
  transform: translateY(-4px) scale(1.03);
  transition: 0.3s ease;
}

.footer-badge { margin-top: 35px; text-align: center; font-size: 0.85rem; color: var(--text-muted); }
.footer-badge img { margin-top: 8px; width: 74px; height: 74px; border-radius: 50%; border: 2px solid rgba(255,255,255,0.35); object-fit: cover; }

/* CALORIE PANEL TOGGLE BUTTON */
#calorieToggle {
  width: 40px;
  height: 40px;
  border-radius: 8px;
  border: 1px solid var(--glass-border);
  background: rgba(255,255,255,0.12);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
  box-shadow: 0 10px 25px rgba(0,0,0,0.5);
  transition: transform 0.3s ease, background 0.3s ease, box-shadow 0.3s ease;
}
#calorieToggle span {
  display: inline-block;
  transition: transform 0.3s ease;
}
#calorieToggle:hover {
  background: rgba(255,255,255,0.18);
  box-shadow: 0 14px 30px rgba(0,0,0,0.6);
  transform: translateY(-1px);
}
#calorieToggle.open span {
  transform: rotate(90deg);
}

/* CALORIE PANEL */
#caloriePanel {
  position: absolute;
  left: 50%;
  top: 50%;
  width: min(320px, 90%);
  padding: 18px;
  border-radius: 18px;
  background: rgba(10,10,20,0.85);
  border: 1px solid rgba(255,255,255,0.25);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  box-shadow: 0 20px 50px rgba(0,0,0,0.75);
  color: var(--text-main);
  font-size: 0.88rem;
  transition: transform 0.3s ease, opacity 0.3s ease;
  transform: translate(-50%, -45%) scale(0.92);
  opacity: 0;
  pointer-events: none;
  z-index: 4;
}
#caloriePanel.open {
  transform: translate(-50%, -50%) scale(1);
  opacity: 1;
  pointer-events: auto;
}
#card.panel-active {
  filter: brightness(0.85);
  transition: filter 0.2s ease;
}
#card.panel-active .food-info,
#card.panel-active .meter-block,
#card.panel-active .slider-row,
#card.panel-active #foodIcon,
#card.panel-active .footer-badge {
  opacity: 0.96;
}
#card.panel-active *:not(#caloriePanel):not(#caloriePanel *) {
  pointer-events: none;
}
#caloriePanel .close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 28px;
  height: 28px;
  border: 0;
  border-radius: 50%;
  background: rgba(255,255,255,0.15);
  color: #fff;
  font-weight: bold;
  line-height: 28px;
  cursor: pointer;
}
#caloriePanel .close-btn:hover {
  background: rgba(255,255,255,0.25);
}



#caloriePanel h3 {
  margin: 0 0 8px;
  font-size: 1rem;
}

.calorie-row {
  margin-top: 10px;
  position: relative;
  padding-right: 80px;
}

.calorie-row label {
  display: block;
  margin-bottom: 4px;
  color: var(--text-muted);
}

.calorie-row .slider-value {
  position: absolute;
  top: 32px;
  right: 12px;
  font-size: 0.8rem;
  font-weight: 700;
  color: #fff;
  background: rgba(0,0,0,0.36);
  border-radius: 999px;
  padding: 2px 8px;
  pointer-events: none;
}
.custom-share-link {
  margin-top: 12px;
  font-size: 0.82rem;
  color: var(--text-muted);
  text-align: center;
}

.custom-share-link a {
  color: #a78bfa;
  text-decoration: underline;
}
.calorie-stats {
  margin-top: 10px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.calorie-row .slider-value {
  display: inline-block;
  margin-left: 10px;
  color: #fff;
  font-weight: 600;
}

.calorie-stats span.value {
  font-weight: 600;
  color: #fff;
}

/* PERSON VISUAL */
.person-visual {
  margin-top: 12px;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  height: 80px;
}

#personShape {
  width: 32px;
  height: 60px;
  border-radius: 8px;
  background: linear-gradient(180deg, #4ade80, #16a34a);
  box-shadow: 0 8px 20px rgba(0,0,0,0.6);
  transition: transform 0.4s cubic-bezier(.25,.46,.45,1.2), background 0.4s ease;
}
</style>
</head>
<body>

<!-- RAINING LEAVES WILL BE INJECTED VIA JS -->

<!-- LOADING SCREEN -->
<div id="loadingScreen">
  <div class="loading-glass">
    <div class="spinner"></div>
    <p>Loading foods…</p>
  </div>
</div>

<div id="container">
  <div id="bg"></div>

  <div class="glass" id="card">
    <div class="header-row">
      <div id="calorieToggle" title="Calorie to weight tracker">
        <span>➤</span>
      </div>
      <div>
        <h1>Food Insight</h1>
      </div>
    </div>

    <div class="slider-row">
      <label>Select a food:</label>
      <input id="foodSlider" type="range" min="0" max="24" step="1" value="0" />
    </div>

    <div class="food-info">
      <h2 id="foodName"></h2>
      <p id="foodDescription"></p>
    </div>

    <div class="meter-block">
      <div class="meter-header"><span>Healthiness</span><span id="healthValue"></span></div>
      <div class="meter-bar"><div id="healthBar" class="meter-fill health"></div></div>
    </div>

    <div class="meter-block">
      <div class="meter-header"><span>Size</span><span id="sizeValue"></span></div>
      <div class="meter-bar"><div id="sizeBar" class="meter-fill size"></div></div>
    </div>

    <div class="meter-block">
      <div class="meter-header"><span>Time until served</span><span id="timeValue"></span></div>
      <div class="meter-bar"><div id="timeBar" class="meter-fill time"></div></div>
    </div>

    <div id="foodIcon"></div>

    <div class="footer-badge">
      Made by Nathan S
      <br>
      <img id="footerImg" src="https://image2url.com/r2/default/images/1773908222056-25c19a5e-7be3-426b-a2ed-68cb6897bdd7.jpeg" alt="Nathan S profile picture" />
    </div>

    <!-- CALORIE PANEL -->
    <div id="caloriePanel">
      <h3>Calorie → Weight tracker</h3>
      <button class="close-btn" id="closeCaloriePanel" title="Close">✕</button>

      <div class="calorie-row">
        <label for="calorieSlider">Daily calories</label>
        <input id="calorieSlider" type="range" min="1200" max="4000" step="10" value="2200" />
        <span id="calorieValue" class="slider-value">2200 kcal</span>
      </div>

      <div class="calorie-row">
        <label for="heightSlider">Height (cm)</label>
        <input id="heightSlider" type="range" min="150" max="200" step="1" value="175" />
        <span id="heightValue" class="slider-value">175 cm</span>
      </div>

      <div class="calorie-row">
        <label for="ageSlider">Age (years)</label>
        <input id="ageSlider" type="range" min="18" max="70" step="1" value="30" />
        <span id="ageValue" class="slider-value">30</span>
      </div>

      <div class="calorie-stats">
        <div>Estimated weight: <span id="weightValue" class="value">70</span> kg</div>
        <div>Health status: <span id="healthStatus" class="value">Healthy</span></div>
      </div>

      <div class="person-visual">
        <div id="personShape"></div>
      </div>

    </div>
  </div>
</div>

<script>
/* FOODS DATA */
const foods = [
  {
    name: "Apple",
    icon: "🍎",
    description: "A crisp fruit rich in fiber and vitamin C.",
    health: 90,
    size: 30,
    time: 60,
    bg: "linear-gradient(135deg, #ff4b5c, #ff9f1c)"
  },
  {
    name: "Carrot",
    icon: "🥕",
    description: "Crunchy root vegetable full of vitamin A and beta-carotene.",
    health: 95,
    size: 25,
    time: 50,
    bg: "linear-gradient(135deg, #ff8c1a, #ffc34d)"
  },
  {
    name: "Broccoli",
    icon: "🥦",
    description: "Nutrient-dense veggie with vitamins and fiber.",
    health: 95,
    size: 35,
    time: 45,
    bg: "linear-gradient(135deg, #2ecc71, #27ae60)"
  },
  {
    name: "Tomato",
    icon: "🍅",
    description: "Juicy and tangy, full of antioxidants.",
    health: 90,
    size: 25,
    time: 35,
    bg: "linear-gradient(135deg, #ff6b6b, #ff4b5c)"
  },
  {
    name: "Spinach",
    icon: "🥬",
    description: "Leafy green high in iron and fiber.",
    health: 94,
    size: 20,
    time: 30,
    bg: "linear-gradient(135deg, #2ecc71, #00b894)"
  },
  {
    name: "Kale",
    icon: "🥬",
    description: "Superfood green with vitamins K and C.",
    health: 93,
    size: 22,
    time: 28,
    bg: "linear-gradient(135deg, #16a34a, #4ade80)"
  },
  {
    name: "Cucumber",
    icon: "🥒",
    description: "Hydrating and low-calorie refreshing veggie.",
    health: 91,
    size: 18,
    time: 25,
    bg: "linear-gradient(135deg, #6ee7b7, #10b981)"
  },
  {
    name: "Bell Pepper",
    icon: "🫑",
    description: "Colorful pepper loaded with vitamin C.",
    health: 89,
    size: 28,
    time: 30,
    bg: "linear-gradient(135deg, #fb7185, #f97316)"
  },
  {
    name: "Sweet Potato",
    icon: "🍠",
    description: "High in fiber and vitamin A, healthy carb.",
    health: 87,
    size: 40,
    time: 55,
    bg: "linear-gradient(135deg, #f59e0b, #f97316)"
  },
  {
    name: "Eggplant",
    icon: "🍆",
    description: "Versatile and rich in antioxidants.",
    health: 78,
    size: 45,
    time: 50,
    bg: "linear-gradient(135deg, #6d28d9, #9333ea)"
  },
  {
    name: "Avocado",
    icon: "🥑",
    description: "Healthy fats and fiber for better heart health.",
    health: 88,
    size: 34,
    time: 45,
    bg: "linear-gradient(135deg, #22c55e, #16a34a)"
  },
  {
    name: "Banana",
    icon: "🍌",
    description: "Rich in potassium, perfect for energy.",
    health: 80,
    size: 35,
    time: 30,
    bg: "linear-gradient(135deg, #ffe066, #facc15)"
  },
  {
    name: "Orange",
    icon: "🍊",
    description: "Citrus fruit rich in vitamin C.",
    health: 87,
    size: 30,
    time: 35,
    bg: "linear-gradient(135deg, #fb923c, #f97316)"
  },
  {
    name: "Strawberries",
    icon: "🍓",
    description: "Sweet and packed with vitamin C.",
    health: 88,
    size: 20,
    time: 30,
    bg: "linear-gradient(135deg, #f43f5e, #fb7185)"
  },
  {
    name: "Blueberries",
    icon: "🫐",
    description: "Antioxidant-rich berry for brain health.",
    health: 89,
    size: 15,
    time: 20,
    bg: "linear-gradient(135deg, #4338ca, #6366f1)"
  },
  {
    name: "Watermelon",
    icon: "🍉",
    description: "Hydrating summer fruit with vitamins.",
    health: 86,
    size: 55,
    time: 25,
    bg: "linear-gradient(135deg, #34d399, #10b981)"
  },
  {
    name: "Grapes",
    icon: "🍇",
    description: "Sweet fruit with antioxidants and fiber.",
    health: 83,
    size: 28,
    time: 30,
    bg: "linear-gradient(135deg, #a855f7, #7c3aed)"
  },
  {
    name: "Mango",
    icon: "🥭",
    description: "Tropical fruit high in vitamin C and A.",
    health: 75,
    size: 43,
    time: 45,
    bg: "linear-gradient(135deg, #f59e0b, #f97316)"
  },
  {
    name: "Pineapple",
    icon: "🍍",
    description: "Tropical source of vitamin C and manganese.",
    health: 80,
    size: 50,
    time: 40,
    bg: "linear-gradient(135deg, #facc15, #eab308)"
  },
  {
    name: "Peach",
    icon: "🍑",
    description: "Juicy fruit with vitamins and fiber.",
    health: 77,
    size: 30,
    time: 30,
    bg: "linear-gradient(135deg, #fb7185, #f97316)"
  },
  {
    name: "Almonds",
    icon: "🥜",
    description: "Healthy nuts high in protein and healthy fats.",
    health: 72,
    size: 10,
    time: 20,
    bg: "linear-gradient(135deg, #a78bfa, #8b5cf6)"
  },
  {
    name: "Tofu",
    icon: "🥡",
    description: "Plant-based protein for balanced meals.",
    health: 79,
    size: 28,
    time: 35,
    bg: "linear-gradient(135deg, #f3f4f6, #e5e7eb)"
  },
  {
    name: "Salmon",
    icon: "🐟",
    description: "Heart-healthy omega-3 protein source.",
    health: 82,
    size: 55,
    time: 40,
    bg: "linear-gradient(135deg, #f97316, #f87171)"
  },
  {
    name: "Brown Rice",
    icon: "🍚",
    description: "Whole grain for sustained energy.",
    health: 68,
    size: 40,
    time: 50,
    bg: "linear-gradient(135deg, #a78bfa, #6d28d9)"
  },
  {
    name: "Chicken Burger",
    icon: "🍔",
    description: "Protein-rich but depends on sauces and bun.",
    health: 60,
    size: 70,
    time: 40,
    bg: "linear-gradient(135deg, #ff9f1c, #ff4040)"
  },
  {
    name: "Chocolate Cake",
    icon: "🍰",
    description: "High sugar dessert — best in moderation.",
    health: 25,
    size: 60,
    time: 35,
    bg: "linear-gradient(135deg, #3f2b2b, #b56576)"
  }
];

const slider = document.getElementById("foodSlider");
slider.max = foods.length - 1;
slider.step = 1;
const foodName = document.getElementById("foodName");
const foodDescription = document.getElementById("foodDescription");
const healthBar = document.getElementById("healthBar");
const sizeBar = document.getElementById("sizeBar");
const timeBar = document.getElementById("timeBar");
const healthValue = document.getElementById("healthValue");
const sizeValue = document.getElementById("sizeValue");
const timeValue = document.getElementById("timeValue");
const foodIcon = document.getElementById("foodIcon");
const bg = document.getElementById("bg");
const card = document.getElementById("card");
const loadingScreen = document.getElementById("loadingScreen");

const calorieToggle = document.getElementById("calorieToggle");
const caloriePanel = document.getElementById("caloriePanel");
const calorieSlider = document.getElementById("calorieSlider");
const heightSlider = document.getElementById("heightSlider");
const ageSlider = document.getElementById("ageSlider");
const calorieValue = document.getElementById("calorieValue");
const heightValue = document.getElementById("heightValue");
const ageValue = document.getElementById("ageValue");
const weightValue = document.getElementById("weightValue");
const healthStatus = document.getElementById("healthStatus");
const personShape = document.getElementById("personShape");

let currentFoodIndex = -1;

/* SIMPLE LOADING SCREEN TIMER */
setTimeout(() => {
  loadingScreen.classList.add("hidden");
}, 900);

/* RAINING LEAVES CREATION */
function createLeaves(count = 30) {
  const emojis = ["🍂", "🍁", "🍃"];
  for (let i = 0; i < count; i++) {
    const leaf = document.createElement("div");
    leaf.className = "leaf";
    leaf.textContent = emojis[Math.floor(Math.random() * emojis.length)];
    const startX = Math.random() * 100;
    const endX = startX + (Math.random() * 20 - 10);
    const duration = 8 + Math.random() * 6;
    const delay = Math.random() * -duration;

    leaf.style.left = startX + "vw";
    leaf.style.setProperty("--x-start", "0vw");
    leaf.style.setProperty("--x-end", (endX - startX) + "vw");
    leaf.style.animationDuration = duration + "s";
    leaf.style.animationDelay = delay + "s";

    document.body.appendChild(leaf);
  }
}
createLeaves();

/* DYNAMIC FAVICON (fruit emoji rotating) */
const fruitFaviconIcons = foods.map(f => f.icon).filter((v, i, a) => a.indexOf(v) === i);
let faviconCycle = 0;

function setFaviconEmoji(emoji) {
  let link = document.querySelector('link[rel~="icon"]');
  if (!link) {
    link = document.createElement('link');
    link.rel = 'icon';
    document.head.appendChild(link);
  }
  const svg = `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64"><text y="50%" x="50%" text-anchor="middle" dominant-baseline="central" font-size="42">${emoji}</text></svg>`;
  link.href = 'data:image/svg+xml;base64,' + btoa(unescape(encodeURIComponent(svg)));
}

function rotateFavicon() {
  setFaviconEmoji(fruitFaviconIcons[faviconCycle]);
  faviconCycle = (faviconCycle + 1) % fruitFaviconIcons.length;
}

if (fruitFaviconIcons.length > 0) {
  rotateFavicon();
  setInterval(rotateFavicon, 1000);
}

/* MAIN FOOD UI UPDATE */
function updateUI(i) {
  const index = Math.round(Math.min(Math.max(Number(i), 0), foods.length - 1));
  if (index === currentFoodIndex) return;
  currentFoodIndex = index;
  const f = foods[index];

  foodName.textContent = f.name;
  foodDescription.textContent = f.description;

  healthBar.style.width = f.health + "%";
  sizeBar.style.width = f.size + "%";
  timeBar.style.width = f.time + "%";

  healthValue.textContent = f.health + "%";
  sizeValue.textContent = f.size + "%";
  timeValue.textContent = f.time + "%";

  foodIcon.classList.remove("visible");
  setTimeout(() => {
    foodIcon.textContent = f.icon;
    foodIcon.classList.add("visible");
  }, 150);

  bg.style.opacity = 0;
  setTimeout(() => {
    bg.style.background = f.bg;
    bg.style.opacity = 1;
  }, 200);
}

slider.addEventListener("input", e => updateUI(e.target.value));

document.addEventListener("mousemove", e => {
  const x = (e.clientX / window.innerWidth - 0.5) * 10;
  const y = (e.clientY / window.innerHeight - 0.5) * 10;
  card.style.transform = `rotateX(${y}deg) rotateY(${-x}deg)`;
});

/* CALORIE PANEL TOGGLE */
const closeCaloriePanel = document.getElementById("closeCaloriePanel");

calorieToggle.addEventListener("click", () => {
  const isOpen = caloriePanel.classList.toggle("open");
  calorieToggle.classList.toggle("open", isOpen);
  card.classList.toggle("panel-active", isOpen);
});

closeCaloriePanel.addEventListener("click", () => {
  caloriePanel.classList.remove("open");
  calorieToggle.classList.remove("open");
  card.classList.remove("panel-active");
});

/* CALORIE → WEIGHT MODEL (very rough, just for visualization) */
function updateCalorieModel() {
  const calories = Number(calorieSlider.value);
  const heightCm = Number(heightSlider.value);
  const age = Number(ageSlider.value);

  calorieValue.textContent = `${calories} kcal`;
  heightValue.textContent = `${heightCm} cm`;
  ageValue.textContent = `${age}`;

  const heightM = heightCm / 100;
  const baseWeight = 70; // reference
  const baseCalories = 2200;

  // crude estimate: +/- 5 kg per ~500 kcal difference
  const deltaKg = ((calories - baseCalories) / 500) * 5;
  let estWeight = baseWeight + deltaKg;

  // small age effect: older → slightly higher
  estWeight += (age - 30) * 0.1;

  estWeight = Math.max(40, Math.min(140, estWeight));
  weightValue.textContent = estWeight.toFixed(1);

  const bmi = estWeight / (heightM * heightM);
  let status = "Healthy";
  let colorStart = "#4ade80";
  let colorEnd = "#16a34a";

  if (bmi < 18.5) {
    status = "Underweight";
    colorStart = "#38bdf8";
    colorEnd = "#0ea5e9";
  } else if (bmi >= 25 && bmi < 30) {
    status = "Overweight";
    colorStart = "#f97316";
    colorEnd = "#ea580c";
  } else if (bmi >= 30) {
    status = "Obese";
    colorStart = "#ef4444";
    colorEnd = "#b91c1c";
  }

  healthStatus.textContent = status;
  personShape.style.background = `linear-gradient(180deg, ${colorStart}, ${colorEnd})`;

  // scale width based on BMI (visual only)
  const scaleX = Math.min(2.2, Math.max(0.6, bmi / 22));
  const scaleY = Math.min(1.3, Math.max(0.8, 22 / bmi));
  personShape.style.transform = `scale(${scaleX}, ${scaleY})`;
}

calorieSlider.addEventListener("input", updateCalorieModel);
heightSlider.addEventListener("input", updateCalorieModel);
ageSlider.addEventListener("input", updateCalorieModel);

/* INITIALIZE */
updateUI(0);
updateCalorieModel();
</script>

</body>
</html>
