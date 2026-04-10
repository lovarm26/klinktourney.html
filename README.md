<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KlinkTourney 💜 • Mobile-Responsive Tournament App</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&amp;display=swap');
        
        :root {
            --tw-color-primary: #a855f7;
        }
        
        body {
            font-family: 'Inter', system_ui, sans-serif;
        }
        
        .page {
            display: none;
        }
        
        .page.active {
            display: block;
        }
        
        .bracket-container {
            overflow-x: auto;
            scrollbar-width: thin;
            scrollbar-color: #a855f7 #27272a;
        }
        
        .match-card {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .match-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 20px 25px -5px rgb(168 85 247 / 0.1);
        }
        
        .modal {
            animation: modalPop 0.3s ease;
        }
        
        @keyframes modalPop {
            0% { opacity: 0; transform: scale(0.95); }
            100% { opacity: 1; transform: scale(1); }
        }
        
        /* Bracket connecting lines */
        .round {
            position: relative;
        }
        
        .round:not(:last-child)::after {
            content: '';
            position: absolute;
            top: 50%;
            right: -12px;
            width: 24px;
            height: 2px;
            background: #a855f7;
            z-index: 10;
        }
    </style>
</head>
<body class="bg-zinc-950 text-zinc-100">
    <!-- NAVBAR -->
    <nav class="bg-zinc-900 border-b border-zinc-800 sticky top-0 z-50 shadow-xl">
        <div class="max-w-screen-2xl mx-auto px-4 sm:px-6">
            <div class="h-16 flex items-center justify-between">
                <!-- Logo -->
                <div class="flex items-center gap-x-3">
                    <div class="w-9 h-9 bg-gradient-to-br from-violet-500 to-fuchsia-500 rounded-2xl flex items-center justify-center text-2xl shadow-inner">💜</div>
                    <div>
                        <span class="text-3xl font-semibold tracking-[-1px]">KlinkTourney</span>
                    </div>
                    <span class="text-xs font-medium px-2.5 py-0.5 bg-violet-500/10 text-violet-400 rounded-full border border-violet-400/30">MESHCHAIN</span>
                </div>

                <!-- Desktop Navigation -->
                <div class="hidden md:flex items-center gap-x-8 text-sm font-medium">
                    <button onclick="navigateTo('dashboard')" class="flex items-center gap-x-1.5 hover:text-violet-400 transition-colors active-nav">
                        <i class="fa-solid fa-house"></i>
                        <span>Dashboard</span>
                    </button>
                    <button onclick="navigateTo('my-tournaments')" class="flex items-center gap-x-1.5 hover:text-violet-400 transition-colors">
                        <i class="fa-solid fa-trophy"></i>
                        <span>My Tournaments</span>
                    </button>
                    <button onclick="navigateTo('teams')" class="flex items-center gap-x-1.5 hover:text-violet-400 transition-colors">
                        <i class="fa-solid fa-users"></i>
                        <span>Teams</span>
                    </button>
                </div>

                <div class="flex items-center gap-x-4">
                    <!-- Quick create -->
                    <button onclick="showCreateModal()" 
                            class="flex items-center bg-violet-600 hover:bg-violet-500 px-5 h-9 rounded-3xl text-sm font-semibold gap-x-2 transition-colors">
                        <i class="fa-solid fa-plus"></i>
                        <span class="hidden sm:inline">New Tournament</span>
                    </button>

                    <!-- User -->
                    <div class="flex items-center gap-x-2 bg-zinc-800 rounded-3xl pr-2 pl-1 py-1 cursor-pointer">
                        <div class="w-8 h-8 bg-gradient-to-br from-violet-400 to-pink-400 rounded-2xl flex items-center justify-center text-lg">💜</div>
                        <div class="hidden sm:block">
                            <p class="text-sm font-semibold">LOVA KLINK</p>
                            <p class="text-[10px] text-zinc-400 -mt-1">Meshchain.ai</p>
                        </div>
                    </div>

                    <!-- Mobile hamburger -->
                    <button onclick="toggleMobileMenu()" class="md:hidden text-3xl text-zinc-400 hover:text-white">
                        <i id="hamburger" class="fa-solid fa-bars"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Menu -->
        <div id="mobileMenu" class="hidden md:hidden bg-zinc-900 border-t border-zinc-700 px-4 py-6 text-lg">
            <div onclick="navigateTo('dashboard');toggleMobileMenu()" class="flex items-center gap-x-4 py-4 border-b border-zinc-700">
                <i class="fa-solid fa-house w-8"></i> Dashboard
            </div>
            <div onclick="navigateTo('my-tournaments');toggleMobileMenu()" class="flex items-center gap-x-4 py-4 border-b border-zinc-700">
                <i class="fa-solid fa-trophy w-8"></i> My Tournaments
            </div>
            <div onclick="navigateTo('teams');toggleMobileMenu()" class="flex items-center gap-x-4 py-4">
                <i class="fa-solid fa-users w-8"></i> Teams
            </div>
            <div class="pt-6 text-center text-xs text-zinc-400">Built as a fully responsive web app • Works perfectly on phones & tablets</div>
        </div>
    </nav>

    <!-- DASHBOARD PAGE -->
    <div id="page-dashboard" class="page active max-w-screen-2xl mx-auto px-4 sm:px-6 py-8">
        <div class="flex flex-col sm:flex-row sm:items-end justify-between mb-8">
            <div>
                <h1 class="text-4xl font-semibold tracking-tighter">Good afternoon, LOVA 💜</h1>
                <p class="text-zinc-400 mt-1">Let's run some tournaments today</p>
            </div>
            <button onclick="showCreateModal()" class="mt-4 sm:mt-0 bg-white text-zinc-900 hover:bg-amber-300 px-8 py-3 rounded-3xl font-semibold flex items-center gap-x-3 text-lg shadow-lg">
                <i class="fa-solid fa-bolt"></i>
                START NEW TOURNAMENT
            </button>
        </div>

        <!-- Live Tournaments -->
        <h2 class="text-xl font-semibold mb-4 flex items-center gap-x-3">
            <span class="relative flex h-3 w-3">
                <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-400 opacity-75"></span>
                <span class="relative inline-flex rounded-full h-3 w-3 bg-green-500"></span>
            </span>
            LIVE NOW
        </h2>
        <div id="live-cards" class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-6"></div>

        <!-- Upcoming -->
        <h2 class="text-xl font-semibold mt-12 mb-4">Upcoming Tournaments</h2>
        <div id="upcoming-cards" class="grid grid-cols-1 md:grid-cols-3 gap-6"></div>
    </div>

    <!-- MY TOURNAMENTS PAGE -->
    <div id="page-my-tournaments" class="page max-w-screen-2xl mx-auto px-4 sm:px-6 py-8 hidden">
        <h1 class="text-3xl font-semibold mb-8">My Tournaments</h1>
        <div id="tournament-list" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6"></div>
    </div>

    <!-- TEAMS PAGE (global) -->
    <div id="page-teams" class="page max-w-screen-2xl mx-auto px-4 sm:px-6 py-8 hidden">
        <h1 class="text-3xl font-semibold mb-8">Your Teams & Players</h1>
        <div class="bg-zinc-900 rounded-3xl p-6">
            <div id="team-list" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4"></div>
        </div>
    </div>

    <!-- TOURNAMENT DETAIL VIEW -->
    <div id="tournament-detail-view" class="page max-w-screen-2xl mx-auto px-4 sm:px-6 py-8 hidden">
        <div class="flex items-center justify-between mb-6">
            <div class="flex items-center gap-x-4">
                <button onclick="backToDashboard()" class="flex items-center gap-x-2 text-violet-400 hover:text-white">
                    <i class="fa-solid fa-arrow-left"></i>
                    <span class="font-medium">Back</span>
                </button>
                <h1 id="detail-title" class="text-4xl font-semibold"></h1>
            </div>
            <div id="detail-status" class="px-5 py-1 rounded-3xl text-sm font-medium"></div>
        </div>

        <!-- TABS -->
        <div class="flex border-b border-zinc-700 mb-8 overflow-x-auto">
            <button onclick="switchTab(0)" id="tab-0" class="tab-button px-8 py-4 font-semibold border-b-2 border-transparent hover:border-violet-400 active">Overview</button>
            <button onclick="switchTab(1)" id="tab-1" class="tab-button px-8 py-4 font-semibold border-b-2 border-transparent hover:border-violet-400">Participants</button>
            <button onclick="switchTab(2)" id="tab-2" class="tab-button px-8 py-4 font-semibold border-b-2 border-transparent hover:border-violet-400">Bracket</button>
            <button onclick="switchTab(3)" id="tab-3" class="tab-button px-8 py-4 font-semibold border-b-2 border-transparent hover:border-violet-400">Matches</button>
            <button onclick="switchTab(4)" id="tab-4" class="tab-button px-8 py-4 font-semibold border-b-2 border-transparent hover:border-violet-400">Leaderboard</button>
        </div>

        <!-- TAB CONTENTS -->
        <div id="tab-content-0" class="tab-content">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                <div class="bg-zinc-900 rounded-3xl p-6">
                    <h3 class="uppercase text-xs tracking-widest mb-4">Tournament Info</h3>
                    <p id="detail-desc" class="text-zinc-400"></p>
                </div>
                <div class="bg-zinc-900 rounded-3xl p-6">
                    <h3 class="uppercase text-xs tracking-widest mb-4">Quick Stats</h3>
                    <div class="space-y-6">
                        <div class="flex justify-between"><span class="text-zinc-400">Participants</span><span id="stat-participants" class="font-semibold"></span></div>
                        <div class="flex justify-between"><span class="text-zinc-400">Matches played</span><span id="stat-matches" class="font-semibold"></span></div>
                        <div class="flex justify-between"><span class="text-zinc-400">Prize pool</span><span id="stat-prize" class="font-semibold text-emerald-400">$2,400</span></div>
                    </div>
                </div>
                <div class="bg-zinc-900 rounded-3xl p-6 flex flex-col justify-center text-center">
                    <div id="next-match" class="text-sm uppercase">Next match in 14 minutes</div>
                    <div class="text-5xl font-bold text-violet-400 mt-2">QUARTERFINALS</div>
                </div>
            </div>
        </div>

        <!-- PARTICIPANTS TAB -->
        <div id="tab-content-1" class="tab-content hidden">
            <div class="flex justify-between items-center mb-6">
                <h3 class="font-semibold text-2xl">Participants (8)</h3>
                <button onclick="addParticipant()" class="bg-zinc-800 hover:bg-violet-600 px-6 py-3 rounded-3xl flex items-center gap-x-2">
                    <i class="fa-solid fa-user-plus"></i> Add team
                </button>
            </div>
            <div id="participants-grid" class="grid grid-cols-2 md:grid-cols-4 gap-4"></div>
        </div>

        <!-- BRACKET TAB (MOBILE RESPONSIVE) -->
        <div id="tab-content-2" class="tab-content hidden">
            <div class="bracket-container pb-8">
                <div id="bracket" class="flex gap-8 min-w-[800px] mx-auto">
                    <!-- JS injected rounds -->
                </div>
            </div>
            <div class="text-center text-xs text-zinc-500 mt-4">← Scroll horizontally on mobile to see full bracket →</div>
        </div>

        <!-- MATCHES TAB -->
        <div id="tab-content-3" class="tab-content hidden">
            <h3 class="font-semibold mb-6 text-xl">All Matches</h3>
            <div id="matches-list" class="space-y-3"></div>
        </div>

        <!-- LEADERBOARD TAB -->
        <div id="tab-content-4" class="tab-content hidden">
            <h3 class="font-semibold mb-6 text-xl">Leaderboard</h3>
            <table class="w-full text-left" id="leaderboard-table">
                <thead><tr class="border-b border-zinc-700"><th class="py-4">Rank</th><th>Team</th><th>Wins</th><th>Score</th></tr></thead>
                <tbody class="text-zinc-300" id="leaderboard-body"></tbody>
            </table>
        </div>
    </div>

    <!-- CREATE TOURNAMENT MODAL -->
    <div onclick="if(event.target.id === 'create-modal')hideCreateModal()" id="create-modal" class="hidden fixed inset-0 bg-black/70 backdrop-blur-xl z-[9999] flex items-center justify-center">
        <div onclick="event.stopImmediatePropagation()" class="modal bg-zinc-900 w-full max-w-lg mx-4 rounded-3xl p-8">
            <h2 class="text-3xl font-semibold mb-6">Create New Tournament</h2>
            
            <div class="space-y-6">
                <div>
                    <label class="block text-xs uppercase mb-1">Tournament name</label>
                    <input id="modal-name" type="text" value="Meshchain Spring Clash" 
                           class="w-full bg-zinc-800 rounded-2xl px-5 py-4 outline-none focus:ring-2 focus:ring-violet-400">
                </div>
                
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs uppercase mb-1">Type</label>
                        <select id="modal-type" class="w-full bg-zinc-800 rounded-2xl px-5 py-4 outline-none">
                            <option>Single Elimination</option>
                            <option>Double Elimination</option>
                            <option>Round Robin</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs uppercase mb-1"># of teams</label>
                        <select id="modal-teams" class="w-full bg-zinc-800 rounded-2xl px-5 py-4 outline-none">
                            <option value="4">4 teams</option>
                            <option value="8" selected>8 teams</option>
                            <option value="16">16 teams</option>
                        </select>
                    </div>
                </div>
                
                <div>
                    <label class="block text-xs uppercase mb-1">Start date</label>
                    <input type="date" value="2026-04-18" class="w-full bg-zinc-800 rounded-2xl px-5 py-4 outline-none">
                </div>
                
                <button onclick="createNewTournament()" 
                        class="w-full h-14 bg-gradient-to-r from-violet-500 to-fuchsia-500 text-white font-semibold text-lg rounded-3xl mt-4">
                    CREATE &amp; GENERATE BRACKET
                </button>
            </div>
            
            <button onclick="hideCreateModal()" class="absolute top-6 right-6 text-zinc-400 hover:text-white text-3xl">✕</button>
        </div>
    </div>

    <!-- SUCCESS TOAST -->
    <div id="toast" onclick="this.classList.add('hidden')" class="hidden fixed bottom-6 right-6 bg-emerald-500 text-white px-8 py-4 rounded-3xl shadow-2xl flex items-center gap-x-3">
        <i class="fa-solid fa-check-circle"></i>
        <span id="toast-text" class="font-medium"></span>
    </div>

    <script>
        // Tailwind script initialization
        function initTailwind() {
            tailwind.config = {
                content: [],
                theme: {
                    extend: {}
                }
            }
        }
        
        // Global state
        let currentTournamentId = null
        let tournamentsDB = [
            {
                id: 1,
                name: "Meshchain Spring Clash",
                type: "Single Elimination",
                status: "live",
                participantsCount: 8,
                date: "Apr 18, 2026",
                color: "violet"
            },
            {
                id: 2,
                name: "Klink League Cup",
                type: "Double Elimination",
                status: "upcoming",
                participantsCount: 16,
                date: "Apr 25, 2026",
                color: "fuchsia"
            }
        ]
        
        let activeTournament = {
            id: 1,
            name: "Meshchain Spring Clash",
            type: "Single Elimination",
            status: "live",
            description: "8-team single-elimination bracket powered by Meshchain.ai. Winner takes home $2,400 prize pool.",
            participants: [
                { id: 1, name: "Team Alpha", seed: 1 },
                { id: 2, name: "Beta Squad", seed: 2 },
                { id: 3, name: "Gamma Force", seed: 3 },
                { id: 4, name: "Delta Knights", seed: 4 },
                { id: 5, name: "Epsilon Crew", seed: 5 },
                { id: 6, name: "Zeta Legends", seed: 6 },
                { id: 7, name: "Theta Warriors", seed: 7 },
                { id: 8, name: "Iota Phoenix", seed: 8 }
            ],
            matches: [
                { id: 1, round: "Quarterfinals", team1: "Team Alpha", team2: "Iota Phoenix", score1: 3, score2: 1, winner: "Team Alpha" },
                { id: 2, round: "Quarterfinals", team1: "Beta Squad", team2: "Theta Warriors", score1: 2, score2: 3, winner: "Theta Warriors" },
                { id: 3, round: "Quarterfinals", team1: "Gamma Force", team2: "Zeta Legends", score1: 3, score2: 0, winner: "Gamma Force" },
                { id: 4, round: "Quarterfinals", team1: "Delta Knights", team2: "Epsilon Crew", score1: 1, score2: 3, winner: "Epsilon Crew" },
                { id: 5, round: "Semifinals", team1: "Team Alpha", team2: "Theta Warriors", score1: null, score2: null, winner: null },
                { id: 6, round: "Semifinals", team1: "Gamma Force", team2: "Epsilon Crew", score1: null, score2: null, winner: null },
                { id: 7, round: "Final", team1: null, team2: null, score1: null, score2: null, winner: null }
            ],
            leaderboard: [
                { rank: 1, team: "Team Alpha", wins: 2, score: 540 },
                { rank: 2, team: "Gamma Force", wins: 2, score: 480 },
                { rank: 3, team: "Epsilon Crew", wins: 1, score: 310 }
            ]
        }
        
        // Render live & upcoming cards
        function renderDashboardCards() {
            const liveContainer = document.getElementById('live-cards')
            const upcomingContainer = document.getElementById('upcoming-cards')
            
            liveContainer.innerHTML = `
            <div onclick="openTournamentDetail(1)" class="bg-gradient-to-br from-violet-600/90 to-fuchsia-600/90 rounded-3xl p-6 text-white cursor-pointer">
                <div class="flex justify-between">
                    <div>
                        <div class="uppercase text-xs font-medium tracking-widest">LIVE • QUARTERFINALS</div>
                        <h3 class="text-2xl font-semibold mt-2">${tournamentsDB[0].name}</h3>
                        <p class="text-sm mt-6 opacity-90">8 teams • Single Elimination</p>
                    </div>
                    <div class="text-right">
                        <div class="bg-white/20 text-xs px-4 py-1 rounded-3xl inline-block">NOW</div>
                        <div class="text-6xl font-bold mt-8">4</div>
                        <div class="text-xs">matches left</div>
                    </div>
                </div>
            </div>`
            
            upcomingContainer.innerHTML = `
            <div onclick="openTournamentDetail(2)" class="bg-zinc-900 rounded-3xl p-6 cursor-pointer hover:scale-105 transition-transform">
                <div class="text-emerald-400 text-xs font-medium">APR 25 • 6:00 PM</div>
                <h3 class="text-xl font-semibold mt-3">${tournamentsDB[1].name}</h3>
                <div class="mt-8 flex justify-between text-sm">
                    <div><span class="font-mono">${tournamentsDB[1].participantsCount}</span> teams</div>
                    <div class="text-violet-400">${tournamentsDB[1].type}</div>
                </div>
            </div>`
        }
        
        // Render full tournament list
        function renderTournamentList() {
            const container = document.getElementById('tournament-list')
            container.innerHTML = tournamentsDB.map(t => `
                <div onclick="openTournamentDetail(${t.id})" class="bg-zinc-900 hover:bg-zinc-800 rounded-3xl p-6 cursor-pointer transition-all">
                    <div class="flex justify-between text-xs"><span class="px-3 py-1 bg-zinc-800 rounded-3xl">${t.type}</span><span class="text-emerald-400">${t.date}</span></div>
                    <h4 class="text-2xl font-semibold mt-6">${t.name}</h4>
                    <div class="mt-8 flex items-baseline justify-between">
                        <div><span class="text-4xl font-bold">${t.participantsCount}</span><span class="text-xs ml-1 text-zinc-400">TEAMS</span></div>
                        <div class="text-right">
                            <i class="fa-solid fa-circle-${t.status === 'live' ? 'play' : 'calendar'} text-4xl text-violet-400"></i>
                        </div>
                    </div>
                </div>
            `).join('')
        }
        
        // Render global teams page
        function renderTeamsPage() {
            const container = document.getElementById('team-list')
            container.innerHTML = activeTournament.participants.map(p => `
                <div class="bg-zinc-800 rounded-3xl px-6 py-6 text-center">
                    <div class="text-4xl mb-2">🏆</div>
                    <div class="font-semibold">${p.name}</div>
                    <div class="text-xs text-zinc-400">Seed #${p.seed}</div>
                </div>
            `).join('')
        }
        
        // Open tournament detail
        function openTournamentDetail(id) {
            currentTournamentId = id
            document.getElementById('tournament-detail-view').classList.remove('hidden')
            document.querySelectorAll('.page').forEach(p => { if (p.id !== 'tournament-detail-view') p.classList.add('hidden') })
            
            // Populate header
            document.getElementById('detail-title').innerHTML = activeTournament.name
            document.getElementById('detail-status').innerHTML = `<span class="px-5 py-2 bg-green-400 text-zinc-900 rounded-3xl text-sm">${activeTournament.status.toUpperCase()}</span>`
            document.getElementById('detail-desc').textContent = activeTournament.description
            document.getElementById('stat-participants').textContent = activeTournament.participants.length + " teams"
            document.getElementById('stat-matches').textContent = activeTournament.matches.filter(m => m.winner).length + " / 7"
            
            // Render participants tab
            renderParticipants()
            
            // Render bracket
            renderBracket()
            
            // Render matches list
            renderMatchesList()
            
            // Render leaderboard
            renderLeaderboard()
            
            // Auto open bracket tab
            switchTab(2)
        }
        
        function backToDashboard() {
            document.getElementById('tournament-detail-view').classList.add('hidden')
            document.getElementById('page-dashboard').classList.remove('hidden')
        }
        
        function renderParticipants() {
            const container = document.getElementById('participants-grid')
            container.innerHTML = activeTournament.participants.map(p => `
                <div class="flex items-center gap-x-4 bg-zinc-800 rounded-3xl px-5 py-4">
                    <div class="text-3xl">👤</div>
                    <div class="flex-1">
                        <div class="font-semibold">${p.name}</div>
                        <div class="text-xs text-violet-300">Seed ${p.seed}</div>
                    </div>
                    <button onclick="removeParticipant(${p.id});event.stopImmediatePropagation()" class="text-red-400 hover:text-red-300 text-xl">
                        <i class="fa-solid fa-trash"></i>
                    </button>
                </div>
            `).join('')
        }
        
        function addParticipant() {
            const name = prompt("Enter new team name:")
            if (!name) return
            const newId = Math.max(...activeTournament.participants.map(p => p.id)) + 1
            activeTournament.participants.push({ id: newId, name: name, seed: activeTournament.participants.length + 1 })
            renderParticipants()
            showToast("Team added to tournament!")
        }
        
        function removeParticipant(id) {
            activeTournament.participants = activeTournament.participants.filter(p => p.id !== id)
            renderParticipants()
        }
        
        // Interactive bracket renderer
        function renderBracket() {
            const container = document.getElementById('bracket')
            let html = ''
            
            // Round 1 - Quarterfinals
            html += `<div class="round flex-1 min-w-[210px]">
                <div class="text-xs font-medium text-center mb-3 text-violet-300">QUARTERFINALS</div>`
            activeTournament.matches.filter(m => m.round === "Quarterfinals").forEach(match => {
                html += createMatchHTML(match)
            })
            html += `</div>`
            
            // Round 2 - Semifinals
            html += `<div class="round flex-1 min-w-[210px]">
                <div class="text-xs font-medium text-center mb-3 text-violet-300">SEMIFINALS</div>`
            activeTournament.matches.filter(m => m.round === "Semifinals").forEach(match => {
                html += createMatchHTML(match)
            })
            html += `</div>`
            
            // Final
            html += `<div class="round flex-1 min-w-[210px]">
                <div class="text-xs font-medium text-center mb-3 text-violet-300">FINAL</div>`
            activeTournament.matches.filter(m => m.round === "Final").forEach(match => {
                html += createMatchHTML(match)
            })
            html += `</div>`
            
            container.innerHTML = html
        }
        
        function createMatchHTML(match) {
            let winnerHTML = ''
            if (match.winner) {
                winnerHTML = `<div class="text-xs mt-1 text-emerald-400">${match.winner} advances</div>`
            }
            
            return `
            <div onclick="updateMatchResult(${match.id})" class="match-card bg-white/5 hover:bg-white/10 border border-white/10 rounded-3xl p-4 mb-8 cursor-pointer">
                <div class="flex justify-between text-sm mb-3">
                    <span>${match.team1 || 'TBD'}</span>
                    <span class="font-mono text-violet-400">${match.score1 !== null ? match.score1 : '-'}</span>
                </div>
                <div class="h-px bg-gradient-to-r from-transparent via-violet-400/30 to-transparent my-2"></div>
                <div class="flex justify-between text-sm">
                    <span>${match.team2 || 'TBD'}</span>
                    <span class="font-mono text-violet-400">${match.score2 !== null ? match.score2 : '-'}</span>
                </div>
                ${winnerHTML}
            </div>`
        }
        
        function updateMatchResult(matchId) {
            const match = activeTournament.matches.find(m => m.id === matchId)
            if (!match) return
            
            // Simple interactive prompt (in real app would be beautiful modal)
            const winnerName = prompt(`Who won this match?\n\n1. ${match.team1}\n2. ${match.team2}\n\nType 1 or 2:`, "1")
            
            if (winnerName === "1") match.winner = match.team1
            else if (winnerName === "2") match.winner = match.team2
            
            // Update scores randomly for demo
            if (!match.score1) match.score1 = Math.floor(Math.random() * 4)
            if (!match.score2) match.score2 = Math.floor(Math.random() * 4)
            
            renderBracket()
            renderMatchesList()
            renderLeaderboard()
            showToast(`Match updated! ${match.winner} advances`)
            
            // Auto-progress final if both semis done
            if (activeTournament.matches.filter(m => m.round === "Semifinals" && m.winner).length === 2) {
                const final = activeTournament.matches.find(m => m.round === "Final")
                if (!final.team1) {
                    final.team1 = activeTournament.matches.find(m => m.round === "Semifinals" && m.winner).winner
                    final.team2 = activeTournament.matches.find((m, i) => m.round === "Semifinals" && i !== activeTournament.matches.findIndex(x => x.round === "Semifinals" && x.winner)).winner
                }
            }
        }
        
        function renderMatchesList() {
            const container = document.getElementById('matches-list')
            container.innerHTML = activeTournament.matches.map(m => `
                <div class="flex items-center justify-between bg-zinc-900 rounded-3xl px-6 py-5">
                    <div class="flex-1">
                        <div class="flex items-center justify-between">
                            <span class="font-medium">${m.team1 || '—'}</span>
                            <span class="font-mono text-lg">${m.score1 !== null ? m.score1 : '—'}</span>
                        </div>
                        <div class="flex items-center justify-between mt-1">
                            <span class="font-medium">${m.team2 || '—'}</span>
                            <span class="font-mono text-lg">${m.score2 !== null ? m.score2 : '—'}</span>
                        </div>
                    </div>
                    <div class="ml-8 text-xs font-medium uppercase px-4 py-1 rounded-3xl ${m.winner ? 'bg-emerald-400 text-zinc-900' : 'bg-zinc-700'}">${m.round}</div>
                </div>
            `).join('')
        }
        
        function renderLeaderboard() {
            const tbody = document.getElementById('leaderboard-body')
            tbody.innerHTML = activeTournament.leaderboard.map(l => `
                <tr class="border-b border-zinc-700 last:border-none">
                    <td class="py-5 font-semibold text-lg">${l.rank}</td>
                    <td class="py-5">${l.team}</td>
                    <td class="py-5">${l.wins}</td>
                    <td class="py-5 font-mono">${l.score}</td>
                </tr>
            `).join('')
        }
        
        function switchTab(n) {
            document.querySelectorAll('.tab-button').forEach((btn, i) => {
                btn.classList.toggle('border-violet-400', i === n)
                btn.classList.toggle('text-violet-400', i === n)
            })
            
            document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'))
            document.getElementById(`tab-content-${n}`).classList.remove('hidden')
        }
        
        function createNewTournament() {
            const name = document.getElementById('modal-name').value || "New Tournament"
            const teamsCount = parseInt(document.getElementById('modal-teams').value)
            
            // Create a fresh tournament
            const newT = {
                id: Date.now(),
                name: name,
                type: document.getElementById('modal-type').value,
                status: "upcoming",
                participantsCount: teamsCount,
                date: "Apr 18, 2026"
            }
            tournamentsDB.unshift(newT)
            
            hideCreateModal()
            renderTournamentList()
            renderDashboardCards()
            showToast(`✅ ${name} created! Open it to build the bracket.`)
            
            // Immediately open the new tournament
            setTimeout(() => {
                activeTournament.name = name
                activeTournament.participants = []
                for (let i = 1; i <= teamsCount; i++) {
                    activeTournament.participants.push({ id: i, name: `Team ${String.fromCharCode(64 + i)}`, seed: i })
                }
                openTournamentDetail(newT.id)
            }, 800)
        }
        
        function showCreateModal() {
            document.getElementById('create-modal').classList.remove('hidden')
            document.getElementById('create-modal').classList.add('flex')
        }
        
        function hideCreateModal() {
            const modal = document.getElementById('create-modal')
            modal.classList.add('hidden')
            modal.classList.remove('flex')
        }
        
        function showToast(text) {
            const t = document.getElementById('toast')
            document.getElementById('toast-text').innerHTML = text
            t.classList.remove('hidden')
            t.style.display = 'flex'
            setTimeout(() => {
                t.classList.add('hidden')
            }, 3000)
        }
        
        function navigateTo(page) {
            document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'))
            
            if (page === 'dashboard') document.getElementById('page-dashboard').classList.remove('hidden')
            else if (page === 'my-tournaments') {
                document.getElementById('page-my-tournaments').classList.remove('hidden')
                renderTournamentList()
            }
            else if (page === 'teams') {
                document.getElementById('page-teams').classList.remove('hidden')
                renderTeamsPage()
            }
        }
        
        function toggleMobileMenu() {
            const menu = document.getElementById('mobileMenu')
            const icon = document.getElementById('hamburger')
            if (menu.classList.contains('hidden')) {
                menu.style.display = 'block'
                icon.classList.replace('fa-bars', 'fa-xmark')
            } else {
                menu.style.display = 'none'
                icon.classList.replace('fa-xmark', 'fa-bars')
            }
        }
        
        function toggleTheme() {
            // Demo dark/light toggle (currently always dark)
            showToast("🌙 Dark theme is already active (perfect for late-night tournaments)")
        }
        
        // Initialize everything
        function initializeApp() {
            initTailwind()
            renderDashboardCards()
            renderTournamentList()
            console.log('%c✅ KlinkTourney mobile-responsive tournament app loaded successfully!', 'color:#a855f7; font-size:13px; font-weight:600')
            console.log('💜 Built for LOVA KLINK • Meshchain.ai')
        }
        
        // START THE APP
        window.onload = initializeApp
    </script>
</body>
</html>
