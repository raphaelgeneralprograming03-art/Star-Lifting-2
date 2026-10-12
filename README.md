
<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>S.L.F. - Star Lifting & SAMS Fleet Simulator</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        space: {
                            950: '#02040a',
                            900: '#050b14',
                            800: '#0d1626',
                            700: '#152238',
                        },
                        stellar: {
                            gold: '#ffb703',
                            amber: '#fb8500',
                            plasma: '#00f5ff',
                            magenta: '#f72585',
                            emerald: '#4cc9f0'
                        }
                    },
                    fontFamily: {
                        mono: ['JetBrains Mono', 'Courier New', monospace],
                        orbitron: ['Orbitron', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&family=Orbitron:wght@500;700;900&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            font-family: 'JetBrains Mono', monospace;
            background-color: #02040a;
            color: #f1f5f9;
            overflow-x: hidden;
        }
        h1, h2, h3, .font-orbitron {
            font-family: 'Orbitron', sans-serif;
        }
        .glow-plasma {
            box-shadow: 0 0 25px rgba(0, 245, 255, 0.35);
        }
        .glow-stellar {
            box-shadow: 0 0 30px rgba(255, 183, 3, 0.4);
        }
        .glow-magenta {
            box-shadow: 0 0 25px rgba(247, 37, 133, 0.35);
        }
        .text-glow-plasma {
            text-shadow: 0 0 12px rgba(0, 245, 255, 0.8);
        }
        .text-glow-stellar {
            text-shadow: 0 0 12px rgba(255, 183, 3, 0.8);
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #050b14;
        }
        ::-webkit-scrollbar-thumb {
            background: #152238;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #00f5ff;
        }
        .scanline {
            background: linear-gradient(
                to bottom,
                rgba(255,255,255,0),
                rgba(255,255,255,0) 50%,
                rgba(0, 245, 255, 0.03) 50%,
                rgba(0, 245, 255, 0.03)
            );
            background-size: 100% 4px;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col bg-space-950 scanline">

    <header class="border-b border-stellar-plasma/30 bg-space-900/90 backdrop-blur-md sticky top-0 z-50 px-4 py-3">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row items-center justify-between gap-4">
            
            <div class="flex items-center space-x-3">
                <div class="w-10 h-10 rounded-xl bg-space-800 border border-stellar-plasma flex items-center justify-center glow-plasma">
                    <i class="fa-solid fa-sun text-stellar-gold text-xl animate-spin" style="animation-duration: 20s;"></i>
                </div>
                <div>
                    <h1 class="text-xl font-black tracking-wider text-stellar-plasma text-glow-plasma">S.L.F. - STAR LIFTING</h1>
                    <p class="text-[10px] text-slate-400 uppercase tracking-widest">SAMS & MHD Stellar Mining Network</p>
                </div>
            </div>

            <!-- Simulation Scenario Selector -->
            <div class="flex flex-wrap items-center gap-1.5 bg-space-950/80 p-1.5 rounded-xl border border-space-700">
                <button onclick="setScenario('stable')" id="scen-stable" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all bg-stellar-plasma/20 text-stellar-plasma border border-stellar-plasma/50 glow-plasma">
                    Mineração Estável
                </button>
                <button onclick="setScenario('storm')" id="scen-storm" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all text-slate-400 hover:text-stellar-gold hover:bg-space-800 border border-transparent">
                    Tempestade Solar (CME)
                </button>
                <button onclick="setScenario('deep')" id="scen-deep" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all text-slate-400 hover:text-stellar-gold hover:bg-space-800 border border-transparent">
                    Mergulho Convectivo Profundo
                </button>
                <button onclick="setScenario('fleet')" id="scen-fleet" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all text-slate-400 hover:text-stellar-gold hover:bg-space-800 border border-transparent">
                    Protocolo de Frota Total
                </button>
            </div>

            <!-- Action Buttons & Audio Toggle -->
            <div class="flex items-center space-x-3">
                <button onclick="toggleAudio()" id="btn-audio" class="w-9 h-9 rounded-xl bg-space-800 border border-space-700 hover:border-stellar-plasma text-slate-300 flex items-center justify-center text-xs transition-all" title="Alternar Efeitos Sonoros Web Audio">
                    <i class="fa-solid fa-volume-xmark" id="audio-icon"></i>
                </button>
                <button onclick="openModal()" class="px-3 py-1.5 rounded-xl bg-space-800 border border-stellar-plasma/40 hover:border-stellar-plasma text-stellar-plasma text-xs font-bold flex items-center gap-2 transition-all glow-plasma">
                    <i class="fa-solid fa-book-open"></i> Sobre o Star Lifting
                </button>
            </div>
        </div>
    </header>

    <main class="flex-1 max-w-7xl w-full mx-auto p-4 grid grid-cols-1 lg:grid-cols-4 gap-4">
        
        <!-- Canvas & Visualization Area (3 Cols) -->
        <div class="lg:col-span-3 flex flex-col gap-4">
            
            <div class="relative w-full aspect-[16/10] bg-space-900 rounded-2xl border border-stellar-plasma/30 overflow-hidden glow-plasma flex items-center justify-center">
                
                <!-- Main Canvas Element -->
                <canvas id="simCanvas" class="w-full h-full block cursor-crosshair"></canvas>

                <!-- Floating HUD Overlays -->
                <div class="absolute top-4 left-4 bg-space-950/85 backdrop-blur-md p-3 rounded-xl border border-stellar-plasma/30 text-xs space-y-1 pointer-events-none">
                    <div class="text-stellar-plasma font-bold uppercase tracking-wider flex items-center gap-2">
                        <i class="fa-solid fa-circle-nodes animate-pulse"></i> <span id="hud-status-title">SETOR: CORONA ESTELAR (CLASSIFICADO)</span>
                    </div>
                    <div class="text-slate-400">FPS: <span id="hud-fps" class="text-white">60</span> | NAVES ATIVAS: <span id="hud-ships" class="text-stellar-gold">6 unidades</span></div>
                    <div class="text-slate-400">EFICIÊNCIA SIPHON: <span id="hud-efficiency" class="text-emerald-400">98.4%</span></div>
                </div>

                <!-- Selected Ship Telemetry Tooltip (Dynamic) -->
                <div id="ship-card" class="absolute top-4 right-4 bg-space-950/90 backdrop-blur-md p-4 rounded-xl border border-stellar-gold/40 text-xs space-y-2 hidden max-w-[240px] shadow-2xl">
                    <div class="flex items-center justify-between border-b border-space-700 pb-1">
                        <span id="card-ship-name" class="font-bold text-stellar-gold">SAMS-VANGUARD #01</span>
                        <button onclick="closeShipCard()" class="text-slate-400 hover:text-white"><i class="fa-solid fa-xmark"></i></button>
                    </div>
                    <div class="space-y-1 text-[11px] text-slate-300">
                        <div>Profundidade: <span id="card-depth" class="text-white">1,250 km</span></div>
                        <div>Integridade SAMS: <span id="card-sams" class="text-emerald-400">99.2%</span></div>
                        <div>Temperatura Casco: <span id="card-temp" class="text-amber-400">4,850 K</span></div>
                        <div>Carga Plasma: <span id="card-cargo" class="text-stellar-plasma">420 kt</span></div>
                    </div>
                </div>

                <!-- Simulation Playback Controls Bar -->
                <div class="absolute bottom-4 left-1/2 -translate-x-1/2 bg-space-950/90 backdrop-blur-md px-6 py-3 rounded-2xl border border-stellar-plasma/40 flex items-center gap-4 shadow-2xl">
                    <button onclick="togglePlayPause()" id="btn-play" class="w-10 h-10 rounded-xl bg-stellar-plasma hover:bg-stellar-emerald text-space-950 font-bold flex items-center justify-center transition-all glow-plasma">
                        <i class="fa-solid fa-pause" id="icon-play"></i>
                    </button>
                    <div class="h-6 w-[1px] bg-space-700"></div>
                    <div class="flex items-center gap-2">
                        <span class="text-xs text-slate-400 uppercase">Tempo:</span>
                        <button onclick="setSpeed(1)" id="spd-1" class="px-2.5 py-1 rounded-lg text-xs font-bold bg-stellar-plasma/20 text-stellar-plasma border border-stellar-plasma">1x</button>
                        <button onclick="setSpeed(3)" id="spd-3" class="px-2.5 py-1 rounded-lg text-xs font-bold bg-space-800 text-slate-400 border border-space-700">3x</button>
                        <button onclick="setSpeed(5)" id="spd-5" class="px-2.5 py-1 rounded-lg text-xs font-bold bg-space-800 text-slate-400 border border-space-700">5x</button>
                    </div>
                    <div class="h-6 w-[1px] bg-space-700"></div>
                    <button onclick="triggerEmergencyRecall()" class="px-3 py-1.5 rounded-xl bg-rose-950/80 hover:bg-rose-900 text-xs text-rose-300 border border-rose-500/50 flex items-center gap-2 transition-all">
                        <i class="fa-solid fa-triangle-exclamation"></i> Recall de Emergência
                    </button>
                </div>
            </div>

            <!-- Scenario Description Box -->
            <div class="bg-space-900/90 backdrop-blur-md p-4 rounded-2xl border border-space-700 text-sm">
                <h3 class="font-bold text-stellar-plasma mb-1 flex items-center gap-2"><i class="fa-solid fa-circle-info"></i> Visão Geral da Operação Atual</h3>
                <p id="scenario-desc" class="text-slate-300 text-xs leading-relaxed">
                    Operando no modo de Mineração Estável. Frotas autônomas mantêm órbitas síncronas na alta atmosfera, utilizando campos magnéticos gerados por reatores MHD para extrair plasma puro, ferro e elementos pesados sem comprometer a estabilidade do núcleo estelar.
                </p>
            </div>

        </div>

        <div class="lg:col-span-1 flex flex-col gap-4">
            
            <!-- Telemetry & Metrics Panel -->
            <div class="bg-space-900/90 backdrop-blur-md p-4 rounded-2xl border border-stellar-plasma/30 flex flex-col gap-4">
                <div class="flex items-center justify-between border-b border-space-700 pb-3">
                    <h2 class="font-orbitron font-bold text-sm text-stellar-plasma flex items-center gap-2">
                        <i class="fa-solid fa-gauge-high"></i> Telemetria S.L.F.
                    </h2>
                    <span class="text-[10px] bg-stellar-plasma/20 text-stellar-plasma px-2 py-0.5 rounded border border-stellar-plasma/40">SAMS ATIVO</span>
                </div>

                <!-- Star & Reactor Metrics -->
                <div class="space-y-3 text-xs">
                    <div>
                        <div class="flex justify-between mb-1">
                            <span class="text-slate-400">Temp. Núcleo Estelar</span>
                            <span id="val-temp" class="text-stellar-gold font-bold">15.4M K</span>
                        </div>
                        <div class="w-full bg-space-950 rounded-full h-2 overflow-hidden border border-space-700">
                            <div id="bar-temp" class="bg-gradient-to-r from-stellar-plasma to-stellar-gold h-full transition-all duration-300" style="width: 75%;"></div>
                        </div>
                    </div>

                    <div>
                        <div class="flex justify-between mb-1">
                            <span class="text-slate-400">Integridade Média SAMS</span>
                            <span id="val-sams" class="text-emerald-400 font-bold">99.6%</span>
                        </div>
                        <div class="w-full bg-space-950 rounded-full h-2 overflow-hidden border border-space-700">
                            <div id="bar-sams" class="bg-emerald-400 h-full transition-all duration-300" style="width: 99.6%;"></div>
                        </div>
                    </div>

                    <div>
                        <div class="flex justify-between mb-1">
                            <span class="text-slate-400">Taxa Siphon Magnético</span>
                            <span id="val-siphon" class="text-stellar-plasma font-bold">142.8 MegaT/s</span>
                        </div>
                        <div class="w-full bg-space-950 rounded-full h-2 overflow-hidden border border-space-700">
                            <div id="bar-siphon" class="bg-stellar-plasma h-full transition-all duration-300" style="width: 85%;"></div>
                        </div>
                    </div>
                </div>

                <!-- Extracted Elements Inventory -->
                <div class="pt-2 border-t border-space-700 space-y-2">
                    <h3 class="font-bold text-[11px] text-slate-400 uppercase tracking-wider flex items-center justify-between">
                        <span>Elementos Extraídos</span>
                        <span class="text-stellar-gold">Total: <span id="total-extracted">12,450</span> kt</span>
                    </h3>
                    <div class="grid grid-cols-2 gap-2 text-xs">
                        <div class="bg-space-950 p-2 rounded-xl border border-space-800">
                            <div class="text-slate-400 text-[10px]">Platina & Ouro</div>
                            <div id="res-platinum" class="text-stellar-gold font-bold mt-0.5">1,840 kt</div>
                        </div>
                        <div class="bg-space-950 p-2 rounded-xl border border-space-800">
                            <div class="text-slate-400 text-[10px]">Urânio Pesado</div>
                            <div id="res-uranium" class="text-emerald-400 font-bold mt-0.5">850 kt</div>
                        </div>
                        <div class="bg-space-950 p-2 rounded-xl border border-space-800">
                            <div class="text-slate-400 text-[10px]">Ferro & Níquel</div>
                            <div id="res-iron" class="text-slate-300 font-bold mt-0.5">4,120 kt</div>
                        </div>
                        <div class="bg-space-950 p-2 rounded-xl border border-space-800">
                            <div class="text-slate-400 text-[10px]">Plasma Puro</div>
                            <div id="res-plasma" class="text-stellar-plasma font-bold mt-0.5">5,640 kt</div>
                        </div>
                    </div>
                </div>

                <!-- Interactive Sliders / Controls -->
                <div class="space-y-3 pt-2 border-t border-space-700">
                    <h3 class="font-bold text-xs text-stellar-plasma uppercase">Controles de Campo</h3>
                    
                    <div class="space-y-1">
                        <div class="flex justify-between text-xs">
                            <span class="text-slate-400">Força Campo Reator MHD</span>
                            <span id="label-mhd" class="text-stellar-plasma font-bold">85%</span>
                        </div>
                        <input type="range" id="mhdSlider" min="10" max="100" value="85" 
                            class="w-full accent-stellar-plasma bg-space-950 rounded-lg appearance-none cursor-pointer h-2 border border-space-700"
                            oninput="updateMHDField(this.value)">
                    </div>

                    <div class="space-y-1">
                        <div class="flex justify-between text-xs">
                            <span class="text-slate-400">Potência Escudo SAMS</span>
                            <span id="label-sams" class="text-emerald-400 font-bold">92%</span>
                        </div>
                        <input type="range" id="samsSlider" min="20" max="100" value="92" 
                            class="w-full accent-emerald-400 bg-space-950 rounded-lg appearance-none cursor-pointer h-2 border border-space-700"
                            oninput="updateSAMSShield(this.value)">
                    </div>

                    <div class="space-y-1">
                        <div class="flex justify-between text-xs">
                            <span class="text-slate-400">Profundidade de Mergulho</span>
                            <span id="label-depth" class="text-stellar-gold font-bold">Corona Média</span>
                        </div>
                        <select id="depthSelect" onchange="updateDiveDepth(this.value)" class="w-full bg-space-950 border border-space-700 rounded-lg p-1.5 text-xs text-slate-200 focus:border-stellar-plasma focus:outline-none">
                            <option value="corona">Coroa Externa (Segura)</option>
                            <option value="chromosphere" selected>Cromosfera (Ótima)</option>
                            <option value="convective">Zona Convectiva (Alta Extração)</option>
                        </select>
                    </div>
                </div>

            </div>

            <!-- Real-time Diagnostic Console Log -->
            <div class="bg-space-900/90 backdrop-blur-md p-4 rounded-2xl border border-space-700 flex flex-col flex-1 min-h-[180px]">
                <div class="flex items-center justify-between border-b border-space-700 pb-2 mb-2">
                    <h3 class="font-orbitron font-bold text-xs text-stellar-plasma flex items-center gap-2">
                        <i class="fa-solid fa-terminal"></i> Log de Diagnóstico SAMS
                    </h3>
                    <button onclick="clearLogs()" class="text-[10px] text-slate-400 hover:text-stellar-plasma underline">Limpar</button>
                </div>
                <div id="log-container" class="flex-1 overflow-y-auto space-y-1.5 font-mono text-[11px] text-slate-300 max-h-[160px]">
                    <div class="text-stellar-plasma">[08:12:00] S.L.F. Sistema inicializado. Redes SAMS acopladas.</div>
                    <div class="text-emerald-400">[08:12:01] Reator MHD sincronizado com o campo estelar.</div>
                    <div class="text-slate-400">[08:12:02] Frota de 6 unidades em órbita estável na Cromosfera.</div>
                </div>
            </div>

        </div>

    </main>

    <div id="infoModal" class="fixed inset-0 bg-space-950/80 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-space-900 border border-stellar-plasma/50 rounded-2xl max-w-2xl w-full p-6 space-y-4 glow-plasma relative max-h-[90vh] overflow-y-auto">
            <div class="flex items-center justify-between border-b border-space-700 pb-3">
                <h3 class="font-orbitron font-bold text-lg text-stellar-plasma flex items-center gap-2">
                    <i class="fa-solid fa-sun text-stellar-gold"></i> O Conceito de Star Lifting (Mineração de Estrelas)
                </h3>
                <button onclick="closeModal()" class="text-slate-400 hover:text-white text-lg"><i class="fa-solid fa-xmark"></i></button>
            </div>
            
            <div class="space-y-3 text-xs text-slate-300 leading-relaxed">
                <p>
                    <strong class="text-stellar-gold">Problema Resolvido:</strong> A escassez crônica de elementos pesados, ferro, platina e matéria-prima pura no universo local é resolvida diretamente na fonte primária de matéria cósmica: as estrelas ativas.
                </p>
                <p>
                    <strong class="text-stellar-plasma">SAMS (Smart Autonomous Magnetic Shielding):</strong> Escudos magnetorreológicos inteligentes de última geração que envolvem as naves em campos magnéticos dinâmicos. O SAMS dissipa o calor extremo e neutraliza os gradientes gravitacionais massivos da estrela.
                </p>
                <p>
                    <strong class="text-stellar-emerald">Reator MHD (Magneto-Hidrodinâmico):</strong> O sistema canaliza as linhas de campo magnético da própria estrela para extrair e sugar plasma e elementos pesados ionizados diretamente da atmosfera estelar para tanques de contenção orbital e refinarias automáticas.
                </p>
                <div class="bg-space-950 p-3 rounded-xl border border-space-700 space-y-1">
                    <div class="font-bold text-stellar-plasma">Modos de Operação no Simulador:</div>
                    <ul class="list-disc pl-4 space-y-1 text-slate-400">
                        <li><strong>Mineração Estável:</strong> Operação nominal com alta eficiência e fluxo magnético controlado.</li>
                        <li><strong>Tempestade Solar (CME):</strong> Simulação de ejeções de massa coronal que exigem pico de blindagem SAMS.</li>
                        <li><strong>Mergulho Profundo:</strong> Extração agressiva na zona convectiva com rendimento massivo de elementos pesados.</li>
                    </ul>
                </div>
            </div>

            <div class="pt-3 border-t border-space-700 flex justify-end">
                <button onclick="closeModal()" class="px-4 py-2 rounded-xl bg-stellar-plasma text-space-950 font-bold text-xs hover:bg-stellar-emerald transition-all glow-plasma">
                    Entendido, Fechar Guia
                </button>
            </div>
        </div>
    </div>

    <script>
        // --- GLOBAL SIMULATION STATE ---
        const canvas = document.getElementById('simCanvas');
        const ctx = canvas.getContext('2d');

        let isRunning = true;
        let timeScale = 1;
        let simulationTime = 0;
        let currentScenario = 'stable';
        
        // Metrics
        let starCoreTemp = 15.4; // Million K
        let samsIntegrity = 99.6;
        let mhdFieldPower = 85;
        let samsShieldPower = 92;
        let siphonRate = 142.8;
        
        let extractedTotals = {
            platinum: 1840,
            uranium: 850,
            iron: 4120,
            plasma: 5640
        };

        // Fleet Ships
        let ships = [];
        const numShips = 6;
        
        function initShips() {
            ships = [];
            for (let i = 0; i < numShips; i++) {
                ships.push({
                    id: i + 1,
                    name: `SAMS-VANGUARD #0${i+1}`,
                    angle: (i / numShips) * Math.PI * 2,
                    orbitRadius: 170 + (i % 3) * 25,
                    speed: 0.008 + (i * 0.002),
                    depthKm: 1200 + (i * 150),
                    sams: 99.2 + (Math.random() * 0.7),
                    temp: 4500 + Math.floor(Math.random() * 600),
                    cargo: 300 + Math.floor(Math.random() * 250),
                    selected: false
                });
            }
        }
        initShips();

        // Particles for solar flares / plasma siphons
        let particles = [];
        let flareWaves = [];

        // Resize Canvas
        function resizeCanvas() {
            const rect = canvas.getBoundingClientRect();
            canvas.width = rect.width * window.devicePixelRatio;
            canvas.height = rect.height * window.devicePixelRatio;
            ctx.scale(window.devicePixelRatio, window.devicePixelRatio);
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // --- WEB AUDIO API SOUND SYSTEM ---
        let audioCtx = null;
        let audioMuted = true;
        let ambientOsc = null;
        let ambientGain = null;

        function toggleAudio() {
            audioMuted = !audioMuted;
            const icon = document.getElementById('audio-icon');
            if (!audioMuted) {
                icon.className = "fa-solid fa-volume-high text-stellar-plasma";
                initWebAudio();
                addLog("Áudio Web Audio ativado (Hum ambiente estelar).");
            } else {
                icon.className = "fa-solid fa-volume-xmark";
                if (audioCtx && audioCtx.state === 'running') {
                    audioCtx.suspend();
                }
                addLog("Áudio desativado.");
            }
        }

        function initWebAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
            if (!ambientOsc) {
                ambientOsc = audioCtx.createOscillator();
                ambientGain = audioCtx.createGain();
                
                ambientOsc.type = 'sine';
                ambientOsc.frequency.setValueAtTime(55, audioCtx.currentTime); // Low deep space hum
                ambientGain.gain.setValueAtTime(0.03, audioCtx.currentTime);

                ambientOsc.connect(ambientGain);
                ambientGain.connect(audioCtx.destination);
                ambientOsc.start();
            }
        }

        // --- SCENARIO & CONTROLS ---
        function setScenario(scen) {
            currentScenario = scen;
            ['stable', 'storm', 'deep', 'fleet'].forEach(s => {
                const btn = document.getElementById(`scen-${s}`);
                if (s === scen) {
                    btn.className = "px-3 py-1.5 rounded-lg text-xs font-bold transition-all bg-stellar-plasma/20 text-stellar-plasma border border-stellar-plasma/50 glow-plasma";
                } else {
                    btn.className = "px-3 py-1.5 rounded-lg text-xs font-bold transition-all text-slate-400 hover:text-stellar-gold hover:bg-space-800 border border-transparent";
                }
            });

            const descElem = document.getElementById('scenario-desc');
            if (scen === 'stable') {
                descElem.innerText = "Operando no modo de Mineração Estável. Frotas autônomas mantêm órbitas síncronas na alta atmosfera, utilizando campos magnéticos gerados por reatores MHD para extrair plasma puro.";
                starCoreTemp = 15.4;
                samsIntegrity = 99.6;
                siphonRate = 142.8;
            } else if (scen === 'storm') {
                descElem.innerText = "ALERTA: Tempestade Solar & Ejeção de Massa Coronal (CME) detectada! Naves elevam potência do SAMS em 150% para desviar fluxos intensos de radiação.";
                starCoreTemp = 18.9;
                samsIntegrity = 94.2;
                siphonRate = 220.5;
            } else if (scen === 'deep') {
                descElem.innerText = "Mergulho Convectivo Profundo acionado. Naves penetram na cromosfera interna para extrair depósitos massivos de Platina e Urânio diretamente das plumas.";
                starCoreTemp = 21.0;
                samsIntegrity = 92.8;
                siphonRate = 310.0;
            } else if (scen === 'fleet') {
                descElem.innerText = "Protocolo de Frota Total: Todas as frotas coordenadas por IA AutoNav executam siphoning em malha fechada, maximizando a produção industrial cósmica.";
                starCoreTemp = 16.8;
                samsIntegrity = 98.5;
                siphonRate = 450.2;
            }
            addLog(`Cenário alterado para: ${scen.toUpperCase()}`);
        }

        function togglePlayPause() {
            isRunning = !isRunning;
            const icon = document.getElementById('icon-play');
            const btn = document.getElementById('btn-play');
            if (isRunning) {
                icon.className = "fa-solid fa-pause";
                btn.classList.add('bg-stellar-plasma', 'glow-plasma');
                btn.classList.remove('bg-space-800');
                addLog("Simulação retomada.");
            } else {
                icon.className = "fa-solid fa-play";
                btn.classList.remove('bg-stellar-plasma', 'glow-plasma');
                btn.classList.add('bg-space-800');
                addLog("Simulação pausada.");
            }
        }

        function setSpeed(spd) {
            timeScale = spd;
            [1, 3, 5].forEach(s => {
                const b = document.getElementById(`spd-${s}`);
                if (s === spd) {
                    b.className = "px-2.5 py-1 rounded-lg text-xs font-bold bg-stellar-plasma/20 text-stellar-plasma border border-stellar-plasma";
                } else {
                    b.className = "px-2.5 py-1 rounded-lg text-xs font-bold bg-space-800 text-slate-400 border border-space-700";
                }
            });
            addLog(`Velocidade da simulação definida para ${spd}x.`);
        }

        function triggerEmergencyRecall() {
            addLog("ALERTA MÁXIMO: Recall de emergência acionado! Frotas retornando às órbitas seguras externas.");
            ships.forEach(s => {
                s.orbitRadius += 60;
                s.sams = 100;
            });
            setTimeout(() => {
                ships.forEach(s => s.orbitRadius -= 60);
                addLog("Frota restabilizada em órbita nominal.");
            }, 3000);
        }

        function updateMHDField(val) {
            mhdFieldPower = parseInt(val);
            document.getElementById('label-mhd').innerText = val + "%";
            siphonRate = (val * 1.68).toFixed(1);
            document.getElementById('val-siphon').innerText = siphonRate + " MegaT/s";
            addLog(`Campo do Reator MHD ajustado para ${val}% (Siphon: ${siphonRate} MegaT/s).`);
        }

        function updateSAMSShield(val) {
            samsShieldPower = parseInt(val);
            document.getElementById('label-sams').innerText = val + "%";
            samsIntegrity = (95 + (val / 100) * 4.9).toFixed(1);
            document.getElementById('val-sams').innerText = samsIntegrity + "%";
            addLog(`Potência do Escudo SAMS ajustada para ${val}%.`);
        }

        function updateDiveDepth(val) {
            addLog(`Profundidade de mergulho reconfigurada: ${val.toUpperCase()}`);
            ships.forEach(s => {
                if (val === 'corona') s.orbitRadius = 210;
                else if (val === 'chromosphere') s.orbitRadius = 175;
                else if (val === 'convective') s.orbitRadius = 140;
            });
        }

        function openModal() {
            document.getElementById('infoModal').classList.remove('hidden');
        }
        function closeModal() {
            document.getElementById('infoModal').classList.add('hidden');
        }

        function closeShipCard() {
            document.getElementById('ship-card').classList.add('hidden');
            ships.forEach(s => s.selected = false);
        }

        function clearLogs() {
            document.getElementById('log-container').innerHTML = `<div class="text-slate-400">[Console limpo pelo operador]</div>`;
        }

        function addLog(text) {
            const container = document.getElementById('log-container');
            const now = new Date().toTimeString().split(' ')[0];
            const div = document.createElement('div');
            div.className = "text-slate-300";
            div.innerHTML = `<span class="text-stellar-plasma">[${now}]</span> ${text}`;
            container.appendChild(div);
            container.scrollTop = container.scrollHeight;
        }

        // --- CANVAS INTERACTION (Ship Selection) ---
        canvas.addEventListener('click', (e) => {
            const rect = canvas.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;
            const cx = canvas.clientWidth / 2;
            const cy = canvas.clientHeight / 2;

            let clickedShip = null;
            ships.forEach(ship => {
                let sx = cx + Math.cos(ship.angle) * ship.orbitRadius;
                let sy = cy + Math.sin(ship.angle) * ship.orbitRadius;
                let dist = Math.hypot(x - sx, y - sy);
                if (dist < 20) {
                    clickedShip = ship;
                }
            });

            if (clickedShip) {
                ships.forEach(s => s.selected = false);
                clickedShip.selected = true;
                
                document.getElementById('card-ship-name').innerText = clickedShip.name;
                document.getElementById('card-depth').innerText = clickedShip.depthKm.toLocaleString() + " km";
                document.getElementById('card-sams').innerText = clickedShip.sams.toFixed(1) + "%";
                document.getElementById('card-temp').innerText = clickedShip.temp.toLocaleString() + " K";
                document.getElementById('card-cargo').innerText = clickedShip.cargo + " kt";
                document.getElementById('ship-card').classList.remove('hidden');
                addLog(`Unidade ${clickedShip.name} selecionada para inspeção detalhada.`);
            }
        });

        // --- DRAWING ENGINE ---
        function drawStar(cx, cy, radius) {
            // Corona Glow Aura
            const coronaGradient = ctx.createRadialGradient(cx, cy, radius * 0.8, cx, cy, radius * 2.2);
            coronaGradient.addColorStop(0, 'rgba(255, 183, 3, 0.35)');
            coronaGradient.addColorStop(0.5, 'rgba(251, 133, 0, 0.15)');
            coronaGradient.addColorStop(1, 'transparent');

            ctx.fillStyle = coronaGradient;
            ctx.beginPath();
            ctx.arc(cx, cy, radius * 2.2, 0, Math.PI * 2);
            ctx.fill();

            // Star Core Body
            const starGradient = ctx.createRadialGradient(cx - 30, cy - 30, radius * 0.1, cx, cy, radius);
            starGradient.addColorStop(0, '#ffffff');
            starGradient.addColorStop(0.3, '#ffb703');
            starGradient.addColorStop(0.7, '#fb8500');
            starGradient.addColorStop(1, '#9d0208');

            ctx.fillStyle = starGradient;
            ctx.beginPath();
            ctx.arc(cx, cy, radius, 0, Math.PI * 2);
            ctx.fill();

            // Pulsating Convective Granules Surface
            ctx.save();
            ctx.beginPath();
            ctx.arc(cx, cy, radius, 0, Math.PI * 2);
            ctx.clip();

            ctx.fillStyle = 'rgba(255, 255, 255, 0.15)';
            for (let i = 0; i < 25; i++) {
                let gx = cx + Math.sin(simulationTime * 0.03 + i * 1.5) * (radius * 0.7);
                let gy = cy + Math.cos(simulationTime * 0.02 + i * 2.1) * (radius * 0.7);
                let gRadius = 15 + Math.sin(simulationTime * 0.05 + i) * 8;
                ctx.beginPath();
                ctx.arc(gx, gy, Math.abs(gRadius), 0, Math.PI * 2);
                ctx.fill();
            }
            ctx.restore();

            // Solar Flares / CME in storm mode
            if (currentScenario === 'storm' && Math.random() < 0.4) {
                let flareAngle = Math.random() * Math.PI * 2;
                let fx = cx + Math.cos(flareAngle) * radius;
                let fy = cy + Math.sin(flareAngle) * radius;
                
                ctx.strokeStyle = 'rgba(0, 245, 255, 0.8)';
                ctx.lineWidth = 3;
                ctx.beginPath();
                ctx.moveTo(fx, fy);
                ctx.quadraticCurveTo(fx * 1.3, fy * 1.3, fx * 1.5, fy * 1.5);
                ctx.stroke();
            }
        }

        function drawStarfield(w, h) {
            ctx.fillStyle = '#02040a';
            ctx.fillRect(0, 0, w, h);

            ctx.fillStyle = '#ffffff';
            for (let i = 0; i < 90; i++) {
                let x = (Math.sin(i * 123) * 10000) % w;
                let y = (Math.cos(i * 456) * 10000) % h;
                if (x < 0) x += w;
                if (y < 0) y += h;
                let alpha = 0.2 + (Math.sin(simulationTime * 0.04 + i) * 0.2);
                ctx.globalAlpha = Math.max(0.1, alpha);
                ctx.fillRect(x, y, 1.5, 1.5);
            }
            ctx.globalAlpha = 1.0;
        }

        // Main Simulation Loop
        function simulationLoop() {
            const w = canvas.clientWidth;
            const h = canvas.clientHeight;

            if (canvas.width !== w * window.devicePixelRatio || canvas.height !== h * window.devicePixelRatio) {
                canvas.width = w * window.devicePixelRatio;
                canvas.height = h * window.devicePixelRatio;
                ctx.scale(window.devicePixelRatio, window.devicePixelRatio);
            }

            ctx.clearRect(0, 0, w, h);
            drawStarfield(w, h);

            const cx = w / 2;
            const cy = h / 2;
            const starRadius = 90;

            if (isRunning) {
                simulationTime += 1 * timeScale;
                
                // Increment extracted resources gradually
                if (Math.random() < 0.3) {
                    extractedTotals.plasma += Math.floor(Math.random() * 5) + 1;
                    extractedTotals.platinum += Math.floor(Math.random() * 2);
                    extractedTotals.iron += Math.floor(Math.random() * 4);
                    document.getElementById('total-extracted').innerText = (
                        extractedTotals.platinum + extractedTotals.uranium + extractedTotals.iron + extractedTotals.plasma
                    ).toLocaleString();
                    document.getElementById('res-platinum').innerText = extractedTotals.platinum.toLocaleString() + " kt";
                    document.getElementById('res-plasma').innerText = extractedTotals.plasma.toLocaleString() + " kt";
                    document.getElementById('res-iron').innerText = extractedTotals.iron.toLocaleString() + " kt";
                }
            }

            // Draw Star
            drawStar(cx, cy, starRadius);

            // Draw Orbit Rings
            ctx.strokeStyle = 'rgba(0, 245, 255, 0.12)';
            ctx.lineWidth = 1;
            [140, 175, 210].forEach(r => {
                ctx.beginPath();
                ctx.arc(cx, cy, r, 0, Math.PI * 2);
                ctx.stroke();
            });

            // Update & Draw Ships and Siphon Beams
            ships.forEach(ship => {
                if (isRunning) {
                    ship.angle += ship.speed * 0.01 * timeScale;
                }

                let sx = cx + Math.cos(ship.angle) * ship.orbitRadius;
                let sy = cy + Math.sin(ship.angle) * ship.orbitRadius;

                // Draw Siphon Plasma Beam connecting star surface to ship
                let surfaceX = cx + Math.cos(ship.angle) * starRadius;
                let surfaceY = cy + Math.sin(ship.angle) * starRadius;

                const beamGradient = ctx.createLinearGradient(surfaceX, surfaceY, sx, sy);
                beamGradient.addColorStop(0, 'rgba(255, 183, 3, 0.8)');
                beamGradient.addColorStop(0.5, 'rgba(0, 245, 255, 0.9)');
                beamGradient.addColorStop(1, 'rgba(247, 37, 133, 0.8)');

                ctx.strokeStyle = beamGradient;
                ctx.lineWidth = 2 + (mhdFieldPower / 30);
                ctx.beginPath();
                ctx.moveTo(surfaceX, surfaceY);
                ctx.lineTo(sx, sy);
                ctx.stroke();

                // Draw SAMS Magnetorheological Shield Bubble around ship
                ctx.strokeStyle = ship.selected ? '#ffffff' : 'rgba(0, 245, 255, 0.6)';
                ctx.lineWidth = ship.selected ? 2 : 1.5;
                ctx.fillStyle = 'rgba(0, 245, 255, 0.08)';
                ctx.beginPath();
                ctx.arc(sx, sy, 18, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();

                // Draw Ship Hull
                ctx.save();
                ctx.translate(sx, sy);
                ctx.rotate(ship.angle + Math.PI / 2);

                ctx.fillStyle = ship.selected ? '#ffb703' : '#38bdf8';
                ctx.beginPath();
                ctx.moveTo(0, -10);
                ctx.lineTo(-7, 8);
                ctx.lineTo(7, 8);
                ctx.closePath();
                ctx.fill();

                ctx.restore();

                // Draw ship label tag
                ctx.fillStyle = ship.selected ? '#ffb703' : '#94a3b8';
                ctx.font = '9px JetBrains Mono';
                ctx.fillText(`SAMS-#0${ship.id}`, sx + 12, sy - 4);
            });

            requestAnimationFrame(simulationLoop);
        }

        // Start animation on window load
        window.onload = function() {
            addLog("Módulos SAMS e Reatores MHD operando em capacidade nominal.");
            requestAnimationFrame(simulationLoop);
        };
    </script>
</body>
</html>
