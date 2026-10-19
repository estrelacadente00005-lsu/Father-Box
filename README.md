
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>A Caixa Pai (Father Box) - Engenharia Corrompida e Instabilidade</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        father: {
                            bg: '#050203',
                            panel: 'rgba(20, 5, 8, 0.88)',
                            red: '#ff003c',
                            amber: '#ff6b00',
                            violet: '#a811da',
                            dark: '#120206',
                            border: 'rgba(255, 0, 60, 0.3)'
                        }
                    },
                    fontFamily: {
                        mono: ['Consolas', 'Courier New', 'monospace'],
                        sans: ['Inter', 'Segoe UI', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        ::-webkit-scrollbar { width: 4px; height: 4px; }
        ::-webkit-scrollbar-track { background: rgba(5, 2, 3, 0.95); }
        ::-webkit-scrollbar-thumb { background: rgba(255, 0, 60, 0.4); border-radius: 2px; }
        .neon-glow-red { text-shadow: 0 0 12px rgba(255, 0, 60, 0.7); }
        .neon-glow-amber { text-shadow: 0 0 10px rgba(255, 107, 0, 0.7); }
        .glass-card {
            background: rgba(20, 5, 8, 0.88);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 0, 60, 0.25);
        }
        canvas { touch-action: none; }
    </style>
</head>
<body class="bg-[#050203] text-gray-200 font-sans h-screen flex flex-col overflow-hidden select-none">

    <!-- Header Bar -->
    <header class="h-14 border-b border-father-border bg-father-bg flex items-center justify-between px-4 z-20 shrink-0">
        <div class="flex items-center gap-3">
            <div id="core-beacon" class="w-3.5 h-3.5 rounded-full bg-father-red animate-ping shadow-[0_0_15px_#ff003c]"></div>
            <h1 class="font-bold text-sm sm:text-base tracking-wider text-father-red neon-glow-red uppercase">
                A CAIXA PAI (FATHER BOX) <span class="text-xs text-father-amber font-mono font-normal">| ENGENHARIA CORROMPIDA & ENTROPIA</span>
            </h1>
        </div>
        <div class="flex items-center gap-3 text-xs font-mono">
            <div class="hidden sm:flex items-center gap-2 bg-black/70 px-3 py-1 rounded border border-father-border">
                <span class="text-gray-400">REATOR MHD:</span>
                <span id="hdr-mhd-temp" class="text-father-amber font-bold">1.42×10⁹ K</span>
            </div>
            <div class="hidden md:flex items-center gap-2 bg-black/70 px-3 py-1 rounded border border-father-border">
                <span class="text-gray-400">ESTADO MÉTRO:</span>
                <span id="hdr-singularity-state" class="text-father-red font-bold">SINGULARIDADE NUA</span>
            </div>
            <button id="btn-manual" class="bg-father-red/20 hover:bg-father-red/40 text-father-red border border-father-red px-3 py-1 rounded transition text-xs font-bold">
                Manual de Campo
            </button>
        </div>
    </header>

    <!-- Main Content Workspace -->
    <div class="flex-1 grid grid-cols-1 lg:grid-cols-12 overflow-hidden relative">

        <!-- Controls Sidebar -->
        <aside class="lg:col-span-3 glass-card p-4 flex flex-col gap-3 overflow-y-auto z-10 max-h-[40vh] lg:max-h-full border-r border-father-border">
            
            <!-- Core Mechanism Mode Selector -->
            <div class="space-y-1.5">
                <label class="text-[10px] font-mono uppercase text-gray-400 tracking-wider">Sub-Sistemas do Núcleo Destrutivo</label>
                <div class="grid grid-cols-1 gap-1.5 font-mono text-xs">
                    <button id="mode-singularity" onclick="setCoreMode('SINGULARITY')" class="w-full text-left p-2 rounded border border-father-red bg-father-red/20 text-father-red font-bold transition flex items-center justify-between">
                        <span>1. Singularidade Nua</span>
                        <span class="text-[10px] bg-father-red/30 px-1.5 py-0.5 rounded">MARÉ CAÓTICA</span>
                    </button>
                    <button id="mode-mhd" onclick="setCoreMode('MHD')" class="w-full text-left p-2 rounded border border-father-border bg-black/40 text-gray-400 hover:text-father-amber transition flex items-center justify-between">
                        <span>2. Reator MHD em Sobrecarga</span>
                        <span class="text-[10px] bg-father-amber/30 px-1.5 py-0.5 rounded">COLAPSO 4D</span>
                    </button>
                    <button id="mode-sams" onclick="setCoreMode('SAMS')" class="w-full text-left p-2 rounded border border-father-border bg-black/40 text-gray-400 hover:text-father-violet transition flex items-center justify-between">
                        <span>3. SAMS de Antimatéria</span>
                        <span class="text-[10px] bg-father-violet/30 px-1.5 py-0.5 rounded">DECAIMENTO</span>
                    </button>
                </div>
            </div>

            <!-- Action Trigger Button -->
            <button onclick="triggerForcedDisintegration()" class="w-full bg-gradient-to-r from-father-red via-father-amber to-father-violet hover:opacity-90 text-black font-black text-xs p-3 rounded shadow-lg shadow-father-red/30 transition transform active:scale-95 flex flex-col items-center justify-center gap-0.5 border border-white/40 tracking-wider uppercase">
                <span class="text-sm">☢️ FORÇAR DECAIMENTO MOLECULAR</span>
                <span class="text-[9.5px] font-mono font-semibold text-black/90">DISPARAR SROS & ABSORVER MASSA</span>
            </button>

            <!-- Reactor Overdrive Slider -->
            <div class="space-y-2 bg-black/50 p-3 rounded border border-father-border font-mono text-xs">
                <div class="flex justify-between items-center">
                    <span class="text-gray-300 font-bold">Sobrecarga Magneto-Hidrodinâmica:</span>
                    <span id="val-overdrive" class="text-father-amber font-bold">150%</span>
                </div>
                <input type="range" id="slider-overdrive" min="100" max="300" value="150" class="w-full accent-father-red bg-gray-800 rounded h-1.5 cursor-pointer">
                <div class="text-[10px] text-gray-400 flex justify-between">
                    <span>100% (Limite Físico)</span>
                    <span>300% (Ruptura Entrópica)</span>
                </div>
            </div>

            <!-- Event Log Console -->
            <div class="flex-1 flex flex-col min-h-[110px] bg-black/90 rounded border border-father-border/70 p-2 font-mono text-[11px] overflow-hidden">
                <div class="text-gray-500 border-b border-gray-800 pb-1 mb-1 flex justify-between">
                    <span>> LOG DE CAMPO ENTROPIAS</span>
                    <span id="log-time" class="text-father-red">00:00:00</span>
                </div>
                <div id="log-console" class="flex-1 overflow-y-auto space-y-1 text-gray-300 pr-1">
                    <p class="text-father-red">[NÚCLEO] Singularidade nua ativa sem horizonte de eventos completo.</p>
                    <p class="text-father-amber">[REATOR] Destruindo realidades alternativas para forçar colapso temporal local.</p>
                </div>
            </div>

        </aside>

        <!-- Main Interactive 2D Canvas Display -->
        <main class="lg:col-span-6 relative flex flex-col items-center justify-center bg-black overflow-hidden">
            <canvas id="fatherCanvas" class="w-full h-full cursor-crosshair"></canvas>

            <!-- Dynamic HUD Banner -->
            <div class="absolute top-4 left-4 pointer-events-none font-mono text-xs space-y-1 bg-black/80 p-3 rounded border border-father-border backdrop-blur-md z-10">
                <div class="text-father-red font-bold flex items-center gap-2">
                    <span id="hud-status-dot" class="inline-block w-2.5 h-2.5 rounded-full bg-father-red animate-ping"></span>
                    <span id="hud-mode-title">NÚCLEO: SINGULARIDADE NUA ATIVA</span>
                </div>
                <div class="text-gray-400 text-[11px]" id="hud-mode-sub">Emissão de forças de maré gravitacionais caóticas</div>
                <div class="text-father-amber text-[11px]">Distorção Métria $R_{\mu\nu\rho\sigma}$: <span id="hud-metric-val" class="text-white">9.84×10¹² / m⁴</span></div>
            </div>

            <!-- Pulse Disintegration Banner -->
            <div id="hud-pulse-banner" class="hidden absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 pointer-events-none text-center z-20">
                <h2 class="text-4xl sm:text-6xl font-black text-transparent bg-clip-text bg-gradient-to-r from-father-red via-white to-father-amber neon-glow-red tracking-widest uppercase animate-pulse">
                    DECAIMENTO MOLECULAR
                </h2>
                <div class="text-xs font-mono text-father-red font-bold tracking-widest mt-2 bg-black/90 px-4 py-1.5 rounded border border-father-red inline-block">
                    SROS QUEBRANDO LIGAÇÕES ATÔMICAS FORTES
                </div>
            </div>
        </main>

        <!-- Scientific Telemetry Sidebar -->
        <aside class="lg:col-span-3 glass-card p-4 flex flex-col gap-3.5 overflow-y-auto z-10 max-h-[40vh] lg:max-h-full border-l border-father-border">
            
            <h2 class="text-xs font-mono uppercase text-father-red tracking-wider flex items-center gap-1 border-b border-father-border/40 pb-2 font-bold">
                <span>☢️</span> Telemetria da Caixa Pai
            </h2>

            <!-- Metric Cards -->
            <div class="grid grid-cols-2 gap-2 text-xs font-mono">
                <div class="bg-black/70 p-2 rounded border border-father-red/40">
                    <span class="text-gray-400 block text-[10px]">MARÉ GRAVITACIONAL</span>
                    <span id="tele-tide-force" class="text-father-red text-sm font-bold">4.82 PetaN</span>
                </div>
                <div class="bg-black/70 p-2 rounded border border-father-amber/40">
                    <span class="text-gray-400 block text-[10px]">TEMP. REATOR MHD</span>
                    <span id="tele-mhd-temp" class="text-father-amber text-sm font-bold">1.42×10⁹ K</span>
                </div>
                <div class="bg-black/70 p-2 rounded border border-father-violet/40">
                    <span class="text-gray-400 block text-[10px]">ISÓTOPOS SAMS</span>
                    <span id="tele-sams-isotopes" class="text-father-violet text-sm font-bold">98.4% ANTIMÁT</span>
                </div>
                <div class="bg-black/70 p-2 rounded border border-red-500/40">
                    <span class="text-gray-400 block text-[10px]">EFEITO ENTRÓPICO</span>
                    <span id="tele-entropy-idx" class="text-red-500 text-sm font-bold">99.8 ERG</span>
                </div>
            </div>

            <!-- Progress & Stress Bars -->
            <div class="bg-black/70 p-3 rounded border border-father-border space-y-2.5 font-mono text-xs">
                <div>
                    <div class="flex justify-between text-[11px] mb-1">
                        <span class="text-father-red font-bold">INSTABILIDADE DA SINGULARIDADE</span>
                        <span id="tele-instability-pct" class="text-father-red">88.4%</span>
                    </div>
                    <div class="w-full bg-gray-900 h-2 rounded overflow-hidden p-0.5 border border-gray-800">
                        <div id="bar-instability" class="bg-gradient-to-r from-amber-500 to-father-red h-full w-[88.4%] transition-all duration-300"></div>
                    </div>
                </div>

                <div>
                    <div class="flex justify-between text-[11px] mb-1">
                        <span class="text-father-amber font-bold">COLAPSO DE REALIDADES ADJACENTES</span>
                        <span id="tele-timeline-collapse" class="text-father-amber">94.1%</span>
                    </div>
                    <div class="w-full bg-gray-900 h-2 rounded overflow-hidden p-0.5 border border-gray-800">
                        <div id="bar-timeline" class="bg-gradient-to-r from-orange-600 to-father-amber h-full w-[94.1%] transition-all duration-300"></div>
                    </div>
                </div>

                <div>
                    <div class="flex justify-between text-[11px] mb-1">
                        <span class="text-father-violet font-bold">TAXA DE ABSORÇÃO DE MASSA</span>
                        <span id="tele-absorption-rate" class="text-father-violet">12.4 kg/s</span>
                    </div>
                    <div class="w-full bg-gray-900 h-2 rounded overflow-hidden p-0.5 border border-gray-800">
                        <div id="bar-absorption" class="bg-gradient-to-r from-purple-600 to-father-violet h-full w-[65%] transition-all duration-300"></div>
                    </div>
                </div>
            </div>

            <!-- Narrative Technical Text -->
            <div class="space-y-2 font-mono text-xs">
                <h3 class="text-father-red font-bold text-[11px] uppercase border-b border-father-red/20 pb-1">
                    🔬 Engenharia Corrompida
                </h3>
                <div id="father-doc" class="bg-black/50 p-2.5 rounded border border-gray-800 text-gray-300 text-[11px] leading-relaxed space-y-1.5">
                    <p>
                        A Caixa Pai força o espaço-tempo ao redor a colapsar na linha temporal mais conveniente, desintegrando matéria normal com seu SAMS de antimatéria.
                    </p>
                </div>
            </div>

            <!-- Color Legend -->
            <div class="bg-black/60 p-2.5 rounded border border-father-border/40 space-y-1 text-[11px] font-mono">
                <div class="text-gray-400 mb-0.5 font-bold">Legenda de Componentes:</div>
                <div class="flex items-center gap-2">
                    <span class="w-2.5 h-2.5 rounded-full bg-father-red inline-block"></span>
                    <span class="text-gray-300">Maré Gravitacional da Singularidade Nua</span>
                </div>
                <div class="flex items-center gap-2">
                    <span class="w-2.5 h-2.5 rounded-full bg-father-amber inline-block"></span>
                    <span class="text-gray-300">Colapso Temporal / Fluxo MHD</span>
                </div>
                <div class="flex items-center gap-2">
                    <span class="w-2.5 h-2.5 rounded-full bg-father-violet inline-block"></span>
                    <span class="text-gray-300">SAMS de Antimatéria & Lasers SROS</span>
                </div>
            </div>

        </aside>

    </div>

    <!-- Technical Manual Modal -->
    <div id="info-modal" class="hidden fixed inset-0 bg-black/85 backdrop-blur-md z-50 flex items-center justify-center p-4">
        <div class="glass-card max-w-2xl w-full p-6 rounded-lg border border-father-red space-y-4 max-h-[85vh] overflow-y-auto font-mono">
            <div class="flex justify-between items-center border-b border-father-red/30 pb-3">
                <h2 class="text-base sm:text-lg text-father-red font-bold uppercase">Manual de Engenharia: A Caixa Pai (Father Box)</h2>
                <button id="close-modal-x" class="text-gray-400 hover:text-father-red text-xl font-bold">&times;</button>
            </div>
            
            <div class="space-y-3 text-xs text-gray-300 leading-relaxed">
                <p>
                    Enquanto a Caixa Materna opera em harmonia geométrica, a **Caixa Pai** representa o limite destrutivo da engenharia métrico-dimensional.
                </p>

                <div class="bg-black/60 p-3 rounded border border-father-border space-y-1">
                    <h4 class="text-father-red font-bold">1. Singularidade Nua (Ausência de Horizonte de Eventos)</h4>
                    <p>Sem a casca protetora de Cristais de Brana, a garganta do buraco de minhoca fica desnudada. Isso expõe o ambiente a forças de maré gravitacionais caóticas que rasgam a métrica do espaço-tempo local.</p>
                </div>

                <div class="bg-black/60 p-3 rounded border border-father-border space-y-1">
                    <h4 class="text-father-amber font-bold">2. Reator MHD em Sobrecarga & Aniquilação Temporal</h4>
                    <p>O reator opera em constante superaquecimento. Ele computa probabilidades destruindo linhas temporais alternativas adjacentes, forçando a realidade a colapsar na única linha que favorece seus objetivos.</p>
                </div>

                <div class="bg-black/60 p-3 rounded border border-father-border space-y-1">
                    <h4 class="text-father-violet font-bold">3. SAMS de Antimatéria & Absorção Atômica</h4>
                    <p>O fluido magnetorreológico dopado com isótopos instáveis atua em conjunto com os lasers do SROS para quebrar as ligações fortes dos átomos do alvo, absorvendo sua massa convertida para alimentar o próprio reator.</p>
                </div>
            </div>

            <div class="text-right pt-2 border-t border-father-red/30">
                <button id="close-modal-btn" class="bg-father-red/20 hover:bg-father-red/40 text-father-red border border-father-red px-4 py-2 rounded text-xs font-bold transition">
                    ENTENDIDO - RETORNAR AO NÚCLEO
                </button>
            </div>
        </div>
    </div>

    <!-- JavaScript Canvas & Simulation Logic -->
    <script>
        // --- State Management ---
        const state = {
            mode: 'SINGULARITY',     // SINGULARITY, MHD, SAMS
            overdrive: 1.5,          // 1.0 - 3.0
            
            // Physics Metrics
            tideForce: 4.82,         // PetaN
            mhdTemp: 1.42,           // x 10^9 K
            samsAntimatter: 98.4,    // %
            entropyIdx: 99.8,
            instabilityPct: 88.4,
            timelineCollapse: 94.1,
            absorptionRate: 12.4,    // kg/s
            
            time: 0,
            pulseActive: false,
            pulseRadius: 0
        };

        const modeTexts = {
            SINGULARITY: {
                title: "NÚCLEO: SINGULARIDADE NUA ATIVA",
                sub: "Forças de maré gravitacionais extremas sem horizonte de eventos protetor",
                doc: "<p><strong class=\"text-father-red\">Maré Gravitacional:</strong> A ausência de uma casca geométrica de Brana expõe o espaço-tempo às rugosidades da singularidade, gerando gradientes de gravidade assimétricos.</p>"
            },
            MHD: {
                title: "REATOR MHD: SOBRECARGA & ANIQUILAÇÃO TEMPORAL",
                sub: "Destruindo realidades adjacentes para impor a linha temporal dominante",
                doc: "<p><strong class=\"text-father-amber\">Colapso Temporal 4D:</strong> O reator em sobreaquecimento força o colapso estocástico das linhas temporais alternativas, absorvendo a energia entrópica do processo.</p>"
            },
            SAMS: {
                title: "SAMS DE ANTIMATÉRIA: DECAIMENTO MOLECULAR",
                sub: "Lasers SROS quebrando ligações atômicas fortes e reabastecendo a caixa",
                doc: "<p><strong class=\"text-father-violet\">Decaimento Forçado:</strong> O fluido magnetorreológico dopado ioniza a matéria alvo, desintegrando a estrutura atômica e canalizando a massa ao reator.</p>"
            }
        };

        // Canvas Setup
        const canvas = document.getElementById('fatherCanvas');
        const ctx = canvas.getContext('2d');
        let width = 0;
        let height = 0;

        // Visual Entities
        let tidalParticles = [];
        let targetAtoms = [];
        let samsFluids = [];

        // Web Audio Synthesizer
        let audioCtx = null;

        function initAudio() {
            if (!audioCtx) {
                const AudioContext = window.AudioContext || window.webkitAudioContext;
                audioCtx = new AudioContext();
            }
        }

        function playDisintegrationAudio() {
            try {
                initAudio();
                if (!audioCtx) return;
                const now = audioCtx.currentTime;

                // High pitch laser pulse + low frequency disintegration rumble
                const oscLaser = audioCtx.createOscillator();
                const gainLaser = audioCtx.createGain();

                oscLaser.type = 'sawtooth';
                oscLaser.frequency.setValueAtTime(1800, now);
                oscLaser.frequency.exponentialRampToValueAtTime(300, now + 0.4);

                gainLaser.gain.setValueAtTime(0.35 * (state.overdrive / 1.5), now);
                gainLaser.gain.exponentialRampToValueAtTime(0.001, now + 0.4);

                const oscRumble = audioCtx.createOscillator();
                const gainRumble = audioCtx.createGain();

                oscRumble.type = 'square';
                oscRumble.frequency.setValueAtTime(90, now);
                oscRumble.frequency.exponentialRampToValueAtTime(20, now + 0.6);

                gainRumble.gain.setValueAtTime(0.4 * (state.overdrive / 1.5), now);
                gainRumble.gain.exponentialRampToValueAtTime(0.001, now + 0.6);

                oscLaser.connect(gainLaser);
                gainLaser.connect(audioCtx.destination);
                oscRumble.connect(gainRumble);
                gainRumble.connect(audioCtx.destination);

                oscLaser.start(now);
                oscRumble.start(now);
                oscLaser.stop(now + 0.4);
                oscRumble.stop(now + 0.6);
            } catch(e) {
                console.log("Audio ready.");
            }
        }

        function initEntities() {
            tidalParticles = [];
            targetAtoms = [];
            samsFluids = [];

            // Tidal Gravitational Particles
            for (let i = 0; i < 70; i++) {
                const angle = Math.random() * Math.PI * 2;
                const dist = Math.random() * 180 + 20;
                tidalParticles.push({
                    angle: angle,
                    dist: dist,
                    speed: (Math.random() * 0.04 + 0.01),
                    size: Math.random() * 3 + 1,
                    chaos: Math.random() * 15
                });
            }

            // Target Atoms for Disintegration
            for (let i = 0; i < 40; i++) {
                targetAtoms.push({
                    x: (Math.random() - 0.5) * 220,
                    y: (Math.random() - 0.5) * 220,
                    vx: (Math.random() - 0.5) * 1.5,
                    vy: (Math.random() - 0.5) * 1.5,
                    bonds: true,
                    integrity: 1.0
                });
            }

            // SAMS Antimatter Fluid Strands
            for (let i = 0; i < 30; i++) {
                samsFluids.push({
                    x: (Math.random() - 0.5) * 300,
                    y: (Math.random() - 0.5) * 300,
                    speed: Math.random() * 3 + 1,
                    angle: Math.random() * Math.PI * 2
                });
            }
        }

        function handleResize() {
            const container = canvas.parentElement;
            width = container.clientWidth;
            height = container.clientHeight;
            canvas.width = width;
            canvas.height = height;

            initEntities();
        }

        window.addEventListener('resize', handleResize);

        // --- Main Render Loop ---
        function draw() {
            state.time += 0.03 * state.overdrive;

            ctx.fillStyle = '#050203';
            ctx.fillRect(0, 0, width, height);

            if (state.mode === 'SINGULARITY') {
                renderNakedSingularityView();
            } else if (state.mode === 'MHD') {
                renderMHDOverdriveView();
            } else if (state.mode === 'SAMS') {
                renderSamsAntimatterView();
            }

            if (state.pulseActive) {
                renderDisintegrationPulse();
            }

            updateTelemetryData();
            requestAnimationFrame(draw);
        }

        // ==========================================
        // MODE 1: Singularidade Nua & Maré Gravitacional
        // ==========================================
        function renderNakedSingularityView() {
            const centerX = width / 2;
            const centerY = height / 2;

            ctx.save();
            ctx.translate(centerX, centerY);

            // Chaotic Accretion Disk (No Event Horizon)
            tidalParticles.forEach(p => {
                p.angle += p.speed * state.overdrive;
                const tideDist = p.dist + Math.sin(state.time * 3 + p.chaos) * (20 * state.overdrive);

                const x = Math.cos(p.angle) * tideDist;
                const y = Math.sin(p.angle) * (tideDist * 0.4); // Distorted ellipse

                ctx.fillStyle = p.dist < 60 ? '#ff003c' : (p.dist < 120 ? '#ff6b00' : 'rgba(168, 17, 218, 0.7)');
                ctx.beginPath();
                ctx.arc(x, y, p.size, 0, Math.PI * 2);
                ctx.fill();

                // Chaotic Tidal Stretch Vector Lines
                if (Math.random() > 0.7) {
                    ctx.strokeStyle = 'rgba(255, 0, 60, 0.4)';
                    ctx.lineWidth = 0.8;
                    ctx.beginPath();
                    ctx.moveTo(x, y);
                    ctx.lineTo(x + (Math.random() - 0.5) * 25, y + (Math.random() - 0.5) * 25);
                    ctx.stroke();
                }
            });

            // Naked Singularity Naked Core (Exposed Void Centre)
            ctx.fillStyle = '#ffffff';
            ctx.shadowBlur = 25 * state.overdrive;
            ctx.shadowColor = '#ff003c';
            ctx.beginPath();
            ctx.arc(0, 0, 12 + Math.sin(state.time * 8) * 4, 0, Math.PI * 2);
            ctx.fill();

            ctx.strokeStyle = '#ff003c';
            ctx.lineWidth = 2.5;
            ctx.beginPath();
            ctx.arc(0, 0, 22 + Math.cos(state.time * 6) * 6, 0, Math.PI * 2);
            ctx.stroke();
            ctx.shadowBlur = 0;

            ctx.restore();
        }

        // ==========================================
        // MODE 2: Reator MHD & Aniquilação Temporal 4D
        // ==========================================
        function renderMHDOverdriveView() {
            const centerX = width / 2;
            const centerY = height / 2;

            ctx.save();
            ctx.translate(centerX, centerY);

            // Overheated Magnetic Flux Rings
            const rings = 5;
            for (let r = 1; r <= rings; r++) {
                const radius = r * 32;
                const angleOffset = state.time * (r % 2 === 0 ? 1.2 : -1.2);

                ctx.strokeStyle = r % 2 === 0 ? '#ff6b00' : '#ff003c';
                ctx.lineWidth = 2;
                ctx.beginPath();

                const segments = 12;
                for (let s = 0; s <= segments; s++) {
                    const a = (s / segments) * Math.PI * 2 + angleOffset;
                    const noise = (Math.random() - 0.5) * 10 * state.overdrive;
                    const x = Math.cos(a) * (radius + noise);
                    const y = Math.sin(a) * (radius + noise);

                    if (s === 0) ctx.moveTo(x, y);
                    else ctx.lineTo(x, y);
                }
                ctx.stroke();
            }

            // Alternative Timeline Collapsing Fracture Lines
            for (let i = 0; i < 8; i++) {
                const angle = (i / 8) * Math.PI * 2 + state.time;
                ctx.strokeStyle = 'rgba(168, 17, 218, 0.6)';
                ctx.lineWidth = 1.2;
                ctx.beginPath();
                ctx.moveTo(0, 0);
                ctx.lineTo(Math.cos(angle) * 180, Math.sin(angle) * 180);
                ctx.stroke();
            }

            ctx.restore();
        }

        // ==========================================
        // MODE 3: SAMS de Antimatéria & Decaimento Atômico
        // ==========================================
        function renderSamsAntimatterView() {
            const centerX = width / 2;
            const centerY = height / 2;

            ctx.save();
            ctx.translate(centerX, centerY);

            // SROS Micro-Lasers breaking atomic bonds
            targetAtoms.forEach((atom, idx) => {
                atom.x += atom.vx;
                atom.y += atom.vy;

                if (Math.abs(atom.x) > 140) atom.vx *= -1;
                if (Math.abs(atom.y) > 140) atom.vy *= -1;

                // Render Atom Nucleus
                ctx.fillStyle = atom.integrity < 0.5 ? '#a811da' : '#00ffaa';
                ctx.beginPath();
                ctx.arc(atom.x, atom.y, 4, 0, Math.PI * 2);
                ctx.fill();

                // Draw SROS Disintegration Laser targeting atoms
                if (idx % 3 === 0) {
                    ctx.strokeStyle = 'rgba(255, 0, 60, 0.7)';
                    ctx.lineWidth = 1;
                    ctx.beginPath();
                    ctx.moveTo(0, 0);
                    ctx.lineTo(atom.x, atom.y);
                    ctx.stroke();

                    // Disintegration Sparks
                    ctx.fillStyle = '#ff6b00';
                    ctx.fillRect(atom.x + (Math.random() - 0.5) * 8, atom.y + (Math.random() - 0.5) * 8, 2, 2);
                }
            });

            // Antimatter Fluid Intake Swirl
            samsFluids.forEach(f => {
                f.angle += 0.05 * state.overdrive;
                const rad = 40 + Math.sin(f.angle) * 30;
                const x = Math.cos(f.angle) * rad;
                const y = Math.sin(f.angle) * rad;

                ctx.fillStyle = 'rgba(168, 17, 218, 0.8)';
                ctx.fillRect(x, y, 3, 3);
            });

            ctx.restore();
        }

        // Pulse Effect Rendering
        function renderDisintegrationPulse() {
            state.pulseRadius += 14 * state.overdrive;
            const centerX = width / 2;
            const centerY = height / 2;

            ctx.strokeStyle = `rgba(255, 0, 60, ${Math.max(0, 1 - state.pulseRadius / (width * 0.55))})`;
            ctx.lineWidth = 8;
            ctx.beginPath();
            ctx.arc(centerX, centerY, state.pulseRadius, 0, Math.PI * 2);
            ctx.stroke();

            if (state.pulseRadius > width * 0.55) {
                state.pulseActive = false;
            }
        }

        // --- Controls & System State ---
        function setCoreMode(mode) {
            state.mode = mode;

            ['singularity', 'mhd', 'sams'].forEach(m => {
                const btn = document.getElementById(`mode-${m}`);
                btn.className = 'w-full text-left p-2 rounded border border-father-border bg-black/40 text-gray-400 hover:text-white transition flex items-center justify-between';
            });

            const activeBtn = document.getElementById(`mode-${mode.toLowerCase()}`);
            const colorClass = mode === 'SINGULARITY' ? 'border-father-red bg-father-red/20 text-father-red font-bold' : (mode === 'MHD' ? 'border-father-amber bg-father-amber/20 text-father-amber font-bold' : 'border-father-violet bg-father-violet/20 text-father-violet font-bold');
            activeBtn.className = `w-full text-left p-2 rounded border transition flex items-center justify-between ${colorClass}`;

            const info = modeTexts[mode];
            document.getElementById('hud-mode-title').innerText = info.title;
            document.getElementById('hud-mode-sub').innerText = info.sub;
            document.getElementById('father-doc').innerHTML = info.doc;

            addLog(`[SISTEMA] Modo operacional alterado para: ${mode}`);
        }

        function triggerForcedDisintegration() {
            state.pulseActive = true;
            state.pulseRadius = 5;
            playDisintegrationAudio();

            const banner = document.getElementById('hud-pulse-banner');
            banner.classList.remove('hidden');
            setTimeout(() => { banner.classList.add('hidden'); }, 1400);

            addLog('☢️ [DECAIMENTO MOLECULAR] Feixe SROS disparado. Quebrando ligações fortes e absorvendo massa.', 'red');
        }

        // Dynamic Telemetry Calculation
        function updateTelemetryData() {
            state.overdrive = parseFloat(document.getElementById('slider-overdrive').value) / 100;
            document.getElementById('val-overdrive').innerText = `${Math.round(state.overdrive * 100)}%`;

            state.tideForce = (4.82 * state.overdrive).toFixed(2);
            state.mhdTemp = (1.42 * state.overdrive).toFixed(2);
            state.instabilityPct = Math.min(100, (88.4 * state.overdrive)).toFixed(1);
            state.timelineCollapse = Math.min(100, (94.1 * state.overdrive)).toFixed(1);
            state.absorptionRate = (12.4 * state.overdrive).toFixed(1);

            document.getElementById('hdr-mhd-temp').innerText = `${state.mhdTemp}×10⁹ K`;
            document.getElementById('tele-tide-force').innerText = `${state.tideForce} PetaN`;
            document.getElementById('tele-mhd-temp').innerText = `${state.mhdTemp}×10⁹ K`;

            document.getElementById('tele-instability-pct').innerText = `${state.instabilityPct}%`;
            document.getElementById('bar-instability').style.width = `${state.instabilityPct}%`;

            document.getElementById('tele-timeline-collapse').innerText = `${state.timelineCollapse}%`;
            document.getElementById('bar-timeline').style.width = `${state.timelineCollapse}%`;

            document.getElementById('tele-absorption-rate').innerText = `${state.absorptionRate} kg/s`;
            document.getElementById('bar-absorption').style.width = `${Math.min(100, state.absorptionRate * 3)}%`;

            document.getElementById('hud-metric-val').innerText = `${(9.84 * state.overdrive).toFixed(2)}×10¹² / m⁴`;
            document.getElementById('log-time').innerText = new Date().toTimeString().split(' ')[0];
        }

        function addLog(msg, type = 'red') {
            const consoleBox = document.getElementById('log-console');
            const p = document.createElement('p');

            if (type === 'red') p.className = 'text-father-red font-bold';
            else if (type === 'amber') p.className = 'text-father-amber font-bold';
            else p.className = 'text-father-violet font-bold';

            p.innerText = `${msg}`;
            consoleBox.appendChild(p);
            consoleBox.scrollTop = consoleBox.scrollHeight;
        }

        // Window Load Initialization
        window.onload = () => {
            handleResize();

            document.getElementById('btn-manual').addEventListener('click', () => {
                document.getElementById('info-modal').classList.remove('hidden');
            });
            const closeModal = () => document.getElementById('info-modal').classList.add('hidden');
            document.getElementById('close-modal-x').addEventListener('click', closeModal);
            document.getElementById('close-modal-btn').addEventListener('click', closeModal);

            setCoreMode('SINGULARITY');
            draw();
            addLog('[INICIALIZAÇÃO] Caixa Pai (Father Box) - Engenharia Destrutiva Ativa.');
        };
    </script>
</body>
</html>
