  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ZedSounds Lite | The Home of Zambian Music</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { background-color: #0b0e14; color: #e2e8f0; font-family: 'Inter', sans-serif; }
        .copper-gradient { background: linear-gradient(135deg, #d9480f 0%, #f76707 100%); }
        .glass-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.05); border-radius: 1.5rem; }
        .wa-green { background-color: #25d366; color: #fff; }
        .active-tab { border-bottom: 2px solid #d9480f; color: white; }
    </style>
</head>
<body class="pb-32">

    <!-- Navigation -->
    <nav class="flex items-center justify-between px-4 py-4 sticky top-0 z-50 bg-black/90 backdrop-blur-md border-b border-white/5">
        <div class="flex items-center gap-2">
            <div class="w-8 h-8 copper-gradient rounded-lg flex items-center justify-center font-black text-white italic shadow-lg">ZS</div>
            <h1 class="text-md font-bold tracking-tighter uppercase">ZED<span class="text-orange-500">SOUNDS</span></h1>
        </div>
        <div class="flex items-center gap-2">
            <span class="text-[9px] font-bold text-green-500 bg-green-500/10 px-2 py-0.5 rounded border border-green-500/20">LITE MODE</span>
        </div>
    </nav>

    <main class="px-4 mt-6">
        <!-- Main Featured Release Card -->
        <div class="glass-card p-5 mb-8 relative overflow-hidden">
            <div class="relative z-10">
                <span class="text-[10px] font-black uppercase text-orange-500 tracking-widest mb-1 block">New Trending</span>
                <h2 class="text-2xl font-black italic mb-4">Amapalo</h2>
                
                <div class="flex items-center gap-3 mb-6">
                    <div class="w-14 h-14 bg-slate-800 rounded-xl flex items-center justify-center text-2xl">🎵</div>
                    <div>
                        <p class="font-bold text-lg">Yo Maps</p>
                        <p class="text-[11px] text-slate-500">Zed-Pop • Featured Single</p>
                    </div>
                </div>

                <!-- Action Buttons -->
                <div class="flex flex-col gap-3">
                    <button onclick="playSong('Amapalo', 'Yo Maps')" class="copper-gradient w-full py-4 rounded-xl font-black text-sm uppercase tracking-widest shadow-lg active:scale-95 transition-transform">
                        Play Now
                    </button>
                    
                    <button onclick="shareOnWhatsApp('Amapalo by Yo Maps')" class="wa-green w-full py-3 rounded-xl font-bold text-xs uppercase tracking-widest flex items-center justify-center gap-2 active:scale-95 transition-transform">
                        <span>Share on WhatsApp</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- AI Artist Scout Section -->
        <section class="mb-8 p-5 glass-card border-orange-500/20">
            <div class="flex items-center gap-2 mb-3">
                <span class="text-lg">✨</span>
                <h3 class="font-bold text-sm">Gemini Artist Scout</h3>
            </div>
            <p class="text-[11px] text-slate-400 mb-4">Type a genre to find Zambian talent.</p>
            <div class="flex gap-2">
                <input id="genreInput" type="text" placeholder="e.g. Zamrock" class="flex-1 bg-black/40 border border-white/10 rounded-xl px-4 py-2 text-sm outline-none focus:border-orange-500">
                <button onclick="scoutArtist()" class="bg-white text-black px-4 py-2 rounded-xl text-[10px] font-black uppercase">Find</button>
            </div>
            <div id="scoutOutput" class="mt-3 text-[11px] text-orange-400 font-medium italic min-h-[1rem]"></div>
        </section>

        <!-- Categories -->
        <h3 class="font-bold text-xs mb-4 uppercase tracking-widest text-slate-500 px-1">Browse Genres</h3>
        <div class="grid grid-cols-2 gap-3 mb-8">
            <div class="p-5 glass-card border-green-500/10">
                <h4 class="font-bold text-sm">Kalindula</h4>
                <p class="text-[9px] text-slate-500">Traditional Vibes</p>
            </div>
            <div class="p-5 glass-card border-blue-500/10">
                <h4 class="font-bold text-sm">Zamrock</h4>
                <p class="text-[9px] text-slate-500">70s Rock Heritage</p>
            </div>
        </div>
    </main>

    <!-- Simple Music Player Bar -->
    <div id="playerBar" class="fixed bottom-0 left-0 right-0 bg-slate-900/95 backdrop-blur-xl border-t border-white/10 h-20 px-5 flex items-center justify-between z-50 translate-y-full transition-transform duration-300">
        <div class="flex items-center gap-3 w-2/3">
            <div class="w-10 h-10 copper-gradient rounded-lg flex items-center justify-center text-white font-bold text-xs">ZS</div>
            <div class="overflow-hidden">
                <p id="pTitle" class="font-bold text-xs truncate">Song Title</p>
                <p id="pArtist" class="text-[10px] text-slate-500">Artist</p>
            </div>
        </div>
        <div class="flex items-center gap-4">
            <button onclick="togglePlay()" id="playBtn" class="w-10 h-10 bg-white text-black rounded-full font-bold flex items-center justify-center shadow-lg">▶</button>
        </div>
    </div>

    <script>
        // WhatsApp Sharing
        function shareOnWhatsApp(text) {
            const url = encodeURIComponent(window.location.href);
            window.open(`https://wa.me/?text=Yo! Listen to ${text} on ZedSounds! 🇿🇲 ${url}`, '_blank');
        }

        // Gemini Artist Scout Logic
        function scoutArtist() {
            const input = document.getElementById('genreInput');
            const output = document.getElementById('scoutOutput');
            const genre = input.value.trim().toLowerCase();
            
            if(!genre) return;

            output.innerText = "Consulting the archives...";
            
            setTimeout(() => {
                if(genre.includes('pop')) {
                    output.innerText = "Check out Yo Maps, T-Low, and Chile One Mr Zambia.";
                } else if(genre.includes('rock')) {
                    output.innerText = "WITCH and Amanaz are essential Zamrock listening.";
                } else if(genre.includes('hip')) {
                    output.innerText = "Chef 187, Slapdee, and Macky 2 lead the game.";
                } else {
                    output.innerText = "Try searching for Zed-Pop, Kalindula, or Zamrock!";
                }
            }, 800);
        }

        // Player Controls
        let isPlaying = false;
        function playSong(t, a) {
            document.getElementById('pTitle').innerText = t;
            document.getElementById('pArtist').innerText = a;
            document.getElementById('playerBar').classList.remove('translate-y-full');
            isPlaying = true;
            document.getElementById('playBtn').innerText = '⏸';
        }

        function togglePlay() {
            isPlaying = !isPlaying;
            document.getElementById('playBtn').innerText = isPlaying ? '⏸' : '▶';
        }
    </script>
</body>
</html>          setTimeout(() => {
                if(genre.includes('pop')) {
                    output.innerText = "Check out Yo Maps, T-Low, and Chile One Mr Zambia.";
                } else if(genre.includes('rock')) {
                    output.innerText = "WITCH and Amanaz are essential Zamrock listening.";
                } else if(genre.includes('hip')) {
                    output.innerText = "Chef 187, Slapdee, and Macky 2 lead the game.";
                } else {
                    output.innerText = "Try searching for Zed-Pop, Kalindula, or Zamrock!";
                }
            }, 800);
        }

        // Player Controls
        let isPlaying = false;
        function playSong(t, a) {
            document.getElementById('pTitle').innerText = t;
            document.getElementById('pArtist').innerText = a;
            document.getElementById('playerBar').classList.remove('translate-y-full');
            isPlaying = true;
            document.getElementById('playBtn').innerText = '⏸';
        }

        function togglePlay() {
            isPlaying = !isPlaying;
            document.getElementById('playBtn').innerText = isPlaying ? '⏸' : '▶';
        }
    </script>
</body>
</html>                  <span class="opacity-50 group-hover:opacity-100 transition-opacity">▶</span>
                    </button>
                    <button data-feature="whatsapp" class="tab-btn tab-inactive border-2 text-left px-5 py-4 rounded-2xl font-bold text-lg transition-all duration-300 flex justify-between items-center group">
                        <span class="flex items-center gap-3"><span class="text-2xl">📱</span> WhatsApp Sharing</span>
                        <span class="opacity-50 group-hover:opacity-100 transition-opacity">▶</span>
                    </button>
                    <button data-feature="momo" class="tab-btn tab-inactive border-2 text-left px-5 py-4 rounded-2xl font-bold text-lg transition-all duration-300 flex justify-between items-center group">
                        <span class="flex items-center gap-3"><span class="text-2xl">💸</span> MoMo Tip Jar</span>
                        <span class="opacity-50 group-hover:opacity-100 transition-opacity">▶</span>
                    </button>
                </div>

                <div class="mt-6 bg-alabaster border border-borderline rounded-2xl p-6 min-h-[220px]" id="content-display">
                    <h4 class="text-xl font-bold text-terracotta mb-2" id="detail-title">Data-Saver Search</h4>
                    <p class="text-charcoal/80 leading-relaxed mb-4" id="detail-text">
                        Instead of loading thousands of songs at once, this feature implements a simple search bar that restricts queries to the "Top 20" songs.
                    </p>
                    <div class="bg-white px-4 py-3 rounded-xl border border-borderline/50 inline-block">
                        <span class="text-xs font-bold uppercase tracking-wider text-sage block mb-1">Strategic Goal</span>
                        <span class="font-medium text-sm" id="detail-goal">Keeps file sizes drastically small, ensuring instant load times on 3G networks.</span>
                    </div>
                </div>
            </div>

            <div class="lg:col-span-7 bg-alabaster rounded-3xl p-6 border border-borderline flex flex-col justify-center items-center">
                <div class="text-center mb-4">
                    <h4 class="font-bold text-lg text-charcoal">Projected Impact Matrix</h4>
                    <p class="text-xs text-charcoal/60 uppercase tracking-widest">Relative Score (0-100)</p>
                </div>
                <div class="chart-container w-full">
                    <canvas id="impactChart"></canvas>
                </div>
            </div>

        </div>
    </section>

    <section class="mb-12">
        <h3 class="text-2xl font-bold mb-4 flex items-center gap-2"><span>🚀</span> Deployment Protocol</h3>
        <p class="text-lg leading-relaxed text-charcoal/80 max-w-4xl mb-8">
            This section outlines the actionable steps required to push the Phase 2 updates to the live repository. The process is designed to be executable entirely via mobile browser. Click the steps below as you complete them to track implementation progress and confirm the final live update.
        </p>

        <div class="bg-white rounded-3xl p-6 md:p-10 shadow-sm border border-borderline/50">
            
            <div class="mb-8">
                <div class="flex justify-between items-end mb-2">
                    <span class="font-bold text-sm uppercase tracking-widest text-charcoal/60">Deployment Progress</span>
                    <span class="font-bold text-2xl text-sage" id="progress-text">0%</span>
                </div>
                <div class="w-full bg-alabaster h-4 rounded-full overflow-hidden border border-borderline/30">
                    <div id="progress-bar" class="h-full bg-sage w-0 transition-all duration-700 ease-out"></div>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4" id="checklist">
                
                <button data-step="1" class="step-btn border-2 border-borderline bg-alabaster rounded-2xl p-5 text-left transition-all duration-300 hover:border-sage/50 flex flex-col justify-between h-32">
                    <span class="text-2xl mb-2 block filter grayscale">📂</span>
                    <span class="font-bold text-sm leading-tight">1. Open index.html</span>
                </button>
                
                <button data-step="2" class="step-btn border-2 border-borderline bg-alabaster rounded-2xl p-5 text-left transition-all duration-300 hover:border-sage/50 flex flex-col justify-between h-32">
                    <span class="text-2xl mb-2 block filter grayscale">✏️</span>
                    <span class="font-bold text-sm leading-tight">2. Tap Pencil Icon</span>
                </button>

                <button data-step="3" class="step-btn border-2 border-borderline bg-alabaster rounded-2xl p-5 text-left transition-all duration-300 hover:border-sage/50 flex flex-col justify-between h-32">
                    <span class="text-2xl mb-2 block filter grayscale">📋</span>
                    <span class="font-bold text-sm leading-tight">3. Paste Code</span>
                </button>

                <button data-step="4" class="step-btn border-2 border-borderline bg-alabaster rounded-2xl p-5 text-left transition-all duration-300 hover:border-sage/50 flex flex-col justify-between h-32">
                    <span class="text-2xl mb-2 block filter grayscale">💾</span>
                    <span class="font-bold text-sm leading-tight">4. Commit Changes</span>
                </button>

                <button data-step="5" class="step-btn border-2 border-borderline bg-alabaster rounded-2xl p-5 text-left transition-all duration-300 hover:border-sage/50 flex flex-col justify-between h-32">
                    <span class="text-2xl mb-2 block filter grayscale">🌐</span>
                    <span class="font-bold text-sm leading-tight">5. Wait 30s for Live</span>
                </button>

            </div>

            <div id="success-message" class="mt-8 bg-sage/10 border border-sage/30 rounded-2xl p-6 text-center transform scale-95 opacity-0 transition-all duration-500 hidden">
                <span class="text-4xl block mb-3">🎉</span>
                <h4 class="text-xl font-bold text-sage mb-2">Deployment Complete!</h4>
                <p class="text-charcoal/70 text-sm">Your live link has been successfully updated with the Phase 2 functionality.</p>
            </div>

        </div>
    </section>

</div>

<script>
document.addEventListener('DOMContentLoaded', () => {

    const featureData = {
        search: {
            title: "Data-Saver Search",
            text: "Instead of loading thousands of songs at once, this feature implements a simple search bar that restricts queries to the 'Top 20' songs.",
            goal: "Keeps file sizes drastically small, ensuring instant load times on 3G networks.",
            chart: [30, 95, 20, 40, 85]
        },
        whatsapp: {
            title: "WhatsApp Sharing Integration",
            text: "In Zambia, music spreads predominantly through WhatsApp. We added a 'Share on WhatsApp' button next to every song so users can send direct links to their network instantly.",
            goal: "Facilitates rapid viral spread without requiring complex backend social sharing mechanisms.",
            chart: [90, 75, 40, 95, 60]
        },
        momo: {
            title: "\"Support the Artist\" (Mobile Money)",
            text: "A straightforward button allowing fans to click 'Support' to reveal the Artist's MTN or Airtel Mobile Money number directly on the UI.",
            goal: "The fastest, most culturally relevant way to help local artists monetize without complex payment gateway integrations.",
            chart: [60, 80, 95, 50, 70]
        }
    };

    const ctx = document.getElementById('impactChart').getContext('2d');
    
    let impactChart = new Chart(ctx, {
        type: 'radar',
        data: {
            labels: ['User Engagement', 'Data Efficiency', 'Monetization Potential', 'Viral Reach', 'Ease of Implementation'],
            datasets: [{
                label: 'Feature Impact Score',
                data: featureData.search.chart,
                backgroundColor: 'rgba(194, 122, 101, 0.4)',
                borderColor: 'rgba(194, 122, 101, 1)',
                borderWidth: 2,
                pointBackgroundColor: 'rgba(194, 122, 101, 1)',
                pointBorderColor: '#fff',
                pointHoverBackgroundColor: '#fff',
                pointHoverBorderColor: 'rgba(194, 122, 101, 1)'
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                r: {
                    angleLines: { color: 'rgba(45, 45, 45, 0.1)' },
                    grid: { color: 'rgba(45, 45, 45, 0.1)' },
                    pointLabels: {
                        color: '#2D2D2D',
                        font: { family: 'Inter', size: 11, weight: 'bold' }
                    },
                    ticks: {
                        display: false,
                        min: 0,
                        max: 100
                    }
                }
            },
            plugins: {
                legend: { display: false },
                tooltip: {
                    backgroundColor: '#2D2D2D',
                    titleFont: { family: 'Inter', size: 13 },
                    bodyFont: { family: 'Inter', size: 12 },
                    padding: 10,
                    cornerRadius: 8,
                    displayColors: false
                }
            }
        }
    });

    const tabBtns = document.querySelectorAll('.tab-btn');
    const dTitle = document.getElementById('detail-title');
    const dText = document.getElementById('detail-text');
    const dGoal = document.getElementById('detail-goal');

    tabBtns.forEach(btn => {
        btn.addEventListener('click', () => {
            tabBtns.forEach(b => {
                b.classList.remove('tab-active');
                b.classList.add('tab-inactive');
            });
            
            btn.classList.remove('tab-inactive');
            btn.classList.add('tab-active');

            const key = btn.getAttribute('data-feature');
            const data = featureData[key];

            dTitle.textContent = data.title;
            dText.textContent = data.text;
            dGoal.textContent = data.goal;

            impactChart.data.datasets[0].data = data.chart;
            
            if(key === 'search') {
                impactChart.data.datasets[0].backgroundColor = 'rgba(194, 122, 101, 0.4)';
                impactChart.data.datasets[0].borderColor = 'rgba(194, 122, 101, 1)';
            } else if (key === 'whatsapp') {
                impactChart.data.datasets[0].backgroundColor = 'rgba(139, 168, 142, 0.4)';
                impactChart.data.datasets[0].borderColor = 'rgba(139, 168, 142, 1)';
            } else {
                impactChart.data.datasets[0].backgroundColor = 'rgba(45, 45, 45, 0.4)';
                impactChart.data.datasets[0].borderColor = 'rgba(45, 45, 45, 1)';
            }
            
            impactChart.update();
        });
    });

    const stepBtns = document.querySelectorAll('.step-btn');
    const progressBar = document.getElementById('progress-bar');
    const progressText = document.getElementById('progress-text');
    const successMsg = document.getElementById('success-message');
    let completedSteps = 0;
    const totalSteps = 5;

    stepBtns.forEach(btn => {
        btn.addEventListener('click', () => {
            if(!btn.classList.contains('step-completed')) {
                btn.classList.remove('bg-alabaster', 'border-borderline');
                btn.classList.add('step-completed');
                
                const emoji = btn.querySelector('span');
                emoji.classList.remove('filter', 'grayscale');

                completedSteps++;
                const percentage = (completedSteps / totalSteps) * 100;
                
                progressBar.style.width = percentage + '%';
                progressText.textContent = Math.round(percentage) + '%';

                if(completedSteps === totalSteps) {
                    successMsg.classList.remove('hidden');
                    setTimeout(() => {
                        successMsg.classList.remove('scale-95', 'opacity-0');
                        successMsg.classList.add('scale-100', 'opacity-100');
                    }, 50);
                }
            }
        });
    });

});
</script>

</body>
</html>                        <span class="flex items-center gap-3"><span class="text-2xl">✨</span> Data-Saver Search</span>
                        <span class="opacity-50 group-hover:opacity-100 transition-opacity">▶</span>
                    </button>
                    <button data-feature="whatsapp" class="tab-btn tab-inactive border-2 text-left px-5 py-4 rounded-2xl font-bold text-lg transition-all duration-300 flex justify-between items-center group">
                        <span class="flex items-center gap-3"><span class="text-2xl">📱</span> WhatsApp Sharing</span>
                        <span class="opacity-50 group-hover:opacity-100 transition-opacity">▶</span>
                    </button>
                    <button data-feature="momo" class="tab-btn tab-inactive border-2 text-left px-5 py-4 rounded-2xl font-bold text-lg transition-all duration-300 flex justify-between items-center group">
                        <span class="flex items-center gap-3"><span class="text-2xl">💸</span> MoMo Tip Jar</span>
                        <span class="opacity-50 group-hover:opacity-100 transition-opacity">▶</span>
                    </button>
                </div>

                <div class="mt-6 bg-alabaster border border-borderline rounded-2xl p-6 min-h-[220px]" id="content-display">
                    <h4 class="text-xl font-bold text-terracotta mb-2" id="detail-title">Data-Saver Search</h4>
                    <p class="text-charcoal/80 leading-relaxed mb-4" id="detail-text">
                        Instead of loading thousands of songs at once, this feature implements a simple search bar that restricts queries to the "Top 20" songs.
                    </p>
                    <div class="bg-white px-4 py-3 rounded-xl border border-borderline/50 inline-block">
                        <span class="text-xs font-bold uppercase tracking-wider text-sage block mb-1">Strategic Goal</span>
                        <span class="font-medium text-sm" id="detail-goal">Keeps file sizes drastically small, ensuring instant load times on 3G networks.</span>
                    </div>
                </div>
            </div>

            <div class="lg:col-span-7 bg-alabaster rounded-3xl p-6 border border-borderline flex flex-col justify-center items-center">
                <div class="text-center mb-4">
                    <h4 class="font-bold text-lg text-charcoal">Projected Impact Matrix</h4>
                    <p class="text-xs text-charcoal/60 uppercase tracking-widest">Relative Score (0-100)</p>
                </div>
                <div class="chart-container w-full">
                    <canvas id="impactChart"></canvas>
                </div>
            </div>

        </div>
    </section>

    <section class="mb-12">
        <h3 class="text-2xl font-bold mb-4 flex items-center gap-2"><span>🚀</span> Deployment Protocol</h3>
        <p class="text-lg leading-relaxed text-charcoal/80 max-w-4xl mb-8">
            This section outlines the actionable steps required to push the Phase 2 updates to the live repository. The process is designed to be executable entirely via mobile browser. Click the steps below as you complete them to track implementation progress and confirm the final live update.
        </p>

        <div class="bg-white rounded-3xl p-6 md:p-10 shadow-sm border border-borderline/50">
            
            <div class="mb-8">
                <div class="flex justify-between items-end mb-2">
                    <span class="font-bold text-sm uppercase tracking-widest text-charcoal/60">Deployment Progress</span>
                    <span class="font-bold text-2xl text-sage" id="progress-text">0%</span>
                </div>
                <div class="w-full bg-alabaster h-4 rounded-full overflow-hidden border border-borderline/30">
                    <div id="progress-bar" class="h-full bg-sage w-0 transition-all duration-700 ease-out"></div>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4" id="checklist">
                
                <button data-step="1" class="step-btn border-2 border-borderline bg-alabaster rounded-2xl p-5 text-left transition-all duration-300 hover:border-sage/50 flex flex-col justify-between h-32">
                    <span class="text-2xl mb-2 block filter grayscale">📂</span>
                    <span class="font-bold text-sm leading-tight">1. Open index.html</span>
                </button>
                
                <button data-step="2" class="step-btn border-2 border-borderline bg-alabaster rounded-2xl p-5 text-left transition-all duration-300 hover:border-sage/50 flex flex-col justify-between h-32">
                    <span class="text-2xl mb-2 block filter grayscale">✏️</span>
                    <span class="font-bold text-sm leading-tight">2. Tap Pencil Icon</span>
                </button>

                <button data-step="3" class="step-btn border-2 border-borderline bg-alabaster rounded-2xl p-5 text-left transition-all duration-300 hover:border-sage/50 flex flex-col justify-between h-32">
                    <span class="text-2xl mb-2 block filter grayscale">📋</span>
                    <span class="font-bold text-sm leading-tight">3. Paste Code</span>
                </button>

                <button data-step="4" class="step-btn border-2 border-borderline bg-alabaster rounded-2xl p-5 text-left transition-all duration-300 hover:border-sage/50 flex flex-col justify-between h-32">
                    <span class="text-2xl mb-2 block filter grayscale">💾</span>
                    <span class="font-bold text-sm leading-tight">4. Commit Changes</span>
                </button>

                <button data-step="5" class="step-btn border-2 border-borderline bg-alabaster rounded-2xl p-5 text-left transition-all duration-300 hover:border-sage/50 flex flex-col justify-between h-32">
                    <span class="text-2xl mb-2 block filter grayscale">🌐</span>
                    <span class="font-bold text-sm leading-tight">5. Wait 30s for Live</span>
                </button>

            </div>

            <div id="success-message" class="mt-8 bg-sage/10 border border-sage/30 rounded-2xl p-6 text-center transform scale-95 opacity-0 transition-all duration-500 hidden">
                <span class="text-4xl block mb-3">🎉</span>
                <h4 class="text-xl font-bold text-sage mb-2">Deployment Complete!</h4>
                <p class="text-charcoal/70 text-sm">Your live link has been successfully updated with the Phase 2 functionality.</p>
            </div>

        </div>
    </section>

</div>

<script>
document.addEventListener('DOMContentLoaded', () => {

    const featureData = {
        search: {
            title: "Data-Saver Search",
            text: "Instead of loading thousands of songs at once, this feature implements a simple search bar that restricts queries to the 'Top 20' songs.",
            goal: "Keeps file sizes drastically small, ensuring instant load times on 3G networks.",
            chart: [30, 95, 20, 40, 85]
        },
        whatsapp: {
            title: "WhatsApp Sharing Integration",
            text: "In Zambia, music spreads predominantly through WhatsApp. We added a 'Share on WhatsApp' button next to every song so users can send direct links to their network instantly.",
            goal: "Facilitates rapid viral spread without requiring complex backend social sharing mechanisms.",
            chart: [90, 75, 40, 95, 60]
        },
        momo: {
            title: "\"Support the Artist\" (Mobile Money)",
            text: "A straightforward button allowing fans to click 'Support' to reveal the Artist's MTN or Airtel Mobile Money number directly on the UI.",
            goal: "The fastest, most culturally relevant way to help local artists monetize without complex payment gateway integrations.",
            chart: [60, 80, 95, 50, 70]
        }
    };

    const ctx = document.getElementById('impactChart').getContext('2d');
    
    let impactChart = new Chart(ctx, {
        type: 'radar',
        data: {
            labels: ['User Engagement', 'Data Efficiency', 'Monetization Potential', 'Viral Reach', 'Ease of Implementation'],
            datasets: [{
                label: 'Feature Impact Score',
                data: featureData.search.chart,
                backgroundColor: 'rgba(194, 122, 101, 0.4)',
                borderColor: 'rgba(194, 122, 101, 1)',
                borderWidth: 2,
                pointBackgroundColor: 'rgba(194, 122, 101, 1)',
                pointBorderColor: '#fff',
                pointHoverBackgroundColor: '#fff',
                pointHoverBorderColor: 'rgba(194, 122, 101, 1)'
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                r: {
                    angleLines: { color: 'rgba(45, 45, 45, 0.1)' },
                    grid: { color: 'rgba(45, 45, 45, 0.1)' },
                    pointLabels: {
                        color: '#2D2D2D',
                        font: { family: 'Inter', size: 11, weight: 'bold' }
                    },
                    ticks: {
                        display: false,
                        min: 0,
                        max: 100
                    }
                }
            },
            plugins: {
                legend: { display: false },
                tooltip: {
                    backgroundColor: '#2D2D2D',
                    titleFont: { family: 'Inter', size: 13 },
                    bodyFont: { family: 'Inter', size: 12 },
                    padding: 10,
                    cornerRadius: 8,
                    displayColors: false
                }
            }
        }
    });

    const tabBtns = document.querySelectorAll('.tab-btn');
    const dTitle = document.getElementById('detail-title');
    const dText = document.getElementById('detail-text');
    const dGoal = document.getElementById('detail-goal');

    tabBtns.forEach(btn => {
        btn.addEventListener('click', () => {
            tabBtns.forEach(b => {
                b.classList.remove('tab-active');
                b.classList.add('tab-inactive');
            });
            
            btn.classList.remove('tab-inactive');
            btn.classList.add('tab-active');

            const key = btn.getAttribute('data-feature');
            const data = featureData[key];

            dTitle.textContent = data.title;
            dText.textContent = data.text;
            dGoal.textContent = data.goal;

            impactChart.data.datasets[0].data = data.chart;
            
            if(key === 'search') {
                impactChart.data.datasets[0].backgroundColor = 'rgba(194, 122, 101, 0.4)';
                impactChart.data.datasets[0].borderColor = 'rgba(194, 122, 101, 1)';
            } else if (key === 'whatsapp') {
                impactChart.data.datasets[0].backgroundColor = 'rgba(139, 168, 142, 0.4)';
                impactChart.data.datasets[0].borderColor = 'rgba(139, 168, 142, 1)';
            } else {
                impactChart.data.datasets[0].backgroundColor = 'rgba(45, 45, 45, 0.4)';
                impactChart.data.datasets[0].borderColor = 'rgba(45, 45, 45, 1)';
            }
            
            impactChart.update();
        });
    });

    const stepBtns = document.querySelectorAll('.step-btn');
    const progressBar = document.getElementById('progress-bar');
    const progressText = document.getElementById('progress-text');
    const successMsg = document.getElementById('success-message');
    let completedSteps = 0;
    const totalSteps = 5;

    stepBtns.forEach(btn => {
        btn.addEventListener('click', () => {
            if(!btn.classList.contains('step-completed')) {
                btn.classList.remove('bg-alabaster', 'border-borderline');
                btn.classList.add('step-completed');
                
                const emoji = btn.querySelector('span');
                emoji.classList.remove('filter', 'grayscale');

                completedSteps++;
                const percentage = (completedSteps / totalSteps) * 100;
                
                progressBar.style.width = percentage + '%';
                progressText.textContent = Math.round(percentage) + '%';

                if(completedSteps === totalSteps) {
                    successMsg.classList.remove('hidden');
                    setTimeout(() => {
                        successMsg.classList.remove('scale-95', 'opacity-0');
                        successMsg.classList.add('scale-100', 'opacity-100');
                    }, 50);
                }
            }
        });
    });

});
</script>

</body>
</html>
