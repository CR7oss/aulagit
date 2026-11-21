<!DOCTYPE html>
<html lang="pt-BR" data-theme="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Klick Kwiz · Banco de Questões Vestibulares</title>
  <!-- Tailwind CSS via CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Google Fonts: Inter & Plus Jakarta Sans -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&family=Plus+Jakarta+Sans:wght@700;800&display=swap" rel="stylesheet" />
  <style>
    /* Variáveis de Design Tokens (Tema Dark Navy) */
    :root {
      --bg: #060E2B;
      --surface: #10183F;
      --surface-2: #162256;
      --text: #F1F5F9;
      --text-muted: #94A3B8;
      --border: rgba(96, 165, 250, 0.18);
      --border-subtle: rgba(255, 255, 255, 0.07);
      --shadow: 0 16px 40px -12px rgba(2, 6, 23, 0.85);
      --success: #10B981;
      --danger: #EF4444;
      --warning: #F59E0B;
    }

    * {
      box-sizing: border-box;
    }

    body {
      font-family: 'Inter', system-ui, -apple-system, sans-serif;
      background-color: var(--bg);
      color: var(--text);
      min-height: 100vh;
      overflow-x: hidden;
      background-image: 
        radial-gradient(circle at 10% 15%, rgba(124, 58, 237, 0.12), transparent 45%),
        radial-gradient(circle at 90% 85%, rgba(37, 99, 235, 0.12), transparent 50%);
      background-attachment: fixed;
    }

    .font-brand {
      font-family: 'Plus Jakarta Sans', 'Inter', sans-serif;
    }

    /* Superfícies & Glassmorphism Refinado */
    .kk-card {
      background: linear-gradient(135deg, rgba(16, 24, 63, 0.85) 0%, rgba(11, 22, 53, 0.9) 100%);
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      border: 1px solid var(--border);
      border-radius: 18px;
      box-shadow: var(--shadow);
      transition: border-color 0.25s ease, transform 0.25s ease;
    }
    .kk-card:hover {
      border-color: rgba(59, 130, 246, 0.35);
    }

    .kk-card-subtle {
      background: rgba(22, 34, 86, 0.5);
      border: 1px solid var(--border-subtle);
      border-radius: 14px;
      backdrop-filter: blur(8px);
    }

    .kk-topbar {
      background: rgba(6, 14, 43, 0.82);
      border-bottom: 1px solid var(--border);
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
    }

    /* Botões */
    .kk-btn-primary {
      background: linear-gradient(135deg, #3B82F6 0%, #2563EB 50%, #1D4ED8 100%);
      color: #FFFFFF;
      font-weight: 600;
      border-radius: 12px;
      transition: all 0.22s ease;
      box-shadow: 0 4px 18px rgba(37, 99, 235, 0.4);
    }
    .kk-btn-primary:hover:not(:disabled) {
      background: linear-gradient(135deg, #60A5FA 0%, #3B82F6 50%, #2563EB 100%);
      transform: translateY(-1.5px);
      box-shadow: 0 6px 22px rgba(59, 130, 246, 0.55);
    }
    .kk-btn-primary:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }

    .kk-btn-amber {
      background: linear-gradient(135deg, #F59E0B 0%, #D97706 100%);
      color: #060E2B;
      font-weight: 800;
      border-radius: 12px;
      transition: all 0.22s ease;
      box-shadow: 0 4px 18px rgba(245, 158, 11, 0.35);
    }
    .kk-btn-amber:hover {
      filter: brightness(1.1);
      transform: translateY(-1.5px);
      box-shadow: 0 6px 22px rgba(245, 158, 11, 0.5);
    }

    .kk-btn-outline {
      background: rgba(22, 34, 86, 0.6);
      color: var(--text);
      border: 1px solid var(--border);
      font-weight: 600;
      border-radius: 12px;
      transition: all 0.22s ease;
    }
    .kk-btn-outline:hover:not(:disabled) {
      border-color: #60A5FA;
      background: rgba(59, 130, 246, 0.15);
    }

    /* Inputs e Selects */
    .kk-input {
      background: rgba(16, 24, 63, 0.7);
      border: 1px solid var(--border);
      color: var(--text);
      border-radius: 12px;
      padding: 0.65rem 0.95rem;
      font-size: 0.9rem;
      transition: border-color 0.2s, box-shadow 0.2s;
      width: 100%;
    }
    .kk-input:focus {
      outline: none;
      border-color: #3B82F6;
      box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.28);
    }

    /* Navegação SPA */
    .view-section {
      display: none;
      animation: fadeSlideUp 0.35s cubic-bezier(0.16, 1, 0.3, 1);
    }
    .view-section.active {
      display: block;
    }
    @keyframes fadeSlideUp {
      from { opacity: 0; transform: translateY(12px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .nav-link {
      color: var(--text-muted);
      font-weight: 600;
      font-size: 0.875rem;
      padding: 0.5rem 0.8rem;
      border-radius: 10px;
      transition: all 0.2s ease;
      white-space: nowrap;
    }
    .nav-link:hover {
      color: var(--text);
      background: rgba(59, 130, 246, 0.12);
    }
    .nav-link.active {
      background: linear-gradient(135deg, #2563EB 0%, #1D4ED8 100%);
      color: #FFFFFF;
      box-shadow: 0 3px 12px rgba(37, 99, 235, 0.45);
    }

    /* Hero Brand Navy Fiel */
    .brand-hero {
      background:
        radial-gradient(circle at 50% 20%, rgba(245, 158, 11, 0.18) 0%, transparent 45%),
        radial-gradient(circle at 15% 85%, rgba(124, 58, 237, 0.22) 0%, transparent 55%),
        radial-gradient(circle at 85% 80%, rgba(37, 99, 235, 0.25) 0%, transparent 50%),
        linear-gradient(160deg, #070F30 0%, #0C1A52 55%, #070F30 100%);
      border: 1px solid rgba(96, 165, 250, 0.28);
      color: #FFFFFF;
    }

    /* Alternativas de Questão */
    .alt-option {
      display: flex;
      align-items: flex-start;
      gap: 0.9rem;
      width: 100%;
      text-align: left;
      padding: 1rem 1.1rem;
      border-radius: 14px;
      border: 1.5px solid var(--border);
      background: rgba(22, 34, 86, 0.45);
      color: var(--text);
      transition: all 0.2s ease;
      position: relative;
    }
    .alt-option:hover:not(.locked) {
      border-color: #60A5FA;
      background: rgba(59, 130, 246, 0.1);
      transform: translateX(2px);
    }
    .alt-option.selected {
      border-color: #3B82F6;
      background: rgba(37, 99, 235, 0.2);
      box-shadow: 0 0 0 1px #3B82F6;
    }
    .alt-option.correct {
      border-color: #10B981 !important;
      background: rgba(16, 185, 129, 0.18) !important;
    }
    .alt-option.wrong {
      border-color: #EF4444 !important;
      background: rgba(239, 68, 68, 0.16) !important;
    }
    .alt-option.eliminated {
      opacity: 0.4;
      text-decoration: line-through;
    }
    .alt-key {
      flex-shrink: 0;
      width: 30px;
      height: 30px;
      border-radius: 9px;
      display: grid;
      place-items: center;
      font-weight: 800;
      font-size: 0.8rem;
      background: rgba(11, 22, 53, 0.8);
      border: 1px solid var(--border);
      color: var(--text-muted);
      transition: all 0.2s ease;
    }
    .alt-option.selected .alt-key {
      background: #2563EB;
      color: #FFFFFF;
      border-color: #2563EB;
    }
    .alt-option.correct .alt-key {
      background: #10B981;
      color: #FFFFFF;
      border-color: #10B981;
    }
    .alt-option.wrong .alt-key {
      background: #EF4444;
      color: #FFFFFF;
      border-color: #EF4444;
    }

    @keyframes pulseRays {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.72; transform: scale(1.04); }
    }
    .bulb-rays {
      transform-origin: 60px 38px;
      animation: pulseRays 3s ease-in-out infinite;
    }

    ::-webkit-scrollbar { width: 8px; height: 8px; }
    ::-webkit-scrollbar-track { background: var(--bg); }
    ::-webkit-scrollbar-thumb { background: rgba(59, 130, 246, 0.4); border-radius: 99px; }
    ::-webkit-scrollbar-thumb:hover { background: rgba(59, 130, 246, 0.65); }
  </style>
</head>
<body class="antialiased selection:bg-blue-600 selection:text-white">

  <!-- ==================== TOPBAR FIXA ==================== -->
  <header class="kk-topbar sticky top-0 z-40">
    <div class="max-w-7xl mx-auto px-4 py-3 flex items-center justify-between gap-3">
      <!-- Marca / Logo Compacto -->
      <button type="button" data-nav="inicio" class="flex items-center gap-2.5 text-left group focus:outline-none">
        <div class="w-10 h-10 rounded-xl bg-[#060E2B] border border-blue-500/40 flex items-center justify-center shadow-md group-hover:scale-105 transition-transform">
          <svg viewBox="0 0 120 110" class="w-8 h-8" fill="none" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
            <g stroke="#F59E0B" stroke-width="5" stroke-linecap="round">
              <line x1="60" y1="6" x2="60" y2="14" />
              <line x1="32" y1="15" x2="38" y2="21" />
              <line x1="88" y1="15" x2="82" y2="21" />
            </g>
            <g stroke="#38BDF8" stroke-width="4.5" stroke-linecap="round">
              <line x1="21" y1="33" x2="29" y2="35" />
              <line x1="99" y1="33" x2="91" y2="35" />
            </g>
            <path d="M60 19C46.5 19 37 28.5 37 40.5C37 48.2 41.2 54.4 46.5 58.6C48.3 60 49.5 62.1 49.8 64.4H70.2C70.5 62.1 71.7 60 73.5 58.6C78.8 54.4 83 48.2 83 40.5C83 28.5 73.5 19 60 19Z" stroke="#F59E0B" stroke-width="6" fill="#060E2B"/>
            <polygon points="55,33 55,49 69,41" fill="#F59E0B" />
            <rect x="49" y="67" width="22" height="5.5" rx="2.7" fill="#3B82F6" />
            <rect x="52" y="74.5" width="16" height="5" rx="2.5" fill="#2563EB" />
            <path d="M16 66C31 63 46 68 60 80C74 68 89 63 104 66V87C89 84 74 89 60 99C46 89 31 84 16 87V66Z" fill="#FFFFFF" />
            <path d="M10 73L16 70V87C31 84 46 89 60 99C74 89 89 84 104 87V70L110 73V94C93 90 76 94 60 104C44 94 27 90 10 94V73Z" fill="#2563EB" />
          </svg>
        </div>
        <div>
          <div class="font-brand font-extrabold text-lg leading-none tracking-tight flex items-center">
            <span>kl</span><span class="relative inline-block">i<span class="absolute -top-0.5 left-1/2 -translate-x-1/2 w-1.5 h-1.5 rounded-full bg-amber-500"></span></span><span>ck</span>
            <span class="text-blue-500 ml-1">kwiz</span>
          </div>
          <div class="text-[11px] font-semibold tracking-wider uppercase opacity-70 mt-0.5">Vestibulares</div>
        </div>
      </button>

      <!-- Navegação Desktop -->
      <nav class="hidden lg:flex items-center gap-1" aria-label="Navegação principal">
        <button type="button" class="nav-link active" data-nav="inicio">Início</button>
        <button type="button" class="nav-link" data-nav="treinar">Treinar</button>
        <button type="button" class="nav-link" data-nav="simulado">Simulado</button>
        <button type="button" class="nav-link" data-nav="roteiro">Roteiro</button>
        <button type="button" class="nav-link" data-nav="desempenho">Desempenho</button>
        <button type="button" class="nav-link" data-nav="admin">Admin</button>
        <button type="button" class="nav-link" data-nav="sobre">Sobre</button>
      </nav>

      <!-- Ofensiva + Usuário -->
      <div class="flex items-center gap-2.5">
        <div class="hidden sm:flex items-center gap-2 px-3 py-1.5 rounded-xl kk-card-subtle text-xs font-bold" title="Sua ofensiva de dias consecutivos de estudo">
          <span class="flex items-center gap-1 text-amber-500">🔥 <span id="topStreak">1</span>d</span>
        </div>

        <!-- Área do Usuário (Visitante) -->
        <div id="guestControls" class="flex items-center gap-2">
          <button type="button" id="btnOpenAuth" class="kk-btn-primary px-4 py-2 text-xs sm:text-sm">Entrar</button>
        </div>

        <!-- Área do Usuário (Logado) -->
        <div id="userControls" class="hidden items-center gap-2">
          <button type="button" data-nav="perfil" class="flex items-center gap-2 pl-1.5 pr-3 py-1 rounded-full kk-card-subtle hover:border-blue-500 transition-all">
            <span id="topAvatar" class="w-7 h-7 rounded-full bg-gradient-to-br from-blue-600 to-amber-500 text-white font-extrabold text-xs grid place-items-center">KV</span>
            <span id="topUserName" class="text-xs font-bold max-w-[90px] truncate hidden md:inline">Estudante</span>
          </button>
          <button type="button" id="btnLogout" class="text-xs font-semibold px-2.5 py-1.5 rounded-lg opacity-75 hover:opacity-100 hover:bg-red-500/15 hover:text-red-400 transition-colors">Sair</button>
        </div>

        <!-- Menu Mobile Trigger -->
        <button type="button" id="btnMobileMenu" class="lg:hidden p-2.5 rounded-xl kk-card-subtle text-base" aria-label="Abrir menu">
          ☰
        </button>
      </div>
    </div>

    <!-- Navegação Mobile Dropdown -->
    <div id="mobileMenuDrawer" class="hidden lg:hidden border-t border-blue-500/20 px-4 py-3 space-y-1 bg-[#060E2B]/95 text-white">
      <div class="grid grid-cols-2 gap-1.5">
        <button type="button" class="nav-link text-left active" data-nav="inicio">🏠 Início</button>
        <button type="button" class="nav-link text-left" data-nav="treinar">🧩 Treinar Questões</button>
        <button type="button" class="nav-link text-left" data-nav="simulado">⏱️ Simulado</button>
        <button type="button" class="nav-link text-left" data-nav="roteiro">📅 Roteiro</button>
        <button type="button" class="nav-link text-left" data-nav="desempenho">📊 Desempenho</button>
        <button type="button" class="nav-link text-left" data-nav="perfil">👤 Meu Perfil</button>
        <button type="button" class="nav-link text-left" data-nav="admin">⚙️ Admin Questões</button>
        <button type="button" class="nav-link text-left" data-nav="sobre">ℹ️ Sobre & Contato</button>
      </div>
    </div>
  </header>

  <!-- ==================== CONTEÚDO PRINCIPAL SPA ==================== -->
  <main class="max-w-7xl mx-auto px-4 py-6 sm:py-8">

    <!-- ==================== 1. VIEW: INÍCIO ==================== -->
    <section id="view-inicio" class="view-section active space-y-8">
      <!-- Banner Boas-Vindas Personalizado (se logado) -->
      <div id="homeUserBanner" class="hidden kk-card p-4 sm:p-5 flex flex-wrap items-center justify-between gap-4 border-l-4 border-l-amber-500">
        <div class="flex items-center gap-3.5">
          <div id="bannerAvatar" class="w-11 h-11 rounded-full bg-gradient-to-br from-blue-600 to-amber-500 text-white font-extrabold text-base grid place-items-center shadow">KV</div>
          <div>
            <h2 id="bannerTitle" class="font-bold text-sm sm:text-base">Bem-vindo de volta!</h2>
            <p id="bannerSubtitle" class="text-xs opacity-75">Meta diária: 0/10 questões concluídas hoje.</p>
          </div>
        </div>
        <div class="flex items-center gap-2">
          <button type="button" data-nav="perfil" class="kk-btn-outline px-3 py-1.5 text-xs">Ajustar Meta</button>
          <button type="button" data-nav="treinar" class="kk-btn-primary px-3.5 py-1.5 text-xs">Continuar Treino →</button>
        </div>
      </div>

      <!-- HERO OFICIAL COM IDENTIDADE DO LOGOTIPO -->
      <div class="brand-hero rounded-3xl p-7 sm:p-12 text-center shadow-2xl relative overflow-hidden">
        <!-- Logotipo Vetorial Completo -->
        <div class="max-w-md mx-auto flex flex-col items-center">
          <svg viewBox="0 0 260 155" class="w-52 sm:w-64 h-auto drop-shadow-[0_10px_25px_rgba(245,158,11,0.22)]" fill="none" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Logotipo Klick Kwiz">
            <!-- Raios superiores laranjas -->
            <g class="bulb-rays" stroke="#F59E0B" stroke-width="5.5" stroke-linecap="round">
              <line x1="130" y1="8" x2="130" y2="21" />
              <line x1="94" y1="21" x2="103" y2="30" />
              <line x1="166" y1="21" x2="157" y2="30" />
            </g>
            <!-- Raios laterais azul-celeste -->
            <g stroke="#38BDF8" stroke-width="5" stroke-linecap="round">
              <line x1="80" y1="48" x2="92" y2="51" />
              <line x1="180" y1="48" x2="168" y2="51" />
            </g>
            <!-- Bulbo da Lâmpada Laranja -->
            <path d="M130 28C111.5 28 98 41 98 58C98 68.5 103.6 77.2 111 83C113.5 85 115 88 115.5 91.2H144.5C145 88 146.5 85 149 83C156.4 77.2 162 68.5 162 58C162 41 148.5 28 130 28Z" stroke="#F59E0B" stroke-width="6.5" fill="#060E2B"/>
            <!-- Triângulo Play no centro da Lâmpada -->
            <polygon points="122,47 122,70 143,58.5" fill="#F59E0B" />
            <!-- Rosca/Base azul da lâmpada -->
            <rect x="114" y="95" width="32" height="6.5" rx="3.2" fill="#3B82F6" />
            <rect x="118" y="104.5" width="24" height="6" rx="3" fill="#2563EB" />
            <!-- Páginas Brancas do Livro Aberto -->
            <path d="M68 95C89 91 110 98 130 114C150 98 171 91 192 95V125C171 121 150 128 130 142C110 128 89 121 68 125V95Z" fill="#FFFFFF" />
            <!-- Vinco central sutil do livro -->
            <path d="M130 114V142" stroke="#CBD5E1" stroke-width="2" />
            <!-- Capa Azul do Livro Aberto -->
            <path d="M58 104L68 100V125C89 121 110 128 130 142C150 128 171 121 192 125V100L202 104V135C178 130 153 136 130 150C107 136 82 130 58 135V104Z" fill="#2563EB" />
          </svg>

          <!-- Tipografia Klick Kwiz -->
          <h1 class="font-brand font-extrabold text-4xl sm:text-6xl tracking-tight mt-1 leading-none select-none">
            <span class="text-white">kl</span><span class="relative inline-block text-white">ı<span class="absolute -top-0.5 sm:top-0 left-1/2 -translate-x-1/2 w-2.5 h-2.5 sm:w-3.5 sm:h-3.5 rounded-full bg-amber-500"></span></span><span class="text-white">ck</span>
            <span class="text-[#3B82F6] ml-1.5">kwiz</span>
          </h1>

          <!-- Slogan com pontos laranjas -->
          <div class="flex items-center justify-center gap-2.5 sm:gap-3.5 text-xs sm:text-sm font-extrabold tracking-[0.22em] text-white/95 uppercase mt-3">
            <span>ESTUDE</span>
            <span class="w-1.5 h-1.5 rounded-full bg-amber-500"></span>
            <span>PRATIQUE</span>
            <span class="w-1.5 h-1.5 rounded-full bg-amber-500"></span>
            <span>EVOLUA</span>
          </div>

          <!-- Pílula Azul do Logotipo -->
          <div class="mt-3.5 inline-block bg-[#2563EB] text-white font-extrabold text-[11px] sm:text-xs tracking-[0.14em] uppercase px-5 py-1.5 rounded-full shadow-lg shadow-blue-600/40">
            APRENDER NUNCA FOI TÃO FÁCIL
          </div>
        </div>

        <p class="text-slate-300 text-sm sm:text-base max-w-2xl mx-auto mt-6 leading-relaxed">
          Prepare-se com questões oficiais e inéditas no estilo exato dos <strong class="text-white">Vestibulares</strong>. Treine por disciplina, simule o tempo real de prova e domine os temas de peso 2 do seu curso.
        </p>

        <!-- Botões de Ação Principais -->
        <div class="flex flex-wrap items-center justify-center gap-3 mt-7">
          <button type="button" data-nav="treinar" class="kk-btn-amber px-6 py-3 text-sm sm:text-base flex items-center gap-2">
            <span>🚀 Começar a Treinar</span>
          </button>
          <button type="button" data-nav="simulado" class="kk-btn-primary px-6 py-3 text-sm sm:text-base flex items-center gap-2">
            <span>⏱️ Simulado Cronometrado</span>
          </button>
          <button type="button" data-nav="roteiro" class="px-5 py-3 rounded-xl border border-blue-400/40 bg-white/5 hover:bg-white/10 text-white font-semibold text-sm sm:text-base transition-all">
            📅 Roteiro 8 Semanas
          </button>
        </div>

        <!-- Barra de Métricas Rápidas -->
        <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 max-w-3xl mx-auto mt-8 pt-6 border-t border-white/10 text-left sm:text-center">
          <div class="bg-white/5 rounded-xl p-3">
            <div class="text-xl sm:text-2xl font-extrabold text-amber-400" id="heroTotalQuestions">24</div>
            <div class="text-xs text-slate-300">Questões Comentadas</div>
          </div>
          <div class="bg-white/5 rounded-xl p-3">
            <div class="text-xl sm:text-2xl font-extrabold text-blue-400">9 Áreas</div>
            <div class="text-xs text-slate-300">Matriz Completa Vestibulares</div>
          </div>
          <div class="bg-white/5 rounded-xl p-3">
            <div class="text-xl sm:text-2xl font-extrabold text-emerald-400" id="heroUserSolved">0</div>
            <div class="text-xs text-slate-300">Resolvidas por Você</div>
          </div>
          <div class="bg-white/5 rounded-xl p-3">
            <div class="text-xl sm:text-2xl font-extrabold text-purple-400" id="heroUserAccuracy">0%</div>
            <div class="text-xs text-slate-300">Seu Aproveitamento</div>
          </div>
        </div>
      </div>

      <!-- 4 PILARES DO MÉTODO KLICK KWIZ -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <div class="kk-card p-5 hover:border-blue-500">
          <div class="w-11 h-11 rounded-xl bg-blue-500/15 text-blue-400 grid place-items-center text-xl mb-3">🧩</div>
          <h3 class="font-bold text-base mb-1">Treino Inteligente</h3>
          <p class="text-xs opacity-75 leading-relaxed">Filtre por matéria, edição da prova de Vestibulares, dificuldade e elimine alternativas descartadas como na prova real.</p>
        </div>
        <div class="kk-card p-5 hover:border-amber-500">
          <div class="w-11 h-11 rounded-xl bg-amber-500/15 text-amber-400 grid place-items-center text-xl mb-3">🔁</div>
          <h3 class="font-bold text-base mb-1">Caderno de Erros</h3>
          <p class="text-xs opacity-75 leading-relaxed">Cada questão errada fica salva automaticamente para você revisar e transformar pontos fracos em acertos.</p>
        </div>
        <div class="kk-card p-5 hover:border-emerald-500">
          <div class="w-11 h-11 rounded-xl bg-emerald-500/15 text-emerald-400 grid place-items-center text-xl mb-3">⏱️</div>
          <h3 class="font-bold text-base mb-1">Simulados Reais</h3>
          <p class="text-xs opacity-75 leading-relaxed">Cronômetro regressivo, cartão de navegação rápida e diagnóstico detalhado de desempenho por disciplina.</p>
        </div>
        <div class="kk-card p-5 hover:border-purple-500">
          <div class="w-11 h-11 rounded-xl bg-purple-500/15 text-purple-400 grid place-items-center text-xl mb-3">📊</div>
          <h3 class="font-bold text-base mb-1">Evolução & Analytics</h3>
          <p class="text-xs opacity-75 leading-relaxed">Acompanhe gráficos detalhados por matéria, identifique seus pontos fortes e monitore o histórico dos seus simulados.</p>
        </div>
      </div>

      <!-- MINI QUIZ DE BOAS-VINDAS INTERATIVO -->
      <div class="kk-card p-6 sm:p-8">
        <div class="flex flex-wrap items-center justify-between gap-3 pb-5 border-b border-blue-500/15">
          <div>
            <span class="inline-block text-[11px] font-bold uppercase tracking-wider px-2.5 py-0.5 rounded-full bg-amber-500/15 text-amber-400 mb-1">Desafio Rápido · 4 Questões</span>
            <h2 class="text-xl sm:text-2xl font-extrabold">Mini Quiz Diagnóstico Vestibulares</h2>
            <p class="text-xs sm:text-sm opacity-75 mt-0.5">Teste seus conhecimentos agora mesmo e receba gabarito comentado instantâneo.</p>
          </div>
          <div class="flex items-center gap-2">
            <span id="miniQuizBadge" class="px-3 py-1 rounded-full text-xs font-bold kk-card-subtle">Questão 1 de 4</span>
            <button type="button" id="btnResetMiniQuiz" class="kk-btn-outline px-3 py-1.5 text-xs">Reiniciar</button>
          </div>
        </div>
        <div id="miniQuizContainer" class="mt-6"></div>
      </div>
    </section>

    <!-- ==================== 2. VIEW: TREINAR (BANCO DE QUESTÕES) ==================== -->
    <section id="view-treinar" class="view-section space-y-6">
      <div class="flex flex-wrap items-center justify-between gap-4">
        <div>
          <h1 class="text-2xl sm:text-3xl font-extrabold">Banco de Questões Vestibulares</h1>
          <p class="text-xs sm:text-sm opacity-75 mt-0.5">Use <kbd class="px-1.5 py-0.5 rounded bg-blue-500/15 text-blue-400 font-mono text-xs">A–E</kbd> para responder, <kbd class="px-1.5 py-0.5 rounded bg-blue-500/15 text-blue-400 font-mono text-xs">Enter</kbd> para confirmar ou clique na tesoura ✂️ para riscar alternativas.</p>
        </div>
        <div class="flex items-center gap-2">
          <span id="trainFilteredCount" class="px-3 py-1.5 rounded-xl kk-card-subtle text-xs font-bold text-blue-400">24 questões</span>
        </div>
      </div>

      <!-- Barra de Filtros -->
      <div class="kk-card p-4 sm:p-5 space-y-3">
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-5 gap-3">
          <div>
            <label class="block text-xs font-bold opacity-75 mb-1">Matéria</label>
            <select id="filterSubject" class="kk-input">
              <option value="ALL">Todas as Matérias</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-bold opacity-75 mb-1">Edição / Prova</label>
            <select id="filterExam" class="kk-input">
              <option value="ALL">Todas as Provas</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-bold opacity-75 mb-1">Dificuldade</label>
            <select id="filterDifficulty" class="kk-input">
              <option value="ALL">Todas</option>
              <option value="Fácil">Fácil</option>
              <option value="Média">Média</option>
              <option value="Difícil">Difícil</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-bold opacity-75 mb-1">Status de Estudo</label>
            <select id="filterStatus" class="kk-input">
              <option value="ALL">Todas as Questões</option>
              <option value="UNANSWERED">Não Respondidas</option>
              <option value="WRONG">Somente Erradas (Revisão)</option>
              <option value="FAVORITE">Favoritadas ⭐</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-bold opacity-75 mb-1">Buscar palavra-chave</label>
            <input type="text" id="filterSearch" class="kk-input" placeholder="Ex: Vargas, algoritmo, clima..." />
          </div>
        </div>
        <div class="flex flex-wrap items-center justify-between gap-2 pt-2 border-t border-blue-500/10 text-xs">
          <div class="flex flex-wrap items-center gap-2">
            <button type="button" id="btnQuickReviewWrong" class="px-3 py-1.5 rounded-lg bg-red-500/15 text-red-400 font-bold hover:bg-red-500/25 transition-colors">
              🔁 Revisar Meus Erros (<span id="wrongCountBadge">0</span>)
            </button>
            <button type="button" id="btnClearFilters" class="px-3 py-1.5 rounded-lg kk-card-subtle font-semibold hover:border-blue-500">
              Limpar Filtros
            </button>
          </div>
          <div class="flex items-center gap-2">
            <label class="flex items-center gap-1.5 cursor-pointer font-semibold">
              <input type="checkbox" id="chkAutoAdvance" class="accent-blue-600 w-4 h-4" />
              <span>Avançar automaticamente ao acertar</span>
            </label>
          </div>
        </div>
      </div>

      <!-- Área Principal da Questão -->
      <div id="trainQuestionStage" class="kk-card p-5 sm:p-7"></div>
    </section>

    <!-- ==================== 3. VIEW: SIMULADO CRONOMETRADO ==================== -->
    <section id="view-simulado" class="view-section space-y-6">
      <!-- Tela 1: Configuração do Simulado -->
      <div id="simConfigPanel" class="kk-card p-6 sm:p-8 max-w-4xl mx-auto space-y-6">
        <div class="flex items-start justify-between gap-4 border-b border-blue-500/15 pb-5">
          <div>
            <span class="inline-block text-[11px] font-bold uppercase tracking-wider px-2.5 py-0.5 rounded-full bg-blue-500/15 text-blue-400 mb-1">Modo Prova Real</span>
            <h1 class="text-2xl sm:text-3xl font-extrabold">Simulado Cronometrado Vestibulares</h1>
            <p class="text-xs sm:text-sm opacity-75 mt-1">Treine gestão de tempo e resistência mental. O resultado detalhado com gabarito comentado é liberado ao finalizar.</p>
          </div>
          <div class="text-3xl hidden sm:block">⏱️</div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
          <label class="kk-card-subtle p-4 cursor-pointer border-2 border-blue-500 bg-blue-500/10 transition-all sim-mode-card" data-mode="express">
            <input type="radio" name="simMode" value="express" checked class="hidden" />
            <div class="flex items-center justify-between mb-1">
              <span class="font-extrabold text-base">⚡ Simulado Express</span>
              <span class="text-xs font-bold px-2 py-0.5 rounded bg-amber-500 text-slate-950">Recomendado</span>
            </div>
            <div class="text-2xl font-black text-blue-400 my-1">10 Questões</div>
            <p class="text-xs opacity-75">Tempo limite: <strong>25 minutos</strong> (~2,5 min por questão).</p>
          </label>

          <label class="kk-card-subtle p-4 cursor-pointer border-2 border-transparent hover:border-blue-500/50 transition-all sim-mode-card" data-mode="medio">
            <input type="radio" name="simMode" value="medio" class="hidden" />
            <div class="flex items-center justify-between mb-1">
              <span class="font-extrabold text-base">🎯 Meio Simulado</span>
            </div>
            <div class="text-2xl font-black text-blue-400 my-1">18 Questões</div>
            <p class="text-xs opacity-75">Tempo limite: <strong>45 minutos</strong>. Cobertura multidisciplinar.</p>
          </label>

          <label class="kk-card-subtle p-4 cursor-pointer border-2 border-transparent hover:border-blue-500/50 transition-all sim-mode-card" data-mode="completo">
            <input type="radio" name="simMode" value="completo" class="hidden" />
            <div class="flex items-center justify-between mb-1">
              <span class="font-extrabold text-base">🏆 Maratona Completa</span>
            </div>
            <div class="text-2xl font-black text-blue-400 my-1" id="simAllCountLabel">24 Questões</div>
            <p class="text-xs opacity-75">Tempo limite: <strong>60 minutos</strong>. Todo o banco disponível.</p>
          </label>
        </div>

        <div>
          <label class="block text-xs font-bold uppercase tracking-wider opacity-75 mb-2">Filtrar área específica (opcional)</label>
          <select id="simSubjectFilter" class="kk-input max-w-md">
            <option value="ALL">Todas as Matérias (Prova Geral)</option>
          </select>
        </div>

        <div class="flex flex-wrap items-center justify-between gap-4 pt-4 border-t border-blue-500/15">
          <div class="text-xs opacity-75">
            💡 <strong>Dica de Prova:</strong> Na prova oficial, disciplinas com <strong>Peso 2</strong> variam conforme o curso escolhido.
          </div>
          <button type="button" id="btnStartSimulado" class="kk-btn-amber px-7 py-3 text-sm sm:text-base">
            Iniciar Simulado Agora →
          </button>
        </div>
      </div>

      <!-- Tela 2: Execução da Prova -->
      <div id="simActivePanel" class="hidden grid grid-cols-1 lg:grid-cols-12 gap-6">
        <div class="lg:col-span-8 kk-card p-5 sm:p-7 space-y-5" id="simQuestionBox"></div>

        <aside class="lg:col-span-4 space-y-4">
          <div class="kk-card p-5 sticky top-20 space-y-4">
            <div class="flex items-center justify-between pb-3 border-b border-blue-500/15">
              <div>
                <div class="text-[11px] font-bold uppercase opacity-70">Tempo Restante</div>
                <div id="simTimerDisplay" class="text-3xl font-black font-mono text-amber-400">25:00</div>
              </div>
              <div class="text-right">
                <div class="text-[11px] font-bold uppercase opacity-70">Respondidas</div>
                <div id="simProgressText" class="text-lg font-extrabold text-blue-400">0 / 10</div>
              </div>
            </div>

            <div>
              <div class="text-xs font-bold opacity-75 mb-2">Cartão de Respostas</div>
              <div id="simGridNav" class="grid grid-cols-5 gap-2"></div>
              <div class="flex flex-wrap gap-3 text-[11px] opacity-75 mt-3">
                <span class="flex items-center gap-1"><span class="w-2.5 h-2.5 rounded-sm bg-blue-600 inline-block"></span> Respondida</span>
                <span class="flex items-center gap-1"><span class="w-2.5 h-2.5 rounded-sm bg-amber-500 inline-block"></span> Revisar</span>
                <span class="flex items-center gap-1"><span class="w-2.5 h-2.5 rounded-sm border border-blue-400 inline-block"></span> Atual</span>
              </div>
            </div>

            <div class="pt-3 border-t border-blue-500/15 flex flex-col gap-2">
              <button type="button" id="btnFinishSimulado" class="kk-btn-amber w-full py-2.5 text-sm">
                Finalizar e Corrigir Prova
              </button>
              <button type="button" id="btnAbortSimulado" class="kk-btn-outline w-full py-2 text-xs text-red-400">
                Cancelar Simulado
              </button>
            </div>
          </div>
        </aside>
      </div>

      <!-- Tela 3: Resultado Detalhado -->
      <div id="simResultPanel" class="hidden space-y-6"></div>
    </section>

    <!-- ==================== 4. VIEW: ROTEIRO DE ESTUDOS 8 SEMANAS ==================== -->
    <section id="view-roteiro" class="view-section space-y-6">
      <div class="kk-card p-6 sm:p-8 flex flex-col md:flex-row md:items-center justify-between gap-6">
        <div class="space-y-2 max-w-2xl">
          <span class="inline-block text-[11px] font-bold uppercase tracking-wider px-2.5 py-0.5 rounded-full bg-amber-500/15 text-amber-400">Planejamento Estratégico</span>
          <h1 class="text-2xl sm:text-3xl font-extrabold">Roteiro Intensivo Vestibulares · 8 Semanas</h1>
          <p class="text-xs sm:text-sm opacity-75">Cronograma focado nos tópicos que mais caem nos exames vestibulares, incluindo treino de Redação Dissertativa.</p>
        </div>
        <div class="kk-card-subtle p-4 min-w-[230px] space-y-2">
          <div class="flex items-center justify-between text-xs font-bold">
            <span>Progresso do Plano</span>
            <span id="roadmapPercentText" class="text-amber-400">0%</span>
          </div>
          <div class="w-full h-3 rounded-full bg-slate-800 overflow-hidden">
            <div id="roadmapProgressBar" class="h-full bg-gradient-to-r from-blue-600 to-amber-500 transition-all duration-500" style="width: 0%"></div>
          </div>
          <div class="text-[11px] opacity-70" id="roadmapCountText">0 de 16 metas concluídas</div>
        </div>
      </div>

      <!-- Dica de Pesos por Curso -->
      <div class="kk-card p-4 sm:p-5 border-l-4 border-l-blue-500 flex flex-wrap items-center justify-between gap-4">
        <div class="text-xs sm:text-sm">
          <strong>⚖️ Atenção aos Pesos Universitários:</strong> Cursos de <strong>TI (ADS, Ciência de Dados, Seg. Info)</strong> têm Peso 2 em <em>Matemática e Raciocínio Lógico</em>. Cursos de <strong>Gestão</strong> têm Peso 2 em <em>Matemática e Português</em>.
        </div>
        <button type="button" data-nav="treinar" class="kk-btn-primary px-4 py-2 text-xs">Praticar Questões Agora</button>
      </div>

      <!-- Grid das 8 Semanas -->
      <div id="roadmapContainer" class="grid grid-cols-1 md:grid-cols-2 gap-5"></div>
    </section>

    <!-- ==================== 5. VIEW: DESEMPENHO & ANALYTICS ==================== -->
    <section id="view-desempenho" class="view-section space-y-6">
      <div class="flex flex-wrap items-center justify-between gap-4">
        <div>
          <h1 class="text-2xl sm:text-3xl font-extrabold">Painel de Desempenho & Analytics</h1>
          <p class="text-xs sm:text-sm opacity-75 mt-0.5">Acompanhe sua evolução real, identifique gargalos e exporte seu relatório de estudos.</p>
        </div>
        <div class="flex items-center gap-2">
          <button type="button" id="btnExportStats" class="kk-btn-outline px-3.5 py-2 text-xs">📥 Exportar Relatório (.txt)</button>
          <button type="button" id="btnResetStats" class="px-3.5 py-2 rounded-xl bg-red-500/15 text-red-400 font-semibold text-xs hover:bg-red-500/25 transition-colors">🗑️ Zerar Histórico</button>
        </div>
      </div>

      <!-- KPIs -->
      <div class="grid grid-cols-2 lg:grid-cols-4 gap-4">
        <div class="kk-card p-5">
          <div class="text-xs font-bold uppercase opacity-70">Questões Resolvidas</div>
          <div id="kpiSolved" class="text-3xl font-black text-blue-400 mt-1">0</div>
          <div class="text-xs opacity-70 mt-1" id="kpiCorrectSub">0 acertos registrados</div>
        </div>
        <div class="kk-card p-5">
          <div class="text-xs font-bold uppercase opacity-70">Taxa de Acerto</div>
          <div id="kpiAccuracy" class="text-3xl font-black text-emerald-400 mt-1">0%</div>
          <div class="text-xs opacity-70 mt-1">Meta recomendada: ≥ 70%</div>
        </div>
        <div class="kk-card p-5">
          <div class="text-xs font-bold uppercase opacity-70">Melhor Matéria</div>
          <div id="kpiBestSubject" class="text-xl font-extrabold text-amber-400 mt-2 truncate">—</div>
          <div class="text-xs opacity-70 mt-1" id="kpiBestSubjectRate">Resolva questões para calcular</div>
        </div>
        <div class="kk-card p-5">
          <div class="text-xs font-bold uppercase opacity-70">Ofensiva & Simulados</div>
          <div id="kpiLevel" class="text-2xl font-black text-purple-400 mt-1">1 dia seguido</div>
          <div class="text-xs opacity-70 mt-1" id="kpiSimCountDetail">0 simulados concluídos</div>
        </div>
      </div>

      <!-- Gráfico por Matéria + Conquistas -->
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
        <div class="lg:col-span-7 kk-card p-6 space-y-4">
          <div class="flex items-center justify-between">
            <h2 class="text-lg font-extrabold">Aproveitamento por Matéria</h2>
            <span class="text-xs opacity-70">% de acertos</span>
          </div>
          <div id="subjectChartBars" class="space-y-3.5 pt-2"></div>
        </div>

        <div class="lg:col-span-5 kk-card p-6 space-y-4">
          <h2 class="text-lg font-extrabold">Medalhas & Conquistas</h2>
          <div id="badgesGrid" class="grid grid-cols-2 gap-3"></div>
        </div>
      </div>

      <!-- Histórico de Simulados -->
      <div class="kk-card p-6 space-y-4">
        <h2 class="text-lg font-extrabold">Histórico de Simulados Realizados</h2>
        <div class="overflow-x-auto">
          <table class="w-full text-left border-collapse text-sm">
            <thead>
              <tr class="border-b border-blue-500/20 text-xs uppercase opacity-70">
                <th class="py-2.5 pr-4">Data</th>
                <th class="py-2.5 pr-4">Modalidade</th>
                <th class="py-2.5 pr-4">Acertos</th>
                <th class="py-2.5 pr-4">Aproveitamento</th>
                <th class="py-2.5">Tempo</th>
              </tr>
            </thead>
            <tbody id="simHistoryBody"></tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- ==================== 6. VIEW: PERFIL DO ESTUDANTE ==================== -->
    <section id="view-perfil" class="view-section max-w-3xl mx-auto space-y-6">
      <div class="kk-card p-6 sm:p-8 space-y-6">
        <div class="flex flex-wrap items-center gap-4 pb-6 border-b border-blue-500/15">
          <div id="profileBigAvatar" class="w-16 h-16 rounded-2xl bg-gradient-to-br from-blue-600 to-amber-500 text-white font-black text-2xl grid place-items-center shadow-lg">KV</div>
          <div>
            <span class="text-xs font-bold uppercase tracking-wider text-amber-400">Perfil do Candidato Vestibulares</span>
            <h1 id="profileHeadingName" class="text-2xl font-extrabold">Estudante Visitante</h1>
            <p id="profileHeadingEmail" class="text-xs opacity-75">Modo local · Dados salvos no navegador</p>
          </div>
        </div>

        <form id="profileForm" class="space-y-4">
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-bold mb-1">Nome Completo</label>
              <input type="text" id="profName" class="kk-input" required />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1">Curso Alvo</label>
              <select id="profCourse" class="kk-input">
                <option value="Análise e Desenvolvimento de Sistemas (ADS)">Análise e Desenvolvimento de Sistemas (ADS)</option>
                <option value="Ciência de Dados">Ciência de Dados</option>
                <option value="Desenvolvimento de Software Multiplataforma (DSM)">Desenvolvimento de Software Multiplataforma (DSM)</option>
                <option value="Segurança da Informação">Segurança da Informação</option>
                <option value="Gestão Empresarial">Gestão Empresarial</option>
                <option value="Logística">Logística</option>
                <option value="Automação Industrial">Automação Industrial</option>
                <option value="Comércio Exterior">Comércio Exterior</option>
              </select>
            </div>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-bold mb-1">Instituição / Campus Desejado</label>
              <input type="text" id="profCampus" class="kk-input" placeholder="Ex: USP, Unicamp, Unesp, Vestibulares..." />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1">Meta Diária de Questões</label>
              <input type="number" id="profDailyGoal" min="3" max="100" class="kk-input" value="10" />
            </div>
          </div>

          <div class="flex items-center justify-end gap-3 pt-4 border-t border-blue-500/15">
            <button type="submit" class="kk-btn-primary px-6 py-2.5 text-sm">Salvar Alterações</button>
          </div>
        </form>
      </div>
    </section>

    <!-- ==================== 7. VIEW: ADMIN (GERENCIADOR DE QUESTÕES) ==================== -->
    <section id="view-admin" class="view-section space-y-6">
      <div class="flex flex-wrap items-center justify-between gap-4">
        <div>
          <h1 class="text-2xl sm:text-3xl font-extrabold">Admin · Gerenciador do Banco de Questões</h1>
          <p class="text-xs sm:text-sm opacity-75 mt-0.5">Adicione novas questões personalizadas ao Klick Kwiz. Elas ficam imediatamente disponíveis nos treinos e simulados.</p>
        </div>
        <span class="px-3 py-1.5 rounded-xl kk-card-subtle text-xs font-bold text-amber-400">Total no Banco: <span id="adminTotalCount">24</span> questões</span>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
        <!-- Formulário Nova Questão -->
        <form id="adminNewQuestionForm" class="lg:col-span-6 kk-card p-6 space-y-4">
          <h2 class="text-lg font-extrabold border-b border-blue-500/15 pb-3">➕ Cadastrar Nova Questão</h2>

          <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
            <div>
              <label class="block text-xs font-bold mb-1">Matéria</label>
              <select id="admSubject" class="kk-input" required>
                <option value="Matemática e Raciocínio Lógico">Matemática e Raciocínio Lógico</option>
                <option value="Português e Literatura">Português e Literatura</option>
                <option value="História">História</option>
                <option value="Geografia">Geografia</option>
                <option value="Inglês">Inglês</option>
                <option value="Física">Física</option>
                <option value="Química">Química</option>
                <option value="Biologia">Biologia</option>
                <option value="Multidisciplinar">Multidisciplinar</option>
              </select>
            </div>
            <div>
              <label class="block text-xs font-bold mb-1">Edição / Ano</label>
              <input type="text" id="admExam" class="kk-input" value="Vestibular 2026/2" required />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1">Dificuldade</label>
              <select id="admDifficulty" class="kk-input">
                <option value="Fácil">Fácil</option>
                <option value="Média" selected>Média</option>
                <option value="Difícil">Difícil</option>
              </select>
            </div>
          </div>

          <div>
            <label class="block text-xs font-bold mb-1">Texto-base (opcional)</label>
            <textarea id="admContext" rows="2" class="kk-input" placeholder="Trecho de reportagem, poema, código ou contexto da questão..."></textarea>
          </div>

          <div>
            <label class="block text-xs font-bold mb-1">Enunciado da Questão *</label>
            <textarea id="admPrompt" rows="2" class="kk-input" required placeholder="Digite a pergunta principal..."></textarea>
          </div>

          <div class="space-y-2">
            <label class="block text-xs font-bold">Alternativas (A até E) *</label>
            <input type="text" id="admOpt0" class="kk-input" placeholder="Alternativa A" required />
            <input type="text" id="admOpt1" class="kk-input" placeholder="Alternativa B" required />
            <input type="text" id="admOpt2" class="kk-input" placeholder="Alternativa C" required />
            <input type="text" id="admOpt3" class="kk-input" placeholder="Alternativa D" required />
            <input type="text" id="admOpt4" class="kk-input" placeholder="Alternativa E" required />
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
            <div>
              <label class="block text-xs font-bold mb-1">Gabarito Correto</label>
              <select id="admAnswer" class="kk-input">
                <option value="0">Alternativa A</option>
                <option value="1">Alternativa B</option>
                <option value="2">Alternativa C</option>
                <option value="3">Alternativa D</option>
                <option value="4">Alternativa E</option>
              </select>
            </div>
            <div class="sm:col-span-2">
              <label class="block text-xs font-bold mb-1">Resolução Comentada *</label>
              <input type="text" id="admExplanation" class="kk-input" required placeholder="Explique por que esta alternativa é a correta..." />
            </div>
          </div>

          <button type="submit" class="kk-btn-amber w-full py-2.5 text-sm">Salvar Questão no Banco</button>
        </form>

        <!-- Lista de Questões Cadastradas -->
        <div class="lg:col-span-6 kk-card p-6 space-y-4">
          <div class="flex items-center justify-between border-b border-blue-500/15 pb-3">
            <h2 class="text-lg font-extrabold">📚 Questões no Acervo</h2>
            <span class="text-xs opacity-70">Questões customizadas podem ser removidas</span>
          </div>
          <div id="adminQuestionsList" class="space-y-2.5 max-h-[620px] overflow-y-auto pr-1"></div>
        </div>
      </div>
    </section>

    <!-- ==================== 8. VIEW: SOBRE & CONTATO ==================== -->
    <section id="view-sobre" class="view-section max-w-4xl mx-auto space-y-6">
      <div class="kk-card p-6 sm:p-8 space-y-4">
        <span class="inline-block text-[11px] font-bold uppercase tracking-wider px-2.5 py-0.5 rounded-full bg-blue-500/15 text-blue-400">Sobre a Plataforma</span>
        <h1 class="text-2xl sm:text-3xl font-extrabold">Klick Kwiz · Preparatório Inteligente Vestibulares</h1>
        <p class="text-sm opacity-85 leading-relaxed">
          O <strong>Klick Kwiz</strong> nasceu com a missão de democratizar o acesso às principais universidades e faculdades públicas do país. Através da prática ativa com questões comentadas, simulados cronometrados e análise inteligente de desempenho, transformamos o estudo diário em evolução mensurável.
        </p>
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 pt-3">
          <div class="kk-card-subtle p-4">
            <div class="font-extrabold text-amber-400 text-sm mb-1">ESTUDE</div>
            <p class="text-xs opacity-75">Roteiro estruturado de 8 semanas cobrindo todos os eixos exigidos nos principais exames vestibulares.</p>
          </div>
          <div class="kk-card-subtle p-4">
            <div class="font-extrabold text-blue-400 text-sm mb-1">PRATIQUE</div>
            <p class="text-xs opacity-75">Questões comentadas com ferramentas reais de prova como eliminação de alternativas e simulados.</p>
          </div>
          <div class="kk-card-subtle p-4">
            <div class="font-extrabold text-emerald-400 text-sm mb-1">EVOLUA</div>
            <p class="text-xs opacity-75">Revisão automática dos seus erros e métricas por disciplina para maximizar sua nota final.</p>
          </div>
        </div>
      </div>

      <!-- Formulário de Contato -->
      <div class="kk-card p-6 sm:p-8 space-y-4">
        <h2 class="text-xl font-extrabold">Fale com a Equipe Klick Kwiz</h2>
        <p class="text-xs sm:text-sm opacity-75">Sugestões de provas, correções de gabarito ou dúvidas sobre os vestibulares? Envie sua mensagem.</p>
        <form id="contactForm" class="space-y-4">
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-bold mb-1">Seu Nome</label>
              <input type="text" id="contactName" class="kk-input" required placeholder="Ex: Mariana Silva" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1">E-mail</label>
              <input type="email" id="contactEmail" class="kk-input" required placeholder="mariana@email.com" />
            </div>
          </div>
          <div>
            <label class="block text-xs font-bold mb-1">Mensagem</label>
            <textarea id="contactMsg" rows="3" class="kk-input" required placeholder="Escreva sua dúvida ou sugestão..."></textarea>
          </div>
          <button type="submit" class="kk-btn-primary px-6 py-2.5 text-sm">Enviar Mensagem</button>
        </form>
      </div>
    </section>

  </main>

  <!-- ==================== FOOTER ==================== -->
  <footer class="border-t border-blue-500/15 mt-12 py-8 text-center text-xs opacity-75">
    <div class="max-w-7xl mx-auto px-4 flex flex-col sm:flex-row items-center justify-between gap-4">
      <div class="flex items-center gap-2 font-brand font-extrabold text-sm">
        <span>klick <span class="text-blue-500">kwiz</span></span>
        <span>·</span>
        <span class="font-normal">ESTUDE • PRATIQUE • EVOLUA</span>
      </div>
      <div>Plataforma gratuita de preparação para Vestibulares · Aprender nunca foi tão fácil.</div>
    </div>
  </footer>

  <!-- ==================== MODAL DE AUTENTICAÇÃO (LOGIN / CADASTRO) ==================== -->
  <div id="authModal" class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-950/75 backdrop-blur-sm p-4">
    <div class="kk-card w-full max-w-md p-6 sm:p-7 space-y-5 relative animate-[fadeSlideUp_0.2s_ease]">
      <button type="button" id="btnCloseAuth" class="absolute top-4 right-4 w-8 h-8 rounded-lg kk-card-subtle grid place-items-center text-sm hover:text-red-400" aria-label="Fechar modal">✕</button>

      <div class="text-center space-y-1">
        <div class="font-brand font-extrabold text-2xl">klick <span class="text-blue-500">kwiz</span></div>
        <p class="text-xs opacity-75">Salve seu progresso, histórico de erros e simulados</p>
      </div>

      <!-- Tabs Login / Cadastro -->
      <div class="grid grid-cols-2 gap-1 p-1 rounded-xl kk-card-subtle">
        <button type="button" id="tabLogin" class="py-2 rounded-lg text-xs font-bold bg-blue-600 text-white transition-all">Entrar</button>
        <button type="button" id="tabRegister" class="py-2 rounded-lg text-xs font-bold opacity-75 hover:opacity-100 transition-all">Criar Conta</button>
      </div>

      <!-- Form Login -->
      <form id="loginForm" class="space-y-3.5">
        <div>
          <label class="block text-xs font-bold mb-1">E-mail</label>
          <input type="email" id="loginEmail" class="kk-input" required placeholder="seu@email.com" />
        </div>
        <div>
          <label class="block text-xs font-bold mb-1">Senha</label>
          <input type="password" id="loginPassword" class="kk-input" required placeholder="••••••••" />
        </div>
        <button type="submit" class="kk-btn-primary w-full py-2.5 text-sm">Acessar Plataforma</button>

        <div class="p-3 rounded-xl border border-dashed border-amber-500/40 bg-amber-500/5 text-xs flex items-center justify-between gap-2">
          <div>
            <div class="font-bold text-amber-400">Acesso Rápido Demo</div>
            <div class="opacity-75">Testar com conta pré-configurada</div>
          </div>
          <button type="button" id="btnFillDemo" class="kk-btn-amber px-3 py-1.5 text-xs">Entrar Demo</button>
        </div>
      </form>

      <!-- Form Cadastro -->
      <form id="registerForm" class="hidden space-y-3.5">
        <div>
          <label class="block text-xs font-bold mb-1">Nome Completo</label>
          <input type="text" id="regName" class="kk-input" required placeholder="Ex: Lucas Almeida" />
        </div>
        <div>
          <label class="block text-xs font-bold mb-1">E-mail</label>
          <input type="email" id="regEmail" class="kk-input" required placeholder="lucas@email.com" />
        </div>
        <div>
          <label class="block text-xs font-bold mb-1">Curso Pretendido</label>
          <select id="regCourse" class="kk-input">
            <option value="Análise e Desenvolvimento de Sistemas (ADS)">Análise e Desenvolvimento de Sistemas (ADS)</option>
            <option value="Ciência de Dados">Ciência de Dados</option>
            <option value="Desenvolvimento de Software Multiplataforma (DSM)">Desenvolvimento de Software Multiplataforma (DSM)</option>
            <option value="Gestão Empresarial">Gestão Empresarial</option>
            <option value="Segurança da Informação">Segurança da Informação</option>
          </select>
        </div>
        <div>
          <label class="block text-xs font-bold mb-1">Senha</label>
          <input type="password" id="regPassword" class="kk-input" required minlength="4" placeholder="Mínimo 4 caracteres" />
        </div>
        <button type="submit" class="kk-btn-amber w-full py-2.5 text-sm">Criar Conta Grátis</button>
      </form>
    </div>
  </div>

  <!-- ==================== TOAST NOTIFICATION CONTAINER ==================== -->
  <div id="toastContainer" class="fixed bottom-4 right-4 z-50 flex flex-col gap-2 max-w-sm w-[calc(100vw-2rem)] pointer-events-none"></div>

  <!-- ==================== JAVASCRIPT COMPLETO DA APLICAÇÃO ==================== -->
  <script>
    (function () {
      "use strict";

      /* ---------------------------------------------------------
         1. BANCO DE QUESTÕES OFICIAL ESTILO VESTIBULARES (24 QUESTÕES)
      --------------------------------------------------------- */
      const DEFAULT_QUESTIONS = [
        {
          id: "q1",
          subject: "História",
          exam: "Vestibular 2026/1",
          difficulty: "Média",
          context: "A Era Vargas (1930–1945) promoveu profunda reestruturação do Estado brasileiro, combinando centralização política, incentivo à industrialização de base e criação de legislação trabalhista consolidada na CLT (1943).",
          prompt: "Sobre a política trabalhista e econômica implementada durante o governo de Getúlio Vargas, é correto afirmar que:",
          options: [
            "eliminou a intervenção estatal na economia, adotando o liberalismo clássico desde 1930.",
            "articulou o trabalhismo e o controle sindical pelo Estado, ao mesmo tempo em que impulsionou indústrias estatais como a CSN.",
            "garantiu ampla liberdade de greve e autonomia total dos sindicatos operários durante o Estado Novo.",
            "priorizou exclusivamente a monocultura cafeeira em detrimento da diversificação industrial urbana.",
            "extinguiu o salário mínimo e transferiu a previdência social inteiramente para bancos estrangeiros."
          ],
          answer: 1,
          explanation: "Vargas consolidou o chamado 'trabalhismo' (concessão de direitos sociais atrelada ao controle dos sindicatos pelo Ministério do Trabalho) e investiu na industrialização de base, criando a Companhia Siderúrgica Nacional (CSN) e a Vale."
        },
        {
          id: "q2",
          subject: "Geografia",
          exam: "Vestibular 2026/1",
          difficulty: "Média",
          context: "As ilhas de calor urbanas são fenômenos microclimáticos frequentes nas grandes metrópoles brasileiras, como a Região Metropolitana de São Paulo, elevando em até 5 °C a temperatura nas áreas centrais em comparação com zonas periféricas arborizadas.",
          prompt: "Assinale a alternativa que apresenta corretamente duas causas diretas da formação das ilhas de calor e uma medida mitigadora eficiente:",
          options: [
            "Alta densidade de asfalto/concreto e emissão de poluentes; ampliação de parques urbanos e telhados verdes.",
            "Excesso de áreas verdes e baixa circulação de veículos; impermeabilização total do solo urbano.",
            "Inversão térmica oceânica e chuvas ácidas; verticalização intensa sem recuos entre edifícios.",
            "Baixa capacidade térmica do concreto e ventos alísios constantes; substituição de árvores por fachadas espelhadas.",
            "Desmatamento da Amazônia e correntes marítimas frias; canalização fechada de todos os rios urbanos."
          ],
          answer: 0,
          explanation: "Superfícies impermeáveis (asfalto e concreto) absorvem e retêm grande radiação solar, somadas ao calor gerado por veículos e indústrias. A arborização urbana e os telhados verdes aumentam a evapotranspiração e reduzem a temperatura local."
        },
        {
          id: "q3",
          subject: "Matemática e Raciocínio Lógico",
          exam: "Vestibular 2026/1",
          difficulty: "Fácil",
          context: "Uma equipe de desenvolvimento universitária precisa configurar um servidor em nuvem. O plano básico custa R$ 120,00 fixos por mês mais R$ 0,40 por gigabyte (GB) de tráfego excedente.",
          prompt: "Se em determinado mês a fatura total do servidor foi de R$ 210,00, a quantidade de gigabytes excedentes consumidos nesse mês foi de:",
          options: [
            "180 GB",
            "200 GB",
            "225 GB",
            "250 GB",
            "300 GB"
          ],
          answer: 2,
          explanation: "Montando a função do 1º grau: 120 + 0,40 · x = 210 → 0,40x = 90 → x = 90 / 0,40 = 225 GB."
        },
        {
          id: "q4",
          subject: "Português e Literatura",
          exam: "Vestibular 2025/2",
          difficulty: "Média",
          context: "Leia o trecho de 'Vidas Secas', de Graciliano Ramos:\n\"Fabiano, você é um homem, exclamou em voz alta. Conteve-se, notou que os meninos estavam perto, com certeza iam admirar-se ouvindo-o falar só. E, pensando bem, ele não era homem: era apenas um cabra ocupado em guardar coisas dos outros.\"",
          prompt: "No fragmento acima, a oscilação de Fabiano entre afirmar-se 'homem' e reconhecer-se como 'bicho/cabra' evidencia uma característica central do Romance de 30, a saber:",
          options: [
            "a idealização romântica do sertanejo como herói medieval invencível diante da natureza.",
            "a denúncia social da desumanização provocada pela seca e pelas estruturas latifundiárias opressoras.",
            "o culto parnasiano à forma perfeita e ao vocabulário rebuscado distante da realidade nordestina.",
            "a exaltação ufanista da modernização agrária no interior nordestino durante a República Velha.",
            "o escapismo simbolista voltado exclusivamente aos conflitos psicológicos da aristocracia urbana."
          ],
          answer: 1,
          explanation: "Graciliano Ramos, expoente da segunda fase do Modernismo (Romance de 30), utiliza linguagem seca e concisa para denunciar a marginalização socioeconômica que reduz o retirante à condição sub-humana."
        },
        {
          id: "q5",
          subject: "Inglês",
          exam: "Vestibular 2025/2",
          difficulty: "Fácil",
          context: "\"Artificial Intelligence tools are rapidly reshaping software engineering. Rather than replacing developers, these assistants automate repetitive boilerplate code, allowing engineers to focus on system architecture, security, and creative problem-solving.\"",
          prompt: "De acordo com o texto em inglês, o principal impacto das ferramentas de Inteligência Artificial na engenharia de software é:",
          options: [
            "substituir completamente os engenheiros de software nos próximos anos.",
            "eliminar a necessidade de testes de segurança e arquitetura de sistemas.",
            "automatizar tarefas repetitivas de código, liberando profissionais para arquitetura e resolução criativa de problemas.",
            "tornar o desenvolvimento de software mais lento devido à complexidade das ferramentas.",
            "restringir o trabalho criativo apenas às máquinas autônomas."
          ],
          answer: 2,
          explanation: "A expressão 'Rather than replacing developers' significa 'Em vez de substituir os desenvolvedores', e 'automate repetitive boilerplate code' indica a automação de código repetitivo."
        },
        {
          id: "q6",
          subject: "Matemática e Raciocínio Lógico",
          exam: "Vestibular 2025/2",
          difficulty: "Média",
          context: "Em uma turma de 60 estudantes de Ciência de Dados universitária, 38 dominam Python, 27 dominam SQL e 15 dominam ambas as linguagens.",
          prompt: "Escolhendo-se ao acaso um estudante dessa turma, a probabilidade de ele NÃO dominar nenhuma dessas duas linguagens é de:",
          options: [
            "10%",
            "15%",
            "16,7%",
            "20%",
            "25%"
          ],
          answer: 2,
          explanation: "Pelo princípio da inclusão-exclusão: n(Python ∪ SQL) = 38 + 27 - 15 = 50 alunos. Logo, 60 - 50 = 10 alunos não dominam nenhuma das duas. Probabilidade = 10/60 = 1/6 ≈ 16,67% (16,7%)."
        },
        {
          id: "q7",
          subject: "Física",
          exam: "Vestibular 2025/2",
          difficulty: "Média",
          context: "Um automóvel elétrico de massa 1.500 kg trafega em uma via retilínea horizontal com velocidade constante de 20 m/s (72 km/h) quando o motorista aciona os freios regenerativos, parando completamente após percorrer 50 metros em desaceleração uniforme.",
          prompt: "O módulo da força resultante média que atuou sobre o veículo durante a frenagem foi de:",
          options: [
            "3.000 N",
            "4.500 N",
            "6.000 N",
            "7.500 N",
            "12.000 N"
          ],
          answer: 2,
          explanation: "Pela equação de Torricelli: v² = v₀² + 2·a·Δs → 0 = 20² + 2·a·50 → 100a = -400 → |a| = 4 m/s². Pela 2ª Lei de Newton: F = m·a = 1500 · 4 = 6.000 N."
        },
        {
          id: "q8",
          subject: "Química",
          exam: "Vestibular 2025/1",
          difficulty: "Média",
          context: "O etanol hidratado (C₂H₅OH) produzido a partir da cana-de-açúcar é amplamente utilizado como biocombustível no Brasil, apresentando balanço de carbono mais favorável que a gasolina.",
          prompt: "Na combustão completa de 1 mol de etanol (C₂H₅OH), a proporção estequiométrica correta entre os reagentes (etanol : O₂) e os produtos (CO₂ : H₂O) é, respectivamente:",
          options: [
            "1 : 2 e 2 : 2",
            "1 : 3 e 2 : 3",
            "1 : 3 e 3 : 2",
            "2 : 5 e 4 : 6",
            "1 : 4 e 2 : 3"
          ],
          answer: 1,
          explanation: "Equação balanceada: 1 C₂H₅OH + 3 O₂ → 2 CO₂ + 3 H₂O. Conferindo átomos: 2 C, 6 H e 7 O em ambos os lados."
        },
        {
          id: "q9",
          subject: "Biologia",
          exam: "Vestibular 2025/1",
          difficulty: "Fácil",
          context: "O descarte irregular de esgoto doméstico e fertilizantes agrícolas em rios e represas provoca o enriquecimento excessivo da água por nutrientes como nitrogênio e fósforo, desencadeando o fenômeno da eutrofização.",
          prompt: "Na etapa crítica da eutrofização artificial, a mortandade de peixes e organismos aquáticos aeróbicos ocorre diretamente devido à:",
          options: [
            "redução drástica do oxigênio dissolvido na água causada pela proliferação de bactérias decompositoras aeróbicas.",
            "diminuição imediata da temperatura da água provocada pelo bloqueio da luz solar pelas algas.",
            "transformação direta do nitrogênio atmosférico em metais pesados tóxicos no fundo do rio.",
            "elevação extrema da salinidade da água doce decorrente da fotossíntese das cianobactérias.",
            "ausência total de matéria orgânica disponível para a cadeia alimentar aquática."
          ],
          answer: 0,
          explanation: "O excesso de nutrientes causa proliferação de algas (floração), que bloqueiam a luz e morrem. A decomposição dessa matéria orgânica por bactérias aeróbicas consome quase todo o O₂ dissolvido, asfixiando os peixes."
        },
        {
          id: "q10",
          subject: "Multidisciplinar",
          exam: "Vestibular 2026/1",
          difficulty: "Média",
          context: "A transição energética global impulsionou a demanda por minerais críticos como lítio, cobalto e terras-raras, essenciais para baterias de veículos elétricos e turbinas eólicas.",
          prompt: "Considerando aspectos geopolíticos e ambientais da transição energética atual, é correto afirmar que:",
          options: [
            "a extração de lítio e terras-raras dispensa licenciamento ambiental por se tratar de energia limpa.",
            "a concentração do refino desses minerais em poucos países gera novas disputas geopolíticas e exige cadeias produtivas sustentáveis.",
            "o Brasil não possui reservas relevantes de lítio, nióbio ou grafite em seu território.",
            "as baterias de íons de lítio têm vida útil infinita e não geram passivos de reciclagem.",
            "a energia eólica e solar substituíram integralmente os combustíveis fósseis na matriz elétrica mundial."
          ],
          answer: 1,
          explanation: "Embora reduzam emissões na operação, tecnologias verdes dependem de minerais estratégicos cuja cadeia de refino é altamente concentrada (especialmente na Ásia), criando novos desafios geopolíticos e socioambientais."
        },
        {
          id: "q11",
          subject: "História",
          exam: "Vestibular 2025/1",
          difficulty: "Média",
          context: "A Constituição Brasileira de 1988, apelidada de 'Constituição Cidadã' por Ulysses Guimarães, representou o marco jurídico da redemocratização após 21 anos de regime militar.",
          prompt: "Entre as inovações garantidas pelo texto original da Constituição de 1988, destaca-se:",
          options: [
            "a criação do voto censitário baseado na renda anual dos cidadãos alfabetizados.",
            "a extensão do direito de voto facultativo aos analfabetos e aos jovens de 16 e 17 anos.",
            "o restabelecimento das eleições indiretas para governadores e prefeitos das capitais.",
            "a extinção do Sistema Único de Saúde (SUS) em favor de planos privados obrigatórios.",
            "a supressão dos direitos territoriais originários dos povos indígenas."
          ],
          answer: 1,
          explanation: "A Constituição de 1988 ampliou historicamente a cidadania política no Brasil ao instituir o sufrágio universal com voto facultativo para analfabetos e jovens entre 16 e 17 anos."
        },
        {
          id: "q12",
          subject: "Geografia",
          exam: "Vestibular 2025/2",
          difficulty: "Fácil",
          context: "O agronegócio brasileiro apresenta elevada produtividade na região Centro-Oeste, apoiado por corredores logísticos de exportação que conectam áreas produtoras aos portos do Arco Norte e do Sudeste.",
          prompt: "Um dos principais gargalos estruturais historicamente apontados para a competitividade logística do Brasil no escoamento de cargas é:",
          options: [
            "a dependência excessiva do modal rodoviário em detrimento de ferrovias e hidrovias de longa distância.",
            "o excesso de malha ferroviária de alta velocidade subutilizada na região Norte.",
            "a ausência de portos marítimos profundos ao longo de todo o litoral brasileiro.",
            "a proibição legal do transporte intermodal entre rodovias e ferrovias.",
            "o custo zero dos fretes rodoviários em estradas não pavimentadas."
          ],
          answer: 0,
          explanation: "Mais de 60% da matriz de transporte de cargas no Brasil concentra-se no modal rodoviário, que possui maior custo por tonelada/km e maior emissão de CO₂ para longas distâncias."
        },
        {
          id: "q13",
          subject: "Matemática e Raciocínio Lógico",
          exam: "Vestibular 2025/1",
          difficulty: "Difícil",
          context: "Um sistema de autenticação gera senhas temporárias formadas por 2 letras distintas escolhidas entre {A, B, C, D, E} seguidas de 3 algarismos distintos escolhidos entre {1, 2, 3, 4}.",
          prompt: "O número total de senhas distintas que esse sistema pode gerar é:",
          options: [
            "120",
            "240",
            "360",
            "480",
            "600"
          ],
          answer: 3,
          explanation: "Pelo Princípio Fundamental da Contagem: 5 opções para 1ª letra × 4 opções para 2ª letra = 20 pares de letras. Para os 3 algarismos distintos: 4 × 3 × 2 = 24. Total = 20 × 24 = 480 senhas."
        },
        {
          id: "q14",
          subject: "Português e Literatura",
          exam: "Vestibular 2026/1",
          difficulty: "Fácil",
          context: "Observe a frase: \"Embora os investimentos em energia solar tenham crescido, ainda há desafios na distribuição elétrica.\"",
          prompt: "A conjunção 'Embora' estabelece entre as orações uma relação semântica de:",
          options: [
            "condição",
            "consequência",
            "concessão",
            "proporcionalidade",
            "finalidade"
          ],
          answer: 2,
          explanation: "'Embora' é uma conjunção subordinativa concessiva, introduzindo um fato contrário à ação principal sem impedir que esta se realize."
        },
        {
          id: "q15",
          subject: "Inglês",
          exam: "Vestibular 2025/1",
          difficulty: "Média",
          context: "\"Cybersecurity experts warn that phishing attacks remain the primary entry point for ransomware in small businesses, often exploiting human trust rather than technical flaws.\"",
          prompt: "Segundo o alerta dos especialistas em cibersegurança no texto, os ataques de phishing ocorrem principalmente porque:",
          options: [
            "exploram a confiança humana com mais frequência do que falhas técnicas.",
            "afetam exclusivamente grandes corporações governamentais.",
            "não têm relação com a entrada de ransomware nas empresas.",
            "dependem apenas de defeitos físicos nos cabos de rede.",
            "foram totalmente bloqueados pelos antivírus modernos."
          ],
          answer: 0,
          explanation: "O trecho 'often exploiting human trust rather than technical flaws' traduz-se literalmente por 'frequentemente explorando a confiança humana em vez de falhas técnicas'."
        },
        {
          id: "q16",
          subject: "Física",
          exam: "Vestibular 2024/2",
          difficulty: "Fácil",
          context: "Um laboratório de informática universitária possui 20 computadores idênticos, cada um operando com potência média de 250 W durante 4 horas por dia.",
          prompt: "O consumo total de energia elétrica desses 20 computadores em um dia de funcionamento é de:",
          options: [
            "5 kWh",
            "10 kWh",
            "15 kWh",
            "20 kWh",
            "25 kWh"
          ],
          answer: 3,
          explanation: "Potência total = 20 × 250 W = 5.000 W = 5 kW. Energia = Potência × tempo = 5 kW × 4 h = 20 kWh."
        },
        {
          id: "q17",
          subject: "Química",
          exam: "Vestibular 2024/2",
          difficulty: "Fácil",
          context: "A chuva ácida é um problema ambiental agravado pela queima de combustíveis fósseis ricos em enxofre e nitrogênio, liberando óxidos como SO₂ e NO₂ na atmosfera.",
          prompt: "Em contato com a umidade atmosférica, esses óxidos ácidos formam principalmente:",
          options: [
            "ácido sulfúrico (H₂SO₄) e ácido nítrico (HNO₃), reduzindo o pH da água da chuva.",
            "hidróxido de sódio (NaOH) e amônia (NH₃), tornando a chuva fortemente alcalina.",
            "cloreto de sódio (NaCl) e gás metano (CH₄), neutralizando a acidez dos solos.",
            "ácido clorídrico (HCl) puro e gás hélio (He), elevando o pH para valores acima de 10.",
            "carbonato de cálcio (CaCO₃), restaurando monumentos históricos de mármore."
          ],
          answer: 0,
          explanation: "Óxidos ácidos como SO₃ e NO₂ reagem com a água da chuva originando H₂SO₄ e HNO₃, responsáveis por baixar o pH da precipitação abaixo de 5,6."
        },
        {
          id: "q18",
          subject: "Biologia",
          exam: "Vestibular 2024/2",
          difficulty: "Média",
          context: "As vacinas de RNA mensageiro (mRNA) representaram um marco biotecnológico recente no combate a doenças infecciosas virais.",
          prompt: "O mecanismo de ação dessas vacinas no organismo humano consiste em:",
          options: [
            "inserir o vírus vivo e plenamente ativo para causar infecção grave controlada.",
            "fornecer instruções genéticas para que as células produzam uma proteína viral inofensiva, estimulando a resposta imune.",
            "alterar permanentemente o DNA nuclear humano para impedir a divisão celular.",
            "substituir os linfócitos B e T por antibióticos sintéticos de longa duração.",
            "destruir todas as bactérias da microbiota intestinal para evitar inflamações."
          ],
          answer: 1,
          explanation: "O mRNA vacinal é traduzido nos ribossomos do citoplasma celular para produzir uma proteína antigênica (como a Spike), ativando anticorpos e memória imunológica sem entrar no núcleo celular."
        },
        {
          id: "q19",
          subject: "Matemática e Raciocínio Lógico",
          exam: "Vestibular 2024/2",
          difficulty: "Média",
          context: "Considere a proposição lógica composta: \"Se o algoritmo é otimizado, então o tempo de resposta diminui.\"",
          prompt: "Do ponto de vista da Lógica Formal, uma proposição logicamente equivalente à afirmação acima é:",
          options: [
            "Se o tempo de resposta diminui, então o algoritmo é otimizado.",
            "Se o algoritmo não é otimizado, então o tempo de resposta não diminui.",
            "Se o tempo de resposta não diminui, então o algoritmo não é otimizado.",
            "O algoritmo é otimizado e o tempo de resposta não diminui.",
            "O algoritmo não é otimizado e o tempo de resposta diminui."
          ],
          answer: 2,
          explanation: "A equivalência contrapositiva de 'Se P, então Q' (P → Q) é 'Se não Q, então não P' (~Q → ~P)."
        },
        {
          id: "q20",
          subject: "História",
          exam: "Vestibular 2024/2",
          difficulty: "Fácil",
          context: "A Revolução Industrial iniciada na Inglaterra na segunda metade do século XVIII transformou radicalmente as relações de trabalho, a urbanização e o ritmo produtivo.",
          prompt: "Entre os fatores que explicam o pioneirismo inglês na Primeira Revolução Industrial, destaca-se:",
          options: [
            "a abundância de reservas de carvão mineral e ferro, aliada ao acúmulo de capitais e à estabilidade política pós-Revolução Gloriosa.",
            "a abolição imediata do trabalho assalariado e a distribuição igualitária das fábricas aos operários.",
            "o isolamento comercial total da Inglaterra em relação às rotas marítimas internacionais.",
            "a predominância da energia nuclear e da robótica digital nas primeiras indústrias têxteis.",
            "a inexistência de mão de obra disponível nas cidades devido à reforma agrária camponesa."
          ],
          answer: 0,
          explanation: "A Inglaterra reuniu condições ideais: jazidas de carvão e ferro, capital acumulado pelo comércio marítimo, mão de obra liberada pelos cercamentos (enclosures) e instituições favoráveis aos negócios."
        },
        {
          id: "q21",
          subject: "Geografia",
          exam: "Vestibular 2026/1",
          difficulty: "Média",
          context: "A transição demográfica brasileira nas últimas décadas caracteriza-se pela queda acentuada da taxa de fecundidade e pelo aumento da expectativa de vida.",
          prompt: "Uma consequência socioeconômica direta desse processo para o Brasil nas próximas décadas é:",
          options: [
            "o estreitamento da base da pirâmide etária e o aumento da demanda por políticas de previdência e saúde para idosos.",
            "o crescimento explosivo da população infantil e a redução da idade média da população.",
            "o fim definitivo da urbanização com retorno maciço da população para áreas rurais.",
            "a diminuição da proporção de idosos em relação à população economicamente ativa.",
            "o aumento contínuo da taxa de natalidade acima de 6 filhos por mulher."
          ],
          answer: 0,
          explanation: "Com menos nascimentos e maior longevidade, o Brasil passa por rápido envelhecimento populacional, exigindo adaptações previdenciárias, urbanas e no sistema de saúde."
        },
        {
          id: "q22",
          subject: "Português e Literatura",
          exam: "Vestibular 2025/2",
          difficulty: "Média",
          context: "Assinale a alternativa em que o emprego do sinal indicativo de crase está plenamente de acordo com a norma-padrão da língua portuguesa:",
          prompt: "Escolha a frase gramaticalmente correta:",
          options: [
            "Os candidatos dirigiram-se à sala de provas às 12h30 para assistir à palestra.",
            "O edital refere-se à todos os cursos superiores de tecnologia universitária.",
            "Entregamos o projeto final à ela antes do prazo estipulado.",
            "A partir de amanhã, começaremos à estudar para o simulado geral.",
            "Fomos à São Paulo conhecer o campus histórico da instituição."
          ],
          answer: 0,
          explanation: "Em 'dirigiram-se à sala' (a + a sala), 'às 12h30' (locução adverbial feminina de hora exata) e 'assistir à palestra' (no sentido de ver/presenciar, exige preposição 'a'), a crase é obrigatória."
        },
        {
          id: "q23",
          subject: "Multidisciplinar",
          exam: "Vestibular 2025/2",
          difficulty: "Fácil",
          context: "A Economia Circular propõe substituir o modelo linear tradicional ('extrair, produzir, descartar') por sistemas regenerativos baseados em ecodesign, reutilização, remanufatura e reciclagem.",
          prompt: "No setor de tecnologia da informação, uma prática alinhada aos princípios da Economia Circular é:",
          options: [
            "a obsolescência programada para forçar a troca anual de smartphones funcionais.",
            "a logística reversa de lixo eletrônico (e-waste) para recuperação de metais preciosos e recondicionamento de hardware.",
            "o descarte de baterias de íons de lítio em aterros sanitários comuns.",
            "a fabricação de equipamentos com peças coladas que impedem qualquer reparo.",
            "o aumento do consumo energético de data centers sem reaproveitamento térmico."
          ],
          answer: 1,
          explanation: "A logística reversa e o recondicionamento estendem o ciclo de vida dos eletrônicos e recuperam matérias-primas valiosas, reduzindo a extração mineral."
        },
        {
          id: "q24",
          subject: "Matemática e Raciocínio Lógico",
          exam: "Vestibular 2026/1",
          difficulty: "Média",
          context: "Um reservatório cilíndrico de água de uma unidade universitária tem raio da base igual a 2 metros e altura igual a 3 metros. (Considere π ≈ 3,14).",
          prompt: "A capacidade máxima desse reservatório, em litros, é aproximadamente igual a:",
          options: [
            "18.840 litros",
            "25.120 litros",
            "37.680 litros",
            "42.500 litros",
            "50.240 litros"
          ],
          answer: 2,
          explanation: "Volume do cilindro: V = π · r² · h = 3,14 · 2² · 3 = 3,14 · 12 = 37,68 m³. Como 1 m³ = 1.000 litros, temos 37.680 litros."
        }
      ];

      /* ---------------------------------------------------------
         2. ROTEIRO DE 8 SEMANAS VESTIBULARES
      --------------------------------------------------------- */
      const ROADMAP_WEEKS = [
        {
          week: 1,
          title: "Fundamentos & Diagnóstico",
          focus: "Matemática Básica + Interpretação Textual",
          tasks: [
            { id: "w1t1", text: "Revisar Porcentagem, Razão, Proporção e Regra de Três (alta incidência nos Vestibulares)." },
            { id: "w1t2", text: "Resolver 15 questões de interpretação de texto e gêneros jornalísticos." }
          ]
        },
        {
          week: 2,
          title: "Funções & Brasil Contemporâneo",
          focus: "Funções 1º/2º Grau + Era Vargas à Redemocratização",
          tasks: [
            { id: "w2t1", text: "Praticar gráficos de Função Afim e Quadrática aplicados a custos e receitas." },
            { id: "w2t2", text: "Estudar Era Vargas, Ditadura Militar e Constituição de 1988." }
          ]
        },
        {
          week: 3,
          title: "Lógica, Conjuntos & Geopolítica",
          focus: "Raciocínio Lógico + Globalização e Energia",
          tasks: [
            { id: "w3t1", text: "Dominar Tabelas-Verdade, Equivalências Lógicas e Diagramas de Venn." },
            { id: "w3t2", text: "Revisar Transição Energética, Blocos Econômicos e Urbanização Brasileira." }
          ]
        },
        {
          week: 4,
          title: "Ciências da Natureza Aplicadas",
          focus: "Mecânica, Eletricidade & Meio Ambiente",
          tasks: [
            { id: "w4t1", text: "Resolver exercícios de Cinemática, Leis de Newton e Consumo Elétrico (kWh)." },
            { id: "w4t2", text: "Revisar Ecologia (eutrofização, ciclos biogeoquímicos) e Química Ambiental." }
          ]
        },
        {
          week: 5,
          title: "Inglês Instrumental & Gramática",
          focus: "Leitura Técnica em Inglês + Norma Padrão",
          tasks: [
            { id: "w5t1", text: "Treinar cognatos, falsos cognatos e conectivos (however, although, therefore) em textos de TI." },
            { id: "w5t2", text: "Revisar Crase, Concordância Verbal e Coesão Referencial para a Redação." }
          ]
        },
        {
          week: 6,
          title: "Geometria & Combinatória",
          focus: "Áreas, Volumes & Probabilidade",
          tasks: [
            { id: "w6t1", text: "Calcular áreas planas e volumes de prismas e cilindros em problemas contextualizados." },
            { id: "w6t2", text: "Praticar Princípio Fundamental da Contagem e Probabilidade simples." }
          ]
        },
        {
          week: 7,
          title: "Redação Dissertativa Vestibulares",
          focus: "Estrutura Nota Máxima & Atualidades",
          tasks: [
            { id: "w7t1", text: "Escrever 2 redações dissertativo-argumentativas sobre tecnologia, trabalho e sustentabilidade." },
            { id: "w7t2", text: "Refazer todas as questões marcadas no Caderno de Erros." }
          ]
        },
        {
          week: 8,
          title: "Reta Final & Simulados Reais",
          focus: "Gestão de Tempo & Revisão Estratégica",
          tasks: [
            { id: "w8t1", text: "Realizar 2 Simulados Cronometrados completos controlando o tempo por questão." },
            { id: "w8t2", text: "Revisar fórmulas essenciais e descansar bem na véspera da prova." }
          ]
        }
      ];

      /* ---------------------------------------------------------
         3. ESTADO GLOBAL E PERSISTÊNCIA (LOCALSTORAGE)
      --------------------------------------------------------- */
      const STORAGE_KEYS = {
        QUESTIONS: "kk_vestibulares_questions_v2",
        USER: "kk_vestibulares_user_v2",
        PROGRESS: "kk_vestibulares_progress_v2"
      };

      const defaultProgress = {
        answered: {},
        favorites: [],
        eliminated: {},
        roadmapDone: [],
        simHistory: [],
        streak: 1,
        lastActiveDate: new Date().toISOString().slice(0, 10),
        dailySolvedCount: 0
      };

      let questions = [];
      let currentUser = null;
      let progress = JSON.parse(JSON.stringify(defaultProgress));
      let miniQuizIndex = 0;
      let miniQuizAnswers = {};
      let trainFiltered = [];
      let trainCurrentIndex = 0;
      let simState = { active: false };

      function escapeHTML(str) {
        if (str == null) return "";
        return String(str)
          .replace(/&/g, "&amp;")
          .replace(/</g, "&lt;")
          .replace(/>/g, "&gt;")
          .replace(/"/g, "&quot;")
          .replace(/'/g, "&#39;");
      }

      function showToast(title, message, type = "info") {
        const container = document.getElementById("toastContainer");
        if (!container) return;
        const borderColors = {
          success: "border-l-emerald-500",
          error: "border-l-red-500",
          amber: "border-l-amber-500",
          info: "border-l-blue-500"
        };
        const toast = document.createElement("div");
        toast.className = `kk-card p-3.5 border-l-4 ${borderColors[type] || borderColors.info} pointer-events-auto shadow-xl transition-all duration-300`;
        toast.innerHTML = `
          <div class="font-extrabold text-xs">${escapeHTML(title)}</div>
          <div class="text-xs opacity-80 mt-0.5">${escapeHTML(message)}</div>
        `;
        container.appendChild(toast);
        setTimeout(() => {
          toast.style.opacity = "0";
          toast.style.transform = "translateY(8px)";
          setTimeout(() => toast.remove(), 300);
        }, 3200);
      }

      function loadState() {
        try {
          const savedCustom = JSON.parse(localStorage.getItem(STORAGE_KEYS.QUESTIONS) || "[]");
          questions = [...DEFAULT_QUESTIONS, ...savedCustom];
        } catch (e) {
          questions = [...DEFAULT_QUESTIONS];
        }

        try {
          currentUser = JSON.parse(localStorage.getItem(STORAGE_KEYS.USER) || "null");
        } catch (e) {
          currentUser = null;
        }

        try {
          const savedProg = JSON.parse(localStorage.getItem(STORAGE_KEYS.PROGRESS) || "null");
          if (savedProg) {
            progress = { ...JSON.parse(JSON.stringify(defaultProgress)), ...savedProg };
          }
        } catch (e) {
          progress = JSON.parse(JSON.stringify(defaultProgress));
        }

        const today = new Date().toISOString().slice(0, 10);
        if (progress.lastActiveDate !== today) {
          const yesterday = new Date(Date.now() - 86400000).toISOString().slice(0, 10);
          progress.streak = progress.lastActiveDate === yesterday ? (progress.streak || 1) + 1 : 1;
          progress.lastActiveDate = today;
          progress.dailySolvedCount = 0;
          saveProgress();
        }
      }

      function saveProgress() {
        localStorage.setItem(STORAGE_KEYS.PROGRESS, JSON.stringify(progress));
        updateGlobalHUD();
      }

      function saveCustomQuestions() {
        const customOnly = questions.filter((q) => q.isCustom);
        localStorage.setItem(STORAGE_KEYS.QUESTIONS, JSON.stringify(customOnly));
        populateFilterOptions();
      }

      function navigateTo(viewName) {
        document.querySelectorAll(".view-section").forEach((sec) => sec.classList.remove("active"));
        const target = document.getElementById(`view-${viewName}`);
        if (target) target.classList.add("active");

        document.querySelectorAll(".nav-link").forEach((btn) => {
          btn.classList.toggle("active", btn.getAttribute("data-nav") === viewName);
        });

        const drawer = document.getElementById("mobileMenuDrawer");
        if (drawer) drawer.classList.add("hidden");

        window.scrollTo({ top: 0, behavior: "smooth" });

        if (viewName === "treinar") {
          populateFilterOptions();
          applyTrainFilters(false);
        } else if (viewName === "desempenho") {
          renderAnalytics();
        } else if (viewName === "roteiro") {
          renderRoadmap();
        } else if (viewName === "admin") {
          renderAdminList();
        } else if (viewName === "perfil") {
          renderProfileView();
        } else if (viewName === "simulado") {
          populateSimSubjects();
        }
      }

      /* ---------------------------------------------------------
         5. ATUALIZAÇÃO DE HUD GLOBAL E AUTENTICAÇÃO
      --------------------------------------------------------- */
      function getInitials(name) {
        if (!name) return "KV";
        const parts = name.trim().split(/\s+/);
        return ((parts[0]?.[0] || "") + (parts[1]?.[0] || parts[0]?.[1] || "")).toUpperCase();
      }

      function updateGlobalHUD() {
        const answeredArr = Object.values(progress.answered);
        const totalSolved = answeredArr.length;
        const totalCorrect = answeredArr.filter((a) => a.correct).length;
        const accuracy = totalSolved > 0 ? Math.round((totalCorrect / totalSolved) * 100) : 0;

        document.getElementById("topStreak").textContent = progress.streak || 1;
        document.getElementById("heroTotalQuestions").textContent = questions.length;
        document.getElementById("heroUserSolved").textContent = totalSolved;
        document.getElementById("heroUserAccuracy").textContent = `${accuracy}%`;
        document.getElementById("simAllCountLabel").textContent = `${questions.length} Questões`;
        document.getElementById("adminTotalCount").textContent = questions.length;

        const wrongCount = answeredArr.filter((a) => !a.correct).length;
        document.getElementById("wrongCountBadge").textContent = wrongCount;

        const guestBox = document.getElementById("guestControls");
        const userBox = document.getElementById("userControls");
        const homeBanner = document.getElementById("homeUserBanner");

        if (currentUser) {
          guestBox.classList.add("hidden");
          userBox.classList.remove("hidden");
          userBox.classList.add("flex");

          const initials = getInitials(currentUser.name);
          document.getElementById("topAvatar").textContent = initials;
          document.getElementById("topUserName").textContent = currentUser.name.split(" ")[0];

          homeBanner.classList.remove("hidden");
          document.getElementById("bannerAvatar").textContent = initials;
          document.getElementById("bannerTitle").textContent = `Olá, ${currentUser.name}! Foco em ${currentUser.course}`;
          const goal = currentUser.dailyGoal || 10;
          document.getElementById("bannerSubtitle").textContent = `Meta de hoje: ${progress.dailySolvedCount}/${goal} questões concluídas · ${totalCorrect} acertos totais.`;
        } else {
          guestBox.classList.remove("hidden");
          userBox.classList.add("hidden");
          userBox.classList.remove("flex");
          homeBanner.classList.add("hidden");
        }
      }

      /* ---------------------------------------------------------
         6. MINI QUIZ DE BOAS-VINDAS (HOME)
      --------------------------------------------------------- */
      function renderMiniQuiz() {
        const container = document.getElementById("miniQuizContainer");
        const badge = document.getElementById("miniQuizBadge");
        const miniQuestions = questions.slice(0, 4);
        const letters = ["A", "B", "C", "D", "E"];

        if (miniQuizIndex >= miniQuestions.length) {
          const hits = miniQuestions.filter((q, idx) => miniQuizAnswers[idx] === q.answer).length;
          badge.textContent = "Concluído!";
          container.innerHTML = `
            <div class="text-center py-6 space-y-4">
              <div class="text-4xl">${hits >= 3 ? "🏆" : "💡"}</div>
              <h3 class="text-2xl font-extrabold">Você acertou ${hits} de ${miniQuestions.length} questões!</h3>
              <p class="text-sm opacity-75 max-w-md mx-auto">
                ${hits >= 3 ? "Excelente base! Você já está no ritmo dos Vestibulares. Avance para o Banco Completo ou Simulado." : "Ótimo começo! Explore o Banco de Questões comentado para dominar os padrões da banca."}
              </p>
              <div class="flex flex-wrap justify-center gap-3 pt-2">
                <button type="button" data-nav="treinar" class="kk-btn-primary px-5 py-2.5 text-sm">Ir para Banco Completo (${questions.length} questões)</button>
                <button type="button" id="btnMiniRestartEnd" class="kk-btn-outline px-4 py-2.5 text-sm">Refazer Mini Quiz</button>
              </div>
            </div>
          `;
          document.getElementById("btnMiniRestartEnd").addEventListener("click", () => {
            miniQuizIndex = 0;
            miniQuizAnswers = {};
            renderMiniQuiz();
          });
          return;
        }

        const q = miniQuestions[miniQuizIndex];
        const chosen = miniQuizAnswers[miniQuizIndex];
        const hasAnswered = chosen !== undefined;
        badge.textContent = `Questão ${miniQuizIndex + 1} de ${miniQuestions.length}`;

        container.innerHTML = `
          <div class="space-y-4">
            <div class="flex flex-wrap items-center gap-2 text-xs">
              <span class="px-2.5 py-1 rounded-md bg-blue-500/20 text-blue-400 font-bold">${escapeHTML(q.subject)}</span>
              <span class="px-2.5 py-1 rounded-md kk-card-subtle font-semibold">${escapeHTML(q.exam)}</span>
            </div>
            ${q.context ? `<div class="p-3.5 rounded-xl kk-card-subtle text-xs sm:text-sm leading-relaxed opacity-90 border-l-2 border-blue-500">${escapeHTML(q.context)}</div>` : ""}
            <p class="font-bold text-sm sm:text-base">${escapeHTML(q.prompt)}</p>
            <div class="space-y-2.5">
              ${q.options.map((opt, idx) => {
                let cls = "alt-option";
                if (hasAnswered) {
                  cls += " locked";
                  if (idx === q.answer) cls += " correct";
                  else if (idx === chosen) cls += " wrong";
                }
                return `
                  <button type="button" class="${cls}" data-mini-opt="${idx}" ${hasAnswered ? "disabled" : ""}>
                    <span class="alt-key">${letters[idx]}</span>
                    <span class="text-xs sm:text-sm pt-0.5">${escapeHTML(opt)}</span>
                  </button>
                `;
              }).join("")}
            </div>
            ${hasAnswered ? `
              <div class="p-4 rounded-xl bg-emerald-500/10 border border-emerald-500/30 text-xs sm:text-sm space-y-1">
                <div class="font-extrabold ${chosen === q.answer ? "text-emerald-400" : "text-amber-400"}">
                  ${chosen === q.answer ? "✅ Resposta Correta!" : `❌ Resposta Incorreta. Gabarito: ${letters[q.answer]}`}
                </div>
                <p class="opacity-85 leading-relaxed">${escapeHTML(q.explanation)}</p>
              </div>
              <div class="flex justify-end">
                <button type="button" id="btnMiniNext" class="kk-btn-amber px-5 py-2 text-xs sm:text-sm">
                  ${miniQuizIndex < miniQuestions.length - 1 ? "Próxima Questão →" : "Ver Resultado Final"}
                </button>
              </div>
            ` : ""}
          </div>
        `;

        container.querySelectorAll("[data-mini-opt]").forEach((btn) => {
          btn.addEventListener("click", () => {
            const idx = Number(btn.getAttribute("data-mini-opt"));
            miniQuizAnswers[miniQuizIndex] = idx;
            recordQuestionAttempt(q.id, idx === q.answer, idx);
            renderMiniQuiz();
          });
        });

        const nextBtn = document.getElementById("btnMiniNext");
        if (nextBtn) {
          nextBtn.addEventListener("click", () => {
            miniQuizIndex++;
            renderMiniQuiz();
          });
        }
      }

      /* ---------------------------------------------------------
         7. TREINAR (BANCO DE QUESTÕES)
      --------------------------------------------------------- */
      function populateFilterOptions() {
        const subSelect = document.getElementById("filterSubject");
        const examSelect = document.getElementById("filterExam");
        const curSub = subSelect.value;
        const curExam = examSelect.value;

        const subjects = [...new Set(questions.map((q) => q.subject))].sort();
        const exams = [...new Set(questions.map((q) => q.exam))].sort().reverse();

        subSelect.innerHTML = `<option value="ALL">Todas as Matérias (${questions.length})</option>` +
          subjects.map((s) => `<option value="${escapeHTML(s)}">${escapeHTML(s)}</option>`).join("");
        if ([...subSelect.options].some((o) => o.value === curSub)) subSelect.value = curSub;

        examSelect.innerHTML = `<option value="ALL">Todas as Provas</option>` +
          exams.map((e) => `<option value="${escapeHTML(e)}">${escapeHTML(e)}</option>`).join("");
        if ([...examSelect.options].some((o) => o.value === curExam)) examSelect.value = curExam;
      }

      function applyTrainFilters(resetIndex = true) {
        const sub = document.getElementById("filterSubject").value;
        const exam = document.getElementById("filterExam").value;
        const diff = document.getElementById("filterDifficulty").value;
        const status = document.getElementById("filterStatus").value;
        const query = document.getElementById("filterSearch").value.trim().toLowerCase();

        trainFiltered = questions.filter((q) => {
          if (sub !== "ALL" && q.subject !== sub) return false;
          if (exam !== "ALL" && q.exam !== exam) return false;
          if (diff !== "ALL" && q.difficulty !== diff) return false;

          const ans = progress.answered[q.id];
          if (status === "UNANSWERED" && ans) return false;
          if (status === "WRONG" && (!ans || ans.correct)) return false;
          if (status === "FAVORITE" && !progress.favorites.includes(q.id)) return false;

          if (query) {
            const haystack = `${q.subject} ${q.exam} ${q.context || ""} ${q.prompt} ${q.options.join(" ")}`.toLowerCase();
            if (!haystack.includes(query)) return false;
          }
          return true;
        });

        if (resetIndex) trainCurrentIndex = 0;
        if (trainCurrentIndex >= trainFiltered.length) trainCurrentIndex = Math.max(0, trainFiltered.length - 1);

        document.getElementById("trainFilteredCount").textContent = `${trainFiltered.length} ${trainFiltered.length === 1 ? "questão" : "questões"}`;
        renderTrainQuestion();
      }

      function recordQuestionAttempt(qId, isCorrect, selectedIdx) {
        const prev = progress.answered[qId];
        progress.answered[qId] = {
          selected: selectedIdx,
          correct: isCorrect,
          timestamp: Date.now()
        };
        if (!prev) {
          progress.dailySolvedCount = (progress.dailySolvedCount || 0) + 1;
        }
        saveProgress();
      }

      let trainPendingSelection = null;

      function renderTrainQuestion() {
        const stage = document.getElementById("trainQuestionStage");
        const letters = ["A", "B", "C", "D", "E"];

        if (!trainFiltered.length) {
          stage.innerHTML = `
            <div class="text-center py-12 space-y-3">
              <div class="text-4xl">🔍</div>
              <h3 class="text-xl font-extrabold">Nenhuma questão encontrada com esses filtros</h3>
              <p class="text-xs sm:text-sm opacity-75">Tente limpar os filtros ou selecionar outra disciplina.</p>
              <button type="button" id="btnEmptyReset" class="kk-btn-primary px-5 py-2 text-xs">Mostrar Todas as Questões</button>
            </div>
          `;
          document.getElementById("btnEmptyReset").addEventListener("click", () => {
            document.getElementById("filterSubject").value = "ALL";
            document.getElementById("filterExam").value = "ALL";
            document.getElementById("filterDifficulty").value = "ALL";
            document.getElementById("filterStatus").value = "ALL";
            document.getElementById("filterSearch").value = "";
            applyTrainFilters(true);
          });
          return;
        }

        const q = trainFiltered[trainCurrentIndex];
        const savedAns = progress.answered[q.id];
        const isFav = progress.favorites.includes(q.id);
        const elimList = progress.eliminated[q.id] || [];

        stage.innerHTML = `
          <div class="space-y-5">
            <div class="flex flex-wrap items-center justify-between gap-3 pb-4 border-b border-blue-500/15">
              <div class="flex flex-wrap items-center gap-2 text-xs">
                <span class="px-3 py-1 rounded-lg bg-blue-600 text-white font-extrabold">Questão ${trainCurrentIndex + 1} de ${trainFiltered.length}</span>
                <span class="px-2.5 py-1 rounded-lg bg-blue-500/20 text-blue-400 font-bold">${escapeHTML(q.subject)}</span>
                <span class="px-2.5 py-1 rounded-lg kk-card-subtle font-semibold">${escapeHTML(q.exam)}</span>
                <span class="px-2.5 py-1 rounded-lg kk-card-subtle font-semibold">${escapeHTML(q.difficulty)}</span>
              </div>
              <div class="flex items-center gap-2">
                <button type="button" id="btnToggleFav" class="px-3 py-1.5 rounded-lg kk-card-subtle text-xs font-bold hover:border-amber-500 transition-colors" title="Favoritar questão">
                  ${isFav ? "⭐ Favoritada" : "☆ Favoritar"}
                </button>
                ${savedAns ? `
                  <button type="button" id="btnRedoQuestion" class="px-3 py-1.5 rounded-lg kk-card-subtle text-xs font-semibold hover:border-blue-500">
                    🔄 Refazer
                  </button>
                ` : ""}
              </div>
            </div>

            ${q.context ? `
              <div class="p-4 rounded-xl kk-card-subtle text-xs sm:text-sm leading-relaxed whitespace-pre-line border-l-4 border-l-blue-500">
                ${escapeHTML(q.context)}
              </div>
            ` : ""}

            <h2 class="text-base sm:text-lg font-bold leading-snug">${escapeHTML(q.prompt)}</h2>

            <div class="space-y-2.5">
              ${q.options.map((opt, idx) => {
                const isEliminated = elimList.includes(idx);
                let cls = "alt-option";
                if (isEliminated && !savedAns) cls += " eliminated";
                if (savedAns) {
                  cls += " locked";
                  if (idx === q.answer) cls += " correct";
                  else if (idx === savedAns.selected) cls += " wrong";
                } else if (trainPendingSelection === idx) {
                  cls += " selected";
                }

                return `
                  <div class="flex items-center gap-2">
                    <button type="button" class="${cls} flex-1" data-train-opt="${idx}" ${savedAns ? "disabled" : ""}>
                      <span class="alt-key">${letters[idx]}</span>
                      <span class="text-xs sm:text-sm pt-0.5 leading-relaxed">${escapeHTML(opt)}</span>
                    </button>
                    ${!savedAns ? `
                      <button type="button" data-elim-opt="${idx}" class="p-2.5 rounded-xl kk-card-subtle text-xs opacity-60 hover:opacity-100 hover:text-red-400 transition-all" title="Riscar / Eliminar alternativa">
                        ✂️
                      </button>
                    ` : ""}
                  </div>
                `;
              }).join("")}
            </div>

            ${savedAns ? `
              <div class="p-4 sm:p-5 rounded-2xl ${savedAns.correct ? "bg-emerald-500/10 border border-emerald-500/35" : "bg-red-500/10 border border-red-500/35"} space-y-2">
                <div class="flex items-center justify-between">
                  <span class="font-extrabold text-sm ${savedAns.correct ? "text-emerald-400" : "text-red-400"}">
                    ${savedAns.correct ? "✅ Excelente! Você acertou" : `❌ Alternativa incorreta · Gabarito oficial: ${letters[q.answer]}`}
                  </span>
                </div>
                <p class="text-xs sm:text-sm opacity-90 leading-relaxed"><strong>Resolução comentada:</strong> ${escapeHTML(q.explanation)}</p>
              </div>
            ` : ""}

            <div class="flex flex-wrap items-center justify-between gap-3 pt-4 border-t border-blue-500/15">
              <div class="flex items-center gap-2">
                <button type="button" id="btnTrainPrev" class="kk-btn-outline px-4 py-2 text-xs sm:text-sm" ${trainCurrentIndex === 0 ? "disabled" : ""}>
                  ← Anterior
                </button>
                <button type="button" id="btnTrainNext" class="kk-btn-outline px-4 py-2 text-xs sm:text-sm" ${trainCurrentIndex >= trainFiltered.length - 1 ? "disabled" : ""}>
                  Próxima →
                </button>
              </div>

              ${!savedAns ? `
                <button type="button" id="btnConfirmTrainAnswer" class="kk-btn-amber px-6 py-2.5 text-xs sm:text-sm" ${trainPendingSelection === null ? "disabled style='opacity:0.5;cursor:not-allowed;'" : ""}>
                  Confirmar Resposta (Enter)
                </button>
              ` : `
                <button type="button" id="btnNextAfterAnswer" class="kk-btn-primary px-6 py-2.5 text-xs sm:text-sm" ${trainCurrentIndex >= trainFiltered.length - 1 ? "disabled" : ""}>
                  Avançar para Próxima →
                </button>
              `}
            </div>
          </div>
        `;

        stage.querySelectorAll("[data-train-opt]").forEach((btn) => {
          btn.addEventListener("click", () => {
            trainPendingSelection = Number(btn.getAttribute("data-train-opt"));
            renderTrainQuestion();
          });
        });

        stage.querySelectorAll("[data-elim-opt]").forEach((btn) => {
          btn.addEventListener("click", () => {
            const idx = Number(btn.getAttribute("data-elim-opt"));
            const arr = progress.eliminated[q.id] || [];
            if (arr.includes(idx)) {
              progress.eliminated[q.id] = arr.filter((i) => i !== idx);
            } else {
              progress.eliminated[q.id] = [...arr, idx];
              if (trainPendingSelection === idx) trainPendingSelection = null;
            }
            saveProgress();
            renderTrainQuestion();
          });
        });

        const btnConfirm = document.getElementById("btnConfirmTrainAnswer");
        if (btnConfirm) {
          btnConfirm.addEventListener("click", confirmCurrentTrainQuestion);
        }

        const btnFav = document.getElementById("btnToggleFav");
        if (btnFav) {
          btnFav.addEventListener("click", () => {
            if (progress.favorites.includes(q.id)) {
              progress.favorites = progress.favorites.filter((id) => id !== q.id);
              showToast("Removida dos favoritos", "Questão desmarcada.", "info");
            } else {
              progress.favorites.push(q.id);
              showToast("Questão favoritada ⭐", "Salva na sua lista de revisão.", "amber");
            }
            saveProgress();
            renderTrainQuestion();
          });
        }

        const btnRedo = document.getElementById("btnRedoQuestion");
        if (btnRedo) {
          btnRedo.addEventListener("click", () => {
            delete progress.answered[q.id];
            trainPendingSelection = null;
            saveProgress();
            renderTrainQuestion();
          });
        }

        const btnPrev = document.getElementById("btnTrainPrev");
        if (btnPrev) {
          btnPrev.addEventListener("click", () => {
            if (trainCurrentIndex > 0) {
              trainCurrentIndex--;
              trainPendingSelection = null;
              renderTrainQuestion();
            }
          });
        }

        const btnNext = document.getElementById("btnTrainNext");
        const btnNextAfter = document.getElementById("btnNextAfterAnswer");
        [btnNext, btnNextAfter].forEach((b) => {
          if (b) {
            b.addEventListener("click", () => {
              if (trainCurrentIndex < trainFiltered.length - 1) {
                trainCurrentIndex++;
                trainPendingSelection = null;
                renderTrainQuestion();
              }
            });
          }
        });
      }

      function confirmCurrentTrainQuestion() {
        if (!trainFiltered.length || trainPendingSelection === null) return;
        const q = trainFiltered[trainCurrentIndex];
        if (progress.answered[q.id]) return;

        const isCorrect = trainPendingSelection === q.answer;
        recordQuestionAttempt(q.id, isCorrect, trainPendingSelection);
        trainPendingSelection = null;
        renderTrainQuestion();

        const autoAdv = document.getElementById("chkAutoAdvance")?.checked;
        if (isCorrect && autoAdv && trainCurrentIndex < trainFiltered.length - 1) {
          setTimeout(() => {
            trainCurrentIndex++;
            renderTrainQuestion();
          }, 950);
        }
      }

      /* ---------------------------------------------------------
         8. SIMULADO CRONOMETRADO
      --------------------------------------------------------- */
      function populateSimSubjects() {
        const sel = document.getElementById("simSubjectFilter");
        const subjects = [...new Set(questions.map((q) => q.subject))].sort();
        sel.innerHTML = `<option value="ALL">Todas as Matérias (Prova Geral)</option>` +
          subjects.map((s) => `<option value="${escapeHTML(s)}">${escapeHTML(s)}</option>`).join("");
      }

      function startSimulado() {
        const mode = document.querySelector('input[name="simMode"]:checked')?.value || "express";
        const subj = document.getElementById("simSubjectFilter").value;

        let pool = subj === "ALL" ? [...questions] : questions.filter((q) => q.subject === subj);
        pool.sort(() => Math.random() - 0.5);

        let count = 10;
        let mins = 25;
        let label = "Simulado Express (10q)";
        if (mode === "medio") {
          count = Math.min(18, pool.length);
          mins = 45;
          label = `Meio Simulado (${count}q)`;
        } else if (mode === "completo") {
          count = pool.length;
          mins = Math.max(25, Math.round(count * 2.5));
          label = `Maratona Completa (${count}q)`;
        } else {
          count = Math.min(10, pool.length);
          mins = 25;
          label = `Simulado Express (${count}q)`;
        }

        if (pool.length === 0) {
          showToast("Sem questões", "Nenhuma questão encontrada para essa área.", "error");
          return;
        }

        simState = {
          active: true,
          questions: pool.slice(0, count),
          answers: {},
          flagged: {},
          currentIdx: 0,
          secondsLeft: mins * 60,
          totalSeconds: mins * 60,
          timerId: null,
          modeName: label
        };

        document.getElementById("simConfigPanel").classList.add("hidden");
        document.getElementById("simResultPanel").classList.add("hidden");
        document.getElementById("simActivePanel").classList.remove("hidden");

        updateSimTimerUI();
        simState.timerId = setInterval(() => {
          simState.secondsLeft--;
          updateSimTimerUI();
          if (simState.secondsLeft <= 0) {
            finishSimulado(true);
          }
        }, 1000);

        renderSimQuestion();
      }

      function updateSimTimerUI() {
        const m = Math.floor(Math.max(0, simState.secondsLeft) / 60);
        const s = Math.max(0, simState.secondsLeft) % 60;
        const display = document.getElementById("simTimerDisplay");
        display.textContent = `${String(m).padStart(2, "0")}:${String(s).padStart(2, "0")}`;
        if (simState.secondsLeft <= 120) {
          display.classList.remove("text-amber-400");
          display.classList.add("text-red-500", "animate-pulse");
        } else {
          display.classList.add("text-amber-400");
          display.classList.remove("text-red-500", "animate-pulse");
        }
      }

      function renderSimQuestion() {
        const q = simState.questions[simState.currentIdx];
        const letters = ["A", "B", "C", "D", "E"];
        const chosen = simState.answers[simState.currentIdx];
        const isFlagged = !!simState.flagged[simState.currentIdx];

        const answeredCount = Object.keys(simState.answers).length;
        document.getElementById("simProgressText").textContent = `${answeredCount} / ${simState.questions.length}`;

        const box = document.getElementById("simQuestionBox");
        box.innerHTML = `
          <div class="flex flex-wrap items-center justify-between gap-2 pb-4 border-b border-blue-500/15">
            <div class="flex items-center gap-2 text-xs">
              <span class="px-3 py-1 rounded-lg bg-blue-600 text-white font-extrabold">Questão ${simState.currentIdx + 1} de ${simState.questions.length}</span>
              <span class="px-2.5 py-1 rounded-lg bg-blue-500/20 text-blue-400 font-bold">${escapeHTML(q.subject)}</span>
            </div>
            <button type="button" id="btnSimFlag" class="px-3 py-1.5 rounded-lg kk-card-subtle text-xs font-bold ${isFlagged ? "text-amber-400 border-amber-500" : ""}">
              ${isFlagged ? "🚩 Marcada para Revisão" : "🏳️ Marcar Revisão"}
            </button>
          </div>

          ${q.context ? `<div class="p-4 rounded-xl kk-card-subtle text-xs sm:text-sm leading-relaxed border-l-4 border-l-blue-500">${escapeHTML(q.context)}</div>` : ""}

          <h2 class="text-base sm:text-lg font-bold leading-snug">${escapeHTML(q.prompt)}</h2>

          <div class="space-y-2.5">
            ${q.options.map((opt, idx) => `
              <button type="button" class="alt-option ${chosen === idx ? "selected" : ""}" data-sim-opt="${idx}">
                <span class="alt-key">${letters[idx]}</span>
                <span class="text-xs sm:text-sm pt-0.5">${escapeHTML(opt)}</span>
              </button>
            `).join("")}
          </div>

          <div class="flex items-center justify-between pt-4 border-t border-blue-500/15">
            <button type="button" id="btnSimPrev" class="kk-btn-outline px-4 py-2 text-xs sm:text-sm" ${simState.currentIdx === 0 ? "disabled" : ""}>← Anterior</button>
            <button type="button" id="btnSimNext" class="kk-btn-primary px-5 py-2 text-xs sm:text-sm" ${simState.currentIdx >= simState.questions.length - 1 ? "disabled" : ""}>Próxima →</button>
          </div>
        `;

        box.querySelectorAll("[data-sim-opt]").forEach((btn) => {
          btn.addEventListener("click", () => {
            simState.answers[simState.currentIdx] = Number(btn.getAttribute("data-sim-opt"));
            renderSimQuestion();
          });
        });

        document.getElementById("btnSimFlag").addEventListener("click", () => {
          simState.flagged[simState.currentIdx] = !simState.flagged[simState.currentIdx];
          renderSimQuestion();
        });

        document.getElementById("btnSimPrev").addEventListener("click", () => {
          if (simState.currentIdx > 0) {
            simState.currentIdx--;
            renderSimQuestion();
          }
        });

        document.getElementById("btnSimNext").addEventListener("click", () => {
          if (simState.currentIdx < simState.questions.length - 1) {
            simState.currentIdx++;
            renderSimQuestion();
          }
        });

        const grid = document.getElementById("simGridNav");
        grid.innerHTML = simState.questions.map((_, idx) => {
          const isAns = simState.answers[idx] !== undefined;
          const isFlg = !!simState.flagged[idx];
          const isCur = idx === simState.currentIdx;
          let bg = "kk-card-subtle";
          if (isFlg) bg = "bg-amber-500 text-slate-950 font-extrabold";
          else if (isAns) bg = "bg-blue-600 text-white font-bold";

          return `
            <button type="button" data-sim-jump="${idx}" class="h-9 rounded-lg text-xs ${bg} ${isCur ? "ring-2 ring-amber-400" : ""} transition-all">
              ${idx + 1}
            </button>
          `;
        }).join("");

        grid.querySelectorAll("[data-sim-jump]").forEach((btn) => {
          btn.addEventListener("click", () => {
            simState.currentIdx = Number(btn.getAttribute("data-sim-jump"));
            renderSimQuestion();
          });
        });
      }

      function finishSimulado(autoTimeout = false) {
        if (simState.timerId) clearInterval(simState.timerId);
        simState.active = false;

        let hits = 0;
        const bySubject = {};

        simState.questions.forEach((q, idx) => {
          const userAns = simState.answers[idx];
          const isCorrect = userAns === q.answer;
          if (isCorrect) hits++;

          if (!bySubject[q.subject]) bySubject[q.subject] = { total: 0, hits: 0 };
          bySubject[q.subject].total++;
          if (isCorrect) bySubject[q.subject].hits++;

          if (userAns !== undefined) {
            recordQuestionAttempt(q.id, isCorrect, userAns);
          }
        });

        const total = simState.questions.length;
        const percent = Math.round((hits / total) * 100);
        const spentSec = simState.totalSeconds - Math.max(0, simState.secondsLeft);
        const spentStr = `${Math.floor(spentSec / 60)}m ${spentSec % 60}s`;

        progress.simHistory.unshift({
          date: new Date().toLocaleDateString("pt-BR"),
          mode: simState.modeName,
          score: `${hits}/${total}`,
          percent,
          durationStr: spentStr
        });
        saveProgress();

        document.getElementById("simActivePanel").classList.add("hidden");
        const resPanel = document.getElementById("simResultPanel");
        resPanel.classList.remove("hidden");

        const letters = ["A", "B", "C", "D", "E"];
        resPanel.innerHTML = `
          <div class="brand-hero rounded-3xl p-6 sm:p-8 text-center space-y-4">
            <span class="inline-block px-3 py-1 rounded-full bg-amber-500 text-slate-950 font-extrabold text-xs uppercase">
              ${autoTimeout ? "⏰ Tempo Esgotado · Prova Finalizada" : "🏁 Simulado Concluído"}
            </span>
            <h2 class="text-3xl sm:text-4xl font-black">Nota Final: <span class="text-amber-400">${percent}%</span></h2>
            <p class="text-sm text-slate-300">Você acertou <strong>${hits} de ${total}</strong> questões em <strong>${spentStr}</strong>.</p>
            <div class="pt-2">
              <button type="button" id="btnNewSimulado" class="kk-btn-amber px-6 py-2.5 text-sm">Fazer Novo Simulado</button>
            </div>
          </div>

          <div class="kk-card p-6 space-y-4">
            <h3 class="text-lg font-extrabold">📊 Desempenho por Matéria neste Simulado</h3>
            <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3">
              ${Object.entries(bySubject).map(([subj, st]) => {
                const p = Math.round((st.hits / st.total) * 100);
                return `
                  <div class="kk-card-subtle p-3.5 space-y-1.5">
                    <div class="flex justify-between text-xs font-bold">
                      <span class="truncate">${escapeHTML(subj)}</span>
                      <span class="${p >= 70 ? "text-emerald-400" : "text-amber-400"}">${st.hits}/${st.total} (${p}%)</span>
                    </div>
                    <div class="w-full h-2 rounded-full bg-slate-900 overflow-hidden">
                      <div class="h-full ${p >= 70 ? "bg-emerald-500" : "bg-amber-500"}" style="width:${p}%"></div>
                    </div>
                  </div>
                `;
              }).join("")}
            </div>
          </div>

          <div class="kk-card p-6 space-y-4">
            <h3 class="text-lg font-extrabold">📝 Gabarito Comentado Questão a Questão</h3>
            <div class="space-y-4">
              ${simState.questions.map((q, idx) => {
                const u = simState.answers[idx];
                const ok = u === q.answer;
                return `
                  <div class="p-4 rounded-xl kk-card-subtle border-l-4 ${ok ? "border-l-emerald-500" : "border-l-red-500"} space-y-2">
                    <div class="flex flex-wrap items-center justify-between gap-2 text-xs">
                      <span class="font-extrabold">Questão ${idx + 1} · ${escapeHTML(q.subject)}</span>
                      <span class="font-bold ${ok ? "text-emerald-400" : "text-red-400"}">
                        ${u === undefined ? "Não respondida" : (ok ? `✅ Acertou (${letters[u]})` : `❌ Marcou ${letters[u]} · Correta: ${letters[q.answer]}`)}
                      </span>
                    </div>
                    <p class="text-xs sm:text-sm font-semibold">${escapeHTML(q.prompt)}</p>
                    <p class="text-xs opacity-80 leading-relaxed"><strong>Comentário:</strong> ${escapeHTML(q.explanation)}</p>
                  </div>
                `;
              }).join("")}
            </div>
          </div>
        `;

        document.getElementById("btnNewSimulado").addEventListener("click", () => {
          resPanel.classList.add("hidden");
          document.getElementById("simConfigPanel").classList.remove("hidden");
        });
      }

      /* ---------------------------------------------------------
         9. ROTEIRO DE ESTUDOS 8 SEMANAS
      --------------------------------------------------------- */
      function renderRoadmap() {
        const container = document.getElementById("roadmapContainer");
        let totalTasks = 0;
        let doneTasks = 0;

        ROADMAP_WEEKS.forEach((w) => {
          w.tasks.forEach((t) => {
            totalTasks++;
            if (progress.roadmapDone.includes(t.id)) doneTasks++;
          });
        });

        const pct = totalTasks > 0 ? Math.round((doneTasks / totalTasks) * 100) : 0;
        document.getElementById("roadmapPercentText").textContent = `${pct}%`;
        document.getElementById("roadmapProgressBar").style.width = `${pct}%`;
        document.getElementById("roadmapCountText").textContent = `${doneTasks} de ${totalTasks} metas concluídas`;

        container.innerHTML = ROADMAP_WEEKS.map((w) => {
          const allDone = w.tasks.every((t) => progress.roadmapDone.includes(t.id));
          return `
            <div class="kk-card p-5 space-y-3 ${allDone ? "border-emerald-500/50" : ""}">
              <div class="flex items-center justify-between">
                <span class="px-2.5 py-0.5 rounded-md text-xs font-extrabold ${allDone ? "bg-emerald-500/20 text-emerald-400" : "bg-blue-500/20 text-blue-400"}">
                  Semana ${w.week} ${allDone ? "✓" : ""}
                </span>
                <span class="text-[11px] font-semibold opacity-70">${escapeHTML(w.focus)}</span>
              </div>
              <h3 class="font-extrabold text-base">${escapeHTML(w.title)}</h3>
              <div class="space-y-2 pt-1">
                ${w.tasks.map((t) => {
                  const checked = progress.roadmapDone.includes(t.id);
                  return `
                    <label class="flex items-start gap-2.5 p-2.5 rounded-xl kk-card-subtle cursor-pointer hover:border-blue-500/40 transition-colors">
                      <input type="checkbox" data-task-id="${t.id}" ${checked ? "checked" : ""} class="mt-0.5 w-4 h-4 accent-blue-600 rounded" />
                      <span class="text-xs leading-relaxed ${checked ? "line-through opacity-60" : ""}">${escapeHTML(t.text)}</span>
                    </label>
                  `;
                }).join("")}
              </div>
            </div>
          `;
        }).join("");

        container.querySelectorAll("[data-task-id]").forEach((chk) => {
          chk.addEventListener("change", () => {
            const id = chk.getAttribute("data-task-id");
            if (chk.checked) {
              if (!progress.roadmapDone.includes(id)) progress.roadmapDone.push(id);
              showToast("Meta concluída!", "Seu cronograma Vestibulares foi atualizado.", "success");
            } else {
              progress.roadmapDone = progress.roadmapDone.filter((x) => x !== id);
            }
            saveProgress();
            renderRoadmap();
          });
        });
      }

      /* ---------------------------------------------------------
         10. ANALYTICS & DESEMPENHO
      --------------------------------------------------------- */
      function renderAnalytics() {
        const entries = Object.entries(progress.answered);
        const solved = entries.length;
        const correct = entries.filter(([, v]) => v.correct).length;
        const acc = solved > 0 ? Math.round((correct / solved) * 100) : 0;

        document.getElementById("kpiSolved").textContent = solved;
        document.getElementById("kpiCorrectSub").textContent = `${correct} acertos · ${solved - correct} erros`;
        document.getElementById("kpiAccuracy").textContent = `${acc}%`;

        const streakDays = progress.streak || 1;
        document.getElementById("kpiLevel").textContent = `${streakDays} ${streakDays === 1 ? "dia seguido" : "dias seguidos"}`;
        document.getElementById("kpiSimCountDetail").textContent = `${progress.simHistory.length} simulados concluídos`;

        const mapBySub = {};
        questions.forEach((q) => {
          if (!mapBySub[q.subject]) mapBySub[q.subject] = { total: 0, answered: 0, hits: 0 };
          mapBySub[q.subject].total++;
          const ans = progress.answered[q.id];
          if (ans) {
            mapBySub[q.subject].answered++;
            if (ans.correct) mapBySub[q.subject].hits++;
          }
        });

        let bestSub = "—";
        let bestRate = -1;
        Object.entries(mapBySub).forEach(([sub, st]) => {
          if (st.answered > 0) {
            const rate = Math.round((st.hits / st.answered) * 100);
            if (rate > bestRate) {
              bestRate = rate;
              bestSub = sub;
            }
          }
        });

        document.getElementById("kpiBestSubject").textContent = bestSub;
        document.getElementById("kpiBestSubjectRate").textContent = bestRate >= 0 ? `${bestRate}% de aproveitamento` : "Resolva questões para calcular";

        const barsBox = document.getElementById("subjectChartBars");
        barsBox.innerHTML = Object.entries(mapBySub).map(([sub, st]) => {
          const pct = st.answered > 0 ? Math.round((st.hits / st.answered) * 100) : 0;
          return `
            <div class="space-y-1">
              <div class="flex justify-between text-xs font-bold">
                <span>${escapeHTML(sub)}</span>
                <span class="opacity-80">${st.hits}/${st.answered} respondidas (${pct}%)</span>
              </div>
              <div class="w-full h-2.5 rounded-full bg-slate-900 overflow-hidden">
                <div class="h-full bg-gradient-to-r from-blue-600 to-amber-500 transition-all duration-500" style="width:${pct}%"></div>
              </div>
            </div>
          `;
        }).join("");

        const badges = [
          { icon: "🚀", name: "Primeiro Passo", desc: "Resolva 1 questão", unlocked: solved >= 1 },
          { icon: "🔥", name: "Ritmo Forte", desc: "Resolva 10 questões", unlocked: solved >= 10 },
          { icon: "🎯", name: "Alta Precisão", desc: "Atingir ≥ 75% de acerto", unlocked: solved >= 5 && acc >= 75 },
          { icon: "⏱️", name: "Simulado Feito", desc: "Conclua 1 simulado", unlocked: progress.simHistory.length >= 1 }
        ];
        document.getElementById("badgesGrid").innerHTML = badges.map((b) => `
          <div class="p-3.5 rounded-xl kk-card-subtle border ${b.unlocked ? "border-amber-500/50 bg-amber-500/5" : "opacity-45"} space-y-1">
            <div class="text-2xl">${b.icon}</div>
            <div class="font-extrabold text-xs">${b.name}</div>
            <div class="text-[11px] opacity-75">${b.desc}</div>
          </div>
        `).join("");

        const tbody = document.getElementById("simHistoryBody");
        if (!progress.simHistory.length) {
          tbody.innerHTML = `<tr><td colspan="5" class="py-6 text-center text-xs opacity-65">Nenhum simulado finalizado ainda. Experimente na aba Simulado!</td></tr>`;
        } else {
          tbody.innerHTML = progress.simHistory.map((h) => `
            <tr class="border-b border-blue-500/10 text-xs">
              <td class="py-2.5 pr-4">${escapeHTML(h.date)}</td>
              <td class="py-2.5 pr-4 font-bold">${escapeHTML(h.mode)}</td>
              <td class="py-2.5 pr-4">${escapeHTML(h.score)}</td>
              <td class="py-2.5 pr-4 font-extrabold ${h.percent >= 70 ? "text-emerald-400" : "text-amber-400"}">${h.percent}%</td>
              <td class="py-2.5">${escapeHTML(h.durationStr)}</td>
            </tr>
          `).join("");
        }
      }

      /* ---------------------------------------------------------
         11. ADMIN
      --------------------------------------------------------- */
      function renderAdminList() {
        const list = document.getElementById("adminQuestionsList");
        list.innerHTML = questions.slice().reverse().map((q) => `
          <div class="p-3.5 rounded-xl kk-card-subtle flex items-start justify-between gap-3">
            <div class="space-y-1 min-w-0">
              <div class="flex flex-wrap items-center gap-1.5 text-[11px]">
                <span class="px-2 py-0.5 rounded bg-blue-500/25 text-blue-400 font-bold">${escapeHTML(q.subject)}</span>
                <span class="opacity-70">${escapeHTML(q.exam)}</span>
                ${q.isCustom ? `<span class="px-2 py-0.5 rounded bg-amber-500/25 text-amber-400 font-bold">Customizada</span>` : ""}
              </div>
              <p class="text-xs font-semibold line-clamp-2">${escapeHTML(q.prompt)}</p>
            </div>
            ${q.isCustom ? `
              <button type="button" data-del-q="${q.id}" class="px-2.5 py-1 rounded-lg bg-red-500/20 text-red-400 text-xs font-bold hover:bg-red-500/30 shrink-0">
                Excluir
              </button>
            ` : `<span class="text-[11px] opacity-45 shrink-0">Oficial</span>`}
          </div>
        `).join("");

        list.querySelectorAll("[data-del-q]").forEach((btn) => {
          btn.addEventListener("click", () => {
            const id = btn.getAttribute("data-del-q");
            questions = questions.filter((item) => item.id !== id);
            saveCustomQuestions();
            updateGlobalHUD();
            renderAdminList();
            showToast("Questão removida", "O acervo foi atualizado.", "info");
          });
        });
      }

      /* ---------------------------------------------------------
         12. PERFIL
      --------------------------------------------------------- */
      function renderProfileView() {
        if (currentUser) {
          document.getElementById("profileBigAvatar").textContent = getInitials(currentUser.name);
          document.getElementById("profileHeadingName").textContent = currentUser.name;
          document.getElementById("profileHeadingEmail").textContent = `${currentUser.email} · Conta Ativa`;
          document.getElementById("profName").value = currentUser.name;
          document.getElementById("profCourse").value = currentUser.course || "Análise e Desenvolvimento de Sistemas (ADS)";
          document.getElementById("profCampus").value = currentUser.campus || "";
          document.getElementById("profDailyGoal").value = currentUser.dailyGoal || 10;
        } else {
          document.getElementById("profileBigAvatar").textContent = "VS";
          document.getElementById("profileHeadingName").textContent = "Estudante Visitante";
          document.getElementById("profileHeadingEmail").textContent = "Faça login ou salve abaixo para criar seu perfil local";
          document.getElementById("profName").value = "Candidato Vestibulares";
        }
      }

      /* ---------------------------------------------------------
         13. INICIALIZAÇÃO
      --------------------------------------------------------- */
      window.addEventListener("DOMContentLoaded", () => {
        loadState();
        updateGlobalHUD();
        renderMiniQuiz();
        populateFilterOptions();
        applyTrainFilters(true);
        renderRoadmap();
        renderAnalytics();
        renderAdminList();

        document.addEventListener("click", (e) => {
          const navBtn = e.target.closest("[data-nav]");
          if (navBtn) {
            e.preventDefault();
            navigateTo(navBtn.getAttribute("data-nav"));
          }
        });

        document.getElementById("btnMobileMenu").addEventListener("click", () => {
          document.getElementById("mobileMenuDrawer").classList.toggle("hidden");
        });

        document.getElementById("btnResetMiniQuiz").addEventListener("click", () => {
          miniQuizIndex = 0;
          miniQuizAnswers = {};
          renderMiniQuiz();
        });

        ["filterSubject", "filterExam", "filterDifficulty", "filterStatus"].forEach((id) => {
          document.getElementById(id).addEventListener("change", () => applyTrainFilters(true));
        });
        document.getElementById("filterSearch").addEventListener("input", () => applyTrainFilters(true));

        document.getElementById("btnQuickReviewWrong").addEventListener("click", () => {
          document.getElementById("filterStatus").value = "WRONG";
          applyTrainFilters(true);
        });

        document.getElementById("btnClearFilters").addEventListener("click", () => {
          document.getElementById("filterSubject").value = "ALL";
          document.getElementById("filterExam").value = "ALL";
          document.getElementById("filterDifficulty").value = "ALL";
          document.getElementById("filterStatus").value = "ALL";
          document.getElementById("filterSearch").value = "";
          applyTrainFilters(true);
        });

        document.addEventListener("keydown", (e) => {
          const trainActive = document.getElementById("view-treinar").classList.contains("active");
          const modalOpen = !document.getElementById("authModal").classList.contains("hidden");
          if (!trainActive || modalOpen) return;
          if (["INPUT", "TEXTAREA", "SELECT"].includes(document.activeElement?.tagName)) return;

          const keyMap = { a: 0, b: 1, c: 2, d: 3, e: 4 };
          const k = e.key.toLowerCase();
          if (k in keyMap && trainFiltered.length) {
            const q = trainFiltered[trainCurrentIndex];
            if (!progress.answered[q.id]) {
              trainPendingSelection = keyMap[k];
              renderTrainQuestion();
            }
          } else if (e.key === "Enter" && trainFiltered.length) {
            const q = trainFiltered[trainCurrentIndex];
            if (!progress.answered[q.id] && trainPendingSelection !== null) {
              confirmCurrentTrainQuestion();
            } else if (progress.answered[q.id] && trainCurrentIndex < trainFiltered.length - 1) {
              trainCurrentIndex++;
              trainPendingSelection = null;
              renderTrainQuestion();
            }
          }
        });

        document.querySelectorAll(".sim-mode-card").forEach((card) => {
          card.addEventListener("click", () => {
            document.querySelectorAll(".sim-mode-card").forEach((c) => {
              c.classList.remove("border-blue-500", "bg-blue-500/15");
              c.classList.add("border-transparent");
            });
            card.classList.remove("border-transparent");
            card.classList.add("border-blue-500", "bg-blue-500/15");
            card.querySelector("input").checked = true;
          });
        });

        document.getElementById("btnStartSimulado").addEventListener("click", startSimulado);
        document.getElementById("btnFinishSimulado").addEventListener("click", () => finishSimulado(false));
        document.getElementById("btnAbortSimulado").addEventListener("click", () => {
          if (simState.timerId) clearInterval(simState.timerId);
          simState.active = false;
          document.getElementById("simActivePanel").classList.add("hidden");
          document.getElementById("simConfigPanel").classList.remove("hidden");
          showToast("Simulado cancelado", "Você voltou para a tela de configuração.", "info");
        });

        document.getElementById("btnExportStats").addEventListener("click", () => {
          const solved = Object.keys(progress.answered).length;
          const hits = Object.values(progress.answered).filter((a) => a.correct).length;
          const text = [
            "=== RELATÓRIO DE DESEMPENHO · KLICK KWIZ VESTIBULARES ===",
            `Data: ${new Date().toLocaleString("pt-BR")}`,
            `Estudante: ${currentUser ? currentUser.name : "Visitante"}`,
            `Questões Resolvidas: ${solved}`,
            `Acertos: ${hits} (${solved ? Math.round((hits / solved) * 100) : 0}%)`,
            `Ofensiva: ${progress.streak || 1} dia(s)`,
            `Simulados Concluídos: ${progress.simHistory.length}`
          ].join("\n");

          const blob = new Blob([text], { type: "text/plain;charset=utf-8" });
          const url = URL.createObjectURL(blob);
          const a = document.createElement("a");
          a.href = url;
          a.download = "relatorio-klick-kwiz-vestibulares.txt";
          a.click();
          URL.revokeObjectURL(url);
          showToast("Relatório exportado!", "Arquivo .txt baixado com sucesso.", "success");
        });

        document.getElementById("btnResetStats").addEventListener("click", () => {
          progress = JSON.parse(JSON.stringify(defaultProgress));
          saveProgress();
          renderAnalytics();
          renderRoadmap();
          applyTrainFilters(true);
          showToast("Histórico zerado", "Suas estatísticas foram reiniciadas.", "info");
        });

        document.getElementById("adminNewQuestionForm").addEventListener("submit", (e) => {
          e.preventDefault();
          const newQ = {
            id: "custom_" + Date.now(),
            subject: document.getElementById("admSubject").value,
            exam: document.getElementById("admExam").value.trim(),
            difficulty: document.getElementById("admDifficulty").value,
            context: document.getElementById("admContext").value.trim(),
            prompt: document.getElementById("admPrompt").value.trim(),
            options: [
              document.getElementById("admOpt0").value.trim(),
              document.getElementById("admOpt1").value.trim(),
              document.getElementById("admOpt2").value.trim(),
              document.getElementById("admOpt3").value.trim(),
              document.getElementById("admOpt4").value.trim()
            ],
            answer: Number(document.getElementById("admAnswer").value),
            explanation: document.getElementById("admExplanation").value.trim(),
            isCustom: true
          };

          questions.push(newQ);
          saveCustomQuestions();
          updateGlobalHUD();
          renderAdminList();
          e.target.reset();
          document.getElementById("admExam").value = "Vestibular 2026/2";
          showToast("Questão cadastrada! 🎉", "Já está disponível na aba Treinar e nos Simulados.", "success");
        });

        document.getElementById("profileForm").addEventListener("submit", (e) => {
          e.preventDefault();
          currentUser = {
            name: document.getElementById("profName").value.trim(),
            email: currentUser?.email || "estudante@vestibulares.com.br",
            course: document.getElementById("profCourse").value,
            campus: document.getElementById("profCampus").value.trim(),
            dailyGoal: Number(document.getElementById("profDailyGoal").value) || 10
          };
          localStorage.setItem(STORAGE_KEYS.USER, JSON.stringify(currentUser));
          updateGlobalHUD();
          renderProfileView();
          showToast("Perfil atualizado!", "Suas preferências de curso e meta foram salvas.", "success");
        });

        document.getElementById("contactForm").addEventListener("submit", (e) => {
          e.preventDefault();
          e.target.reset();
          showToast("Mensagem enviada!", "Obrigado pelo contato com a equipe Klick Kwiz.", "success");
        });

        const authModal = document.getElementById("authModal");
        const loginForm = document.getElementById("loginForm");
        const registerForm = document.getElementById("registerForm");
        const tabLogin = document.getElementById("tabLogin");
        const tabRegister = document.getElementById("tabRegister");

        document.getElementById("btnOpenAuth").addEventListener("click", () => {
          authModal.classList.remove("hidden");
          authModal.classList.add("flex");
        });

        document.getElementById("btnCloseAuth").addEventListener("click", () => {
          authModal.classList.add("hidden");
          authModal.classList.remove("flex");
        });

        tabLogin.addEventListener("click", () => {
          tabLogin.classList.add("bg-blue-600", "text-white");
          tabRegister.classList.remove("bg-blue-600", "text-white");
          loginForm.classList.remove("hidden");
          registerForm.classList.add("hidden");
        });

        tabRegister.addEventListener("click", () => {
          tabRegister.classList.add("bg-blue-600", "text-white");
          tabLogin.classList.remove("bg-blue-600", "text-white");
          registerForm.classList.remove("hidden");
          loginForm.classList.add("hidden");
        });

        document.getElementById("btnFillDemo").addEventListener("click", () => {
          currentUser = {
            name: "Lucas Almeida",
            email: "lucas.vestibulares@demo.com",
            course: "Análise e Desenvolvimento de Sistemas (ADS)",
            campus: "Campus Principal",
            dailyGoal: 10
          };
          localStorage.setItem(STORAGE_KEYS.USER, JSON.stringify(currentUser));
          authModal.classList.add("hidden");
          authModal.classList.remove("flex");
          updateGlobalHUD();
          showToast("Bem-vindo, Lucas!", "Conta demonstrativa conectada com sucesso.", "success");
        });

        loginForm.addEventListener("submit", (e) => {
          e.preventDefault();
          const email = document.getElementById("loginEmail").value.trim();
          const nameFromEmail = email.split("@")[0].replace(/[._]/g, " ").replace(/\b\w/g, (l) => l.toUpperCase());
          currentUser = {
            name: nameFromEmail || "Estudante Vestibulares",
            email,
            course: "Análise e Desenvolvimento de Sistemas (ADS)",
            campus: "Geral",
            dailyGoal: 10
          };
          localStorage.setItem(STORAGE_KEYS.USER, JSON.stringify(currentUser));
          authModal.classList.add("hidden");
          authModal.classList.remove("flex");
          updateGlobalHUD();
          showToast("Sessão iniciada!", `Bem-vindo(a), ${currentUser.name}.`, "success");
        });

        registerForm.addEventListener("submit", (e) => {
          e.preventDefault();
          currentUser = {
            name: document.getElementById("regName").value.trim(),
            email: document.getElementById("regEmail").value.trim(),
            course: document.getElementById("regCourse").value,
            campus: "Geral",
            dailyGoal: 10
          };
          localStorage.setItem(STORAGE_KEYS.USER, JSON.stringify(currentUser));
          authModal.classList.add("hidden");
          authModal.classList.remove("flex");
          updateGlobalHUD();
          showToast("Conta criada com sucesso! 🚀", `Foco total na aprovação em ${currentUser.course}.`, "success");
        });

        document.getElementById("btnLogout").addEventListener("click", () => {
          currentUser = null;
          localStorage.removeItem(STORAGE_KEYS.USER);
          updateGlobalHUD();
          navigateTo("inicio");
          showToast("Sessão encerrada", "Você voltou para o modo visitante.", "info");
        });
      });
    })();
  </script>
</body>
</html>
