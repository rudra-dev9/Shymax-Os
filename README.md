# Shymax-Os
Shymax Os v1 --> v7 Enjoy!

# Shymax UI - Workspace Edition 🖥️✨

Shymax UI is a lightweight, high-performance web-based desktop environment. Built with a focus on **Cryo-Glass (Glassmorphism)** aesthetics, it offers a fluid, interactive workspace experience directly in the browser.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Version](https://img.shields.io/badge/version-0.7.2-orange.svg)

## 🌟 Key Features

- **Cryo-Glass Interface:** Advanced CSS backdrop-filters for a frosted-glass look.
- **Dynamic Workspace:** Upload your own wallpapers locally without refreshing.
- **Draggable Widgets:** Spawn real-time weather and clock widgets that you can move anywhere.
- **Multi-State Dock:** Toggle between a "Shoreline Island" dock or a "Joined" unified dock.
- **Summit Mode:** A high-contrast, technical dark mode for focused work.
- **Responsive Horizon Bar:** Top-level system controls and real-time clock.

## 🚀 Live Demo

[https://shymaxospreview.edgeone.dev/]

## 🛠️ Built With

- **HTML5** - Semantic structure.
- **CSS3** - Custom variables, Flexbox, and Backdrop Filters.
- **Vanilla JavaScript** - DOM manipulation and custom dragging logic.

## 📂 Installation & Usage

Since Shymax UI is built with pure web technologies, no installation is required.
The Below Code is  Shymax Os v0.7 Future Updates may happen! 

```lua 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shymax UI - Workspace Edition</title>
    <style>
        :root {
            --bg-teal: #0EA5E9;
            --glass-surface: rgba(255, 255, 255, 0.1);
            --glass-border: rgba(255, 255, 255, 0.2);
            --glass-frost: rgba(255, 255, 255, 0.4);
            --shadow-elevation: 0 8px 32px 0 rgba(0, 0, 0, 0.5);
            --font-main: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            --radius: 20px;
            --accent: var(--bg-teal);
        }

        body.summit-active {
            --glass-surface: rgba(0, 0, 0, 0.8);
            --glass-border: rgba(255, 255, 255, 0.1);
            --font-main: 'Courier New', Courier, monospace;
            --radius: 2px;
            --accent: #ffffff;
            filter: grayscale(100%) contrast(120%);
        }

        * { box-sizing: border-box; margin: 0; padding: 0; user-select: none; transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); }

        body {
            height: 100vh; width: 100vw; overflow: hidden;
            font-family: var(--font-main);
            background-color: #0F172A;
            background-image: url('1000016766.jpg'), url('https://images.unsplash.com/photo-1501785888041-af3ef285b470?q=80&w=2070&auto=format&fit=crop');
            background-size: cover; background-position: center;
            position: relative;
        }

        .cryo-glass {
            background: var(--glass-surface);
            backdrop-filter: blur(25px);
            -webkit-backdrop-filter: blur(25px);
            border: 1px solid var(--glass-border);
            border-top: 1px solid var(--glass-frost);
            box-shadow: var(--shadow-elevation);
            border-radius: var(--radius);
        }

        .hidden { display: none !important; }

        /* --- HORIZON BAR --- */
        .horizon-bar {
            position: absolute; top: 10px; left: 1%; width: 98%; height: 32px;
            display: flex; justify-content: space-between; align-items: center;
            padding: 0 20px; color: white; font-size: 13px;
            background: rgba(0, 0, 0, 0.3); border-radius: 50px; z-index: 1000;
        }

        .horizon-right { display: flex; gap: 10px; }
        .control-trigger { cursor: pointer; padding: 5px 12px; border-radius: 20px; font-weight: 500; }
        .control-trigger:hover { background: rgba(255,255,255,0.15); }

        /* --- MENUS --- */
        .dropdown-menu {
            position: absolute; top: 45px; right: 20px; width: 260px;
            padding: 15px; z-index: 1001; color: white;
        }

        .menu-title { font-size: 11px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.5; margin-bottom: 15px; }

        .feature-toggle {
            display: flex; justify-content: space-between; align-items: center;
            margin-bottom: 12px; font-size: 14px;
        }

        .menu-btn {
            width: 100%; background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.1);
            color: white; padding: 8px; border-radius: 10px; cursor: pointer; margin-top: 5px; font-size: 12px;
        }
        .menu-btn:hover { background: rgba(255,255,255,0.2); }

        /* Toggle Switch */
        .switch { position: relative; display: inline-block; width: 34px; height: 20px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: rgba(255,255,255,0.2); border-radius: 34px; }
        input:checked + .slider { background-color: var(--accent); }
        .slider:before { position: absolute; content: ""; height: 14px; width: 14px; left: 3px; bottom: 3px; background-color: white; border-radius: 50%; }
        input:checked + .slider:before { transform: translateX(14px); }

        /* --- WIDGETS --- */
        .widget {
            position: absolute; padding: 20px; width: 180px; height: 180px;
            cursor: grab; z-index: 100; color: white;
        }
        .widget:active { cursor: grabbing; }

        .weather-temp { font-size: 42px; font-weight: 200; margin: 10px 0; }
        .weather-city { font-size: 14px; font-weight: 600; }

        /* --- THE DOCK --- */
        .shoreline-container {
            position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%);
            display: flex; gap: 15px; align-items: flex-end; z-index: 2000; padding: 5px;
        }
        .dock-island { height: 70px; display: flex; align-items: center; padding: 0 15px; gap: 15px; border-radius: var(--radius); }
        .shoreline-container.joined { gap: 0px; background: var(--glass-surface); backdrop-filter: blur(30px); border: 1px solid var(--glass-border); border-radius: calc(var(--radius) + 10px); }
        .shoreline-container.joined .dock-island { background: transparent; border: none; box-shadow: none; }

        .app-icon {
            width: 48px; height: 48px; border-radius: calc(var(--radius) / 1.5);
            display: flex; justify-content: center; align-items: center;
            font-size: 22px; cursor: pointer; color: white; text-decoration: none;
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
        }

        .icon-finder { background: linear-gradient(180deg, #1e3a8a, #3b82f6); }
        .icon-settings { background: linear-gradient(180deg, #475569, #94a3b8); }
        .icon-browser { background: linear-gradient(180deg, #eab308, #fef08a); }
        .icon-mail { background: linear-gradient(180deg, #1d4ed8, #60a5fa); }
        .icon-music { background: linear-gradient(180deg, #be123c, #f43f5e); }
        .icon-util { background: rgba(255,255,255,0.15); border: 1px solid rgba(255,255,255,0.1); }

    </style>
</head>
<body class="ripple-active" id="shymax-body">

    <div class="horizon-bar">
        <div class="horizon-left"><b>Shymax UI</b></div>
        <div id="clock">12:00 PM</div>
        <div class="horizon-right">
            <span class="control-trigger" onclick="toggleMenu('workspace-menu')">Workspace ▾</span>
            <span class="control-trigger" onclick="toggleMenu('control-menu')">System Control ▾</span>
        </div>
    </div>

    <div class="dropdown-menu cryo-glass hidden" id="workspace-menu" style="right: 160px;">
        <div class="menu-title">Personalization</div>
        <div class="feature-toggle">
            <span>Custom Wallpaper</span>
            <input type="file" id="wall-upload" accept="image/*" style="display:none" onchange="changeWallpaper(event)">
            <button class="menu-btn" onclick="document.getElementById('wall-upload').click()">Upload Image</button>
        </div>
        <hr style="opacity: 0.1; margin: 15px 0;">
        <div class="menu-title">Widgets</div>
        <button class="menu-btn" onclick="spawnWidget('weather')">+ Weather Widget</button>
        <button class="menu-btn" onclick="spawnWidget('clock')">+ Large Clock</button>
    </div>

    <div class="dropdown-menu cryo-glass hidden" id="control-menu">
        <div class="menu-title">Toggles</div>
        <div class="feature-toggle">
            <span>Lake Ripple</span>
            <label class="switch"><input type="checkbox" id="ripple-switch" checked onchange="updateFeatures()"><span class="slider"></span></label>
        </div>
        <div class="feature-toggle">
            <span>Joined Dock</span>
            <label class="switch"><input type="checkbox" id="join-switch" onchange="updateFeatures()"><span class="slider"></span></label>
        </div>
        <div class="feature-toggle">
            <span>Summit Mode</span>
            <label class="switch"><input type="checkbox" id="summit-switch" onchange="updateFeatures()"><span class="slider"></span></label>
        </div>
    </div>

    <div class="shoreline-container" id="dock-container">
        <div class="dock-island cryo-glass">
            <div class="app-icon icon-finder">☺</div>
            <div class="app-icon icon-settings">⚙</div>
        </div>
        <div class="dock-island cryo-glass">
            <a href="https://google.com" target="_blank" class="app-icon icon-browser">●</a>
            <a href="mailto:" class="app-icon icon-mail">✉</a>
            <a href="https://open.spotify.com" target="_blank" class="app-icon icon-music">♫</a>
        </div>
        <div class="dock-island cryo-glass">
            <div class="app-icon icon-util">📁</div>
            <div class="app-icon icon-util">🗑</div>
        </div>
    </div>

    <script>
        // Toggle Menus
        function toggleMenu(id) {
            const menu = document.getElementById(id);
            const isHidden = menu.classList.contains('hidden');
            // Close all first
            document.querySelectorAll('.dropdown-menu').forEach(m => m.classList.add('hidden'));
            if(isHidden) menu.classList.remove('hidden');
        }

        // Change Wallpaper
        function changeWallpaper(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    document.body.style.backgroundImage = `url(${e.target.result})`;
                }
                reader.readAsDataURL(file);
            }
        }

        // Spawn Draggable Widgets
        function spawnWidget(type) {
            const widget = document.createElement('div');
            widget.className = 'widget cryo-glass';
            widget.style.top = '100px';
            widget.style.left = '100px';
            
            if(type === 'weather') {
                widget.innerHTML = `
                    <div class="weather-city">TAHOE NORTH</div>
                    <div class="weather-temp">18°C</div>
                    <div style="font-size: 12px; opacity: 0.7;">Mostly Clear</div>
                `;
            } else {
                widget.innerHTML = `
                    <div class="weather-city">LOCAL TIME</div>
                    <div id="big-clock" style="font-size: 32px; margin: 10px 0;">00:00</div>
                    <div style="font-size: 12px; opacity: 0.7;">Summit Active</div>
                `;
                // Start a local timer for the widget clock
                setInterval(() => {
                    const now = new Date();
                    const clockEl = widget.querySelector('#big-clock');
                    if(clockEl) clockEl.innerText = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
                }, 1000);
            }

            // Dragging Logic
            let pos1 = 0, pos2 = 0, pos3 = 0, pos4 = 0;
            widget.onmousedown = (e) => {
                pos3 = e.clientX;
                pos4 = e.clientY;
                document.onmouseup = () => {
                    document.onmouseup = null;
                    document.onmousemove = null;
                };
                document.onmousemove = (e) => {
                    pos1 = pos3 - e.clientX;
                    pos2 = pos4 - e.clientY;
                    pos3 = e.clientX;
                    pos4 = e.clientY;
                    widget.style.top = (widget.offsetTop - pos2) + "px";
                    widget.style.left = (widget.offsetLeft - pos1) + "px";
                };
            };

            document.body.appendChild(widget);
            toggleMenu('workspace-menu'); // Close menu after spawning
        }

        function updateFeatures() {
            const isRipple = document.getElementById('ripple-switch').checked;
            const isJoined = document.getElementById('join-switch').checked;
            const isSummit = document.getElementById('summit-switch').checked;
            const body = document.getElementById('shymax-body');
            const dock = document.getElementById('dock-container');

            if(isRipple) body.classList.add('ripple-active'); else body.classList.remove('ripple-active');
            if(isJoined) dock.classList.add('joined'); else dock.classList.remove('joined');
            if(isSummit) body.classList.add('summit-active'); else body.classList.remove('summit-active');
        }

        function updateClock() {
            const now = new Date();
            document.getElementById('clock').innerText = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
        }
        setInterval(updateClock, 1000);
        updateClock();
    </script>
</body>
</html>```
