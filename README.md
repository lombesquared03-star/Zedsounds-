     import React, { useState, useEffect, useRef } from 'react';
import { Search, Play, Pause, Bell, CheckCircle, Music, Flame, Calendar, Layers, Share2, Volume2 } from 'lucide-react';

const App = () => {
  // --- DATABASE ---
  const songDatabase = [
    { id: 1, title: "Amapalo", artist: "Yo Maps", cat: "Zed-Pop", art: "bg-gradient-to-br from-orange-500 to-red-600", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3", trend: true, plays: "1.2M" },
    { id: 2, title: "Psychology", artist: "Chef 187", cat: "Hip-Hop", art: "bg-gradient-to-br from-blue-600 to-indigo-700", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3", trend: true, plays: "940K" },
    { id: 3, title: "Konka", artist: "Chile One", cat: "Zed-Pop", art: "bg-gradient-to-br from-emerald-500 to-teal-700", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3", trend: false, plays: "420K" },
    { id: 4, title: "Never Again", artist: "Macky 2", cat: "Hip-Hop", art: "bg-gradient-to-br from-slate-700 to-gray-900", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-4.mp3", trend: true, plays: "2.1M" },
    { id: 5, title: "Zambian Anthem", artist: "Heritage Singers", cat: "Kalindula", art: "bg-gradient-to-br from-green-600 to-yellow-600", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-5.mp3", trend: false, plays: "150K" }
  ];

  // --- STATE ---
  const [activeTab, setActiveTab] = useState('trending');
  const [searchQuery, setSearchQuery] = useState('');
  const [currentSong, setCurrentSong] = useState(null);
  const [isPlaying, setIsPlaying] = useState(false);
  const [progress, setProgress] = useState(0);
  const [duration, setDuration] = useState(0);
  const [currentTime, setCurrentTime] = useState(0);
  
  const audioRef = useRef(null);

  // --- AUDIO LOGIC ---
  useEffect(() => {
    if (audioRef.current) {
      if (isPlaying) {
        audioRef.current.play().catch(() => setIsPlaying(false));
      } else {
        audioRef.current.pause();
      }
    }
  }, [isPlaying, currentSong]);

  const onTimeUpdate = () => {
    const curr = audioRef.current.currentTime;
    const dur = audioRef.current.duration;
    if (dur) {
      setProgress((curr / dur) * 100);
      setCurrentTime(curr);
      setDuration(dur);
    }
  };

  const formatTime = (time) => {
    const mins = Math.floor(time / 60);
    const secs = Math.floor(time % 60);
    return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
  };

  const handlePlaySong = (song) => {
    if (currentSong?.id === song.id) {
      setIsPlaying(!isPlaying);
    } else {
      setCurrentSong(song);
      setIsPlaying(true);
    }
  };

  const handleSeek = (e) => {
    const bounds = e.currentTarget.getBoundingClientRect();
    const x = e.clientX - bounds.left;
    const width = bounds.width;
    const percentage = x / width;
    audioRef.current.currentTime = percentage * duration;
  };

  // --- FILTERING ---
  const filteredSongs = songDatabase.filter(s => 
    s.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
    s.artist.toLowerCase().includes(searchQuery.toLowerCase()) ||
    s.cat.toLowerCase().includes(searchQuery.toLowerCase())
  );

  const displayList = searchQuery 
    ? filteredSongs 
    : activeTab === 'trending' 
      ? songDatabase.filter(s => s.trend) 
      : activeTab === 'new' 
        ? [...songDatabase].reverse() 
        : [];

  return (
    <div className="min-h-screen bg-[#020617] text-slate-100 font-sans pb-44 selection:bg-orange-500/30">
      <audio 
        ref={audioRef} 
        src={currentSong?.url} 
        onTimeUpdate={onTimeUpdate}
        onEnded={() => setIsPlaying(false)}
      />

      {/* TOP NAVIGATION */}
      <nav className="sticky top-0 z-[60] bg-[#0f172a]/70 backdrop-blur-xl px-6 py-4 flex items-center justify-between border-b border-white/5">
        <div className="flex items-center gap-3">
          <div className="w-10 h-10 bg-orange-600 rounded-2xl flex items-center justify-center rotate-3 shadow-xl shadow-orange-600/20">
            <span className="text-white font-black italic text-lg">ZS</span>
          </div>
          <div>
            <h1 className="text-xl font-extrabold tracking-tighter leading-none uppercase">
              ZED<span className="text-orange-500">SOUNDS</span>
            </h1>
            <span className="text-[9px] uppercase tracking-[0.2em] opacity-50 font-bold">Pro Architecture</span>
          </div>
        </div>
        <div className="flex gap-3">
          <button className="w-10 h-10 rounded-full bg-white/5 border border-white/10 flex items-center justify-center text-slate-400 hover:text-white transition-colors">
            <Bell size={18} />
          </button>
        </div>
      </nav>

      {/* SEARCH SYSTEM */}
      <div className="px-6 mt-8">
        <div className="relative group">
          <input 
            type="text" 
            placeholder="Search artists, songs, genres..." 
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
            className="w-full bg-white/5 border border-white/10 rounded-[1.5rem] py-4 pl-14 pr-4 text-sm outline-none focus:border-orange-500/50 focus:bg-white/10 transition-all shadow-inner placeholder:text-slate-600"
          />
          <div className="absolute left-5 top-1/2 -translate-y-1/2 text-slate-500 group-focus-within:text-orange-500 transition-colors">
            <Search size={20} />
          </div>
        </div>
      </div>

      {/* TABS */}
      {!searchQuery && (
        <div className="flex items-center justify-around px-6 mt-8 border-b border-white/5 text-sm font-bold text-slate-500">
          <button 
            onClick={() => setActiveTab('trending')}
            className={`pb-4 transition-all relative ${activeTab === 'trending' ? 'text-orange-500' : ''}`}
          >
            <div className="flex items-center gap-2">
              <Flame size={16} /> <span>Trending</span>
            </div>
            {activeTab === 'trending' && <div className="absolute bottom-[-1px] left-0 right-0 h-[2px] bg-orange-500 rounded-full" />}
          </button>
          <button 
            onClick={() => setActiveTab('new')}
            className={`pb-4 transition-all relative ${activeTab === 'new' ? 'text-orange-500' : ''}`}
          >
            <div className="flex items-center gap-2">
              <Calendar size={16} /> <span>New Hits</span>
            </div>
            {activeTab === 'new' && <div className="absolute bottom-[-1px] left-0 right-0 h-[2px] bg-orange-500 rounded-full" />}
          </button>
          <button 
            onClick={() => setActiveTab('genres')}
            className={`pb-4 transition-all relative ${activeTab === 'genres' ? 'text-orange-500' : ''}`}
          >
            <div className="flex items-center gap-2">
              <Layers size={16} /> <span>Categories</span>
            </div>
            {activeTab === 'genres' && <div className="absolute bottom-[-1px] left-0 right-0 h-[2px] bg-orange-500 rounded-full" />}
          </button>
        </div>
      )}

      {/* CONTENT AREA */}
      <div className="px-6 mt-8 animate-in fade-in duration-500">
        {searchQuery && (
          <p className="text-[10px] font-black uppercase tracking-widest text-slate-500 mb-6">
            Search Results for "{searchQuery}"
          </p>
        )}

        {activeTab === 'genres' && !searchQuery ? (
          <div className="grid grid-cols-2 gap-4">
            {['Zed-Pop', 'Hip-Hop', 'Kalindula', 'Gospel', 'Zamrock', 'R&B'].map((genre) => (
              <div key={genre} className="h-36 bg-white/5 rounded-[2rem] border border-white/5 p-6 flex flex-col justify-end group cursor-pointer hover:bg-white/10 transition-all">
                <Music className="text-orange-500 mb-auto opacity-40 group-hover:opacity-100 transition-opacity" size={24} />
                <span className="text-[9px] font-bold text-slate-500 uppercase tracking-tighter">Genre</span>
                <h4 className="font-black text-xl leading-none group-hover:text-orange-500 transition-colors">{genre}</h4>
              </div>
            ))}
          </div>
        ) : (
          <div className="space-y-4">
            {displayList.map((song) => (
              <div 
                key={song.id} 
                onClick={() => handlePlaySong(song)}
                className={`group p-4 rounded-[1.8rem] flex items-center justify-between transition-all cursor-pointer border ${currentSong?.id === song.id ? 'bg-white/10 border-orange-500/30' : 'bg-white/5 border-transparent hover:bg-white/10'}`}
              >
                <div className="flex items-center gap-4 min-w-0">
                  <div className={`w-14 h-14 ${song.art} rounded-2xl flex-shrink-0 flex items-center justify-center font-black text-xs shadow-lg shadow-black/40`}>ZS</div>
                  <div className="truncate">
                    <h4 className={`font-bold text-sm truncate transition-colors ${currentSong?.id === song.id ? 'text-orange-500' : ''}`}>
                      {song.title}
                    </h4>
                    <div className="flex items-center gap-1.5">
                      <p className="text-[11px] text-slate-400 font-medium truncate">{song.artist}</p>
                      <CheckCircle size={10} className="text-blue-400 fill-blue-400/10" />
                    </div>
                  </div>
                </div>
                <div className="flex items-center gap-3">
                  <span className="text-[10px] font-bold text-slate-600 hidden md:inline">{song.plays} plays</span>
                  <div className={`w-10 h-10 rounded-full flex items-center justify-center transition-all ${currentSong?.id === song.id && isPlaying ? 'bg-orange-500 text-white' : 'bg-white/5 text-orange-500 group-hover:bg-orange-500 group-hover:text-white'}`}>
                    {currentSong?.id === song.id && isPlaying ? <Pause size={18} /> : <Play size={18} className="ml-1" />}
                  </div>
                </div>
              </div>
            ))}
          </div>
        )}
      </div>

      {/* MASTER PLAYER ENGINE */}
      <div className={`fixed bottom-6 left-4 right-4 bg-[#0f172a]/90 backdrop-blur-2xl rounded-[2.5rem] p-5 shadow-2xl shadow-black border border-white/10 z-[100] transition-all duration-700 ${currentSong ? 'translate-y-0 opacity-100' : 'translate-y-[200%] opacity-0'}`}>
        <div className="flex items-center gap-4 mb-5">
          <div className={`w-14 h-14 ${currentSong?.art || 'bg-slate-800'} rounded-2xl flex-shrink-0 flex items-center justify-center font-black text-xl shadow-lg animate-pulse`}>ZS</div>
          <div className="flex-1 min-w-0">
            <h4 className="font-extrabold text-sm truncate leading-tight">{currentSong?.title || 'Nothing Playing'}</h4>
            <div className="flex items-center gap-1.5">
              <p className="text-[11px] text-slate-400 font-medium truncate">{currentSong?.artist}</p>
              <CheckCircle size={10} className="text-blue-400" />
            </div>
          </div>
          <div className="flex items-center gap-2">
             <button className="w-10 h-10 rounded-full flex items-center justify-center text-slate-400 hover:text-white">
              <Share2 size={18} />
            </button>
            <button 
              onClick={() => setIsPlaying(!isPlaying)}
              className="w-14 h-14 bg-white text-black rounded-[1.4rem] flex items-center justify-center shadow-xl hover:scale-105 active:scale-95 transition-all"
            >
              {isPlaying ? <Pause size={28} fill="black" /> : <Play size={28} fill="black" className="ml-1" />}
            </button>
          </div>
        </div>

        {/* SCRUBBER */}
        <div className="px-1">
          <div className="relative w-full h-1.5 bg-white/10 rounded-full cursor-pointer group" onClick={handleSeek}>
            <div 
              className="absolute top-0 left-0 h-full bg-orange-500 rounded-full transition-all duration-100 flex items-center justify-end"
              style={{ width: `${progress}%` }}
            >
              <div className="w-3 h-3 bg-white rounded-full scale-0 group-hover:scale-100 transition-transform shadow-lg" />
            </div>
          </div>
          <div className="flex justify-between mt-2 text-[10px] font-bold text-slate-500 font-mono tracking-tighter">
            <span>{formatTime(currentTime)}</span>
            <div className="flex items-center gap-1 opacity-40">
              <Volume2 size={10} />
              <span>High Quality</span>
            </div>
            <span>{formatTime(duration)}</span>
          </div>
        </div>
      </div>
    </div>
  );
};

export default App;                           <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM9.555 7.168A1 1 0 008 8v4a1 1 0 001.555.832l3-2a1 1 0 000-1.664l-3-2z" clip-rule="evenodd" />
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
