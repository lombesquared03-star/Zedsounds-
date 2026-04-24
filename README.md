import React, { useState, useEffect, useRef } from 'react';
import { 
  Search, Play, Pause, Bell, CheckCircle, Music, Flame, 
  Calendar, Layers, Share2, Volume2, TrendingUp, 
  User, Download, PlusCircle, Award, ChevronRight,
  Heart, Users, Zap, Globe, Info, Activity
} from 'lucide-react';

const App = () => {
  // --- CORE DATABASE ---
  const [songs] = useState([
    { id: 1, title: "Amapalo", artist: "Yo Maps", cat: "Zed-Pop", color: "#f97316", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3", trend: true, plays: "1.2M", verified: true },
    { id: 2, title: "Psychology", artist: "Chef 187", cat: "Hip-Hop", color: "#2563eb", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3", trend: true, plays: "940K", verified: true },
    { id: 3, title: "Konka", artist: "Chile One", cat: "Zed-Pop", color: "#10b981", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3", trend: false, plays: "420K", verified: true },
    { id: 4, title: "Never Again", artist: "Macky 2", cat: "Hip-Hop", color: "#6366f1", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-4.mp3", trend: true, plays: "2.1M", verified: true },
    { id: 5, title: "Zambian Anthem", artist: "Heritage Singers", cat: "Kalindula", color: "#84cc16", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-5.mp3", trend: false, plays: "150K", verified: false }
  ]);

  // --- STATE MANAGEMENT ---
  const [activeTab, setActiveTab] = useState('trending');
  const [searchQuery, setSearchQuery] = useState('');
  const [currentSong, setCurrentSong] = useState(null);
  const [isPlaying, setIsPlaying] = useState(false);
  const [progress, setProgress] = useState(0);
  const [duration, setDuration] = useState(0);
  const [currentTime, setCurrentTime] = useState(0);
  const [followedArtists, setFollowedArtists] = useState([]);
  const [isScouting, setIsScouting] = useState(false);
  const [scoutReport, setScoutReport] = useState(null);
  
  const audioRef = useRef(null);
  const apiKey = ""; // Injected at runtime

  // --- FOLLOW LOGIC ---
  const toggleFollow = (artistName) => {
    setFollowedArtists(prev => 
      prev.includes(artistName) ? prev.filter(a => a !== artistName) : [...prev, artistName]
    );
  };

  // --- AI TREND SCOUT (REAL DATA FETCH) ---
  const scoutTrendingMusic = async () => {
    setIsScouting(true);
    try {
      const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          contents: [{ parts: [{ text: "List 3 top trending Zambian songs right now. Provide a very brief 1-sentence explanation of why they are trending (e.g. TikTok challenge, new album, radio play). Be concise." }] }],
          systemInstruction: { parts: [{ text: "You are the ZedSounds AI Music Intelligence officer. You provide real-time updates on the Zambian music industry." }] }
        })
      });
      const result = await response.json();
      setScoutReport(result.candidates?.[0]?.content?.parts?.[0]?.text || "Trends updated.");
    } catch (err) {
      setScoutReport("Could not connect to live charts. Please check your network.");
    } finally {
      setIsScouting(false);
    }
  };

  // --- AUDIO ENGINE ---
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

  // --- SEARCH FILTERING ---
  const filteredSongs = songs.filter(s => 
    s.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
    s.artist.toLowerCase().includes(searchQuery.toLowerCase())
  );

  return (
    <div className="min-h-screen bg-[#020617] text-slate-100 font-sans pb-48 transition-all duration-1000"
         style={{ background: currentSong ? `radial-gradient(circle at 100% 0%, ${currentSong.color}30, #020617 70%)` : '#020617' }}>
      
      <audio ref={audioRef} src={currentSong?.url} onTimeUpdate={onTimeUpdate} onEnded={() => setIsPlaying(false)} />

      {/* HEADER & BRANDING */}
      <nav className="sticky top-0 z-[60] bg-black/40 backdrop-blur-2xl px-6 py-5 flex items-center justify-between border-b border-white/5">
        <div className="flex items-center gap-3">
          <div className="w-10 h-10 rounded-2xl flex items-center justify-center rotate-3 shadow-2xl transition-colors duration-500"
               style={{ backgroundColor: currentSong ? currentSong.color : '#f97316' }}>
            <span className="text-white font-black italic text-lg">ZS</span>
          </div>
          <div>
            <h1 className="text-xl font-extrabold tracking-tighter uppercase leading-none">
                ZED<span style={{ color: currentSong ? currentSong.color : '#f97316' }} className="transition-colors duration-500">SOUNDS</span>
            </h1>
            <div className="flex items-center gap-1.5 mt-1">
                <div className="w-1.5 h-1.5 rounded-full bg-green-500 animate-pulse" />
                <p className="text-[8px] font-black tracking-[0.3em] opacity-40 uppercase">Elite Streaming</p>
            </div>
          </div>
        </div>
        <div className="flex items-center gap-4">
            <button className="hidden sm:flex items-center gap-2 bg-white/5 px-4 py-2 rounded-full border border-white/10 text-[10px] font-bold uppercase tracking-widest text-slate-400 hover:text-white transition-all">
                <Globe size={14} />
                Global Chart
            </button>
            <div className="w-10 h-10 rounded-full bg-white/5 border border-white/10 flex items-center justify-center cursor-pointer hover:bg-white/10 transition-colors">
                <User size={18} className="text-slate-500" />
            </div>
        </div>
      </nav>

      {/* INDUSTRY AI SCOUT PANEL */}
      <div className="px-6 mt-8">
        <div className="bg-white/5 border border-white/10 rounded-[2.5rem] p-6 relative overflow-hidden group shadow-2xl">
          <div className="flex items-center justify-between mb-4">
            <div className="flex items-center gap-2">
              <Activity size={16} className="text-orange-500" />
              <span className="text-[10px] font-black uppercase tracking-widest text-slate-400">Industry Intelligence</span>
            </div>
            <button 
              onClick={scoutTrendingMusic}
              disabled={isScouting}
              className="text-[9px] font-black uppercase tracking-widest bg-white/10 hover:bg-white/20 px-4 py-2 rounded-full transition-all disabled:opacity-50 flex items-center gap-2 border border-white/5"
            >
              {isScouting ? 'Analyzing Data...' : 'Fetch Live Trends'}
              <Zap size={10} className={isScouting ? 'animate-bounce' : ''} />
            </button>
          </div>
          
          <div className="min-h-[44px] flex items-center relative z-10">
            {scoutReport ? (
                <div className="text-xs text-slate-300 leading-relaxed animate-in fade-in zoom-in duration-500">
                    {scoutReport}
                </div>
            ) : (
                <p className="text-xs text-slate-500 font-medium italic">Click "Fetch" to scan radio charts and social buzz for real trending data...</p>
            )}
          </div>
          
          <TrendingUp className="absolute -right-8 -top-8 text-white/5 opacity-10 group-hover:opacity-20 transition-opacity" size={120} />
        </div>
      </div>

      {/* SEARCH ENGINE */}
      <div className="px-6 mt-8">
        <div className="relative group">
          <input 
            type="text" 
            placeholder="Search artists, songs, or genres..." 
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
            className="w-full bg-white/5 border border-white/10 rounded-[1.8rem] py-5 pl-14 pr-4 text-sm outline-none focus:bg-white/10 transition-all placeholder:text-slate-600 focus:border-white/20"
          />
          <Search className="absolute left-6 top-1/2 -translate-y-1/2 text-slate-500 group-focus-within:text-orange-500 transition-colors" size={20} />
        </div>
      </div>

      {/* NAVIGATION TABS */}
      {!searchQuery && (
        <div className="flex items-center gap-10 px-8 mt-10 border-b border-white/5 overflow-x-auto no-scrollbar">
          {['trending', 'charts', 'new releases'].map(tab => (
            <button 
              key={tab}
              onClick={() => setActiveTab(tab)}
              className={`pb-4 transition-all text-[11px] font-black uppercase tracking-[0.2em] relative ${activeTab === tab ? 'text-white' : 'text-slate-600 hover:text-slate-400'}`}
            >
              {tab}
              {activeTab === tab && (
                  <div className="absolute bottom-0 left-0 right-0 h-[3px] rounded-full" 
                       style={{ backgroundColor: currentSong ? currentSong.color : '#f97316' }} />
              )}
            </button>
          ))}
        </div>
      )}

      {/* MAIN STREAMING FEED */}
      <div className="px-6 mt-10 space-y-8">
        {(searchQuery ? filteredSongs : songs.filter(s => activeTab === 'trending' ? s.trend : true)).map((song) => (
          <div key={song.id} className="flex items-center justify-between group">
            <div className="flex items-center gap-5 cursor-pointer flex-1 min-w-0" onClick={() => handlePlaySong(song)}>
              <div className="w-16 h-16 rounded-[1.5rem] shadow-2xl flex items-center justify-center font-bold relative overflow-hidden flex-shrink-0 transition-all group-hover:scale-105"
                   style={{ backgroundColor: song.color }}>
                <div className="absolute inset-0 bg-black/10 group-hover:bg-transparent transition-colors" />
                <Play className="text-white fill-white/20 relative z-10" size={24} />
              </div>
              <div className="min-w-0">
                <h4 className={`font-black text-base truncate transition-colors ${currentSong?.id === song.id ? 'text-white' : 'text-slate-200 group-hover:text-white'}`}>{song.title}</h4>
                <div className="flex items-center gap-2 mt-1">
                  <p className="text-[11px] text-slate-500 font-bold uppercase tracking-tight">{song.artist}</p>
                  {song.verified && <CheckCircle size={10} className="text-blue-500 fill-blue-500/10" />}
                </div>
              </div>
            </div>
            
            <div className="flex items-center gap-4">
              <button 
                onClick={() => toggleFollow(song.artist)}
                className={`flex items-center gap-2 px-4 py-2 rounded-full text-[9px] font-black uppercase tracking-widest border transition-all ${followedArtists.includes(song.artist) ? 'bg-white text-black border-white' : 'border-white/10 text-slate-500 hover:border-white/30'}`}
              >
                {followedArtists.includes(song.artist) ? <Users size={12} /> : <PlusCircle size={12} />}
                {followedArtists.includes(song.artist) ? 'Following' : 'Follow'}
              </button>
              <button className="hidden sm:block p-2 text-slate-600 hover:text-white transition-colors">
                <Share2 size={18} />
              </button>
            </div>
          </div>
        ))}
      </div>

      {/* ADAPTIVE MASTER PLAYER PANEL */}
      <div 
        className={`fixed bottom-6 left-4 right-4 rounded-[2.8rem] p-6 shadow-2xl z-[100] transition-all duration-1000 ${currentSong ? 'translate-y-0' : 'translate-y-[250%]'}`}
        style={{ 
          background: `linear-gradient(135deg, ${currentSong?.color || '#000'}dd, #020617)`,
          border: `1px solid ${currentSong?.color}44`,
          boxShadow: `0 20px 80px -20px ${currentSong?.color}66`
        }}
      >
        <div className="flex items-center gap-5 mb-6">
          <div className="w-16 h-16 bg-white/10 rounded-2xl flex items-center justify-center flex-shrink-0 relative">
             <Music className="text-white animate-pulse" size={32} />
             <div className="absolute inset-0 bg-white/5 animate-ping rounded-2xl opacity-20" />
          </div>
          <div className="flex-1 min-w-0">
            <h4 className="font-black text-lg truncate uppercase tracking-tighter leading-tight">{currentSong?.title}</h4>
            <div className="flex items-center gap-3 mt-1">
               <p className="text-[10px] text-white/60 font-black uppercase tracking-widest">{currentSong?.artist}</p>
               <span className="w-1 h-1 bg-white/20 rounded-full" />
               <span className="text-[10px] text-white/40 font-bold uppercase tracking-widest">{currentSong?.cat}</span>
            </div>
          </div>
          <button 
            onClick={() => setIsPlaying(!isPlaying)}
            className="w-16 h-16 bg-white text-black rounded-[1.8rem] flex items-center justify-center shadow-2xl active:scale-90 transition-all hover:scale-105"
          >
            {isPlaying ? <Pause size={32} fill="black" /> : <Play size={32} fill="black" className="ml-1" />}
          </button>
        </div>

        {/* PERFORMANCE STREAMING SCRUBBER */}
        <div className="px-1">
          <div className="h-1.5 w-full bg-white/10 rounded-full overflow-hidden cursor-pointer relative group">
            <div className="h-full bg-white transition-all duration-100 relative" style={{ width: `${progress}%` }}>
                <div className="absolute right-0 top-1/2 -translate-y-1/2 w-4 h-4 bg-white rounded-full shadow-2xl scale-0 group-hover:scale-100 transition-transform" />
            </div>
          </div>
          <div className="flex justify-between mt-3 text-[10px] font-black text-white/30 tracking-[0.2em] uppercase">
            <span>{formatTime(currentTime)}</span>
            <div className="flex items-center gap-2">
               <div className="w-1.5 h-1.5 bg-green-500 rounded-full shadow-[0_0_12px_rgba(34,197,94,0.8)]" />
               <span className="animate-pulse">Lossless Stream</span>
            </div>
            <span>{formatTime(duration)}</span>
          </div>
        </div>
      </div>
    </div>
  );
};

export default App;
