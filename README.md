<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mini Netacad - Plataforma Educativa Interactiva</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <!-- Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@500;700&family=Fredoka:wght@400;500;600;700&family=Inter:wght@400;600;700;800;900&display=swap" rel="stylesheet">
  
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            brand: {
              pink: '#ec4899',
              pinkLight: '#fce7f3',
              orange: '#f97316',
              orangeLight: '#ffedd5',
              purple: '#8b5cf6',
              purpleLight: '#f3e8ff',
              green: '#10b981',
              greenLight: '#d1fae5',
              blue: '#06b6d4',
              blueLight: '#cffaff',
              yellow: '#eab308'
            }
          },
          fontFamily: {
            sans: ['Fredoka', 'Inter', 'sans-serif'],
            mono: ['Fira Code', 'monospace']
          }
        }
      }
    }
  </script>

  <style>
    ::-webkit-scrollbar {
      width: 10px;
      height: 10px;
    }
    ::-webkit-scrollbar-track {
      background: #f1f5f9;
    }
    ::-webkit-scrollbar-thumb {
      background: #cbd5e1;
      border-radius: 5px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #94a3b8;
    }
    @keyframes pulse-star {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.35) rotate(12deg); }
    }
    .star-anim {
      animation: pulse-star 0.45s ease-in-out;
    }
    @keyframes shake-err {
      0%, 100% { transform: translateX(0); }
      20%, 60% { transform: translateX(-8px); }
      40%, 80% { transform: translateX(8px); }
    }
    .animate-shake {
      animation: shake-err 0.4s ease-in-out;
    }
  </style>
