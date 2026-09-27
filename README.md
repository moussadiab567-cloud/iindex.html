<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no" />
  <title>Control Hub</title>
  
  <!-- PWA & iOS Meta Tags -->
  <link rel="manifest" href="manifest.json" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <meta name="apple-mobile-web-app-title" content="ControlHub" />
  <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/3074/3074058.png" />
  <meta name="theme-color" content="#0f172a" />

  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: { extend: { colors: { darkbg: '#0f172a', cardbg: '#1e293b' } } }
    }
  </script>
</head>
<body class="bg-darkbg text-slate-100 min-h-screen pb-12 font-sans antialiased">

  <!-- Header & Streak -->
  <header class="p-5 bg-cardbg border-b border-slate-700 flex justify-between items-center sticky top-0 z-10 shadow-lg">
    <div>
      <h1 class="text-xl font-bold text-blue-400">Control Hub</h1>
      <p class="text-xs text-slate-400">Mind, Body & Focus</p>
    </div>
    <div class="flex items-center space-x-2 bg-slate-800 px-3 py-1.5 rounded-full border border-orange-500/30">
      <span class="text-lg">🔥</span>
      <span id="streakCount" class="font-bold text-orange-400 text-sm">0 Days</span>
    </div>
  </header>

  <main class="max-w-md mx-auto p-4 space-y-6">

    <!-- Section 1: Daily Check-In & Mental Focus -->
    <section class="bg-cardbg p-5 rounded-2xl border border-slate-700/60 shadow">
      <h2 class="text-lg font-semibold mb-3 flex items-center gap-2 text-indigo-400">
        🧠 Self-Control & Mood Log
      </h2>
      <div class="space-y-3">
        <label class="block text-xs text-slate-400">How are you managing your emotions today?</label>
        <select id="moodSelect" class="w-full bg-slate-900 border border-slate-700 rounded-lg p-2.5 text-sm text-slate-200 focus:outline-none focus:border-blue-500">
          <option value="Calm & In Control">🧘 Calm & In Control</option>
          <option value="Focused & Driven">⚡ Focused & Driven</option>
          <option value="Anxious / Stressed">🌧️ Anxious / Stressed</option>
          <option value="Impulsive / Tempted">⚠️ Impulsive / Tempted</option>
        </select>
        <textarea id="reflectionNote" rows="2" placeholder="Brief reflection or trigger note..." class="w-full bg-slate-900 border border-slate-700 rounded-lg p-2.5 text-sm text-slate-200 focus:outline-none focus:border-blue-500"></textarea>
        <button onclick="logCheckIn()" class="w-full bg-indigo-600 hover:bg-indigo-500 text-white font-medium py-2 rounded-lg text-sm transition">
          Log Entry & Claim Daily Streak
        </button>
      </div>
    </section>

    <!-- Section 2: To-Do List -->
    <section class="bg-cardbg p-5 rounded-2xl border border-slate-700/60 shadow">
      <h2 class="text-lg font-semibold mb-3 text-emerald-400">📋 Daily Priorities</h2>
      <div class="flex gap-2 mb-3">
        <input type="text" id="taskInput" placeholder="Add new task..." class="flex-1 bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-slate-200 focus:outline-none focus:border-emerald-500" />
        <button onclick="addTask()" class="bg-emerald-600 hover:bg-emerald-500 px-4 py-2 rounded-lg font-bold text-sm text-white">+</button>
      </div>
      <ul id="taskList" class="space-y-2 max-h-48 overflow-y-auto"></ul>
    </section>

    <!-- Section 3: Gym & Macro Calculator -->
    <section class="bg-cardbg p-5 rounded-2xl border border-slate-700/60 shadow">
      <h2 class="text-lg font-semibold mb-3 text-cyan-400">🏋️ Macro & Gym Calculator</h2>
      <div class="grid grid-cols-2 gap-3 mb-3">
        <div>
          <label class="block text-xs text-slate-400 mb-1">Weight (kg)</label>
          <input type="number" id="weightInput" placeholder="70" class="w-full bg-slate-900 border border-slate-700 rounded-lg p-2 text-sm text-slate-200" />
        </div>
        <div>
          <label class="block text-xs text-slate-400 mb-1">Goal</label>
          <select id="fitnessGoal" class="w-full bg-slate-900 border border-slate-700 rounded-lg p-2 text-sm text-slate-200">
            <option value="cut">Fat Loss (Cut)</option>
            <option value="maintain">Maintain</option>
            <option value="bulk">Muscle Gain (Bulk)</option>
          </select>
        </div>
      </div>
      <button onclick="calculateMacros()" class="w-full bg-cyan-600 hover:bg-cyan-500 text-white font-medium py-2 rounded-lg text-sm transition mb-4">
        Calculate Targets
      </button>

      <div id="macroResults" class="hidden grid grid-cols-3 gap-2 text-center">
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-800">
          <p class="text-xs text-slate-400">Calories</p>
          <p id="calResult" class="text-base font-bold text-amber-400">0</p>
        </div>
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-800">
          <p class="text-xs text-slate-400">Protein</p>
          <p id="proteinResult" class="text-base font-bold text-cyan-400">0g</p>
        </div>
        <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-800">
          <p class="text-xs text-slate-400">Carbs</p>
          <p id="carbResult" class="text-base font-bold text-emerald-400">0g</p>
        </div>
      </div>
    </section>

    <!-- Section 4: Data Backup & Restore -->
    <section class="bg-cardbg p-5 rounded-2xl border border-slate-700/60 shadow">
      <h2 class="text-lg font-semibold mb-3 text-slate-300">💾 Data & Backup</h2>
      <div class="grid grid-cols-2 gap-3">
        <button onclick="exportData()" class="bg-slate-800 border border-slate-600 hover:bg-slate-700 text-xs font-semibold py-2.5 rounded-lg">
          ⬇️ Export JSON
        </button>
        <label class="bg-slate-800 border border-slate-600 hover:bg-slate-700 text-xs font-semibold py-2.5 rounded-lg text-center cursor-pointer">
          ⬆️ Restore JSON
          <input type="file" id="importFile" onchange="importData(event)" class="hidden" accept=".json" />
        </label>
      </div>
    </section>

  </main>

  <script>
    // State initialization
    let state = JSON.parse(localStorage.getItem('controlHubData')) || {
      streak: 0,
      lastLogDate: null,
      tasks: [],
      logs: []
    };

    function saveState() {
      localStorage.setItem('controlHubData', JSON.stringify(state));
      render();
    }

    // Streak and Mood Logging
    function logCheckIn() {
      const today = new Date().toISOString().split('T')[0];
      const mood = document.getElementById('moodSelect').value;
      const note = document.getElementById('reflectionNote').value;

      if (state.lastLogDate !== today) {
        const yesterday = new Date(Date.now() - 86400000).toISOString().split('T')[0];
        if (state.lastLogDate === yesterday) {
          state.streak += 1;
        } else {
          state.streak = 1;
        }
        state.lastLogDate = today;
      }

      state.logs.push({ date: today, mood, note });
      document.getElementById('reflectionNote').value = '';
      saveState();
      alert('Entry saved and streak updated!');
    }

    // To-Do List Functions
    function addTask() {
      const input = document.getElementById('taskInput');
      if (!input.value.trim()) return;
      state.tasks.push({ text: input.value.trim(), done: false });
      input.value = '';
      saveState();
    }

    function toggleTask(index) {
      state.tasks[index].done = !state.tasks[index].done;
      saveState();
    }

    function deleteTask(index) {
      state.tasks.splice(index, 1);
      saveState();
    }

    // Macro Calculator
    function calculateMacros() {
      const weight = parseFloat(document.getElementById('weightInput').value);
      const goal = document.getElementById('fitnessGoal').value;
      if (!weight || weight <= 0) return alert('Please enter a valid weight.');

      let protein = weight * 2.0; // 2g per kg
      let calories = weight * 32;  // Baseline multiplier

      if (goal === 'cut') calories -= 400;
      if (goal === 'bulk') calories += 350;

      let fat = (calories * 0.25) / 9;
      let carb = (calories - (protein * 4 + fat * 9)) / 4;

      document.getElementById('calResult').innerText = Math.round(calories);
      document.getElementById('proteinResult').innerText = Math.round(protein) + 'g';
      document.getElementById('carbResult').innerText = Math.round(carb) + 'g';
      document.getElementById('macroResults').classList.remove('hidden');
    }

    // Export & Import
    function exportData() {
      const blob = new Blob([JSON.stringify(state, null, 2)], { type: 'application/json' });
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = `control-hub-backup-${new Date().toISOString().split('T')[0]}.json`;
      a.click();
    }

    function importData(e) {
      const reader = new FileReader();
      reader.onload = (event) => {
        try {
          state = JSON.parse(event.target.result);
          saveState();
          alert('Backup restored successfully!');
        } catch {
          alert('Invalid backup file.');
        }
      };
      if (e.target.files[0]) reader.readAsText(e.target.files[0]);
    }

    // Render Function
    function render() {
      document.getElementById('streakCount').innerText = `${state.streak} Days`;
      const taskList = document.getElementById('taskList');
      taskList.innerHTML = state.tasks.map((task, i) => `
        <li class="flex justify-between items-center bg-slate-900 p-2.5 rounded-lg border border-slate-800 text-sm">
          <span onclick="toggleTask(${i})" class="cursor-pointer flex-1 ${task.done ? 'line-through text-slate-500' : 'text-slate-200'}">
            ${task.done ? '✅' : '⚪'} ${task.text}
          </span>
          <button onclick="deleteTask(${i})" class="text-xs text-red-400 hover:text-red-300 ml-2">✕</button>
        </li>
      `).join('');
    }

    // Register Service Worker for PWA installation
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('sw.js').catch(console.error);
    }

    render();
  </script>
</body>
</html>
