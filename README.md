       <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ZedSounds | Zambian Music Hub</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { background-color: #0b0e14; color: #e2e8f0; font-family: 'Inter', sans-serif; }
        .copper-gradient { background: linear-gradient(135deg, #d9480f 0%, #f76707 100%); }
        .glass-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.05); transition: 0.3s; }
        .glass-card:hover { background: rgba(255, 255, 255, 0.06); transform: translateY(-5px); }
        .music-player { background: rgba(15, 23, 42, 0.95); backdrop-filter: blur(20px); border-top: 1px solid rgba(255, 255, 255, 0.1); }
        .active-tab { border-bottom: 2px solid #d9480f; color: white; }
    </style>
</head>
<body class="pb-32">

    <nav class="flex items-center justify-between px-6 py-4 sticky top-0 z-50 bg-black/80 backdrop-blur-md border-b border-white/5">
        <div class="flex items-center gap-2 cursor-pointer" onclick="showPage('home')">
            <div class="w-10 h-10 copper-gradient rounded-xl flex items-center justify-center font-black text-white italic shadow-lg shadow-orange-900/40">ZS</div>
            <h1 class="text-xl font-bold tracking-tighter">ZED<span class="text-orange-500">SOUNDS</span></h1>
        </div>
        
        <div class="hidden md:flex items-center gap-8 text-sm font-medium text-slate-400">
            <button onclick="showPage('home')" class="hover:text-white transition-colors">Home</button>
            <button onclick="showPage('artist')" class="hover:text-white transition-colors">Artist Portal</button>
        </div>

        <button onclick="showPage('profile')" class="flex items-center gap-2 bg-white/5 hover:bg-white/10 px-4 py-2 rounded-full border border-white/10 transition-all">
            <div class="w-6 h-6 bg-orange-500 rounded-full flex items-center justify-center text-[10px] font-bold">ZS</div>
            <span class="text-xs font-bold">Library</span>
        </button>
    </nav>

    <div id="homePage" class="page-content px-6 mt-8">
        <header class="relative h-64 overflow-hidden rounded-3xl mb-12">
            <img src="https://images.unsplash.com/photo-1514525253344-99a429996592?auto=format&fit=crop&q=80&w=1200" class="w-full h-full object-cover opacity-50" alt="Concert">
            <div class="absolute inset-0 bg-gradient-to-t from-[#0b0e14] to-transparent"></div>
            <div class="absolute bottom-8 left-8">
                <h2 class="text-4xl font-black mb-2 italic">Zambian Bangers 🇿🇲</h2>
                <p class="text-slate-300 text-sm">Streaming the heart of Lusaka & the Copperbelt.</p>
            </div>
        </header>

        <section class="mb-12 p-6 rounded-3xl bg-orange-600/5 border border-orange-500/10">
            <div class="flex items-center gap-3 mb-2">
                <span class="text-lg">✨</span>
                <h3 class="font-bold">Gemini Artist Scout</h3>
            </div>
            <p class="text-xs text-slate-400 mb-4">Discover local talent. Ask for a genre (e.g., Hip Hop, Kalindula).</p>
            <div class="flex gap-2">
                <input id="genreQuery" type="text" placeholder="Enter genre..." class="flex-1 bg-black/40 border border-white/10 rounded-xl px-4 py-2 text-sm outline-none focus:border-orange-500">
                <button onclick="getAiRec()" class="bg-orange-600 px-4 py-2 rounded-xl text-xs font-bold uppercase">Find</button>
            </div>
            <div id="aiOutput" class="mt-3 text-xs text-orange-400 font-medium italic"></div>
        </section>

        <section class="mb-12">
            <h3 class="text-xl font-bold mb-6 italic">Featured Genres</h3>
            <div class="grid grid-cols-2 gap-4">
                <div class="h-28 rounded-2xl p-4 bg-green-900/40 border border-green-500/20 flex flex-col justify-end cursor-pointer">
                    <span class="text-[10px] font-black uppercase text-green-400">Roots</span>
                    <h4 class="font-bold">Kalindula</h4>
                </div>
                <div class="h-28 rounded-2xl p-4 bg-orange-900/40 border border-orange-500/20 flex flex-col justify-end cursor-pointer">
                    <span class="text-[10px] font-black uppercase text-orange-400">Pop</span>
                    <h4 class="font-bold">Zed-Pop</h4>
                </div>
            </div>
        </section>
    </div>

    <div id="artistPage" class="page-content px-6 mt-8 hidden">
        <div class="glass-card p-8 rounded-3xl text-center mb-8">
            <div class="w-24 h-24 bg-orange-500 rounded-full mx-auto mb-4 flex items-center justify-center text-3xl font-black">YM</div>
            <h2 class="text-2xl font-black italic">Yo Maps</h2>
            <p class="text-orange-500 text-xs font-bold tracking-widest uppercase">Verifie                         <h3 class="font-bold text-sm">${song.title}</h3>
                                <p class="text-[10px] opacity-50 uppercase font-bold tracking-tighter">${song.artist} • ${song.genre}</p>
                            </div>
                        </div>
                        <div class="text-copper">
                             <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                                <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM9.555 7.168A1 1 0 008 8v4a1 1 0 001.555.832l3-2a1 1 0 000-1.664l-3-2z" clip-rule="evenodd" />
                            </svg>
                        </div>
                    </div>
                `;
            });
        }

        // Load and Play Song
        function loadSong(index) {
            const song = playlist[index];
            document.getElementById('current-title').innerText = song.title;
            document.getElementById('current-artist').innerText = song.artist;
            document.getElementById('download-link').href = song.url;
            
            audio.src = song.url;
            audio.play();
            
            playerBar.classList.remove('translate-y-full');
            playBtn.innerText = '⏸';
        }

        function togglePlay() {
            if (audio.paused) {
                audio.play();
                playBtn.innerText = '⏸';
            } else {
                audio.pause();
                playBtn.innerText = '▶';
            }
        }

        // Update Progress Bar
        audio.ontimeupdate = () => {
            const perc = (audio.currentTime / audio.duration) * 100;
            document.getElementById('progress').style.width = perc + '%';
        };

        // Filter by Genre
        function filterGenre(genre) {
            if (genre === 'All') {
                displaySongs(playlist);
            } else {
                const filtered = playlist.filter(s => s.genre === genre);
                displaySongs(filtered);
            }
        }

        // Initial Load
        displaySongs(playlist);
    </script>
</body>
</html>     }

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