</head>
<body class="bg-gradient-to-br from-amber-50 via-purple-50 to-pink-50 text-slate-800 font-sans min-h-screen flex flex-col antialiased selection:bg-pink-300 selection:text-pink-900">

  <header class="bg-white/90 backdrop-blur-md border-b-2 border-purple-100 sticky top-0 z-50 shadow-sm">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
      
      <!-- Logo Mini Netacad -->
      <div class="flex items-center space-x-3 cursor-pointer" onclick="switchTab('dashboard')">
        <div class="w-11 h-11 rounded-2xl bg-gradient-to-tr from-pink-500 via-purple-500 to-amber-400 flex items-center justify-center text-white font-black text-2xl shadow-lg shadow-purple-200 hover:rotate-6 transition transform">
          <i class="fa-solid fa-graduation-cap"></i>
        </div>
        <div>
          <span class="text-xl sm:text-2xl font-black tracking-tight bg-gradient-to-r from-pink-600 via-purple-600 to-amber-500 bg-clip-text text-transparent">
            Mini Netacad <span class="text-xs px-2.5 py-0.5 rounded-full bg-amber-100 text-amber-700 font-bold border border-amber-300/60 ml-1">Kids LMS</span>
          </span>
          <p class="text-xs text-slate-500 font-medium hidden sm:block">Aprende, Juega y Gana Estrellas ⭐</p>
        </div>
      </div>

      <!-- Header Widgets: Permanent Star Score Widget & Overall Progress -->
      <div class="flex items-center space-x-3 sm:space-x-6">
        
        <!-- Permanent Star Score Widget (Esquina Superior Permanente) -->
        <div id="star-widget" class="flex items-center space-x-2 bg-gradient-to-r from-amber-400 to-yellow-400 border-2 border-amber-300 px-4 py-1.5 rounded-full shadow-lg shadow-amber-200">
          <i class="fa-solid fa-star text-white text-xl sm:text-2xl drop-shadow-[0_2px_4px_rgba(0,0,0,0.2)]"></i>
          <span class="text-xs sm:text-sm font-black text-slate-900 uppercase tracking-wider">
            <span id="star-count-display" class="text-lg sm:text-2xl text-slate-950 font-black font-mono">0</span> Estrellas
          </span>
        </div>

        <!-- Overall Progress Counter -->
        <div class="hidden md:flex flex-col items-end">
          <div class="flex items-center space-x-2 text-xs font-bold text-slate-600">
            <span>Progreso General:</span>
            <span id="overall-progress-text" class="font-extrabold text-purple-600">0 de 5 completados</span>
          </div>
          <div class="w-44 bg-slate-200 h-3 rounded-full mt-1 overflow-hidden p-0.5 border border-slate-300">
            <div id="overall-progress-bar" class="bg-gradient-to-r from-pink-500 via-purple-500 to-emerald-400 h-full w-0 transition-all duration-500 rounded-full"></div>
          </div>
        </div>

        <!-- Certificate Button (Unlocks when 5/5 completed) -->
        <button id="cert-btn" onclick="openCertificateModal()" class="hidden items-center space-x-2 bg-gradient-to-r from-amber-500 to-yellow-500 text-slate-950 font-black border-2 border-amber-300 hover:scale-105 px-3.5 py-1.5 rounded-2xl text-xs shadow-md transition animate-bounce">
          <i class="fa-solid fa-award text-base text-slate-950"></i>
          <span class="hidden sm:inline">Certificado</span>
        </button>

      </div>
    </div>
  </header>

  <div class="flex-1 flex flex-col md:flex-row max-w-7xl w-full mx-auto p-4 md:p-6 gap-6">
    
    <!-- Sidebar Navigation -->
    <aside class="w-full md:w-72 flex-shrink-0 space-y-4">
      <div class="bg-white rounded-3xl p-4 border-2 border-slate-100 shadow-xl shadow-purple-100/50">
        <h2 class="text-xs font-black text-purple-600 uppercase tracking-wider mb-3 px-2 flex items-center gap-2">
          <i class="fa-solid fa-bars"></i>
          <span>Menú de Juegos y Guías</span>
        </h2>
        
        <nav class="space-y-2">
          <!-- Main Menu / Dashboard Link -->
          <button id="nav-dashboard" onclick="switchTab('dashboard')" class="w-full text-left px-4 py-3 rounded-2xl flex items-center justify-between text-sm font-bold transition bg-gradient-to-r from-purple-600 to-indigo-600 text-white shadow-md shadow-purple-200">
            <div class="flex items-center space-x-3">
              <i class="fa-solid fa-house w-5 text-center text-amber-300 text-base"></i>
              <span>Menú Principal</span>
            </div>
            <i class="fa-solid fa-chevron-right text-xs opacity-80"></i>
          </button>

          <div class="pt-3 pb-1 px-2 text-[11px] font-black text-slate-400 uppercase tracking-wider">
            Módulos Interactivos (5)
          </div>

          <!-- Module 1: Math Quiz (Pink Theme) -->
          <button id="nav-mod1" onclick="switchTab('mod1')" class="w-full text-left px-3.5 py-3 rounded-2xl flex items-center justify-between text-sm font-bold text-slate-700 hover:bg-pink-50 hover:text-pink-600 transition group border border-transparent hover:border-pink-200">
            <div class="flex items-center space-x-3 truncate">
              <span class="w-7 h-7 rounded-xl bg-pink-100 text-pink-600 font-black flex items-center justify-center text-xs flex-shrink-0 group-hover:bg-pink-500 group-hover:text-white transition">1</span>
              <span class="truncate">1. Sumas Matemáticas</span>
            </div>
            <span id="badge-mod1" class="text-[11px] px-2.5 py-0.5 rounded-full bg-slate-100 text-slate-500 font-bold flex-shrink-0">Pendiente</span>
          </button>

          <!-- Module 2: Tie Shoes Guide (Orange Theme) -->
          <button id="nav-mod2" onclick="switchTab('mod2')" class="w-full text-left px-3.5 py-3 rounded-2xl flex items-center justify-between text-sm font-bold text-slate-700 hover:bg-orange-50 hover:text-orange-600 transition group border border-transparent hover:border-orange-200">
            <div class="flex items-center space-x-3 truncate">
              <span class="w-7 h-7 rounded-xl bg-orange-100 text-orange-600 font-black flex items-center justify-center text-xs flex-shrink-0 group-hover:bg-orange-500 group-hover:text-white transition">2</span>
              <span class="truncate">2. Sujetarse Zapatos</span>
            </div>
            <span id="badge-mod2" class="text-[11px] px-2.5 py-0.5 rounded-full bg-slate-100 text-slate-500 font-bold flex-shrink-0">Pendiente</span>
          </button>

          <!-- Module 3: Memory Game (Purple Theme) -->
          <button id="nav-mod3" onclick="switchTab('mod3')" class="w-full text-left px-3.5 py-3 rounded-2xl flex items-center justify-between text-sm font-bold text-slate-700 hover:bg-purple-50 hover:text-purple-600 transition group border border-transparent hover:border-purple-200">
            <div class="flex items-center space-x-3 truncate">
              <span class="w-7 h-7 rounded-xl bg-purple-100 text-purple-600 font-black flex items-center justify-center text-xs flex-shrink-0 group-hover:bg-purple-500 group-hover:text-white transition">3</span>
              <span class="truncate">3. Juego de Memoria</span>
            </div>
            <span id="badge-mod3" class="text-[11px] px-2.5 py-0.5 rounded-full bg-slate-100 text-slate-500 font-bold flex-shrink-0">Pendiente</span>
          </button>

          <!-- Module 4: Motor Physical Activity (Green Theme) -->
          <button id="nav-mod4" onclick="switchTab('mod4')" class="w-full text-left px-3.5 py-3 rounded-2xl flex items-center justify-between text-sm font-bold text-slate-700 hover:bg-emerald-50 hover:text-emerald-600 transition group border border-transparent hover:border-emerald-200">
            <div class="flex items-center space-x-3 truncate">
              <span class="w-7 h-7 rounded-xl bg-emerald-100 text-emerald-600 font-black flex items-center justify-center text-xs flex-shrink-0 group-hover:bg-emerald-500 group-hover:text-white transition">4</span>
              <span class="truncate">4. Actividad Motora</span>
            </div>
            <span id="badge-mod4" class="text-[11px] px-2.5 py-0.5 rounded-full bg-slate-100 text-slate-500 font-bold flex-shrink-0">Pendiente</span>
          </button>

          <!-- Module 5: Hand Washing Guide (Cyan Theme) -->
          <button id="nav-mod5" onclick="switchTab('mod5')" class="w-full text-left px-3.5 py-3 rounded-2xl flex items-center justify-between text-sm font-bold text-slate-700 hover:bg-cyan-50 hover:text-cyan-600 transition group border border-transparent hover:border-cyan-200">
            <div class="flex items-center space-x-3 truncate">
              <span class="w-7 h-7 rounded-xl bg-cyan-100 text-cyan-600 font-black flex items-center justify-center text-xs flex-shrink-0 group-hover:bg-cyan-500 group-hover:text-white transition">5</span>
              <span class="truncate">5. Lavado de Manos</span>
            </div>
            <span id="badge-mod5" class="text-[11px] px-2.5 py-0.5 rounded-full bg-slate-100 text-slate-500 font-bold flex-shrink-0">Pendiente</span>
          </button>
        </nav>
      </div>

      <!-- Quick Info Banner -->
      <div class="bg-gradient-to-br from-amber-400 via-orange-400 to-pink-500 p-4 rounded-3xl text-white shadow-xl shadow-orange-200/60 space-y-1">
        <div class="flex items-center space-x-2 text-white font-black text-sm">
          <i class="fa-solid fa-star text-amber-200 text-lg"></i>
          <span>Puntos y Estrellas</span>
        </div>
        <p class="text-xs text-white/90 leading-relaxed font-medium">
          Gana estrellas ⭐ en cada módulo interactivo para desbloquear tu certificado final.
        </p>
      </div>
    </aside>

    <main class="flex-1 bg-white/80 backdrop-blur-md rounded-3xl border-2 border-white p-5 sm:p-7 flex flex-col justify-between shadow-xl shadow-purple-100/40 relative">

      <!-- Toast Notification Box -->
      <div id="toast-box" class="hidden absolute top-4 right-4 z-50 max-w-md bg-white text-slate-800 border-4 border-pink-400 px-5 py-3.5 rounded-2xl shadow-2xl flex items-center space-x-3 transition-all duration-300">
        <i id="toast-icon" class="fa-solid fa-circle-check text-pink-500 text-2xl"></i>
        <div class="text-xs">
          <p id="toast-title" class="font-black text-slate-900 text-sm">Notificación</p>
          <p id="toast-msg" class="text-slate-600 font-medium">Mensaje de prueba</p>
        </div>
      </div>

      <!-- Dashboard View -->
      <section id="view-dashboard" class="space-y-6">
        <div class="border-b-2 border-slate-100 pb-4">
          <div class="inline-flex items-center space-x-2 px-3.5 py-1 rounded-full bg-pink-100 text-pink-600 font-bold text-xs mb-2">
            <i class="fa-solid fa-gamepad"></i>
            <span>Plataforma Educativa de Aprendizaje Infantil</span>
          </div>
          <h1 class="text-2xl sm:text-4xl font-black text-slate-900 tracking-tight">
            ¡Bienvenido a Mini Netacad!
          </h1>
          <p class="text-slate-600 text-sm mt-1 font-medium">
            Selecciona un juego o guía interactiva. ¡Cada uno tiene un color especial y te da puntos de estrella ⭐!
          </p>
        </div>

        <!-- 5 Cards Dashboard Grid -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-5">
          
          <!-- Card 1: Math Quiz (Pink Theme) -->
          <div onclick="switchTab('mod1')" class="group bg-gradient-to-br from-pink-50 to-rose-100/60 hover:from-pink-100 hover:to-rose-200 p-5 rounded-3xl border-2 border-pink-200 hover:border-pink-400 transition cursor-pointer flex flex-col justify-between shadow-lg shadow-pink-100/50 transform hover:-translate-y-1">
            <div>
              <div class="flex items-center justify-between mb-3">
                <span class="w-10 h-10 rounded-2xl bg-pink-500 text-white flex items-center justify-center font-black text-lg shadow-md shadow-pink-300">1</span>
                <span id="dash-status-mod1" class="text-xs px-3 py-1 rounded-full bg-white text-slate-600 font-extrabold shadow-sm">Pendiente</span>
              </div>
              <h3 class="font-black text-slate-900 text-lg group-hover:text-pink-600 transition flex items-center space-x-2">
                <i class="fa-solid fa-calculator text-pink-500"></i>
                <span>Juego de Sumas</span>
              </h3>
              <p class="text-xs text-slate-600 mt-2 leading-relaxed font-medium">
                Responde preguntas de sumas (2+2, 10+20, 100+100) con respuestas en colores verde y rojo llamativos.
              </p>
            </div>
            <div class="mt-5 pt-3 border-t border-pink-200/80 flex items-center justify-between text-xs text-pink-600 font-black">
              <span>Jugar Ahora</span>
              <i class="fa-solid fa-arrow-right group-hover:translate-x-1 transition"></i>
            </div>
          </div>

          <!-- Card 2: Tie Shoes Guide (Orange Theme) -->
          <div onclick="switchTab('mod2')" class="group bg-gradient-to-br from-amber-50 to-orange-100/60 hover:from-amber-100 hover:to-orange-200 p-5 rounded-3xl border-2 border-orange-200 hover:border-orange-400 transition cursor-pointer flex flex-col justify-between shadow-lg shadow-orange-100/50 transform hover:-translate-y-1">
            <div>
              <div class="flex items-center justify-between mb-3">
                <span class="w-10 h-10 rounded-2xl bg-orange-500 text-white flex items-center justify-center font-black text-lg shadow-md shadow-orange-300">2</span>
                <span id="dash-status-mod2" class="text-xs px-3 py-1 rounded-full bg-white text-slate-600 font-extrabold shadow-sm">Pendiente</span>
              </div>
              <h3 class="font-black text-slate-900 text-lg group-hover:text-orange-600 transition flex items-center space-x-2">
                <i class="fa-solid fa-shoe-prints text-orange-500"></i>
                <span>Sujetarse Zapatos</span>
              </h3>
              <p class="text-xs text-slate-600 mt-2 leading-relaxed font-medium">
                Guía completa pedagógica paso a paso con el método "orejitas de conejo" e ilustraciones vectoriales.
              </p>
            </div>
            <div class="mt-5 pt-3 border-t border-orange-200/80 flex items-center justify-between text-xs text-orange-600 font-black">
              <span>Ver Guía Ilustrada</span>
              <i class="fa-solid fa-arrow-right group-hover:translate-x-1 transition"></i>
            </div>
          </div>

          <!-- Card 3: Memory Game (Purple Theme) -->
          <div onclick="switchTab('mod3')" class="group bg-gradient-to-br from-purple-50 to-indigo-100/60 hover:from-purple-100 hover:to-indigo-200 p-5 rounded-3xl border-2 border-purple-200 hover:border-purple-400 transition cursor-pointer flex flex-col justify-between shadow-lg shadow-purple-100/50 transform hover:-translate-y-1">
            <div>
              <div class="flex items-center justify-between mb-3">
                <span class="w-10 h-10 rounded-2xl bg-purple-500 text-white flex items-center justify-center font-black text-lg shadow-md shadow-purple-300">3</span>
                <span id="dash-status-mod3" class="text-xs px-3 py-1 rounded-full bg-white text-slate-600 font-extrabold shadow-sm">Pendiente</span>
              </div>
              <h3 class="font-black text-slate-900 text-lg group-hover:text-purple-600 transition flex items-center space-x-2">
                <i class="fa-solid fa-brain text-purple-500"></i>
                <span>Juego de Memoria</span>
              </h3>
              <p class="text-xs text-slate-600 mt-2 leading-relaxed font-medium">
                Encuentra las parejas de cartas iguales para ejercitar la atención, memoria visual y concentración.
              </p>
            </div>
            <div class="mt-5 pt-3 border-t border-purple-200/80 flex items-center justify-between text-xs text-purple-600 font-black">
              <span>Voltear Parejas</span>
              <i class="fa-solid fa-arrow-right group-hover:translate-x-1 transition"></i>
            </div>
          </div>

          <!-- Card 4: Physical Motor Activity (Green Theme) -->
          <div onclick="switchTab('mod4')" class="group bg-gradient-to-br from-emerald-50 to-teal-100/60 hover:from-emerald-100 hover:to-teal-200 p-5 rounded-3xl border-2 border-emerald-200 hover:border-emerald-400 transition cursor-pointer flex flex-col justify-between shadow-lg shadow-emerald-100/50 transform hover:-translate-y-1">
            <div>
              <div class="flex items-center justify-between mb-3">
                <span class="w-10 h-10 rounded-2xl bg-emerald-500 text-white flex items-center justify-center font-black text-lg shadow-md shadow-emerald-300">4</span>
                <span id="dash-status-mod4" class="text-xs px-3 py-1 rounded-full bg-white text-slate-600 font-extrabold shadow-sm">Pendiente</span>
              </div>
              <h3 class="font-black text-slate-900 text-lg group-hover:text-emerald-600 transition flex items-center space-x-2">
                <i class="fa-solid fa-child text-emerald-500"></i>
                <span>Actividad Motora (5 años)</span>
              </h3>
              <p class="text-xs text-slate-600 mt-2 leading-relaxed font-medium">
                Circuito de ejercicios físicos (saltos de rana, cuerda floja y estiramientos) para potenciar destrezas.
              </p>
            </div>
            <div class="mt-5 pt-3 border-t border-emerald-200/80 flex items-center justify-between text-xs text-emerald-600 font-black">
              <span>Iniciar Circuito</span>
              <i class="fa-solid fa-arrow-right group-hover:translate-x-1 transition"></i>
            </div>
          </div>

          <!-- Card 5: Hand Washing (Cyan Theme) -->
          <div onclick="switchTab('mod5')" class="group bg-gradient-to-br from-cyan-50 to-sky-100/60 hover:from-cyan-100 hover:to-sky-200 p-5 rounded-3xl border-2 border-cyan-200 hover:border-cyan-400 transition cursor-pointer flex flex-col justify-between shadow-lg shadow-cyan-100/50 transform hover:-translate-y-1">
            <div>
              <div class="flex items-center justify-between mb-3">
                <span class="w-10 h-10 rounded-2xl bg-cyan-500 text-white flex items-center justify-center font-black text-lg shadow-md shadow-cyan-300">5</span>
                <span id="dash-status-mod5" class="text-xs px-3 py-1 rounded-full bg-white text-slate-600 font-extrabold shadow-sm">Pendiente</span>
              </div>
              <h3 class="font-black text-slate-900 text-lg group-hover:text-cyan-600 transition flex items-center space-x-2">
                <i class="fa-solid fa-soap text-cyan-500"></i>
                <span>Lavado de Manos</span>
              </h3>
              <p class="text-xs text-slate-600 mt-2 leading-relaxed font-medium">
                Técnica correcta de higiene con imágenes vectoriales y temporizador interactivo de 20 segundos.
              </p>
            </div>
            <div class="mt-5 pt-3 border-t border-cyan-200/80 flex items-center justify-between text-xs text-cyan-600 font-black">
              <span>Aprender Higiene</span>
              <i class="fa-solid fa-arrow-right group-hover:translate-x-1 transition"></i>
            </div>
          </div>

        </div>
      </section>

      <!-- MODULE 1: Math Quiz (Bright Pink Theme) -->
      <section id="view-mod1" class="hidden space-y-6">
        <div class="flex items-center justify-between border-b-2 border-pink-100 pb-4">
          <div>
            <span class="text-xs text-pink-600 font-black uppercase tracking-wider bg-pink-100 px-3 py-1 rounded-full">Módulo 1 de 5</span>
            <h2 class="text-2xl sm:text-3xl font-black text-slate-900 mt-1">Juego de Sumas Matemáticas 💖</h2>
          </div>
          <button onclick="switchTab('dashboard')" class="bg-pink-100 hover:bg-pink-200 text-pink-700 px-4 py-2 rounded-2xl text-xs font-black transition border border-pink-300">
            <i class="fa-solid fa-arrow-left mr-1"></i> Volver al Menú
          </button>
        </div>

        <div class="max-w-2xl mx-auto bg-gradient-to-b from-pink-50 to-white p-6 sm:p-8 rounded-3xl border-2 border-pink-200 shadow-xl space-y-6">
          
          <div class="flex justify-between items-center text-xs font-black text-slate-500 border-b border-pink-100 pb-3">
            <span id="quiz-progress-lbl" class="bg-pink-200 text-pink-800 px-3 py-1 rounded-full">Pregunta 1 de 3</span>
            <span id="quiz-score-lbl" class="text-amber-500 flex items-center gap-1 text-sm">
              <i class="fa-solid fa-star text-amber-400"></i> +1 Estrella por acertar
            </span>
          </div>

          <!-- Question Content Box -->
          <div id="quiz-question-box" class="space-y-6">
            <!-- Dynamic Questions Rendered via JavaScript -->
          </div>

          <!-- Finish Screen Module 1 -->
          <div id="quiz-finish-card" class="hidden text-center space-y-5 p-6 bg-gradient-to-b from-green-100 to-emerald-50 border-4 border-green-400 rounded-3xl shadow-lg">
            <div class="w-20 h-20 rounded-full bg-green-500 text-white flex items-center justify-center mx-auto text-4xl shadow-lg shadow-green-300 animate-bounce">
              <i class="fa-solid fa-trophy"></i>
            </div>
            <h3 class="text-2xl font-black text-green-700">¡Excelente Trabajo!</h3>
            <p class="text-sm text-slate-700 font-bold max-w-md mx-auto">
              Has respondido todas las preguntas de sumas correctamente y has ganado tus estrellas ⭐.
            </p>
            <div class="p-4 bg-white rounded-2xl border-2 border-green-300 text-amber-600 font-black text-sm shadow-sm">
              Muchas gracias por jugar. Puedes volver al menú para seguir jugando.
            </div>
            <button onclick="switchTab('dashboard')" class="bg-gradient-to-r from-green-500 to-emerald-600 hover:from-green-600 hover:to-emerald-700 text-white font-black px-8 py-3.5 rounded-2xl shadow-lg shadow-green-200 transition transform hover:scale-105 text-sm">
              <i class="fa-solid fa-house mr-2"></i> Volver al Menú Principal
            </button>
          </div>

        </div>
      </section>

      <!-- MODULE 2: Tie Shoes Guide (Bright Orange/Amber Theme) -->
      <section id="view-mod2" class="hidden space-y-6">
        <div class="flex items-center justify-between border-b-2 border-orange-100 pb-4">
          <div>
            <span class="text-xs text-orange-600 font-black uppercase tracking-wider bg-orange-100 px-3 py-1 rounded-full">Módulo 2 de 5</span>
            <h2 class="text-2xl sm:text-3xl font-black text-slate-900 mt-1">Guía Ilustrada: Sujetarse los Zapatos 👟</h2>
          </div>
          <button onclick="switchTab('dashboard')" class="bg-orange-100 hover:bg-orange-200 text-orange-700 px-4 py-2 rounded-2xl text-xs font-black transition border border-orange-300">
            <i class="fa-solid fa-arrow-left mr-1"></i> Volver al Menú
          </button>
        </div>

        <div class="space-y-6">
          
          <!-- Paso 1 -->
          <div class="bg-gradient-to-r from-amber-50 to-orange-50 p-6 rounded-3xl border-2 border-orange-200 shadow-md space-y-4">
            <div class="flex items-center space-x-3 text-orange-600 border-b border-orange-200 pb-3">
              <span class="w-9 h-9 rounded-2xl bg-orange-500 text-white flex items-center justify-center font-black shadow-md">1</span>
              <h3 class="text-lg font-black text-slate-900">Paso 1 — Preparar y motivar al niño</h3>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 text-xs font-semibold">
              <div class="bg-white p-4 rounded-2xl border border-orange-200 space-y-1 shadow-sm">
                <span class="font-black text-orange-600 block text-sm">Elegir el momento adecuado:</span>
                <p class="text-slate-600">El niño debe estar receptivo y concentrado, generalmente entre los 4 y 6 años.</p>
              </div>
              <div class="bg-white p-4 rounded-2xl border border-orange-200 space-y-1 shadow-sm">
                <span class="font-black text-orange-600 block text-sm">Zapatos adecuados:</span>
                <p class="text-slate-600">Usar cordones de colores contrastantes (rojo y azul) para hacer visibles los pasos.</p>
              </div>
              <div class="bg-white p-4 rounded-2xl border border-orange-200 space-y-1 shadow-sm">
                <span class="font-black text-orange-600 block text-sm">Motivar:</span>
                <p class="text-slate-600">Explicar que aprender a amarrárselos lo hará más independiente y elogiar sus esfuerzos.</p>
              </div>
            </div>
          </div>

          <!-- Paso 2 con Ilustraciones SVG paso a paso -->
          <div class="bg-gradient-to-r from-orange-50 to-amber-50 p-6 rounded-3xl border-2 border-orange-200 shadow-md space-y-4">
            <div class="flex items-center space-x-3 text-orange-600 border-b border-orange-200 pb-3">
              <span class="w-9 h-9 rounded-2xl bg-orange-500 text-white flex items-center justify-center font-black shadow-md">2</span>
              <h3 class="text-lg font-black text-slate-900">Paso 2 — Enseñanza paso a paso (Método "Orejitas de Conejo")</h3>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
              
              <!-- Sub-step 1 -->
              <div class="bg-white p-4 rounded-2xl border-2 border-orange-100 shadow-sm space-y-3 text-center">
                <div class="h-28 bg-amber-50 rounded-xl flex items-center justify-center p-2 border border-amber-200">
                  <svg class="w-full h-full" viewBox="0 0 100 80">
                    <path d="M 20,60 Q 50,10 80,60" fill="none" stroke="#ef4444" stroke-width="5" stroke-linecap="round"/>
                    <path d="M 80,60 Q 50,10 20,60" fill="none" stroke="#3b82f6" stroke-width="5" stroke-linecap="round"/>
                    <circle cx="50" cy="30" r="5" fill="#f59e0b"/>
                  </svg>
                </div>
                <h4 class="font-black text-orange-600 text-xs">1. Demostración visual</h4>
                <p class="text-[11px] text-slate-600 font-medium">Muéstrale cómo se hace, hablando en voz alta sobre cada movimiento.</p>
              </div>

              <!-- Sub-step 2 -->
              <div class="bg-white p-4 rounded-2xl border-2 border-orange-100 shadow-sm space-y-3 text-center">
                <div class="h-28 bg-amber-50 rounded-xl flex items-center justify-center p-2 border border-amber-200">
                  <svg class="w-full h-full" viewBox="0 0 100 80">
                    <ellipse cx="35" cy="35" rx="12" ry="25" fill="none" stroke="#ef4444" stroke-width="5"/>
                    <ellipse cx="65" cy="35" rx="12" ry="25" fill="none" stroke="#3b82f6" stroke-width="5"/>
                  </svg>
                </div>
                <h4 class="font-black text-orange-600 text-xs">2. Orejitas de conejo</h4>
                <p class="text-[11px] text-slate-600 font-medium">Formar dos lazos (orejas) con los cordones y luego cruzarlos.</p>
              </div>

              <!-- Sub-step 3 -->
              <div class="bg-white p-4 rounded-2xl border-2 border-orange-100 shadow-sm space-y-3 text-center">
                <div class="h-28 bg-amber-50 rounded-xl flex items-center justify-center p-2 border border-amber-200">
                  <svg class="w-full h-full" viewBox="0 0 100 80">
                    <ellipse cx="45" cy="40" rx="15" ry="20" fill="none" stroke="#ef4444" stroke-width="5" transform="rotate(-20 45 40)"/>
                    <ellipse cx="55" cy="40" rx="15" ry="20" fill="none" stroke="#3b82f6" stroke-width="5" transform="rotate(20 55 40)"/>
                  </svg>
                </div>
                <h4 class="font-black text-orange-600 text-xs">3. Repetición guiada</h4>
                <p class="text-[11px] text-slate-600 font-medium">Ayuda al niño a manipular los cordones corrigiendo suavemente.</p>
              </div>

              <!-- Sub-step 4 -->
              <div class="bg-white p-4 rounded-2xl border-2 border-orange-100 shadow-sm space-y-3 text-center">
                <div class="h-28 bg-amber-50 rounded-xl flex items-center justify-center p-2 border border-amber-200">
                  <svg class="w-full h-full" viewBox="0 0 100 80">
                    <circle cx="50" cy="40" r="16" fill="none" stroke="#10b981" stroke-width="6"/>
                    <path d="M 35,40 L 65,40" stroke="#f59e0b" stroke-width="4"/>
                  </svg>
                </div>
                <h4 class="font-black text-orange-600 text-xs">4. Pasos pequeños</h4>
                <p class="text-[11px] text-slate-600 font-medium">Primero nudo simple, luego orejas y cruzar. Evita frustración.</p>
              </div>

            </div>
          </div>

          <!-- Paso 3 & Paso 4 -->
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            
            <div class="bg-white p-5 rounded-3xl border-2 border-amber-200 shadow-sm space-y-2">
              <h3 class="font-black text-amber-600 text-sm flex items-center gap-2">
                <i class="fa-solid fa-icons text-amber-500"></i>
                <span>Paso 3 — Recursos visuales y lúdicos</span>
              </h3>
              <ul class="space-y-1.5 text-xs text-slate-600 font-medium list-disc list-inside">
                <li><strong class="text-slate-900">Tarjetas ilustradas o videos:</strong> Representación gráfica de cada movimiento.</li>
                <li><strong class="text-slate-900">Cantar o rimar:</strong> Asociar cada acción con una frase divertida.</li>
                <li><strong class="text-slate-900">Práctica con juegos:</strong> Amarrar cordones en un juguete antes de su zapato.</li>
              </ul>
            </div>

            <div class="bg-white p-5 rounded-3xl border-2 border-orange-200 shadow-sm space-y-2">
              <h3 class="font-black text-orange-600 text-sm flex items-center gap-2">
                <i class="fa-solid fa-heart text-orange-500"></i>
                <span>Paso 4 — Refuerzo positivo y práctica continua</span>
              </h3>
              <ul class="space-y-1.5 text-xs text-slate-600 font-medium list-disc list-inside">
                <li><strong class="text-slate-900">Elogios frecuentes:</strong> Resaltar el esfuerzo constante.</li>
                <li><strong class="text-slate-900">Paciencia:</strong> La motricidad fina toma tiempo y varias semanas de práctica.</li>
                <li><strong class="text-slate-900">Revisión diaria:</strong> Practicar todos los días a su propio ritmo.</li>
              </ul>
            </div>

          </div>

          <!-- Shoe Completion Action Box -->
          <div class="p-6 bg-gradient-to-r from-amber-100 to-orange-100 border-4 border-orange-300 rounded-3xl text-center space-y-4 shadow-lg">
            <p id="shoe-thanks-msg" class="text-sm font-black text-orange-700 hidden">
              ¡Muchas gracias por jugar y aprender! Puedes ir al menú para seguir jugando.
            </p>
            <button id="shoe-complete-btn" onclick="completeShoeModule()" class="bg-gradient-to-r from-orange-500 to-amber-500 hover:from-orange-600 hover:to-amber-600 text-white font-black px-8 py-3.5 rounded-2xl shadow-md transition text-sm transform hover:scale-105">
              <i class="fa-solid fa-star mr-2 text-amber-200"></i> ¡He aprendido a sujetarme los zapatos! (+1 Estrella ⭐)
            </button>
          </div>

        </div>
      </section>

      <!-- MODULE 3: Memory Game (Bright Purple Theme) -->
      <section id="view-mod3" class="hidden space-y-6">
        <div class="flex items-center justify-between border-b-2 border-purple-100 pb-4">
          <div>
            <span class="text-xs text-purple-600 font-black uppercase tracking-wider bg-purple-100 px-3 py-1 rounded-full">Módulo 3 de 5</span>
            <h2 class="text-2xl sm:text-3xl font-black text-slate-900 mt-1">Juego de Memoria Visual 🧠</h2>
          </div>
          <button onclick="switchTab('dashboard')" class="bg-purple-100 hover:bg-purple-200 text-purple-700 px-4 py-2 rounded-2xl text-xs font-black transition border border-purple-300">
            <i class="fa-solid fa-arrow-left mr-1"></i> Volver al Menú
          </button>
        </div>

        <div class="max-w-xl mx-auto bg-gradient-to-b from-purple-50 to-white p-6 rounded-3xl border-2 border-purple-200 shadow-xl space-y-6 text-center">
          <p class="text-xs text-slate-600 font-bold">
            Haz clic en las cartas para darles la vuelta y encuentra las 4 parejas de figuras iguales:
          </p>

          <div id="memory-grid" class="grid grid-cols-4 gap-3.5 max-w-md mx-auto">
            <!-- Dynamic 8 Memory Cards in JS -->
          </div>

          <div id="memory-finish-msg" class="hidden p-5 bg-gradient-to-r from-purple-100 to-indigo-100 border-4 border-purple-400 rounded-2xl space-y-3">
            <p class="text-base font-black text-purple-800">
              ¡Felicitaciones! Has encontrado todas las parejas de memoria.
            </p>
            <p class="text-xs text-amber-700 font-extrabold">
              Muchas gracias por jugar. Puedes volver al menú para seguir jugando.
            </p>
            <button onclick="switchTab('dashboard')" class="bg-purple-600 hover:bg-purple-700 text-white font-black px-6 py-2.5 rounded-2xl text-xs shadow-md">
              Volver al Menú Principal
            </button>
          </div>
        </div>
      </section>

      <!-- MODULE 4: Motor Physical Activity (Bright Emerald Theme) -->
      <section id="view-mod4" class="hidden space-y-6">
        <div class="flex items-center justify-between border-b-2 border-emerald-100 pb-4">
          <div>
            <span class="text-xs text-emerald-600 font-black uppercase tracking-wider bg-emerald-100 px-3 py-1 rounded-full">Módulo 4 de 5</span>
            <h2 class="text-2xl sm:text-3xl font-black text-slate-900 mt-1">Actividad Física Motora (5 años) 🏃</h2>
          </div>
          <button onclick="switchTab('dashboard')" class="bg-emerald-100 hover:bg-emerald-200 text-emerald-700 px-4 py-2 rounded-2xl text-xs font-black transition border border-emerald-300">
            <i class="fa-solid fa-arrow-left mr-1"></i> Volver al Menú
          </button>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          
          <div class="bg-gradient-to-br from-emerald-50 to-teal-100/80 p-5 rounded-3xl border-2 border-emerald-200 shadow-sm space-y-2">
            <div class="flex justify-between items-center text-emerald-700 font-black text-sm">
              <span>1. Salto de la Rana</span>
              <i class="fa-solid fa-frog text-2xl text-emerald-600"></i>
            </div>
            <p class="text-xs text-slate-600 font-medium">Dar 5 saltos agachándose y estirando las manos hacia arriba con impulso.</p>
          </div>

          <div class="bg-gradient-to-br from-emerald-50 to-teal-100/80 p-5 rounded-3xl border-2 border-emerald-200 shadow-sm space-y-2">
            <div class="flex justify-between items-center text-emerald-700 font-black text-sm">
              <span>2. La Cuerda Floja</span>
              <i class="fa-solid fa-person-walking text-2xl text-emerald-600"></i>
            </div>
            <p class="text-xs text-slate-600 font-medium">Caminar en línea recta poniendo un pie justo delante del otro en equilibrio.</p>
          </div>

          <div class="bg-gradient-to-br from-emerald-50 to-teal-100/80 p-5 rounded-3xl border-2 border-emerald-200 shadow-sm space-y-2">
            <div class="flex justify-between items-center text-emerald-700 font-black text-sm">
              <span>3. Tiro al Blanco</span>
              <i class="fa-solid fa-bullseye text-2xl text-emerald-600"></i>
            </div>
            <p class="text-xs text-slate-600 font-medium">Lanzar una pelota suave a un cesto a 2 metros ejercitando coordinación.</p>
          </div>

          <div class="bg-gradient-to-br from-emerald-50 to-teal-100/80 p-5 rounded-3xl border-2 border-emerald-200 shadow-sm space-y-2">
            <div class="flex justify-between items-center text-emerald-700 font-black text-sm">
              <span>4. El Súper Estiramiento</span>
              <i class="fa-solid fa-child-reaching text-2xl text-emerald-600"></i>
            </div>
            <p class="text-xs text-slate-600 font-medium">Tocar las puntas de los pies sin doblar rodillas durante 10 segundos.</p>
          </div>

        </div>

        <div class="p-6 bg-emerald-100 border-4 border-emerald-300 rounded-3xl text-center space-y-3 shadow-lg">
          <p class="text-xs text-emerald-900 font-extrabold">Realiza la rutina completa de ejercicios motrices con los niños.</p>
          <button onclick="completeMotorModule()" class="bg-gradient-to-r from-emerald-500 to-teal-600 hover:from-emerald-600 hover:to-teal-700 text-white font-black px-8 py-3 rounded-2xl text-xs shadow-md transition transform hover:scale-105">
            <i class="fa-solid fa-check-circle mr-1"></i> Completar Circuito Físico (+1 Estrella ⭐)
          </button>
        </div>
      </section>

      <!-- MODULE 5: Hand Washing Guide (Bright Sky Blue/Cyan Theme) -->
      <section id="view-mod5" class="hidden space-y-6">
        <div class="flex items-center justify-between border-b-2 border-cyan-100 pb-4">
          <div>
            <span class="text-xs text-cyan-600 font-black uppercase tracking-wider bg-cyan-100 px-3 py-1 rounded-full">Módulo 5 de 5</span>
            <h2 class="text-2xl sm:text-3xl font-black text-slate-900 mt-1">Guía Ilustrada: Lavado de Manos 🧼</h2>
          </div>
          <button onclick="switchTab('dashboard')" class="bg-cyan-100 hover:bg-cyan-200 text-cyan-700 px-4 py-2 rounded-2xl text-xs font-black transition border border-cyan-300">
            <i class="fa-solid fa-arrow-left mr-1"></i> Volver al Menú
          </button>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
          
          <div class="bg-white p-5 rounded-3xl border-2 border-cyan-200 text-center space-y-2 shadow-sm">
            <div class="w-12 h-12 bg-cyan-500 text-white rounded-2xl flex items-center justify-center mx-auto text-xl font-black shadow-md">1</div>
            <h4 class="font-black text-cyan-600 text-xs">1. Mojar y Enjabonar</h4>
            <p class="text-[11px] text-slate-600 font-medium">Usar agua limpia y suficiente jabón hasta hacer bastante espuma.</p>
          </div>

          <div class="bg-white p-5 rounded-3xl border-2 border-cyan-200 text-center space-y-2 shadow-sm">
            <div class="w-12 h-12 bg-cyan-500 text-white rounded-2xl flex items-center justify-center mx-auto text-xl font-black shadow-md">2</div>
            <h4 class="font-black text-cyan-600 text-xs">2. Frotar 20 Segundos</h4>
            <p class="text-[11px] text-slate-600 font-medium">Frotar palmas, dorso, entre los dedos y debajo de las uñas.</p>
          </div>

          <div class="bg-white p-5 rounded-3xl border-2 border-cyan-200 text-center space-y-2 shadow-sm">
            <div class="w-12 h-12 bg-cyan-500 text-white rounded-2xl flex items-center justify-center mx-auto text-xl font-black shadow-md">3</div>
            <h4 class="font-black text-cyan-600 text-xs">3. Enjuagar con Agua</h4>
            <p class="text-[11px] text-slate-600 font-medium">Remover todo el jabón con abundante agua corriente.</p>
          </div>

          <div class="bg-white p-5 rounded-3xl border-2 border-cyan-200 text-center space-y-2 shadow-sm">
            <div class="w-12 h-12 bg-cyan-500 text-white rounded-2xl flex items-center justify-center mx-auto text-xl font-black shadow-md">4</div>
            <h4 class="font-black text-cyan-600 text-xs">4. Secar Bien</h4>
            <p class="text-[11px] text-slate-600 font-medium">Secar con una toalla limpia o papel desechable.</p>
          </div>

        </div>

        <!-- 20s Interactive Timer Card -->
        <div class="bg-gradient-to-b from-cyan-100 to-sky-50 p-6 rounded-3xl border-4 border-cyan-300 text-center space-y-4 max-w-md mx-auto shadow-xl">
          <h3 class="font-black text-cyan-800 text-sm">Temporizador Interactivo de 20 Segundos</h3>
          <div id="hand-timer-display" class="text-5xl font-mono font-black text-cyan-600 drop-shadow-sm">20s</div>
          <button onclick="startHandTimer()" class="bg-gradient-to-r from-cyan-500 to-blue-500 hover:from-cyan-600 hover:to-blue-600 text-white font-black px-8 py-3 rounded-2xl text-xs shadow-md transition transform hover:scale-105">
            <i class="fa-solid fa-stopwatch mr-1"></i> Iniciar Conteo de Lavado
          </button>
        </div>
      </section>

      <!-- Footer Info -->
      <footer class="mt-8 pt-4 border-t border-slate-200 flex flex-col sm:flex-row justify-between items-center text-xs text-slate-500 font-medium gap-2">
        <span>&copy; Mini Netacad Kids LMS - 5 Módulos Educativos Activos.</span>
        <button onclick="resetAllProgress()" class="hover:text-pink-600 transition text-[11px] font-bold">
          <i class="fa-solid fa-rotate-left mr-1"></i> Reiniciar Progreso
        </button>
      </footer>

    </main>

  </div>

  <!-- Certificate Modal -->
  <div id="cert-modal" class="hidden fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-md flex items-center justify-center p-4">
    <div class="bg-white border-4 border-amber-400 rounded-3xl max-w-2xl w-full p-6 text-center space-y-6 shadow-2xl relative">
      <button onclick="closeCertificateModal()" class="absolute top-4 right-4 text-slate-400 hover:text-slate-800">
        <i class="fa-solid fa-xmark text-2xl"></i>
      </button>

      <div class="border-4 border-double border-amber-300 p-6 rounded-2xl bg-gradient-to-b from-amber-50 to-yellow-50 space-y-4">
        <i class="fa-solid fa-award text-7xl text-amber-500 drop-shadow-md"></i>
        <h2 class="text-3xl font-black tracking-widest text-amber-700 uppercase">Certificado Mini Netacad</h2>
        <p class="text-xs text-slate-500 font-bold">Otorgado por completar los 5 módulos educativos e interactivos</p>
        
        <p class="text-sm text-slate-700 font-bold my-4 leading-relaxed">
          ¡Felicitaciones al gran estudiante por haber completado exitosamente los <strong class="text-amber-600">5 Módulos de Mini Netacad</strong> (Matemáticas, Sujetarse Zapatos, Memoria, Actividad Motora y Lavado de Manos)!
        </p>

        <div class="flex justify-between items-center pt-3 border-t border-amber-200 text-xs text-slate-600 font-black">
          <span>Mini Netacad Kids Edition</span>
          <span class="text-emerald-600">5/5 Módulos Completados ✓</span>
        </div>
      </div>

      <button onclick="window.print()" class="bg-amber-500 hover:bg-amber-400 text-slate-950 font-black px-8 py-3 rounded-2xl shadow-md transition text-sm">
        <i class="fa-solid fa-print mr-1"></i> Imprimir Certificado
      </button>
    </div>
  </div>

  <script>
    // Global Application State
    const appState = {
      stars: 0,
      completedModules: {
        mod1: false,
        mod2: false,
        mod3: false,
        mod4: false,
        mod5: false
      },
      currentTab: 'dashboard',
      quizStep: 0,
      memoryCards: [],
      flippedCards: [],
      matchedPairs: 0
    };

    // Math Quiz Questions (Pink Theme)
    const quizQuestions = [
      {
        q: "¿Cuánto es 2 más 2?",
        options: [
          { text: "A. 1", correct: false },
          { text: "B. 3", correct: false },
          { text: "C. 4", correct: true },
          { text: "D. 5", correct: false }
        ]
      },
      {
        q: "¿Cuánto es 10 más 20?",
        options: [
          { text: "A. 20", correct: false },
          { text: "B. 30", correct: true },
          { text: "C. 40", correct: false },
          { text: "D. 50", correct: false }
        ]
      },
      {
        q: "¿Cuánto es 100 más 100?",
        options: [
          { text: "A. 100", correct: false },
          { text: "B. 800", correct: false },
          { text: "C. 488", correct: false },
          { text: "D. 200", correct: true }
        ]
      }
    ];

    // Audio Synthesizer (Web Audio API)
    let audioCtx = null;
    function initAudio() {
      if (!audioCtx) {
        audioCtx = new (window.AudioContext || window.webkitAudioContext)();
      }
    }

    function playSound(freq, duration, type = 'sine') {
      try {
        initAudio();
        if (!audioCtx) return;
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.type = type;
        osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
        gain.gain.setValueAtTime(0.2, audioCtx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + duration);
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        osc.start();
        osc.stop(audioCtx.currentTime + duration);
      } catch (e) {}
    }

    function playSuccessSound() {
      playSound(523.25, 0.1);
      setTimeout(() => playSound(659.25, 0.15), 100);
      setTimeout(() => playSound(783.99, 0.25), 200);
    }

    function playErrorSound() {
      playSound(220, 0.15, 'sawtooth');
      setTimeout(() => playSound(180, 0.25, 'sawtooth'), 120);
    }

    window.onload = function() {
      renderQuizQuestion();
      setupMemoryGame();
      updateDashboardUI();
    };

    function switchTab(tabId) {
      const views = ['dashboard', 'mod1', 'mod2', 'mod3', 'mod4', 'mod5'];
      views.forEach(v => {
        const el = document.getElementById(`view-${v}`);
        if (el) el.classList.add('hidden');
        
        const navBtn = document.getElementById(`nav-${v}`);
        if (navBtn) {
          navBtn.classList.remove('bg-gradient-to-r', 'from-purple-600', 'to-indigo-600', 'text-white', 'shadow-md');
          navBtn.classList.add('text-slate-700');
        }
      });

      const targetView = document.getElementById(`view-${tabId}`);
      if (targetView) targetView.classList.remove('hidden');

      const targetNav = document.getElementById(`nav-${tabId}`);
      if (targetNav) {
        targetNav.classList.add('bg-gradient-to-r', 'from-purple-600', 'to-indigo-600', 'text-white', 'shadow-md');
        targetNav.classList.remove('text-slate-700');
      }

      appState.currentTab = tabId;
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    function addStar(count = 1) {
      appState.stars += count;
      const display = document.getElementById('star-count-display');
      const widget = document.getElementById('star-widget');
      
      if (display) display.textContent = appState.stars;
      if (widget) {
        widget.classList.add('star-anim');
        setTimeout(() => widget.classList.remove('star-anim'), 450);
      }
    }

    function markModuleComplete(modKey) {
      if (!appState.completedModules[modKey]) {
        appState.completedModules[modKey] = true;
        addStar(1);
        updateDashboardUI();
      }
    }

    function updateDashboardUI() {
      let completedCount = 0;
      Object.keys(appState.completedModules).forEach(key => {
        const isDone = appState.completedModules[key];
        if (isDone) completedCount++;

        const navBadge = document.getElementById(`badge-${key}`);
        if (navBadge) {
          navBadge.textContent = isDone ? 'Completado ✓' : 'Pendiente';
          navBadge.className = isDone 
            ? 'text-[11px] px-2.5 py-0.5 rounded-full bg-emerald-100 text-emerald-700 font-black border border-emerald-300'
            : 'text-[11px] px-2.5 py-0.5 rounded-full bg-slate-100 text-slate-500 font-bold';
        }

        const dashBadge = document.getElementById(`dash-status-${key}`);
        if (dashBadge) {
          dashBadge.textContent = isDone ? 'Completado ✓' : 'Pendiente';
          dashBadge.className = isDone 
            ? 'text-xs px-3 py-1 rounded-full bg-emerald-500 text-white font-black shadow-sm'
            : 'text-xs px-3 py-1 rounded-full bg-white text-slate-600 font-extrabold shadow-sm';
        }
      });

      // Show "0 de 5 completados"
      document.getElementById('overall-progress-text').textContent = `${completedCount} de 5 completados`;
      
      const pct = Math.round((completedCount / 5) * 100);
      document.getElementById('overall-progress-bar').style.width = `${pct}%`;

      const certBtn = document.getElementById('cert-btn');
      if (completedCount === 5) {
        certBtn.classList.remove('hidden');
        certBtn.classList.add('flex');
      }
    }

    function renderQuizQuestion() {
      const box = document.getElementById('quiz-question-box');
      const progress = document.getElementById('quiz-progress-lbl');
      if (!box) return;

      if (appState.quizStep >= quizQuestions.length) {
        box.classList.add('hidden');
        document.getElementById('quiz-finish-card').classList.remove('hidden');
        markModuleComplete('mod1');
        return;
      }

      const qData = quizQuestions[appState.quizStep];
      progress.textContent = `Pregunta ${appState.quizStep + 1} de ${quizQuestions.length}`;

      box.innerHTML = `
        <h3 class="text-2xl sm:text-3xl font-black text-slate-900 text-center py-2">${qData.q}</h3>
        <div id="options-grid" class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          ${qData.options.map((opt, idx) => `
            <button onclick="checkMathAnswer(${idx})" id="opt-btn-${idx}" class="w-full text-left p-4 rounded-2xl bg-white border-2 border-pink-200 hover:border-pink-500 text-slate-800 font-black text-base transition shadow-sm flex items-center justify-between hover:bg-pink-50">
              <span>${opt.text}</span>
              <i class="fa-regular fa-circle text-slate-400 text-lg" id="opt-icon-${idx}"></i>
            </button>
          `).join('')}
        </div>
        <div id="quiz-feedback" class="hidden text-center p-3.5 rounded-2xl font-black text-sm"></div>
      `;
    }

    function checkMathAnswer(selectedIndex) {
      const qData = quizQuestions[appState.quizStep];
      const selectedOpt = qData.options[selectedIndex];
      const feedback = document.getElementById('quiz-feedback');

      qData.options.forEach((_, idx) => {
        const btn = document.getElementById(`opt-btn-${idx}`);
        if (btn) btn.onclick = null;
      });

      if (selectedOpt.correct) {
        // Bright Green Highlight for Correct Answer
        playSuccessSound();
        const btn = document.getElementById(`opt-btn-${selectedIndex}`);
        btn.className = "w-full text-left p-4 rounded-2xl bg-green-500 border-4 border-green-400 text-white font-black text-base shadow-lg shadow-green-200 transition flex items-center justify-between";
        document.getElementById(`opt-icon-${selectedIndex}`).className = "fa-solid fa-circle-check text-white text-2xl";

        feedback.className = "text-center p-3.5 rounded-2xl font-black text-sm bg-green-100 text-green-800 border-2 border-green-400 shadow-sm";
        feedback.textContent = "¡Respuesta Correcta! Excelente trabajo. +1 Estrella ⭐";
        feedback.classList.remove('hidden');

        addStar(1);

        setTimeout(() => {
          appState.quizStep++;
          renderQuizQuestion();
        }, 1200);

      } else {
        // Bright Red Highlight for Incorrect Answer
        playErrorSound();
        const btn = document.getElementById(`opt-btn-${selectedIndex}`);
        btn.className = "w-full text-left p-4 rounded-2xl bg-red-500 border-4 border-red-400 text-white font-black text-base shadow-lg shadow-red-200 transition flex items-center justify-between animate-shake";
        document.getElementById(`opt-icon-${selectedIndex}`).className = "fa-solid fa-circle-xmark text-white text-2xl";

        feedback.className = "text-center p-3.5 rounded-2xl font-black text-sm bg-red-100 text-red-800 border-2 border-red-400 shadow-sm";
        feedback.textContent = "Respuesta Incorrecta. Inténtalo de nuevo.";
        feedback.classList.remove('hidden');

        setTimeout(() => {
          renderQuizQuestion();
        }, 1400);
      }
    }

    // Module 2 Complete
    function completeShoeModule() {
      markModuleComplete('mod2');
      playSuccessSound();
      document.getElementById('shoe-thanks-msg').classList.remove('hidden');
      const btn = document.getElementById('shoe-complete-btn');
      btn.disabled = true;
      btn.classList.add('opacity-50', 'cursor-not-allowed');
      showToast('¡Guía completada! Te has ganado 1 estrella ⭐', 'success');
    }

    function setupMemoryGame() {
      const icons = ['fa-star', 'fa-heart', 'fa-rocket', 'fa-car'];
      appState.memoryCards = [...icons, ...icons].sort(() => Math.random() - 0.5);
      appState.flippedCards = [];
      appState.matchedPairs = 0;

      const grid = document.getElementById('memory-grid');
      if (!grid) return;
      grid.innerHTML = '';

      appState.memoryCards.forEach((icon, idx) => {
        const card = document.createElement('div');
        card.className = "h-20 bg-purple-100 border-2 border-purple-300 rounded-2xl flex items-center justify-center text-3xl text-purple-600 cursor-pointer hover:border-purple-500 hover:scale-105 transition shadow-sm";
        card.id = `mem-card-${idx}`;
        card.innerHTML = `<i class="fa-solid fa-question text-purple-300"></i>`;
        card.onclick = () => flipMemoryCard(idx, icon);
        grid.appendChild(card);
      });
    }

    function flipMemoryCard(idx, icon) {
      if (appState.flippedCards.length >= 2) return;
      const card = document.getElementById(`mem-card-${idx}`);
      if (card.classList.contains('matched')) return;

      card.innerHTML = `<i class="fa-solid ${icon}"></i>`;
      card.className = "h-20 bg-purple-500 border-4 border-purple-400 rounded-2xl flex items-center justify-center text-3xl text-white shadow-md transition";

      appState.flippedCards.push({ idx, icon });

      if (appState.flippedCards.length === 2) {
        const [c1, c2] = appState.flippedCards;
        if (c1.icon === c2.icon && c1.idx !== c2.idx) {
          playSuccessSound();
          document.getElementById(`mem-card-${c1.idx}`).className = "h-20 bg-green-500 border-4 border-green-400 rounded-2xl flex items-center justify-center text-3xl text-white matched shadow-md";
          document.getElementById(`mem-card-${c2.idx}`).className = "h-20 bg-green-500 border-4 border-green-400 rounded-2xl flex items-center justify-center text-3xl text-white matched shadow-md";
          appState.matchedPairs++;
          appState.flippedCards = [];

          if (appState.matchedPairs === 4) {
            markModuleComplete('mod3');
            document.getElementById('memory-finish-msg').classList.remove('hidden');
          }
        } else {
          playErrorSound();
          setTimeout(() => {
            document.getElementById(`mem-card-${c1.idx}`).className = "h-20 bg-purple-100 border-2 border-purple-300 rounded-2xl flex items-center justify-center text-3xl text-purple-600 cursor-pointer hover:border-purple-500 hover:scale-105 transition shadow-sm";
            document.getElementById(`mem-card-${c2.idx}`).className = "h-20 bg-purple-100 border-2 border-purple-300 rounded-2xl flex items-center justify-center text-3xl text-purple-600 cursor-pointer hover:border-purple-500 hover:scale-105 transition shadow-sm";
            document.getElementById(`mem-card-${c1.idx}`).innerHTML = `<i class="fa-solid fa-question text-purple-300"></i>`;
            document.getElementById(`mem-card-${c2.idx}`).innerHTML = `<i class="fa-solid fa-question text-purple-300"></i>`;
            appState.flippedCards = [];
          }, 900);
        }
      }
    }

    // Module 4 Complete
    function completeMotorModule() {
      markModuleComplete('mod4');
      playSuccessSound();
      showToast('¡Circuito motor completado! +1 Estrella ⭐', 'success');
    }

    // Module 5 Hand Washing Timer
    function startHandTimer() {
      let sec = 20;
      const display = document.getElementById('hand-timer-display');
      display.textContent = `${sec}s`;

      const timer = setInterval(() => {
        sec--;
        display.textContent = `${sec}s`;
        if (sec <= 0) {
          clearInterval(timer);
          playSuccessSound();
          markModuleComplete('mod5');
          showToast('¡Lavado de manos de 20s completado! +1 Estrella ⭐', 'success');
        }
      }, 1000);
    }

    // Helpers
    function openCertificateModal() {
      document.getElementById('cert-modal').classList.remove('hidden');
    }

    function closeCertificateModal() {
      document.getElementById('cert-modal').classList.add('hidden');
    }

    function showToast(msg, type = 'info') {
      const box = document.getElementById('toast-box');
      const icon = document.getElementById('toast-icon');
      const msgEl = document.getElementById('toast-msg');

      msgEl.textContent = msg;
      
      if (type === 'success') {
        icon.className = 'fa-solid fa-circle-check text-green-500 text-2xl';
        box.style.borderColor = '#10b981';
      } else {
        icon.className = 'fa-solid fa-circle-info text-pink-500 text-2xl';
        box.style.borderColor = '#ec4899';
      }

      box.classList.remove('hidden');
      setTimeout(() => box.classList.add('hidden'), 3500);
    }

    function resetAllProgress() {
      appState.stars = 0;
      appState.quizStep = 0;
      Object.keys(appState.completedModules).forEach(k => appState.completedModules[k] = false);
      document.getElementById('star-count-display').textContent = '0';
      renderQuizQuestion();
      setupMemoryGame();
      updateDashboardUI();
      showToast('Progreso e historial reiniciados.', 'info');
    }
  </script>
</body>
</html>
