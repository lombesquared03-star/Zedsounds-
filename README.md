
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ZedSounds Lite: Phase 2 Strategic Report</title>
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<!-- Chosen Palette: Warm Zambian Neutrals (Alabaster Background, Soft Terracotta Accents, Deep Charcoal Text, Sage Green highlights) -->
<!-- Application Structure Plan: The application uses a dashboard-style interactive layout designed to synthesize the Phase 2 upgrade plan. It prioritizes user understanding by dividing the content into three logical phases: Strategic Overview (context), Feature Analysis (interactive exploration of the 3 key features via tabs alongside dynamic data visualization), and Actionable Deployment (interactive implementation checklist). This structure was chosen because it transforms a flat text document into an exploratory analytical tool, allowing stakeholders to understand the 'why' (metrics/impact) alongside the 'what' (features). -->
<!-- Visualization & Content Choices: Source Report -> Goal: Compare/Inform -> Viz: Chart.js Radar Chart & Interactive Tabs & Progress Bar -> Interaction: Tab clicking updates chart datasets and content panes; checklist clicking updates progress bar -> Justification: A radar chart perfectly illustrates the multidimensional impact (Engagement, Data Efficiency, Monetization) of each feature. Tabs prevent cognitive overload. The progress bar turns static deployment steps into a satisfying interactive process. -> Library: Chart.js (Canvas), Vanilla JS, HTML/CSS. Confirming NO SVG or Mermaid used. -->
<!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

<style>
.chart-container {
    position: relative;
    width: 100%;
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    height: 300px;
    max-height: 400px;
}
@media (min-width: 768px) {
    .chart-container {
        height: 350px;
    }
}
.tab-active {
    background-color: #C27A65;
    color: #F5F2E9;
    border-color: #C27A65;
}
.tab-inactive {
    background-color: transparent;
    color: #2D2D2D;
    border-color: #D1CFC7;
}
.step-completed {
    background-color: #8BA88E;
    color: #F5F2E9;
    border-color: #8BA88E;
}
.custom-scrollbar::-webkit-scrollbar {
    width: 6px;
}
.custom-scrollbar::-webkit-scrollbar-track {
    background: transparent;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
    background-color: #D1CFC7;
    border-radius: 20px;
}
</style>
<script>
    tailwind.config = {
        theme: {
            extend: {
                colors: {
                    alabaster: '#F5F2E9',
                    charcoal: '#2D2D2D',
                    terracotta: '#C27A65',
                    sage: '#8BA88E',
                    borderline: '#D1CFC7'
                },
                fontFamily: {
                    sans: ['Inter', 'system-ui', 'sans-serif'],
                }
            }
        }
    }
</script>
</head>
<body class="bg-alabaster text-charcoal font-sans antialiased overflow-x-hidden custom-scrollbar">

<div class="max-w-6xl mx-auto px-4 py-8 md:py-12">
    
    <header class="mb-12 border-b border-borderline pb-8">
        <div class="flex items-center gap-3 mb-4">
            <span class="text-4xl">🇿🇲</span>
            <h1 class="text-4xl md:text-5xl font-black tracking-tight text-charcoal">ZedSounds Lite</h1>
        </div>
        <h2 class="text-xl md:text-2xl text-terracotta font-medium tracking-wide">Phase 2 Interactive Strategic Report</h2>
    </header>

    <section class="mb-16">
        <h3 class="text-2xl font-bold mb-4 flex items-center gap-2"><span>📊</span> Strategic Overview</h3>
        <p class="text-lg leading-relaxed text-charcoal/80 max-w-4xl">
            This section contextualizes the transition from a conceptual prototype to a live, functional product at <code class="bg-borderline/30 px-2 py-1 rounded text-terracotta text-sm font-bold">lombesquared03.github.io/zedsounds/</code>. The primary directive of Phase 2 is to maximize platform functionality while strictly minimizing data load, catering directly to the Zambian mobile market constraints. The following interactive dashboard analyzes the three core features designed to achieve this balance.
        </p>
    </section>

    <section class="mb-16 bg-white rounded-3xl p-6 md:p-10 shadow-sm border border-borderline/50">
        <div class="mb-8">
            <h3 class="text-2xl font-bold mb-3 flex items-center gap-2"><span>⚙️</span> Feature Analysis & Impact</h3>
            <p class="text-md leading-relaxed text-charcoal/70">
                Explore the technical features proposed for the Phase 2 upgrade. Click on the tabs below to reveal the specific functionality of each feature, its strategic purpose, and observe how it alters the projected performance metrics on the multidimensional radar chart. This visualizes the trade-offs and benefits of each lightweight addition.
            </p>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-10 items-start">
            
            <div class="lg:col-span-5 flex flex-col gap-4">
                <div class="flex flex-col gap-3" id="tab-container">
                    <button data-feature="search" class="tab-btn tab-active border-2 text-left px-5 py-4 rounded-2xl font-bold text-lg transition-all duration-300 flex justify-between items-center group">
                        <span class="flex items-center gap-3"><span class="text-2xl">✨</span> Data-Saver Search</span>
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
