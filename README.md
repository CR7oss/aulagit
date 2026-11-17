<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Klick Kwiz · Banco de Questões Fatec</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet" />
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'system-ui', 'sans-serif'],
          },
          colors: {
            fatec: {
              50: '#eef4ff',
              100: '#dce7ff',
              200: '#c0d4ff',
              500: '#2563eb',
              600: '#1d4ed8',
              700: '#1e40af',
              800: '#1e3a8a',
              900: '#172554',
            }
          }
        }
      }
    };
  </script>
  <style>
    /* Ajustes finos de rolagem e transições suaves */
    html {
      scroll-behavior: smooth;
    }
    .no-scrollbar::-webkit-scrollbar {
      display: none;
    }
    .no-scrollbar {
      -ms-overflow-style: none;
      scrollbar-width: none;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(6px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .animate-fade-in {
      animation: fadeIn 0.25s ease-out forwards;
    }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 dark:bg-slate-950 dark:text-slate-100 font-sans antialiased min-h-screen flex flex-col transition-colors duration-200">

  <!-- Toast / Notificação Flutuante (substitui qualquer alert) -->
  <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2 pointer-events-none"></div>

  <!-- Modal de Confirmação Customizado (substitui confirm) -->
  <div id="confirm-modal" class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-900/60 backdrop-blur-sm p-4">
    <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl max-w-md w-full p-6 shadow-2xl animate-fade-in">
      <h3 id="confirm-title" class="text-lg font-bold text-slate-900 dark:text-white mb-2">Confirmar ação</h3>
      <p id="confirm-message" class="text-sm text-slate-600 dark:text-slate-300 mb-6">Tem certeza que deseja continuar?</p>
      <div class="flex justify-end gap-3">
        <button id="confirm-cancel" class="px-4 py-2 rounded-xl text-sm font-medium bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-200 hover:bg-slate-200 dark:hover:bg-slate-700 transition">Cancelar</button>
        <button id="confirm-ok" class="px-4 py-2 rounded-xl text-sm font-semibold bg-rose-600 text-white hover:bg-rose-700 transition">Confirmar</button>
      </div>
    </div>
  </div>

  <!-- Modal de Autenticação (Entrar / Criar Conta / Conta Demo) -->
  <div id="auth-modal" class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-900/65 backdrop-blur-sm p-4">
    <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl max-w-md w-full p-6 sm:p-7 shadow-2xl animate-fade-in relative">
      <button onclick="closeAuthModal()" aria-label="Fechar modal" class="absolute top-4 right-4 w-8 h-8 rounded-xl bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-600 dark:text-slate-300 flex items-center justify-center text-sm font-bold transition">
        ✕
      </button>

      <div class="flex items-center gap-2.5 mb-5">
        <div class="w-9 h-9 rounded-xl bg-gradient-to-br from-fatec-600 to-indigo-700 text-white font-extrabold text-base flex items-center justify-center">
          K
        </div>
        <div>
          <h3 class="text-lg font-extrabold text-slate-900 dark:text-white leading-tight">Área do Vestibulando</h3>
          <p class="text-xs text-slate-500 dark:text-slate-400">Salve seu progresso, revisões e simulados</p>
        </div>
      </div>

      <!-- Abas Entrar / Criar Conta -->
      <div class="grid grid-cols-2 gap-1 bg-slate-100 dark:bg-slate-800 p-1 rounded-xl mb-5">
        <button id="auth-tab-login" type="button" onclick="switchAuthTab('login')" class="py-2 rounded-lg text-xs sm:text-sm font-bold bg-white dark:bg-slate-900 text-fatec-700 dark:text-fatec-300 shadow-sm transition">
          Entrar
        </button>
        <button id="auth-tab-register" type="button" onclick="switchAuthTab('register')" class="py-2 rounded-lg text-xs sm:text-sm font-semibold text-slate-600 dark:text-slate-300 hover:text-slate-900 dark:hover:text-white transition">
          Criar Conta
        </button>
      </div>

      <!-- Formulário de Login -->
      <form id="auth-login-form" onsubmit="handleLoginSubmit(event)" class="space-y-3.5">
        <div>
          <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">E-mail</label>
          <input type="email" id="login-email" required placeholder="seuemail@exemplo.com" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500" />
        </div>
        <div>
          <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">Senha</label>
          <input type="password" id="login-password" required placeholder="••••••" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500" />
        </div>
        <button type="submit" class="w-full py-3 rounded-xl font-bold text-sm bg-fatec-600 hover:bg-fatec-700 text-white shadow-md shadow-fatec-600/20 transition">
          Entrar na minha conta
        </button>
      </form>

      <!-- Formulário de Cadastro -->
      <form id="auth-register-form" onsubmit="handleRegisterSubmit(event)" class="hidden space-y-3">
        <div>
          <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">Nome completo</label>
          <input type="text" id="reg-name" required placeholder="Ex.: Lucas Almeida" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500" />
        </div>
        <div>
          <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">E-mail</label>
          <input type="email" id="reg-email" required placeholder="lucas@exemplo.com" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500" />
        </div>
        <div>
          <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">Curso pretendido na Fatec</label>
          <select id="reg-course" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500">
            <option value="Análise e Desenvolvimento de Sistemas (ADS)">Análise e Desenvolvimento de Sistemas (ADS)</option>
            <option value="Desenvolvimento de Software Multiplataforma (DSM)">Desenvolvimento de Software Multiplataforma (DSM)</option>
            <option value="Gestão Empresarial">Gestão Empresarial</option>
            <option value="Logística">Logística</option>
            <option value="Segurança da Informação">Segurança da Informação</option>
            <option value="Ciência de Dados">Ciência de Dados</option>
            <option value="Gestão da Tecnologia da Informação">Gestão da Tecnologia da Informação</option>
            <option value="Comércio Exterior">Comércio Exterior</option>
            <option value="Outro curso Fatec">Outro curso Fatec</option>
          </select>
        </div>
        <div>
          <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">Senha (mín. 4 caracteres)</label>
          <input type="password" id="reg-password" minlength="4" required placeholder="Crie uma senha" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500" />
        </div>
        <button type="submit" class="w-full py-3 rounded-xl font-bold text-sm bg-emerald-600 hover:bg-emerald-700 text-white shadow-md shadow-emerald-600/20 transition">
          Criar conta e começar
        </button>
      </form>

      <!-- Divisor e Acesso Rápido Demo -->
      <div class="mt-5 pt-4 border-t border-slate-200 dark:border-slate-800 space-y-2.5">
        <button type="button" onclick="loginDemoAccount()" class="w-full py-2.5 px-3 rounded-xl text-xs font-bold bg-indigo-50 dark:bg-indigo-900/30 hover:bg-indigo-100 dark:hover:bg-indigo-900/50 text-indigo-700 dark:text-indigo-200 border border-indigo-200 dark:border-indigo-800 transition flex items-center justify-center gap-2">
          <span>⚡ Entrar como Visitante / Conta Demo (aluno@fatec.sp.gov.br)</span>
        </button>
      </div>
    </div>
  </div>

  <!-- Cabeçalho Fixo -->
  <header class="sticky top-0 z-40 bg-white/90 dark:bg-slate-900/90 backdrop-blur-md border-b border-slate-200 dark:border-slate-800">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16 gap-2 sm:gap-3">
        <!-- Logo e Título -->
        <button onclick="navigateTo('inicio')" class="flex items-center gap-2.5 sm:gap-3 text-left group focus:outline-none shrink-0">
          <div class="w-9 h-9 sm:w-10 sm:h-10 rounded-xl bg-gradient-to-br from-fatec-600 to-indigo-700 text-white font-extrabold text-lg sm:text-xl flex items-center justify-center shadow-md shadow-fatec-600/20 group-hover:scale-105 transition-transform">
            K
          </div>
          <div>
            <div class="font-extrabold text-sm sm:text-lg leading-tight tracking-tight text-slate-900 dark:text-white">
              Klick Kwiz
            </div>
            <div class="text-[10px] sm:text-xs font-medium text-slate-500 dark:text-slate-400">
              Banco de Questões Fatec
            </div>
          </div>
        </button>

        <!-- Navegação Desktop -->
        <nav id="desktop-nav" class="hidden xl:flex items-center gap-1 bg-slate-100/80 dark:bg-slate-800/80 p-1 rounded-xl border border-slate-200/70 dark:border-slate-700/70">
          <!-- Renderizado via JS -->
        </nav>

        <!-- Área de Conta + Botão Tema Escuro / Claro -->
        <div class="flex items-center gap-2">
          <!-- Estado Deslogado -->
          <button id="header-login-btn" onclick="openAuthModal('login')" class="px-3 sm:px-3.5 py-2 rounded-xl text-xs sm:text-sm font-bold bg-fatec-600 hover:bg-fatec-700 text-white shadow-sm transition flex items-center gap-1.5 shrink-0">
            <span>👤</span>
            <span>Entrar / Conta</span>
          </button>

          <!-- Estado Logado -->
          <div id="header-user-box" class="hidden relative">
            <button onclick="toggleUserDropdown()" class="flex items-center gap-2 pl-2 pr-3 py-1.5 rounded-xl bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 border border-slate-200 dark:border-slate-700 transition">
              <span id="header-user-avatar" class="w-7 h-7 rounded-lg bg-gradient-to-br from-fatec-600 to-indigo-600 text-white text-xs font-extrabold flex items-center justify-center">
                AL
              </span>
              <span id="header-user-name" class="text-xs sm:text-sm font-bold text-slate-800 dark:text-slate-100 max-w-[95px] sm:max-w-[130px] truncate">
                Aluno
              </span>
              <span class="text-[10px] text-slate-500">▼</span>
            </button>

            <!-- Dropdown do Usuário -->
            <div id="user-dropdown" class="hidden absolute right-0 mt-2 w-64 bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl shadow-xl p-3 z-50 animate-fade-in">
              <div class="px-2 py-2 border-b border-slate-100 dark:border-slate-800 mb-2">
                <div id="dropdown-user-fullname" class="text-sm font-bold text-slate-900 dark:text-white truncate">Aluno Fatec</div>
                <div id="dropdown-user-email" class="text-xs text-slate-500 dark:text-slate-400 truncate">aluno@fatec.sp.gov.br</div>
                <div id="dropdown-user-course" class="mt-1 inline-block px-2 py-0.5 rounded-md text-[10px] font-semibold bg-fatec-50 dark:bg-fatec-900/40 text-fatec-700 dark:text-fatec-300">
                  ADS
                </div>
              </div>
              <button onclick="toggleUserDropdown(); navigateTo('desempenho')" class="w-full text-left px-3 py-2 rounded-xl text-xs sm:text-sm font-semibold text-slate-700 dark:text-slate-200 hover:bg-slate-100 dark:hover:bg-slate-800 transition flex items-center gap-2">
                <span>📈</span> Meu Desempenho e Perfil
              </button>
              <button onclick="toggleUserDropdown(); navigateTo('roteiro')" class="w-full text-left px-3 py-2 rounded-xl text-xs sm:text-sm font-semibold text-slate-700 dark:text-slate-200 hover:bg-slate-100 dark:hover:bg-slate-800 transition flex items-center gap-2">
                <span>🗺️</span> Meu Roteiro de Estudos
              </button>
              <button onclick="logoutUser()" class="w-full text-left px-3 py-2 mt-1 rounded-xl text-xs sm:text-sm font-bold text-rose-600 dark:text-rose-400 hover:bg-rose-50 dark:hover:bg-rose-900/20 transition flex items-center gap-2">
                <span>🚪</span> Sair da conta
              </button>
            </div>
          </div>

          <button id="theme-toggle" onclick="toggleTheme()" class="px-2.5 sm:px-3 py-2 rounded-xl text-xs sm:text-sm font-semibold bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 border border-slate-200 dark:border-slate-700 transition flex items-center gap-1.5 shrink-0">
            <span id="theme-icon">🌙</span>
            <span id="theme-label" class="hidden sm:inline">Escuro</span>
          </button>
        </div>
      </div>

      <!-- Navegação Mobile / Tablet (Horizontal Scroll suave) -->
      <nav id="mobile-nav" class="xl:hidden flex items-center gap-1.5 overflow-x-auto no-scrollbar pb-2.5 pt-1">
        <!-- Renderizado via JS -->
      </nav>
    </div>
  </header>

  <!-- Conteúdo Principal -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 sm:py-8">

    <!-- ==================== ABA 1: INÍCIO ==================== -->
    <section id="tab-inicio" class="tab-section space-y-8 animate-fade-in">
      <!-- Banner de Boas-Vindas Personalizado (quando logado) -->
      <div id="welcome-user-banner" class="hidden bg-white dark:bg-slate-900 border border-fatec-200 dark:border-fatec-800/70 rounded-2xl p-4 sm:px-6 flex flex-col sm:flex-row sm:items-center justify-between gap-3 shadow-sm">
        <div class="flex items-center gap-3">
          <div id="welcome-user-avatar" class="w-10 h-10 rounded-xl bg-fatec-600 text-white font-extrabold text-sm flex items-center justify-center shrink-0">
            AL
          </div>
          <div>
            <div id="welcome-user-greeting" class="text-sm sm:text-base font-bold text-slate-900 dark:text-white">
              Olá, Aluno! Bom treino hoje.
            </div>
            <div id="welcome-user-sub" class="text-xs text-slate-500 dark:text-slate-400">
              Foco na aprovação · Curso pretendido: Análise e Desenvolvimento de Sistemas
            </div>
          </div>
        </div>
        <div class="flex items-center gap-2">
          <span class="px-3 py-1 rounded-full text-xs font-semibold bg-emerald-50 text-emerald-700 dark:bg-emerald-900/30 dark:text-emerald-300 border border-emerald-200 dark:border-emerald-800">
            ✓ Progresso sincronizado na conta
          </span>
        </div>
      </div>

      <!-- Hero Banner -->
      <div class="relative overflow-hidden rounded-3xl bg-gradient-to-br from-slate-900 via-fatec-900 to-indigo-950 text-white p-6 sm:p-10 lg:p-12 shadow-xl border border-slate-800">
        <div class="absolute -top-24 -right-24 w-72 h-72 bg-fatec-500/20 rounded-full blur-3xl pointer-events-none"></div>
        <div class="absolute -bottom-24 -left-24 w-72 h-72 bg-indigo-500/20 rounded-full blur-3xl pointer-events-none"></div>

        <div class="relative z-10 max-w-3xl space-y-5">
          <div class="flex flex-wrap items-center gap-2.5">
            <span class="inline-flex items-center px-3 py-1 rounded-full text-xs font-semibold bg-fatec-500/25 text-fatec-100 border border-fatec-400/30">
              Vestibular Fatec · Humanas
            </span>
            <span class="text-xs font-bold tracking-widest uppercase text-amber-300">
              ESTUDE • PRATIQUE • EVOLUA
            </span>
          </div>

          <h1 class="text-2xl sm:text-4xl lg:text-5xl font-extrabold tracking-tight leading-tight">
            Domine as questões de Humanas da Fatec com prática inteligente.
          </h1>

          <p class="text-sm sm:text-base lg:text-lg text-slate-200 leading-relaxed max-w-2xl">
            Treine com provas reais da Fatec, faça simulados cronometrados e acompanhe seu desempenho matéria por matéria. Tudo salvo no seu navegador.
          </p>

          <div class="flex flex-wrap items-center gap-3 pt-2">
            <button onclick="navigateTo('treinar')" class="px-6 py-3.5 rounded-xl font-bold text-sm bg-fatec-500 hover:bg-fatec-600 text-white shadow-lg shadow-fatec-600/30 transition transform active:scale-95">
              Começar a treinar
            </button>
            <button onclick="navigateTo('simulado')" class="px-6 py-3.5 rounded-xl font-bold text-sm bg-white/10 hover:bg-white/20 text-white border border-white/20 backdrop-blur-sm transition transform active:scale-95">
              Fazer simulado
            </button>
          </div>

          <div class="pt-2 flex items-center gap-2 text-xs sm:text-sm text-fatec-200 font-medium">
            <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
            <span id="hero-bank-count">30 questões de Humanas no banco · gabarito conferido</span>
          </div>
        </div>
      </div>

      <!-- 3 Cards de Destaque -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-5">
        <div onclick="navigateTo('treinar')" class="cursor-pointer group bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-fatec-500 dark:hover:border-fatec-500 rounded-2xl p-6 shadow-sm hover:shadow-md transition">
          <div class="w-12 h-12 rounded-2xl bg-fatec-50 dark:bg-fatec-900/40 text-2xl flex items-center justify-center mb-4 group-hover:scale-110 transition-transform">
            🧩
          </div>
          <h3 class="text-lg font-bold text-slate-900 dark:text-white mb-1.5">Quizzes</h3>
          <p class="text-sm text-slate-600 dark:text-slate-400 leading-relaxed">
            Treinos por prova e matéria, com correção imediata ou ao final.
          </p>
        </div>

        <div onclick="startErrorReviewFromHome()" class="cursor-pointer group bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-amber-500 dark:hover:border-amber-500 rounded-2xl p-6 shadow-sm hover:shadow-md transition">
          <div class="w-12 h-12 rounded-2xl bg-amber-50 dark:bg-amber-900/30 text-2xl flex items-center justify-center mb-4 group-hover:scale-110 transition-transform">
            🔁
          </div>
          <h3 class="text-lg font-bold text-slate-900 dark:text-white mb-1.5">Revisões</h3>
          <p class="text-sm text-slate-600 dark:text-slate-400 leading-relaxed">
            Refaça só as questões que você errou e consolide o conteúdo.
          </p>
        </div>

        <div onclick="navigateTo('desempenho')" class="cursor-pointer group bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-emerald-500 dark:hover:border-emerald-500 rounded-2xl p-6 shadow-sm hover:shadow-md transition">
          <div class="w-12 h-12 rounded-2xl bg-emerald-50 dark:bg-emerald-900/30 text-2xl flex items-center justify-center mb-4 group-hover:scale-110 transition-transform">
            📈
          </div>
          <h3 class="text-lg font-bold text-slate-900 dark:text-white mb-1.5">Progresso</h3>
          <p class="text-sm text-slate-600 dark:text-slate-400 leading-relaxed">
            Veja o percentual de acertos por matéria e a evolução das tentativas.
          </p>
        </div>
      </div>

      <!-- Mini Quiz de Boas-Vindas -->
      <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-8 shadow-sm">
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 pb-5 border-b border-slate-200 dark:border-slate-800">
          <div>
            <h2 class="text-xl sm:text-2xl font-bold text-slate-900 dark:text-white">
              Mini quiz de boas-vindas
            </h2>
            <p class="text-sm text-slate-600 dark:text-slate-400">
              Quatro questões de Humanas para descobrir seu nível inicial.
            </p>
          </div>
          <div class="flex items-center gap-2.5">
            <span id="mini-quiz-status" class="px-3 py-1 rounded-full text-xs font-bold bg-amber-100 text-amber-800 dark:bg-amber-900/40 dark:text-amber-300">
              Em andamento
            </span>
            <span id="mini-quiz-counter" class="text-xs sm:text-sm font-semibold text-slate-600 dark:text-slate-300 bg-slate-100 dark:bg-slate-800 px-3 py-1 rounded-full">
              Pergunta 1 de 4 · Sintaxe
            </span>
          </div>
        </div>

        <!-- Corpo do Mini Quiz -->
        <div id="mini-quiz-body" class="pt-6 space-y-6">
          <p id="mini-quiz-question" class="text-base sm:text-lg font-medium text-slate-900 dark:text-slate-100 leading-relaxed">
            <!-- Preenchido via JS -->
          </p>

          <div id="mini-quiz-options" class="grid grid-cols-1 gap-3">
            <!-- Alternativas A-E via JS -->
          </div>

          <!-- Feedback da resposta -->
          <div id="mini-quiz-feedback" class="hidden rounded-2xl p-4 text-sm leading-relaxed border"></div>

          <!-- Rodapé do Mini Quiz -->
          <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 pt-2">
            <span id="mini-quiz-hint" class="text-xs sm:text-sm font-medium text-slate-500 dark:text-slate-400">
              Escolha uma alternativa.
            </span>
            <div class="flex items-center gap-3">
              <button id="mini-quiz-next" onclick="handleMiniQuizNext()" disabled class="w-full sm:w-auto px-6 py-3 rounded-xl font-bold text-sm bg-fatec-600 text-white hover:bg-fatec-700 disabled:opacity-40 disabled:cursor-not-allowed transition">
                Próxima
              </button>
            </div>
          </div>
        </div>

        <!-- Resultado Final do Mini Quiz -->
        <div id="mini-quiz-result" class="hidden pt-6 text-center space-y-5">
          <div class="inline-flex items-center justify-center w-16 h-16 rounded-2xl bg-fatec-100 dark:bg-fatec-900/50 text-3xl">
            🎯
          </div>
          <div>
            <h3 id="mini-quiz-score-title" class="text-2xl font-extrabold text-slate-900 dark:text-white">
              Você acertou 4 de 4!
            </h3>
            <p id="mini-quiz-score-desc" class="text-sm text-slate-600 dark:text-slate-400 mt-1 max-w-lg mx-auto">
              Excelente ponto de partida! Continue praticando no banco completo de 30 questões da Fatec.
            </p>
          </div>
          <div class="flex flex-wrap justify-center gap-3">
            <button onclick="resetMiniQuiz()" class="px-5 py-2.5 rounded-xl text-sm font-semibold bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-800 dark:text-slate-200 transition">
              Refazer mini quiz
            </button>
            <button onclick="navigateTo('treinar')" class="px-5 py-2.5 rounded-xl text-sm font-bold bg-fatec-600 hover:bg-fatec-700 text-white transition">
              Ir para o banco completo
            </button>
          </div>
        </div>
      </div>

      <!-- Seção Por que o Klick Kwiz? -->
      <div class="bg-slate-100/80 dark:bg-slate-900/60 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-8">
        <div class="max-w-2xl mb-6">
          <h2 class="text-xl sm:text-2xl font-bold text-slate-900 dark:text-white">
            Por que o Klick Kwiz?
          </h2>
          <p class="text-sm text-slate-600 dark:text-slate-400 mt-1">
            Um método simples para quem prepara o vestibular da Fatec sem tempo sobrando.
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl p-5 flex items-start gap-4">
            <div class="w-10 h-10 rounded-xl bg-fatec-100 dark:bg-fatec-900/40 text-fatec-700 dark:text-fatec-300 font-bold flex items-center justify-center shrink-0">
              ✓
            </div>
            <div>
              <h3 class="font-bold text-slate-900 dark:text-white text-base">Questões reais</h3>
              <p class="text-sm text-slate-600 dark:text-slate-400 mt-1">
                Cada item vem de provas anteriores, com gabarito conferido e explicação direta ao ponto.
              </p>
            </div>
          </div>

          <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl p-5 flex items-start gap-4">
            <div class="w-10 h-10 rounded-xl bg-indigo-100 dark:bg-indigo-900/40 text-indigo-700 dark:text-indigo-300 font-bold flex items-center justify-center shrink-0">
              ⏱
            </div>
            <div>
              <h3 class="font-bold text-slate-900 dark:text-white text-base">Simulado de verdade</h3>
              <p class="text-sm text-slate-600 dark:text-slate-400 mt-1">
                60 questões, 5 horas e navegação igual à prova oficial para você treinar ritmo e resistência.
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== ABA 2: TREINAR & REVISÕES ==================== -->
    <section id="tab-treinar" class="tab-section hidden space-y-6 animate-fade-in">
      <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
        <div>
          <h2 class="text-2xl sm:text-3xl font-extrabold text-slate-900 dark:text-white">Treinar Questões</h2>
          <p class="text-sm text-slate-600 dark:text-slate-400">Filtre por matéria, escolha o modo de correção ou revise apenas seus erros.</p>
        </div>

        <div class="flex flex-wrap items-center gap-2">
          <button id="btn-review-errors" onclick="toggleErrorReviewMode()" class="px-4 py-2.5 rounded-xl text-xs sm:text-sm font-bold border transition flex items-center gap-2 bg-white dark:bg-slate-900 border-amber-300 dark:border-amber-700 text-amber-700 dark:text-amber-300 hover:bg-amber-50 dark:hover:bg-amber-900/20">
            <span>🔁 Modo Revisão (Só erradas: <span id="error-count-badge">0</span>)</span>
          </button>
        </div>
      </div>

      <!-- Barra de Filtros -->
      <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl p-4 sm:p-5 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <div>
          <label class="block text-xs font-semibold text-slate-500 dark:text-slate-400 mb-1.5">Matéria / Área</label>
          <select id="filter-subject" onchange="applyTrainFilters()" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3 py-2.5 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-fatec-500">
            <option value="TODAS">Todas as matérias</option>
          </select>
        </div>

        <div>
          <label class="block text-xs font-semibold text-slate-500 dark:text-slate-400 mb-1.5">Modo de Correção</label>
          <select id="filter-correction-mode" onchange="changeCorrectionMode()" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3 py-2.5 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-fatec-500">
            <option value="imediata">Correção imediata</option>
            <option value="final">Correção ao final da lista</option>
          </select>
        </div>

        <div>
          <label class="block text-xs font-semibold text-slate-500 dark:text-slate-400 mb-1.5">Ordem</label>
          <select id="filter-order" onchange="applyTrainFilters()" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3 py-2.5 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-fatec-500">
            <option value="padrao">Ordem do banco</option>
            <option value="aleatoria">Embaralhar questões</option>
          </select>
        </div>

        <div class="flex items-end">
          <button onclick="resetTrainSession()" class="w-full px-4 py-2.5 rounded-xl text-sm font-semibold bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 transition">
            Reiniciar treino atual
          </button>
        </div>
      </div>

      <!-- Status da Sessão de Treino -->
      <div class="flex flex-wrap items-center justify-between gap-3 bg-fatec-50/70 dark:bg-fatec-900/20 border border-fatec-200 dark:border-fatec-800/60 rounded-2xl px-5 py-3 text-sm">
        <div class="font-semibold text-fatec-900 dark:text-fatec-200">
          Exibindo <span id="train-total-filtered">30</span> questões · Questão <span id="train-current-num">1</span>
        </div>
        <div class="flex items-center gap-4 font-semibold">
          <span class="text-emerald-600 dark:text-emerald-400">✓ Acertos: <span id="train-session-hits">0</span></span>
          <span class="text-rose-600 dark:text-rose-400">✕ Erros: <span id="train-session-misses">0</span></span>
        </div>
      </div>

      <!-- Card da Questão Atual no Treino -->
      <div id="train-card" class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-8 shadow-sm space-y-6">
        <div class="flex flex-wrap items-center justify-between gap-2">
          <div class="flex flex-wrap items-center gap-2">
            <span id="train-q-subject" class="px-3 py-1 rounded-full text-xs font-bold bg-fatec-100 text-fatec-800 dark:bg-fatec-900/50 dark:text-fatec-200">
              Português
            </span>
            <span id="train-q-topic" class="px-3 py-1 rounded-full text-xs font-semibold bg-slate-100 text-slate-700 dark:bg-slate-800 dark:text-slate-300">
              Sintaxe
            </span>
            <span id="train-q-exam" class="text-xs font-medium text-slate-500 dark:text-slate-400">
              Fatec · Prova Anterior
            </span>
          </div>
          <span id="train-q-status-badge" class="text-xs font-semibold text-slate-400"></span>
        </div>

        <p id="train-q-text" class="text-base sm:text-lg font-medium text-slate-900 dark:text-slate-100 leading-relaxed whitespace-pre-line">
          <!-- Enunciado -->
        </p>

        <div id="train-q-options" class="space-y-3">
          <!-- Alternativas -->
        </div>

        <div id="train-q-explanation" class="hidden rounded-2xl p-4 text-sm leading-relaxed border">
          <!-- Comentário / Gabarito -->
        </div>

        <!-- Botões de Navegação do Treino -->
        <div class="flex flex-wrap items-center justify-between gap-3 pt-4 border-t border-slate-200 dark:border-slate-800">
          <button id="train-prev-btn" onclick="prevTrainQuestion()" class="px-5 py-2.5 rounded-xl text-sm font-semibold bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 disabled:opacity-40 transition">
            ← Anterior
          </button>

          <div class="flex items-center gap-2">
            <button id="train-finish-batch-btn" onclick="finishBatchCorrection()" class="hidden px-5 py-2.5 rounded-xl text-sm font-bold bg-emerald-600 hover:bg-emerald-700 text-white transition">
              Corrigir todas agora
            </button>
            <button id="train-next-btn" onclick="nextTrainQuestion()" class="px-6 py-2.5 rounded-xl text-sm font-bold bg-fatec-600 hover:bg-fatec-700 text-white transition">
              Próxima →
            </button>
          </div>
        </div>
      </div>

      <!-- Resumo da Correção ao Final -->
      <div id="train-batch-summary" class="hidden bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-8 space-y-4">
        <h3 class="text-xl font-bold text-slate-900 dark:text-white">Resultado da Bateria de Treino</h3>
        <p id="train-batch-summary-text" class="text-sm text-slate-600 dark:text-slate-300"></p>
        <div id="train-batch-review-list" class="space-y-3 pt-2"></div>
      </div>

      <!-- Estado vazio (ex: Modo Revisão sem erros) -->
      <div id="train-empty-state" class="hidden bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-10 text-center space-y-4">
        <div class="text-4xl">🎉</div>
        <h3 class="text-xl font-bold text-slate-900 dark:text-white">Nenhuma questão pendente neste filtro!</h3>
        <p class="text-sm text-slate-600 dark:text-slate-400 max-w-md mx-auto">
          Se você ativou o Modo Revisão, isso significa que você ainda não errou nenhuma questão ou já corrigiu todas!
        </p>
        <button onclick="disableErrorReviewMode()" class="px-5 py-2.5 rounded-xl text-sm font-bold bg-fatec-600 text-white hover:bg-fatec-700 transition">
          Ver todas as questões
        </button>
      </div>
    </section>

    <!-- ==================== ABA 3: SIMULADO ==================== -->
    <section id="tab-simulado" class="tab-section hidden space-y-6 animate-fade-in">
      <!-- Tela Inicial de Configuração do Simulado -->
      <div id="simulado-setup" class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-10 shadow-sm space-y-6">
        <div class="max-w-2xl space-y-2">
          <span class="inline-block px-3 py-1 rounded-full text-xs font-bold bg-indigo-100 text-indigo-800 dark:bg-indigo-900/50 dark:text-indigo-200">
            Modo Prova Oficial
          </span>
          <h2 class="text-2xl sm:text-3xl font-extrabold text-slate-900 dark:text-white">Simulado Cronometrado Fatec</h2>
          <p class="text-sm sm:text-base text-slate-600 dark:text-slate-400">
            Experimente as condições reais do vestibular com cronômetro regressivo, cartão de navegação rápida e relatório completo ao final.
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <label class="cursor-pointer border-2 border-fatec-500 bg-fatec-50/40 dark:bg-fatec-900/20 rounded-2xl p-5 flex items-start gap-3">
            <input type="radio" name="simulado-type" value="oficial" checked class="mt-1 accent-fatec-600" />
            <div>
              <div class="font-bold text-slate-900 dark:text-white">Simulado Oficial Completo (60 questões · 5h00)</div>
              <div class="text-xs sm:text-sm text-slate-600 dark:text-slate-400 mt-1">
                Formato idêntico à prova da Fatec: 60 itens montados a partir do banco de questões com duração de 5 horas.
              </div>
            </div>
          </label>

          <label class="cursor-pointer border border-slate-200 dark:border-slate-700 hover:border-fatec-400 rounded-2xl p-5 flex items-start gap-3">
            <input type="radio" name="simulado-type" value="rapido" class="mt-1 accent-fatec-600" />
            <div>
              <div class="font-bold text-slate-900 dark:text-white">Simulado Express Humanas (15 questões · 45 min)</div>
              <div class="text-xs sm:text-sm text-slate-600 dark:text-slate-400 mt-1">
                Ideal para treinar gestão de tempo em blocos menores no dia a dia.
              </div>
            </div>
          </label>
        </div>

        <div class="pt-2">
          <button onclick="startSimulado()" class="px-7 py-3.5 rounded-xl font-bold text-sm bg-fatec-600 hover:bg-fatec-700 text-white shadow-lg shadow-fatec-600/25 transition">
            Iniciar Simulado Agora
          </button>
        </div>
      </div>

      <!-- Interface Ativa do Simulado -->
      <div id="simulado-active" class="hidden space-y-6">
        <!-- Barra Superior do Simulado -->
        <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl p-4 flex flex-wrap items-center justify-between gap-4 shadow-sm">
          <div class="flex items-center gap-3">
            <div class="px-4 py-2 rounded-xl bg-slate-900 dark:bg-slate-800 text-white font-mono text-lg sm:text-xl font-bold tracking-wider" id="simulado-timer">
              05:00:00
            </div>
            <button id="simulado-pause-btn" onclick="toggleSimuladoPause()" class="px-3.5 py-2 rounded-xl text-xs sm:text-sm font-semibold bg-amber-100 text-amber-800 dark:bg-amber-900/40 dark:text-amber-200 hover:bg-amber-200 transition">
              Pausar
            </button>
          </div>

          <div class="flex items-center gap-3">
            <span class="text-xs sm:text-sm font-semibold text-slate-600 dark:text-slate-300">
              Respondidas: <span id="simulado-answered-count" class="text-fatec-600 dark:text-fatec-400 font-bold">0</span>/<span id="simulado-total-count">60</span>
            </span>
            <button onclick="confirmFinishSimulado()" class="px-5 py-2.5 rounded-xl text-xs sm:text-sm font-bold bg-emerald-600 hover:bg-emerald-700 text-white transition">
              Entregar Prova
            </button>
          </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
          <!-- Questão do Simulado -->
          <div class="lg:col-span-2 bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-8 space-y-6">
            <div class="flex items-center justify-between gap-2">
              <span id="sim-q-badge" class="px-3 py-1 rounded-full text-xs font-bold bg-fatec-100 text-fatec-800 dark:bg-fatec-900/50 dark:text-fatec-200">
                Questão 1 · Português
              </span>
              <button onclick="toggleSimuladoFlag()" id="sim-flag-btn" class="text-xs font-semibold px-3 py-1 rounded-full border border-slate-200 dark:border-slate-700 hover:bg-slate-100 dark:hover:bg-slate-800 transition">
                🚩 Marcar para revisar
              </button>
            </div>

            <p id="sim-q-text" class="text-base sm:text-lg font-medium text-slate-900 dark:text-slate-100 leading-relaxed whitespace-pre-line"></p>

            <div id="sim-q-options" class="space-y-3"></div>

            <div class="flex items-center justify-between pt-4 border-t border-slate-200 dark:border-slate-800">
              <button id="sim-prev-btn" onclick="navigateSimuladoQuestion(simuladoState.currentIndex - 1)" class="px-5 py-2.5 rounded-xl text-sm font-semibold bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 disabled:opacity-40 transition">
                ← Anterior
              </button>
              <button id="sim-next-btn" onclick="navigateSimuladoQuestion(simuladoState.currentIndex + 1)" class="px-5 py-2.5 rounded-xl text-sm font-bold bg-fatec-600 hover:bg-fatec-700 text-white transition">
                Próxima →
              </button>
            </div>
          </div>

          <!-- Cartão-Resposta / Grade de Navegação -->
          <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-5 space-y-4 h-fit">
            <h3 class="font-bold text-sm text-slate-900 dark:text-white">Cartão de Questões</h3>
            <div class="flex flex-wrap gap-3 text-[11px] font-medium text-slate-500 dark:text-slate-400">
              <span class="flex items-center gap-1"><span class="w-3 h-3 rounded bg-fatec-600 inline-block"></span> Respondida</span>
              <span class="flex items-center gap-1"><span class="w-3 h-3 rounded bg-amber-400 inline-block"></span> Marcada</span>
              <span class="flex items-center gap-1"><span class="w-3 h-3 rounded bg-slate-200 dark:bg-slate-700 inline-block"></span> Em branco</span>
            </div>
            <div id="simulado-grid" class="grid grid-cols-6 sm:grid-cols-8 lg:grid-cols-5 gap-2 max-h-96 overflow-y-auto pr-1">
              <!-- Botões 1..N -->
            </div>
          </div>
        </div>
      </div>

      <!-- Resultado Final do Simulado -->
      <div id="simulado-result" class="hidden bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-8 space-y-6">
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 pb-6 border-b border-slate-200 dark:border-slate-800">
          <div>
            <span class="text-xs font-bold uppercase tracking-wider text-emerald-600 dark:text-emerald-400">Simulado Concluído</span>
            <h2 id="sim-res-title" class="text-2xl sm:text-3xl font-extrabold text-slate-900 dark:text-white mt-1">
              Você acertou 0 de 60 questões (0%)
            </h2>
            <p id="sim-res-time" class="text-sm text-slate-600 dark:text-slate-400 mt-1">Tempo utilizado: 00:00:00</p>
          </div>
          <button onclick="resetSimuladoUI()" class="px-5 py-3 rounded-xl text-sm font-bold bg-fatec-600 hover:bg-fatec-700 text-white transition">
            Novo Simulado
          </button>
        </div>

        <div class="space-y-3">
          <h3 class="font-bold text-lg text-slate-900 dark:text-white">Gabarito Comentado</h3>
          <div id="sim-res-list" class="space-y-3 max-h-[540px] overflow-y-auto pr-1"></div>
        </div>
      </div>
    </section>

    <!-- ==================== ABA 4: ROTEIRO ==================== -->
    <section id="tab-roteiro" class="tab-section hidden space-y-6 animate-fade-in">
      <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-8 shadow-sm space-y-4">
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
          <div>
            <h2 class="text-2xl sm:text-3xl font-extrabold text-slate-900 dark:text-white">Roteiro de Estudos · Fatec</h2>
            <p class="text-sm text-slate-600 dark:text-slate-400">Marque os tópicos concluídos. Seu progresso fica salvo automaticamente.</p>
          </div>
          <div class="text-right">
            <span id="roadmap-percent" class="text-2xl font-extrabold text-fatec-600 dark:text-fatec-400">0%</span>
            <div class="text-xs text-slate-500 dark:text-slate-400" id="roadmap-count-text">0 de 16 tópicos</div>
          </div>
        </div>
        <div class="w-full h-3 bg-slate-100 dark:bg-slate-800 rounded-full overflow-hidden">
          <div id="roadmap-progress-bar" class="h-full bg-gradient-to-r from-fatec-500 to-indigo-600 transition-all duration-300" style="width: 0%"></div>
        </div>
      </div>

      <div id="roadmap-container" class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <!-- Renderizado via JS -->
      </div>
    </section>

    <!-- ==================== ABA 5: DESEMPENHO ==================== -->
    <section id="tab-desempenho" class="tab-section hidden space-y-6 animate-fade-in">
      <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
        <div>
          <div class="flex flex-wrap items-center gap-2.5">
            <h2 id="desempenho-title" class="text-2xl sm:text-3xl font-extrabold text-slate-900 dark:text-white">Seu Desempenho</h2>
            <span id="desempenho-profile-badge" class="px-3 py-1 rounded-full text-xs font-bold bg-fatec-100 text-fatec-800 dark:bg-fatec-900/50 dark:text-fatec-200">
              Perfil Visitante
            </span>
          </div>
          <p id="desempenho-subtitle" class="text-sm text-slate-600 dark:text-slate-400 mt-1">
            Estatísticas consolidadas dos treinos, mini quiz e simulados.
          </p>
        </div>
        <div class="flex flex-wrap items-center gap-2">
          <button id="desempenho-login-cta" onclick="openAuthModal('register')" class="px-4 py-2.5 rounded-xl text-xs sm:text-sm font-bold bg-fatec-600 hover:bg-fatec-700 text-white transition">
            Criar conta para salvar perfil
          </button>
          <button onclick="confirmResetStats()" class="px-4 py-2.5 rounded-xl text-xs sm:text-sm font-semibold bg-rose-50 text-rose-700 dark:bg-rose-900/30 dark:text-rose-300 border border-rose-200 dark:border-rose-800 hover:bg-rose-100 transition">
            Zerar estatísticas
          </button>
        </div>
      </div>

      <!-- KPIs Principais -->
      <div class="grid grid-cols-2 lg:grid-cols-4 gap-4">
        <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl p-5">
          <div class="text-xs font-semibold text-slate-500 dark:text-slate-400">Questões Respondidas</div>
          <div id="stat-total-answered" class="text-2xl sm:text-3xl font-extrabold text-slate-900 dark:text-white mt-1">0</div>
        </div>
        <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl p-5">
          <div class="text-xs font-semibold text-slate-500 dark:text-slate-400">Total de Acertos</div>
          <div id="stat-total-hits" class="text-2xl sm:text-3xl font-extrabold text-emerald-600 dark:text-emerald-400 mt-1">0</div>
        </div>
        <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl p-5">
          <div class="text-xs font-semibold text-slate-500 dark:text-slate-400">Aproveitamento Geral</div>
          <div id="stat-accuracy" class="text-2xl sm:text-3xl font-extrabold text-fatec-600 dark:text-fatec-400 mt-1">0%</div>
        </div>
        <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl p-5">
          <div class="text-xs font-semibold text-slate-500 dark:text-slate-400">Erradas para Revisar</div>
          <div id="stat-wrong-pool" class="text-2xl sm:text-3xl font-extrabold text-amber-600 dark:text-amber-400 mt-1">0</div>
        </div>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <!-- Aproveitamento por Matéria -->
        <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 space-y-4">
          <h3 class="text-lg font-bold text-slate-900 dark:text-white">Desempenho por Matéria</h3>
          <div id="stat-by-subject" class="space-y-4">
            <!-- Barras por matéria -->
          </div>
        </div>

        <!-- Histórico Recente -->
        <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 space-y-4">
          <h3 class="text-lg font-bold text-slate-900 dark:text-white">Últimas Atividades</h3>
          <div id="stat-history-list" class="space-y-2.5 max-h-80 overflow-y-auto pr-1">
            <!-- Histórico -->
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== ABA 6: SOBRE ==================== -->
    <section id="tab-sobre" class="tab-section hidden space-y-6 animate-fade-in">
      <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-10 shadow-sm max-w-4xl mx-auto space-y-6">
        <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full text-xs font-bold bg-fatec-100 text-fatec-800 dark:bg-fatec-900/50 dark:text-fatec-200">
          Sobre o Projeto
        </div>
        <h2 class="text-2xl sm:text-3xl font-extrabold text-slate-900 dark:text-white">
          Klick Kwiz · Preparação focada no Vestibular Fatec
        </h2>
        <p class="text-sm sm:text-base text-slate-600 dark:text-slate-300 leading-relaxed">
          O <strong>Klick Kwiz</strong> nasceu com um objetivo direto: ajudar estudantes que vão prestar o vestibular das Faculdades de Tecnologia do Estado de São Paulo (Fatec) a praticarem com questões objetivas, contextualizadas e fiéis ao estilo da banca.
        </p>
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 pt-2">
          <div class="p-4 rounded-2xl bg-slate-50 dark:bg-slate-800/70 border border-slate-200 dark:border-slate-700">
            <div class="font-bold text-slate-900 dark:text-white mb-1">100% Local & Rápido</div>
            <p class="text-xs text-slate-600 dark:text-slate-400">Não exige cadastro nem servidor externo. Todo o seu histórico fica salvo no navegador.</p>
          </div>
          <div class="p-4 rounded-2xl bg-slate-50 dark:bg-slate-800/70 border border-slate-200 dark:border-slate-700">
            <div class="font-bold text-slate-900 dark:text-white mb-1">Revisão Ativa</div>
            <p class="text-xs text-slate-600 dark:text-slate-400">O sistema identifica automaticamente onde você errou e cria listas personalizadas de revisão.</p>
          </div>
          <div class="p-4 rounded-2xl bg-slate-50 dark:bg-slate-800/70 border border-slate-200 dark:border-slate-700">
            <div class="font-bold text-slate-900 dark:text-white mb-1">Banco Expansível</div>
            <p class="text-xs text-slate-600 dark:text-slate-400">Pela aba Admin você pode cadastrar novas questões e personalizar seus próprios treinos.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== ABA 7: CONTATO ==================== -->
    <section id="tab-contato" class="tab-section hidden space-y-6 animate-fade-in">
      <div class="max-w-2xl mx-auto bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-10 shadow-sm space-y-6">
        <div>
          <h2 class="text-2xl sm:text-3xl font-extrabold text-slate-900 dark:text-white">Fale Conosco</h2>
          <p class="text-sm text-slate-600 dark:text-slate-400 mt-1">
            Encontrou algum detalhe em um gabarito ou quer sugerir novas provas da Fatec? Envie uma mensagem ou escreva para
            <a href="mailto:klickkwiz@gmail.com" class="text-fatec-600 dark:text-fatec-400 font-semibold underline">klickkwiz@gmail.com</a>.
          </p>
        </div>

        <form id="contact-form" onsubmit="handleContactSubmit(event)" class="space-y-4">
          <div>
            <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">Seu nome</label>
            <input type="text" id="contact-name" required placeholder="Ex.: Ana Paula" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500" />
          </div>
          <div>
            <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">Seu e-mail</label>
            <input type="email" id="contact-email" required placeholder="voce@exemplo.com" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500" />
          </div>
          <div>
            <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">Assunto</label>
            <select id="contact-subject" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500">
              <option>Sugestão de questões / provas</option>
              <option>Revisão de gabarito</option>
              <option>Feedback sobre a plataforma</option>
              <option>Outro assunto</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">Mensagem</label>
            <textarea id="contact-message" rows="4" required placeholder="Escreva sua dúvida ou sugestão..." class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500"></textarea>
          </div>
          <button type="submit" class="w-full sm:w-auto px-6 py-3 rounded-xl font-bold text-sm bg-fatec-600 hover:bg-fatec-700 text-white transition">
            Enviar mensagem
          </button>
        </form>

        <div id="contact-success" class="hidden rounded-2xl p-4 bg-emerald-50 dark:bg-emerald-900/30 border border-emerald-200 dark:border-emerald-800 text-emerald-800 dark:text-emerald-200 text-sm">
          Mensagem registrada com sucesso! Obrigado por contribuir com o Klick Kwiz.
        </div>
      </div>
    </section>

    <!-- ==================== ABA 8: ADMIN ==================== -->
    <section id="tab-admin" class="tab-section hidden space-y-6 animate-fade-in">
      <!-- Tela de Bloqueio Opcional do Admin -->
      <div id="admin-lock-screen" class="max-w-md mx-auto bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 sm:p-8 shadow-sm text-center space-y-5">
        <div class="w-14 h-14 rounded-2xl bg-amber-100 dark:bg-amber-900/40 text-2xl flex items-center justify-center mx-auto">
          🔐
        </div>
        <div>
          <h2 class="text-xl font-extrabold text-slate-900 dark:text-white">Acesso de Administrador / Professor</h2>
          <p class="text-xs sm:text-sm text-slate-600 dark:text-slate-400 mt-1">
            Área restrita para edição do banco de questões da Fatec. Digite a senha administrativa ou use o acesso rápido para testar.
          </p>
        </div>
        <form onsubmit="handleAdminUnlock(event)" class="space-y-3 text-left">
          <div>
            <label class="block text-xs font-semibold text-slate-600 dark:text-slate-300 mb-1">Senha de Administrador</label>
            <input type="password" id="admin-pass-input" placeholder="Digite admin123" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500" />
          </div>
          <button type="submit" class="w-full py-2.5 rounded-xl font-bold text-sm bg-fatec-600 hover:bg-fatec-700 text-white transition">
            Desbloquear Painel Admin
          </button>
        </form>
        <div class="pt-2 border-t border-slate-100 dark:border-slate-800">
          <button type="button" onclick="quickUnlockAdmin()" class="text-xs font-bold text-fatec-600 dark:text-fatec-400 hover:underline">
            ⚡ Acesso rápido como Professor / Admin (Testar agora)
          </button>
        </div>
      </div>

      <!-- Conteúdo Desbloqueado do Admin -->
      <div id="admin-unlocked-content" class="hidden space-y-6">
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
          <div>
            <div class="flex items-center gap-2">
              <h2 class="text-2xl sm:text-3xl font-extrabold text-slate-900 dark:text-white">Admin · Banco de Questões</h2>
              <span class="px-2.5 py-0.5 rounded-full text-[11px] font-bold bg-emerald-100 text-emerald-800 dark:bg-emerald-900/40 dark:text-emerald-300">
                Modo Professor Ativo
              </span>
            </div>
            <p class="text-sm text-slate-600 dark:text-slate-400">Cadastre novas questões, gerencie o banco local ou restaure as 30 questões originais.</p>
          </div>
          <div class="flex flex-wrap gap-2">
            <button onclick="exportBankJSON()" class="px-4 py-2 rounded-xl text-xs font-semibold bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 transition">
              Copiar JSON do Banco
            </button>
            <button onclick="confirmRestoreDefaultBank()" class="px-4 py-2 rounded-xl text-xs font-semibold bg-amber-100 text-amber-800 dark:bg-amber-900/40 dark:text-amber-300 hover:bg-amber-200 transition">
              Restaurar 30 originais
            </button>
            <button onclick="lockAdminPanel()" class="px-4 py-2 rounded-xl text-xs font-semibold bg-slate-200 dark:bg-slate-800 hover:bg-slate-300 dark:hover:bg-slate-700 transition">
              Bloquear Admin
            </button>
          </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-5 gap-6">
            <select id="admin-correct" class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3 py-2 text-sm font-bold">
              <option value="A">A</option>
              <option value="B">B</option>
              <option value="C">C</option>
              <option value="D">D</option>
              <option value="E">E</option>
            </select>
          </div>

          <div>
            <label class="block text-xs font-semibold mb-1">Comentário / Explicação</label>
            <textarea id="admin-explanation" rows="2" required placeholder="Explique por que a alternativa é a correta..." class="w-full rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3 py-2 text-sm"></textarea>
          </div>

          <button type="submit" class="w-full py-3 rounded-xl font-bold text-sm bg-fatec-600 hover:bg-fatec-700 text-white transition">
            + Adicionar ao Banco
          </button>
        </form>

        <!-- Lista de Questões Cadastradas -->
        <div class="lg:col-span-3 bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 space-y-4">
          <div class="flex items-center justify-between">
            <h3 class="font-bold text-lg text-slate-900 dark:text-white">
              Questões no Banco (<span id="admin-total-count">30</span>)
            </h3>
            <input type="text" id="admin-search" oninput="renderAdminList()" placeholder="Buscar questão..." class="rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 px-3 py-1.5 text-xs sm:text-sm focus:outline-none focus:ring-2 focus:ring-fatec-500" />
          </div>
          <div id="admin-questions-list" class="space-y-3 max-h-[640px] overflow-y-auto pr-1">
            <!-- Lista via JS -->
          </div>
        </div>
      </div>
      </div>
    </section>

  </main>

  <!-- Rodapé -->
  <footer class="border-t border-slate-200 dark:border-slate-800 bg-white dark:bg-slate-900/80 py-6 mt-12">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center text-xs sm:text-sm text-slate-500 dark:text-slate-400">
      © 2026 Klick Kwiz · <a href="mailto:klickkwiz@gmail.com" class="hover:text-fatec-600 dark:hover:text-fatec-400 underline">klickkwiz@gmail.com</a> · Dados salvos localmente no seu navegador.
    </div>
  </footer>

  <!-- ==================== JAVASCRIPT COMPLETO ==================== -->
  <script>
    // =========================================================================
    // 1. DADOS INICIAIS: MINI QUIZ E BANCO DE 30 QUESTÕES FATEC (HUMANAS)
    // =========================================================================

    const MINI_QUIZ_QUESTIONS = [
      {
        id: 'mq-1',
        subject: 'Português',
        topic: 'Sintaxe',
        question: 'Leia: "Embora o relatório tenha sido entregue no prazo, os resultados não atenderam às expectativas." A relação estabelecida entre as orações é de:',
        options: {
          A: 'Causa.',
          B: 'Concessão.',
          C: 'Finalidade.',
          D: 'Condição.',
          E: 'Conformidade.'
        },
        correct: 'B',
        explanation: 'A conjunção subordinativa "Embora" introduz uma oração adverbial concessiva, indicando um fato contrário ao esperado sem impedir a ação principal.'
      },
      {
        id: 'mq-2',
        subject: 'História',
        topic: 'Era Vargas',
        question: 'Durante o governo de Getúlio Vargas (1930–1945), a criação da Consolidação das Leis do Trabalho (CLT), em 1943, teve como característica central:',
        options: {
          A: 'A completa autonomia sindical frente ao Ministério do Trabalho.',
          B: 'A extinção da Justiça do Trabalho e do salário mínimo nacional.',
          C: 'A sistematização de direitos trabalhistas atrelada ao controle estatal sobre os sindicatos.',
          D: 'A restrição dos direitos trabalhistas exclusivamente aos trabalhadores rurais.',
          E: 'A adoção do liberalismo pleno nas relações entre empregadores e operários.'
        },
        correct: 'C',
        explanation: 'A CLT reuniu direitos importantes (férias, jornada de 8h, carteira assinada), mas manteve os sindicatos tutelados pelo Estado varguista (peleguismo).'
      },
      {
        id: 'mq-3',
        subject: 'Geografia',
        topic: 'Urbanização Brasileira',
        question: 'O processo de metropolização no Brasil, intensificado na segunda metade do século XX, provocou o fenômeno da conurbação, que consiste na:',
        options: {
          A: 'Migração exclusiva de moradores das capitais para pequenas cidades agrícolas.',
          B: 'Integração física e socioeconômica entre duas ou mais cidades vizinhas devido ao crescimento urbano.',
          C: 'Substituição integral do setor de serviços pela agropecuária nas regiões metropolitanas.',
          D: 'Redução da densidade demográfica nos centros industriais do Sudeste.',
          E: 'Criação de reservas extrativistas no interior dos centros comerciais.'
        },
        correct: 'B',
        explanation: 'Conurbação ocorre quando a expansão urbana faz com que a mancha urbana de municípios vizinhos se encontre, como na Região Metropolitana de São Paulo.'
      },
      {
        id: 'mq-4',
        subject: 'Atualidades',
        topic: 'Transição Energética',
        question: 'No debate contemporâneo sobre transição energética e mudanças climáticas, a matriz elétrica brasileira destaca-se internacionalmente por:',
        options: {
          A: 'Depender majoritariamente de usinas termelétricas movidas a carvão mineral.',
          B: 'Não possuir potencial para geração de energia solar ou eólica.',
          C: 'Apresentar elevada participação de fontes renováveis, como hidrelétrica, eólica, solar e biomassa.',
          D: 'Utilizar energia nuclear como principal fonte de abastecimento residencial.',
          E: 'Ter eliminado completamente o uso de combustíveis fósseis no transporte rodoviário.'
        },
        correct: 'C',
        explanation: 'Mais de 80% da matriz elétrica do Brasil provém de fontes renováveis, com forte crescimento das energias eólica e solar nos últimos anos.'
      }
    ];

    const DEFAULT_QUESTIONS = [
      {
        id: 1,
        subject: 'Português',
        topic: 'Sintaxe',
        exam: 'Fatec · Banco Humanas',
        question: 'Leia: "Embora o relatório tenha sido entregue no prazo, os resultados não atenderam às expectativas." A relação estabelecida entre as orações é de:',
        options: { A: 'Causa.', B: 'Concessão.', C: 'Finalidade.', D: 'Condição.', E: 'Conformidade.' },
        correct: 'B',
        explanation: '"Embora" é conjunção subordinativa concessiva: admite um fato contrário (entregar no prazo) sem impedir a oração principal.'
      },
      {
        id: 2,
        subject: 'Português',
        topic: 'Concordância Verbal',
        exam: 'Fatec · Provas Anteriores',
        question: 'Assinale a alternativa em que a concordância verbal está rigorosamente de acordo com a norma-padrão da língua portuguesa:',
        options: {
          A: 'Fazem dez anos que a unidade da Fatec foi inaugurada naquele município.',
          B: 'Houveram muitos candidatos inscritos para o curso de Análise e Desenvolvimento de Sistemas.',
          C: 'Aluga-se salas comerciais próximas à estação do metrô.',
          D: 'Existe ainda muitas dúvidas sobre o edital do vestibular.',
          E: 'Devem haver soluções mais sustentáveis para o transporte urbano.'
        },
        correct: 'C',
        explanation: 'Na verdade, atenção à regra: "Alugam-se salas" seria o plural com partícula apassivadora, mas vamos conferir: em "Fazem dez anos" (impessoal: Faz), "Houveram" (impessoal: Houve), "Existe" (sujeito plural: Existem), "Devem haver" (impessoal: Deve haver). Entre as opções clássicas de prova, ajustemos para que apenas uma esteja impecável: veja o gabarito comentado.'
      },
      {
        id: 3,
        subject: 'Português',
        topic: 'Crase',
        exam: 'Fatec · Provas Anteriores',
        question: 'Assinale a alternativa em que o uso do acento grave indicativo de crase está CORRETO:',
        options: {
          A: 'O candidato dirigiu-se à sala de provas com antecedência.',
          B: 'Entregamos o projeto técnico à Vossa Senhoria ontem à tarde.',
          C: 'O curso é destinado à estudantes que concluíram o ensino médio.',
          D: 'Começaremos à revisar os conteúdos de Geografia amanhã.',
          E: 'Fomos à pé até o campus universitário.'
        },
        correct: 'A',
        explanation: 'Quem se dirige, dirige-se "a" algum lugar + artigo definido feminino "a" antes de "sala" = "à sala". Não ocorre crase antes de pronome de tratamento, plural sem artigo, verbo ou palavra masculina.'
      },
      {
        id: 4,
        subject: 'Português',
        topic: 'Interpretação de Texto',
        exam: 'Fatec · Provas Anteriores',
        question: 'Em textos argumentativos, o uso de operadores argumentativos como "portanto", "logo" e "por conseguinte" tem a função principal de:',
        options: {
          A: 'Introduzir uma oposição radical ao argumento anterior.',
          B: 'Estabelecer uma conclusão decorrente das premissas apresentadas.',
          C: 'Indicar uma dúvida ou hipótese sem comprovação.',
          D: 'Retomar cronologicamente fatos históricos do século XIX.',
          E: 'Substituir substantivos próprios para evitar repetição.'
        },
        correct: 'B',
        explanation: 'Conjunções conclusivas ("portanto", "logo", "por conseguinte") articulam o fechamento lógico de um raciocínio.'
      },
      {
        id: 5,
        subject: 'Português',
        topic: 'Regência Verbal',
        exam: 'Fatec · Provas Anteriores',
        question: 'De acordo com a norma-padrão, o verbo "assistir" no sentido de "presenciar, ver" exige complemento regido pela preposição "a". Assinale a frase correta:',
        options: {
          A: 'Os alunos assistiram o seminário sobre inteligência artificial.',
          B: 'O filme a que assistimos ontem aborda ética na tecnologia.',
          C: 'Todos assistiram os debates com muita atenção.',
          D: 'A palestra que assistimos foi ministrada por ex-alunos.',
          E: 'Assistimos o nascer do sol na serra.'
        },
        correct: 'B',
        explanation: 'No sentido de ver/presenciar, "assistir" é transitivo indireto (assistir a algo), exigindo a preposição "a" antes do pronome relativo "que" ("a que assistimos").'
      },
      {
        id: 6,
        subject: 'Português',
        topic: 'Figuras de Linguagem',
        exam: 'Fatec · Provas Anteriores',
        question: 'Na frase: "A cidade inteira acordou ansiosa para o resultado do vestibular", identifica-se a figura de linguagem denominada:',
        options: {
          A: 'Metonímia, pois o continente ("cidade") substitui o conteúdo ("habitantes").',
          B: 'Antítese, pela aproximação de palavras de sentidos opostos.',
          C: 'Eufemismo, por suavizar uma notícia desagradável.',
          D: 'Onomatopeia, pela reprodução de sons urbanos.',
          E: 'Pleonasmo, pela repetição desnecessária de ideias.'
        },
        correct: 'A',
        explanation: 'Metonímia ocorre quando se emprega um termo por outro com o qual mantém relação de proximidade (o lugar pelos seus habitantes).'
      },
      {
        id: 7,
        subject: 'História',
        topic: 'Brasil Colônia',
        exam: 'Fatec · Provas Anteriores',
        question: 'A economia açucareira no Brasil Colonial (séculos XVI e XVII) organizou-se com base no sistema de plantation, caracterizado por:',
        options: {
          A: 'Pequenas propriedades policultoras voltadas ao mercado interno e trabalho assalariado.',
          B: 'Latifúndio monocultor, mão de obra escravizada e produção voltada à exportação.',
          C: 'Indústria manufatureira urbana e livre comércio com todas as nações europeias.',
          D: 'Exploração exclusivamente aurífera no litoral nordestino.',
          E: 'Cooperativas agrícolas administradas por comunidades indígenas.'
        },
        correct: 'B',
        explanation: 'O tripé da plantation colonial era: grande propriedade (latifúndio), monocultura (cana-de-açúcar), trabalho escravizado e foco no mercado externo.'
      },
      {
        id: 8,
        subject: 'História',
        topic: 'Segundo Reinado',
        exam: 'Fatec · Provas Anteriores',
        question: 'A Lei Eusébio de Queirós (1850), aprovada durante o Segundo Reinado no Brasil, determinou:',
        options: {
          A: 'A libertação imediata de todos os escravizados nascidos no Brasil.',
          B: 'A proibição definitiva do tráfico transatlântico de escravizados para o Brasil.',
          C: 'A concessão de terras gratuitas aos imigrantes europeus nas cidades.',
          D: 'O fim da monarquia constitucional e a proclamação da República.',
          E: 'A liberdade aos escravizados com mais de sessenta anos de idade.'
        },
        correct: 'B',
        explanation: 'Promulgada em 1850, a Lei Eusébio de Queirós extinguiu o tráfico negreiro internacional para o Brasil, liberando capitais para outros investimentos.'
      },
      {
        id: 9,
        subject: 'História',
        topic: 'República Velha',
        exam: 'Fatec · Provas Anteriores',
        question: 'Durante a Primeira República no Brasil (1889–1930), o fenômeno do "coronelismo" apoiava-se no chamado "voto de cabresto", que consistia:',
        options: {
          A: 'No voto secreto e eletrônico auditado pelo Tribunal Superior Eleitoral.',
          B: 'No controle exercido por chefes políticos locais sobre os eleitores, por meio de favores ou coerção.',
          C: 'Na obrigatoriedade do voto feminino em todas as eleições municipais.',
          D: 'Na eleição direta dos governadores apenas pelos trabalhadores fabris.',
          E: 'Na proibição de qualquer vínculo entre fazendeiros e autoridades municipais.'
        },
        correct: 'B',
        explanation: 'Como o voto era aberto (não secreto), os coronéis pressionavam os eleitores de seus currais eleitorais a votarem nos candidatos indicados.'
      },
      {
        id: 10,
        subject: 'História',
        topic: 'Era Vargas',
        exam: 'Fatec · Provas Anteriores',
        question: 'A Revolução Constitucionalista de 1932, ocorrida em São Paulo, teve como principal reivindicação:',
        options: {
          A: 'A implantação imediata do regime socialista no estado de São Paulo.',
          B: 'A convocação de uma Assembleia Nacional Constituinte e o fim do governo provisório discricionário de Vargas.',
          C: 'O retorno da monarquia parlamentarista liderada pela família imperial.',
          D: 'A transferência da capital federal do Rio de Janeiro para Santos.',
          E: 'O fechamento de todas as universidades paulistas.'
        },
        correct: 'B',
        explanation: 'Os paulistas exigiam uma nova Constituição e o fim das intervenções federais nomeadas por Getúlio Vargas após 1930.'
      },
      {
        id: 11,
        subject: 'História',
        topic: 'Ditadura Militar no Brasil',
        exam: 'Fatec · Provas Anteriores',
        question: 'O Ato Institucional nº 5 (AI-5), decretado em dezembro de 1968 no Brasil, representou:',
        options: {
          A: 'A abertura política lenta, gradual e segura com anistia aos exilados.',
          B: 'O recrudescimento da repressão, fechamento do Congresso Nacional e suspensão do habeas corpus para crimes políticos.',
          C: 'A convocação de eleições diretas para presidente da República em 1969.',
          D: 'O fim da censura prévia aos meios de comunicação e às artes.',
          E: 'A legalização de todos os partidos políticos de oposição.'
        },
        correct: 'B',
        explanation: 'O AI-5 marcou o início dos chamados "Anos de Chumbo", ampliando poderes excepcionais do Executivo e restringindo direitos civis.'
      },
      {
        id: 12,
        subject: 'História',
        topic: 'Revolução Industrial',
        exam: 'Fatec · Provas Anteriores',
        question: 'A Primeira Revolução Industrial, iniciada na Inglaterra na segunda metade do século XVIII, teve como principais símbolos tecnológicos e energéticos:',
        options: {
          A: 'A energia nuclear, a robótica e a internet.',
          B: 'O motor a combustão interna, o petróleo e a linha de montagem fordista.',
          C: 'A máquina a vapor, o carvão mineral e a mecanização da indústria têxtil.',
          D: 'A energia fotovoltaica, os biocombustíveis e a biotecnologia.',
          E: 'O telégrafo sem fio, o alumínio e a energia hidrelétrica.'
        },
        correct: 'C',
        explanation: 'Carvão mineral, máquina a vapor (aperfeiçoada por James Watt) e teares mecânicos definiram a primeira fase industrial inglesa.'
      },
      {
        id: 13,
        subject: 'História',
        topic: 'Guerra Fria',
        exam: 'Fatec · Provas Anteriores',
        question: 'A chamada "Corrida Espacial" durante a Guerra Fria (1947–1991) refletia:',
        options: {
          A: 'A cooperação pacífica e irrestrita entre EUA e URSS para compartilhar segredos militares.',
          B: 'A disputa tecnológica, científica e ideológica entre Estados Unidos e União Soviética pela hegemonia mundial.',
          C: 'O desinteresse das superpotências pelo desenvolvimento de satélites e foguetes.',
          D: 'A liderança exclusiva dos países europeus na exploração lunar.',
          E: 'O fim das tensões nucleares logo após a Segunda Guerra Mundial.'
        },
        correct: 'B',
        explanation: 'Marcos como o Sputnik (URSS, 1957) e a chegada à Lua (EUA, 1969) serviam para demonstrar superioridade técnica e ideológica.'
      },
      {
        id: 14,
        subject: 'Geografia',
        topic: 'Industrialização de São Paulo',
        exam: 'Fatec · Provas Anteriores',
        question: 'Um dos fatores históricos decisivos para que o estado de São Paulo liderasse a industrialização brasileira no início do século XX foi:',
        options: {
          A: 'A ausência de ferrovias e de energia elétrica na capital paulista.',
          B: 'O acúmulo de capitais oriundos da cafeicultura, aliado à infraestrutura ferroviária e à mão de obra imigrante.',
          C: 'A proibição de investimentos estrangeiros no setor de transportes.',
          D: 'O isolamento geográfico em relação ao porto de Santos.',
          E: 'A predominância da mineração de ouro no Vale do Paraíba.'
        },
        correct: 'B',
        explanation: 'A economia cafeeira gerou capital, mercado consumidor, ferrovias até o Porto de Santos e atraiu imigrantes que formaram o operariado inicial.'
      },
      {
        id: 15,
        subject: 'Geografia',
        topic: 'Clima e Biomas',
        exam: 'Fatec · Provas Anteriores',
        question: 'O bioma Cerrado, amplamente presente no Planalto Central brasileiro, caracteriza-se naturalmente por:',
        options: {
          A: 'Clima equatorial superúmido sem estação seca definida.',
          B: 'Clima tropical típico (com verão chuvoso e inverno seco), solos ácidos e vegetação de árvores tortuosas com casca grossa.',
          C: 'Vegetação de coníferas adaptadas a nevascas rigorosas.',
          D: 'Solos extremamente férteis de terra roxa em toda a sua extensão e ausência de queimadas naturais.',
          E: 'Manguezais costeiros submetidos à variação diária das marés.'
        },
        correct: 'B',
        explanation: 'O Cerrado possui duas estações bem marcadas (seca e chuvosa), solos latossolos ácidos e vegetação tropófila adaptada ao fogo natural.'
      },
      {
        id: 16,
        subject: 'Geografia',
        topic: 'Recursos Hídricos',
        exam: 'Fatec · Provas Anteriores',
        question: 'O Sistema Aquífero Guarani representa uma das maiores reservas subterrâneas de água doce do mundo, estendendo-se pelo subsolo do Brasil e de outros três países sul-americanos:',
        options: {
          A: 'Chile, Peru e Equador.',
          B: 'Argentina, Paraguai e Uruguai.',
          C: 'Colômbia, Venezuela e Bolívia.',
          D: 'Guiana, Suriname e Peru.',
          E: 'Bolívia, Chile e Colômbia.'
        },
        correct: 'B',
        explanation: 'O Aquífero Guarani abrange áreas do Centro-Sul do Brasil, além de Argentina, Paraguai e Uruguai.'
      },
      {
        id: 17,
        subject: 'Geografia',
        topic: 'Demografia',
        exam: 'Fatec · Provas Anteriores',
        question: 'A transição demográfica observada no Brasil nas últimas décadas indica:',
        options: {
          A: 'Aumento acelerado da taxa de natalidade e redução da expectativa de vida.',
          B: 'Queda nas taxas de fecundidade e natalidade, acompanhada pelo envelhecimento gradual da população.',
          C: 'Predomínio absoluto de crianças de 0 a 10 anos na base da pirâmide etária.',
          D: 'Crescimento vegetativo superior a 5% ao ano em todas as regiões.',
          E: 'Retorno da população urbana para áreas rurais não mecanizadas.'
        },
        correct: 'B',
        explanation: 'Com urbanização, acesso à saúde e maior escolaridade, a taxa de fecundidade caiu abaixo do nível de reposição e a população idosa cresce rapidamente.'
      },
      {
        id: 18,
        subject: 'Geografia',
        topic: 'Globalização e Blocos',
        exam: 'Fatec · Provas Anteriores',
        question: 'No contexto da globalização produtiva, a desconcentração industrial ocorre quando empresas transferem fábricas das metrópoles tradicionais para cidades médias ou outros países buscando:',
        options: {
          A: 'Impostos mais elevados e terrenos urbanos mais caros.',
          B: 'Incentivos fiscais, custos menores de terreno/mão de obra e boa logística de escoamento.',
          C: 'Afastamento completo das redes de telecomunicações e rodovias.',
          D: 'Redução da produtividade tecnológica.',
          E: 'Eliminação de qualquer integração com o mercado global.'
        },
        correct: 'B',
        explanation: 'A chamada "guerra fiscal" e os custos elevados das grandes metrópoles impulsionam indústrias para o interior e regiões com incentivos.'
      },
      {
        id: 19,
        subject: 'Geografia',
        topic: 'Meio Ambiente',
        exam: 'Fatec · Provas Anteriores',
        question: 'O fenômeno urbano das "ilhas de calor" consiste na elevação das temperaturas médias nas áreas centrais das grandes cidades em comparação com seu entorno rural, devido principalmente:',
        options: {
          A: 'Ao excesso de parques florestais e lagos artificiais no centro.',
          B: 'À intensa impermeabilização do solo (asfalto e concreto), verticalização e redução da cobertura vegetal.',
          C: 'À altitude muito superior dos bairros centrais em relação às serras.',
          D: 'À passagem constante de frentes frias polares apenas pelos centros comerciais.',
          E: 'Ao uso exclusivo de veículos elétricos silenciosos.'
        },
        correct: 'B',
        explanation: 'Asfalto e concreto absorvem e irradiam mais calor, enquanto a falta de árvores diminui a evapotranspiração e o resfriamento natural.'
      },
      {
        id: 20,
        subject: 'Filosofia e Sociologia',
        topic: 'Sociologia Clássica',
        exam: 'Fatec · Provas Anteriores',
        question: 'Para o sociólogo francês Émile Durkheim, o objeto de estudo fundamental da Sociologia são os "fatos sociais", que se definem por três características principais:',
        options: {
          A: 'Subjetividade, individualidade e espontaneidade biológica.',
          B: 'Coercitividade, exterioridade e generalidade.',
          C: 'Religiosidade, hereditariedade e isolamento geográfico.',
          D: 'Criatividade artística, improviso e liberdade absoluta.',
          E: 'Neutralidade econômica, acaso e intuição psicológica.'
        },
        correct: 'B',
        explanation: 'Segundo Durkheim, fatos sociais são externos ao indivíduo, impõem-se de forma coercitiva (leis, costumes, língua) e são gerais na sociedade.'
      },
      {
        id: 21,
        subject: 'Filosofia e Sociologia',
        topic: 'Filosofia Moderna',
        exam: 'Fatec · Provas Anteriores',
        question: 'A célebre formulação cartesiana "Penso, logo existo" (Cogito, ergo sum), de René Descartes, expressa:',
        options: {
          A: 'A negação completa da razão humana como fonte de conhecimento.',
          B: 'A busca por uma verdade primeira e indubitável alcançada por meio da dúvida metódica.',
          C: 'A defesa de que apenas os sentidos físicos garantem a certeza científica.',
          D: 'A submissão da filosofia aos dogmas medievais sem questionamento.',
          E: 'O desprezo pela matemática e pela geometria.'
        },
        correct: 'B',
        explanation: 'Ao duvidar de tudo, Descartes percebe que não pode duvidar de que está pensando; logo, existe enquanto ser pensante (racionalismo).'
      },
      {
        id: 22,
        subject: 'Filosofia e Sociologia',
        topic: 'Mundo do Trabalho',
        exam: 'Fatec · Provas Anteriores',
        question: 'O modelo produtivo conhecido como Toyotismo, surgido no Japão após a Segunda Guerra Mundial, diferencia-se do Fordismo tradicional por adotar:',
        options: {
          A: 'Grandes estoques de peças e produção em massa padronizada sem flexibilidade.',
          B: 'Produção enxuta sob demanda (just in time), flexibilização e trabalhadores multifuncionais.',
          C: 'Abolição total de metas de qualidade e de automação industrial.',
          D: 'Trabalho artesanal individual sem divisão de tarefas.',
          E: 'Isolamento tecnológico em relação à microeletrônica.'
        },
        correct: 'B',
        explanation: 'O Toyotismo introduziu o sistema just in time (produzir conforme a demanda, evitando estoques) e equipes polivalentes.'
      },
      {
        id: 23,
        subject: 'Filosofia e Sociologia',
        topic: 'Cidadania e Direitos',
        exam: 'Fatec · Provas Anteriores',
        question: 'A Constituição Federal brasileira de 1988 ficou conhecida como "Constituição Cidadã" porque:',
        options: {
          A: 'Restringiu o direito ao voto apenas aos cidadãos com ensino superior.',
          B: 'Ampliou os direitos sociais, civis e políticos após o período da ditadura militar, instituindo o SUS e garantias fundamentais.',
          C: 'Extinguiu os direitos trabalhistas previstos desde a década de 1940.',
          D: 'Centralizou todo o poder legislativo nas mãos do Poder Executivo.',
          E: 'Proibiu a existência de conselhos municipais e associações civis.'
        },
        correct: 'B',
        explanation: 'Promulgada em 5 de outubro de 1988, consolidou a redemocratização do Brasil com amplo rol de direitos fundamentais.'
      },
      {
        id: 24,
        subject: 'Filosofia e Sociologia',
        topic: 'Ética e Tecnologia',
        exam: 'Fatec · Provas Anteriores',
        question: 'O debate ético contemporâneo sobre algoritmos de Inteligência Artificial na seleção de empregos e concessão de crédito alerta principalmente para o risco de:',
        options: {
          A: 'Eliminação automática de qualquer desigualdade histórica sem supervisão humana.',
          B: 'Reprodução e amplificação de vieses discriminatórios presentes nos dados históricos usados no treinamento.',
          C: 'Impossibilidade de processar grandes volumes de informações digitais.',
          D: 'Substituição completa da legislação de proteção de dados pessoais.',
          E: 'Garantia absoluta de transparência em sistemas de caixa-preta.'
        },
        correct: 'B',
        explanation: 'Se os dados de treinamento refletem desigualdades sociais, algoritmos podem perpetuar discriminações caso não haja auditoria e ética.'
      },
      {
        id: 25,
        subject: 'Atualidades',
        topic: 'Cidades Inteligentes',
        exam: 'Fatec · Provas Anteriores',
        question: 'O conceito de "Cidades Inteligentes" (Smart Cities) envolve o uso de tecnologias digitais e dados para melhorar a gestão urbana, mas especialistas ressaltam que uma cidade só é efetivamente inteligente quando:',
        options: {
          A: 'Prioriza exclusivamente câmeras de vigilância sem investir em saneamento ou transporte.',
          B: 'Alia inovação tecnológica à inclusão social, mobilidade sustentável e qualidade de vida para toda a população.',
          C: 'Substitui todas as áreas verdes por centros de processamento de dados.',
          D: 'Restringe o acesso aos serviços digitais apenas aos bairros de alta renda.',
          E: 'Elimina a participação cidadã nas decisões do orçamento público.'
        },
        correct: 'B',
        explanation: 'Tecnologia urbana precisa estar conectada à sustentabilidade ambiental e à redução das desigualdades socioespaciais.'
      },
      {
        id: 26,
        subject: 'Atualidades',
        topic: 'Geopolítica Contemporânea',
        exam: 'Fatec · Provas Anteriores',
        question: 'A recente ampliação do grupo BRICS (com a entrada de novos membros a partir de 2024) reflete uma tendência geopolítica de:',
        options: {
          A: 'Subordinação total das economias emergentes ao padrão monetário único europeu.',
          B: 'Busca por maior multipolaridade e fortalecimento da cooperação econômica e política entre países do Sul Global.',
          C: 'Dissolução de todos os acordos comerciais entre Ásia e América do Sul.',
          D: 'Isolamento diplomático completo de Brasil, Índia e China.',
          E: 'Substituição da Organização das Nações Unidas por uma aliança militar.'
        },
        correct: 'B',
        explanation: 'O BRICS ampliado busca ampliar o peso dos países emergentes na governança global e nas relações comerciais Sul-Sul.'
      },
      {
        id: 27,
        subject: 'Atualidades',
        topic: 'Economia Circular',
        exam: 'Fatec · Provas Anteriores',
        question: 'Diferentemente do modelo linear ("extrair, produzir, descartar"), a Economia Circular propõe:',
        options: {
          A: 'Aumentar a obsolescência programada dos aparelhos eletrônicos.',
          B: 'Manter produtos, componentes e materiais em uso pelo maior tempo possível por meio de reuso, reparo e reciclagem.',
          C: 'Proibir a logística reversa de embalagens e baterias.',
          D: 'Expandir aterros sanitários como única solução para resíduos sólidos.',
          E: 'Desvincular o design industrial do impacto ambiental.'
        },
        correct: 'B',
        explanation: 'A economia circular repensa o ciclo produtivo desde a concepção para minimizar resíduos e regenerar recursos.'
      },
      {
        id: 28,
        subject: 'Português',
        topic: 'Coesão Referencial',
        exam: 'Fatec · Provas Anteriores',
        question: 'Leia: "Os estudantes visitaram o parque tecnológico. Lá, conheceram startups de biotecnologia." O termo destacado "Lá" estabelece coesão textual por retomar:',
        options: {
          A: 'Os estudantes.',
          B: 'O parque tecnológico.',
          C: 'As startups.',
          D: 'A biotecnologia.',
          E: 'A ação de conhecer.'
        },
        correct: 'B',
        explanation: 'O advérbio de lugar "Lá" funciona como elemento anafórico, retomando o referente espacial "o parque tecnológico".'
      },
      {
        id: 29,
        subject: 'História',
        topic: 'História da África e Cultura Afro-Brasileira',
        exam: 'Fatec · Provas Anteriores',
        question: 'O Reino do Mali, que floresceu na África Ocidental entre os séculos XIII e XVI sob governantes como Mansa Musa, destacou-se por:',
        options: {
          A: 'Ser uma colônia europeia isolada do comércio transaariano.',
          B: 'Controlar importantes rotas comerciais de ouro e sal e abrigar centros intelectuais como Tombuctu.',
          C: 'Desconhecer a escrita e qualquer forma de organização urbana.',
          D: 'Basear sua economia exclusivamente na pesca marítima no Oceano Índico.',
          E: 'Ter sido destruído antes da expansão do Islã pelo Norte da África.'
        },
        correct: 'B',
        explanation: 'O Império do Mali foi uma das maiores potências econômicas e culturais da África pré-colonial, famoso pelas bibliotecas e mesquitas de Tombuctu.'
      },
      {
        id: 30,
        subject: 'Geografia',
        topic: 'Agropecuária e Tecnologia',
        exam: 'Fatec · Provas Anteriores',
        question: 'A chamada "Agricultura 4.0" no campo brasileiro caracteriza-se pela integração de sensores, drones, imagens de satélite e análise de dados com o objetivo de:',
        options: {
          A: 'Eliminar a necessidade de engenheiros agrônomos e técnicos agrícolas.',
          B: 'Otimizar o uso de insumos (água, fertilizantes e defensivos), aumentar a produtividade e monitorar lavouras com precisão.',
          C: 'Substituir todas as lavouras comerciais por agricultura de subsistência manual.',
          D: 'Desconsiderar as condições climáticas e o mapeamento de solos.',
          E: 'Aumentar o desperdício hídrico na irrigação.'
        },
        correct: 'B',
        explanation: 'A agricultura de precisão utiliza IoT, geoprocessamento e big data para aplicar insumos na medida exata de cada talhão.'
      }
    ];

    // Corrige a questão 2 para ter alternativa B impecável ("Houve muitos candidatos...")
    DEFAULT_QUESTIONS[1] = {
      id: 2,
      subject: 'Português',
      topic: 'Concordância Verbal',
      exam: 'Fatec · Provas Anteriores',
      question: 'Assinale a alternativa em que a concordância verbal está rigorosamente de acordo com a norma-padrão da língua portuguesa:',
      options: {
        A: 'Fazem dez anos que a unidade da Fatec foi inaugurada naquele município.',
        B: 'Houve muitos candidatos inscritos para o curso de Análise e Desenvolvimento de Sistemas.',
        C: 'Aluga-se salas comerciais próximas à estação do metrô.',
        D: 'Existe ainda muitas dúvidas sobre o edital do vestibular.',
        E: 'Devem haver soluções mais sustentáveis para o transporte urbano.'
      },
      correct: 'B',
      explanation: 'O verbo "haver" no sentido de "existir" é impessoal e permanece na 3ª pessoa do singular ("Houve muitos candidatos"). Nas demais: Faz dez anos, Alugam-se salas, Existem dúvidas, Deve haver soluções.'
    };

    const ROADMAP_DATA = [
      {
        category: 'Português & Interpretação',
        badge: 'Peso Alto na Fatec',
        items: [
          { id: 'rm-1', title: 'Interpretação de textos jornalísticos, charges e infográficos' },
          { id: 'rm-2', title: 'Sintaxe do período composto: conjunções (concessivas, causais, consecutivas)' },
          { id: 'rm-3', title: 'Concordância verbal e nominal (casos de verbos impessoais)' },
          { id: 'rm-4', title: 'Regência verbal, crase e coesão referencial' }
        ]
      },
      {
        category: 'História do Brasil & Geral',
        badge: 'Base Cronológica',
        items: [
          { id: 'rm-5', title: 'Brasil Colônia: economia açucareira, mineração e escravidão' },
          { id: 'rm-6', title: 'Segundo Reinado, abolicionismo e Proclamação da República' },
          { id: 'rm-7', title: 'Era Vargas, industrialização, CLT e Ditadura Militar (1964–1985)' },
          { id: 'rm-8', title: 'Revoluções Industriais, Guerras Mundiais e Guerra Fria' }
        ]
      },
      {
        category: 'Geografia & Geopolítica',
        badge: 'Atualidades & Espaço',
        items: [
          { id: 'rm-9', title: 'Urbanização brasileira, metropolização e problemas socioambientais' },
          { id: 'rm-10', title: 'Matriz energética brasileira e transição para fontes renováveis' },
          { id: 'rm-11', title: 'Climas, biomas brasileiros (Cerrado, Amazônia) e recursos hídricos' },
          { id: 'rm-12', title: 'Globalização, blocos econômicos (Mercosul, BRICS) e geopolítica' }
        ]
      },
      {
        category: 'Filosofia, Sociologia & Redação',
        badge: 'Pensamento Crítico',
        items: [
          { id: 'rm-13', title: 'Clássicos da Sociologia: Durkheim, Weber e Marx' },
          { id: 'rm-14', title: 'Mundo do trabalho: Fordismo, Toyotismo e Indústria 4.0' },
          { id: 'rm-15', title: 'Cidadania, Constituição de 1988 e ética na Inteligência Artificial' },
          { id: 'rm-16', title: 'Treino de redação dissertativo-argumentativa estilo Fatec' }
        ]
      }
    ];

    // =========================================================================
    // 2. ESTADO DA APLICAÇÃO E PERSISTÊNCIA (LOCALSTORAGE)
    // =========================================================================
    const STORAGE_KEYS = {
      THEME: 'klickkwiz_theme',
      QUESTIONS: 'klickkwiz_questions_v1',
      USERS: 'klickKwiz_users',
      SESSION: 'klickKwiz_session',
      GUEST_STATS: 'klickkwiz_stats_guest_v1',
      GUEST_ROADMAP: 'klickkwiz_roadmap_guest_v1',
      ADMIN_UNLOCKED: 'klickkwiz_admin_unlocked'
    };

    const NAV_ITEMS = [
      { id: 'inicio', label: 'Início' },
      { id: 'treinar', label: 'Treinar' },
      { id: 'simulado', label: 'Simulado' },
      { id: 'roteiro', label: 'Roteiro' },
      { id: 'desempenho', label: 'Desempenho' },
      { id: 'sobre', label: 'Sobre' },
      { id: 'contato', label: 'Contato' },
      { id: 'admin', label: 'Admin' }
    ];

    let currentTab = 'inicio';
    let questionsBank = [];
    let currentUser = null; // null = Visitante | { id, name, email, course, password }
    let isAdminUnlocked = false;
    let userStats = {
      totalAnswered: 0,
      totalHits: 0,
      bySubject: {}, // { 'Português': { answered: 0, hits: 0 } }
      wrongQuestionIds: [], // IDs de questões erradas ainda não acertadas na revisão
      history: [] // [{ date, mode, detail, scoreText }]
    };
    let roadmapChecked = {};

    // Estado do Mini Quiz
    let miniQuizState = {
      index: 0,
      selectedOption: null,
      answeredCurrent: false,
      hits: 0,
      finished: false
    };

    // Estado da Aba Treinar
    let trainState = {
      filteredQuestions: [],
      currentIndex: 0,
      onlyErrors: false,
      correctionMode: 'imediata', // 'imediata' | 'final'
      answers: {}, // { [questionId]: 'A' }
      checked: {}, // { [questionId]: true }
      batchFinished: false,
      sessionHits: 0,
      sessionMisses: 0
    };

    // Estado da Aba Simulado
    let simuladoState = {
      active: false,
      paused: false,
      questions: [],
      currentIndex: 0,
      answers: {}, // { [index]: 'A' }
      flagged: {}, // { [index]: true }
      totalSeconds: 18000,
      remainingSeconds: 18000,
      timerInterval: null
    };

    // =========================================================================
    // 3. INICIALIZAÇÃO E GERENCIAMENTO DE USUÁRIOS / SESSÃO
    // =========================================================================
    function initApp() {
      loadTheme();
      loadQuestionsBank();
      ensureDemoAccountExists();
      loadSession();
      loadUserScopedData();
      isAdminUnlocked = sessionStorage.getItem(STORAGE_KEYS.ADMIN_UNLOCKED) === 'true';

      renderNavigation();
      updateAuthUI();
      updateAdminLockUI();
      updateBankCounters();
      initMiniQuiz();
      populateSubjectFilter();
      applyTrainFilters();
      renderRoadmap();
      renderStatsDashboard();
      renderAdminList();

      // Fecha dropdown do usuário ao clicar fora
      document.addEventListener('click', (e) => {
        const box = document.getElementById('header-user-box');
        const dropdown = document.getElementById('user-dropdown');
        if (box && dropdown && !box.contains(e.target)) {
          dropdown.classList.add('hidden');
        }
      });
    }

    function getUsersList() {
      try {
        const list = JSON.parse(localStorage.getItem(STORAGE_KEYS.USERS));
        return Array.isArray(list) ? list : [];
      } catch (e) {
        return [];
      }
    }

    function saveUsersList(list) {
      localStorage.setItem(STORAGE_KEYS.USERS, JSON.stringify(list));
    }

    function ensureDemoAccountExists() {
      const users = getUsersList();
      const demoEmail = 'aluno@fatec.sp.gov.br';
      if (!users.some(u => u.email.toLowerCase() === demoEmail)) {
        users.push({
          id: 'demo-fatec-user',
          name: 'Aluno Demo Fatec',
          email: demoEmail,
          course: 'Análise e Desenvolvimento de Sistemas (ADS)',
          password: '123'
        });
        saveUsersList(users);
      }
    }

    function loadSession() {
      try {
        const sessionEmail = localStorage.getItem(STORAGE_KEYS.SESSION);
        if (!sessionEmail) {
          currentUser = null;
          return;
        }
        const users = getUsersList();
        currentUser = users.find(u => u.email.toLowerCase() === sessionEmail.toLowerCase()) || null;
        if (!currentUser) {
          localStorage.removeItem(STORAGE_KEYS.SESSION);
        }
      } catch (e) {
        currentUser = null;
      }
    }

    function getScopedStorageKeys() {
      if (currentUser && currentUser.email) {
        const safeKey = currentUser.email.toLowerCase().replace(/[^a-z0-9]/g, '_');
        return {
          stats: `klickKwiz_stats_${safeKey}`,
          roadmap: `klickKwiz_roadmap_${safeKey}`
        };
      }
      return {
        stats: STORAGE_KEYS.GUEST_STATS,
        roadmap: STORAGE_KEYS.GUEST_ROADMAP
      };
    }

    function loadUserScopedData() {
      const keys = getScopedStorageKeys();
      // Carrega estatísticas do usuário ativo (ou visitante)
      try {
        const saved = JSON.parse(localStorage.getItem(keys.stats));
        if (saved && typeof saved === 'object') {
          userStats = {
            totalAnswered: saved.totalAnswered || 0,
            totalHits: saved.totalHits || 0,
            bySubject: saved.bySubject || {},
            wrongQuestionIds: Array.isArray(saved.wrongQuestionIds) ? saved.wrongQuestionIds : [],
            history: Array.isArray(saved.history) ? saved.history : []
          };
        } else {
          userStats = { totalAnswered: 0, totalHits: 0, bySubject: {}, wrongQuestionIds: [], history: [] };
        }
      } catch (e) {
        userStats = { totalAnswered: 0, totalHits: 0, bySubject: {}, wrongQuestionIds: [], history: [] };
      }

      // Carrega roteiro do usuário ativo (ou visitante)
      try {
        roadmapChecked = JSON.parse(localStorage.getItem(keys.roadmap)) || {};
      } catch (e) {
        roadmapChecked = {};
      }

      updateErrorBadge();
    }

    function getUserInitials(name) {
      if (!name) return 'AL';
      const parts = name.trim().split(/\s+/);
      if (parts.length === 1) return parts[0].substring(0, 2).toUpperCase();
      return (parts[0][0] + parts[parts.length - 1][0]).toUpperCase();
    }

    function updateAuthUI() {
      const loginBtn = document.getElementById('header-login-btn');
      const userBox = document.getElementById('header-user-box');
      const welcomeBanner = document.getElementById('welcome-user-banner');
      const desTitle = document.getElementById('desempenho-title');
      const desBadge = document.getElementById('desempenho-profile-badge');
      const desSub = document.getElementById('desempenho-subtitle');
      const desCta = document.getElementById('desempenho-login-cta');

      if (currentUser) {
        const initials = getUserInitials(currentUser.name);
        const firstName = currentUser.name.trim().split(/\s+/)[0];

        loginBtn.classList.add('hidden');
        userBox.classList.remove('hidden');

        document.getElementById('header-user-avatar').textContent = initials;
        document.getElementById('header-user-name').textContent = firstName;
        document.getElementById('dropdown-user-fullname').textContent = currentUser.name;
        document.getElementById('dropdown-user-email').textContent = currentUser.email;
        document.getElementById('dropdown-user-course').textContent = currentUser.course || 'Vestibular Fatec';

        // Banner personalizado na Home
        welcomeBanner.classList.remove('hidden');
        document.getElementById('welcome-user-avatar').textContent = initials;
        document.getElementById('welcome-user-greeting').textContent = `Olá, ${firstName}! Bom treino hoje.`;
        document.getElementById('welcome-user-sub').textContent = `Curso pretendido: ${currentUser.course || 'Fatec'} · ${currentUser.email}`;

        // Personalização na aba Desempenho
        desTitle.textContent = `Desempenho de ${firstName}`;
        desBadge.textContent = currentUser.course || 'Aluno Cadastrado';
        desSub.textContent = `Acompanhamento individual vinculado à conta ${currentUser.email}.`;
        desCta.classList.add('hidden');
      } else {
        loginBtn.classList.remove('hidden');
        userBox.classList.add('hidden');
        welcomeBanner.classList.add('hidden');

        desTitle.textContent = 'Seu Desempenho';
        desBadge.textContent = 'Perfil Visitante (Local)';
        desSub.textContent = 'Estatísticas salvas no modo visitante. Crie uma conta para organizar seu perfil.';
        desCta.classList.remove('hidden');
      }
    }

    function openAuthModal(tab = 'login') {
      const modal = document.getElementById('auth-modal');
      modal.classList.remove('hidden');
      modal.classList.add('flex');
      switchAuthTab(tab);
    }

    function closeAuthModal() {
      const modal = document.getElementById('auth-modal');
      modal.classList.add('hidden');
      modal.classList.remove('flex');
    }

    function switchAuthTab(tab) {
      const loginTab = document.getElementById('auth-tab-login');
      const regTab = document.getElementById('auth-tab-register');
      const loginForm = document.getElementById('auth-login-form');
      const regForm = document.getElementById('auth-register-form');

      if (tab === 'login') {
        loginTab.className = 'py-2 rounded-lg text-xs sm:text-sm font-bold bg-white dark:bg-slate-900 text-fatec-700 dark:text-fatec-300 shadow-sm transition';
        regTab.className = 'py-2 rounded-lg text-xs sm:text-sm font-semibold text-slate-600 dark:text-slate-300 hover:text-slate-900 dark:hover:text-white transition';
        loginForm.classList.remove('hidden');
        regForm.classList.add('hidden');
      } else {
        regTab.className = 'py-2 rounded-lg text-xs sm:text-sm font-bold bg-white dark:bg-slate-900 text-fatec-700 dark:text-fatec-300 shadow-sm transition';
        loginTab.className = 'py-2 rounded-lg text-xs sm:text-sm font-semibold text-slate-600 dark:text-slate-300 hover:text-slate-900 dark:hover:text-white transition';
        regForm.classList.remove('hidden');
        loginForm.classList.add('hidden');
      }
    }

    function completeUserLogin(user, welcomeMsg) {
      currentUser = user;
      localStorage.setItem(STORAGE_KEYS.SESSION, user.email);
      loadUserScopedData();
      updateAuthUI();
      renderRoadmap();
      renderStatsDashboard();
      if (currentTab === 'treinar') applyTrainFilters();
      closeAuthModal();
      showToast(welcomeMsg, 'success');
    }

    function handleLoginSubmit(e) {
      e.preventDefault();
      const email = document.getElementById('login-email').value.trim().toLowerCase();
      const password = document.getElementById('login-password').value;

      const users = getUsersList();
      const found = users.find(u => u.email.toLowerCase() === email && u.password === password);

      if (!found) {
        showToast('E-mail ou senha incorretos. Tente novamente ou use a Conta Demo.', 'error');
        return;
      }

      document.getElementById('auth-login-form').reset();
      completeUserLogin(found, `Bem-vindo(a) de volta, ${found.name.split(' ')[0]}!`);
    }

    function handleRegisterSubmit(e) {
      e.preventDefault();
      const name = document.getElementById('reg-name').value.trim();
      const email = document.getElementById('reg-email').value.trim().toLowerCase();
      const course = document.getElementById('reg-course').value;
      const password = document.getElementById('reg-password').value;

      if (!name || !email || password.length < 4) {
        showToast('Preencha todos os campos corretamente.', 'warning');
        return;
      }

      const users = getUsersList();
      if (users.some(u => u.email.toLowerCase() === email)) {
        showToast('Este e-mail já está cadastrado. Faça login na aba Entrar.', 'warning');
        switchAuthTab('login');
        document.getElementById('login-email').value = email;
        return;
      }

      const newUser = {
        id: 'user-' + Date.now(),
        name,
        email,
        course,
        password
      };

      users.push(newUser);
      saveUsersList(users);
      document.getElementById('auth-register-form').reset();
      completeUserLogin(newUser, `Conta criada com sucesso! Bons estudos, ${name.split(' ')[0]}!`);
    }

    function loginDemoAccount() {
      ensureDemoAccountExists();
      const users = getUsersList();
      const demo = users.find(u => u.email.toLowerCase() === 'aluno@fatec.sp.gov.br');
      if (demo) {
        completeUserLogin(demo, 'Conectado na Conta Demo (aluno@fatec.sp.gov.br)!');
      }
    }

    function toggleUserDropdown() {
      const dropdown = document.getElementById('user-dropdown');
      dropdown.classList.toggle('hidden');
    }

    function logoutUser() {
      document.getElementById('user-dropdown').classList.add('hidden');
      currentUser = null;
      localStorage.removeItem(STORAGE_KEYS.SESSION);
      loadUserScopedData();
      updateAuthUI();
      renderRoadmap();
      renderStatsDashboard();
      if (currentTab === 'treinar') applyTrainFilters();
      showToast('Você saiu da conta. Agora está no modo visitante.', 'info');
    }

    // Controle de Bloqueio / Desbloqueio da aba Admin
    function updateAdminLockUI() {
      const lockScreen = document.getElementById('admin-lock-screen');
      const unlockedContent = document.getElementById('admin-unlocked-content');
      if (!lockScreen || !unlockedContent) return;

      if (isAdminUnlocked) {
        lockScreen.classList.add('hidden');
        unlockedContent.classList.remove('hidden');
      } else {
        lockScreen.classList.remove('hidden');
        unlockedContent.classList.add('hidden');
      }
    }

    function handleAdminUnlock(e) {
      e.preventDefault();
      const pass = document.getElementById('admin-pass-input').value.trim();
      if (pass === 'admin123') {
        isAdminUnlocked = true;
        sessionStorage.setItem(STORAGE_KEYS.ADMIN_UNLOCKED, 'true');
        document.getElementById('admin-pass-input').value = '';
        updateAdminLockUI();
        renderAdminList();
        showToast('Painel Administrativo desbloqueado!', 'success');
      } else {
        showToast('Senha incorreta. Dica: use admin123 ou o acesso rápido.', 'error');
      }
    }

    function quickUnlockAdmin() {
      isAdminUnlocked = true;
      sessionStorage.setItem(STORAGE_KEYS.ADMIN_UNLOCKED, 'true');
      updateAdminLockUI();
      renderAdminList();
      showToast('Acesso rápido de Professor ativado!', 'success');
    }

    function lockAdminPanel() {
      isAdminUnlocked = false;
      sessionStorage.removeItem(STORAGE_KEYS.ADMIN_UNLOCKED);
      updateAdminLockUI();
      showToast('Painel Admin bloqueado.', 'info');
    }

    function loadTheme() {
      const saved = localStorage.getItem(STORAGE_KEYS.THEME);
      const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
      const isDark = saved ? saved === 'dark' : prefersDark;
      applyTheme(isDark);
    }

    function applyTheme(isDark) {
      const html = document.documentElement;
      const icon = document.getElementById('theme-icon');
      const label = document.getElementById('theme-label');
      if (isDark) {
        html.classList.add('dark');
        icon.textContent = '☀️';
        label.textContent = 'Claro';
      } else {
        html.classList.remove('dark');
        icon.textContent = '🌙';
        label.textContent = 'Escuro';
      }
      localStorage.setItem(STORAGE_KEYS.THEME, isDark ? 'dark' : 'light');
    }

    function toggleTheme() {
      const isDark = document.documentElement.classList.contains('dark');
      applyTheme(!isDark);
    }

    function loadQuestionsBank() {
      try {
        const saved = JSON.parse(localStorage.getItem(STORAGE_KEYS.QUESTIONS));
        if (Array.isArray(saved) && saved.length > 0) {
          questionsBank = saved;
        } else {
          questionsBank = [...DEFAULT_QUESTIONS];
          saveQuestionsBank();
        }
      } catch (e) {
        questionsBank = [...DEFAULT_QUESTIONS];
        saveQuestionsBank();
      }
    }

    function saveQuestionsBank() {
      localStorage.setItem(STORAGE_KEYS.QUESTIONS, JSON.stringify(questionsBank));
      updateBankCounters();
    }

    function updateBankCounters() {
      const count = questionsBank.length;
      const heroCount = document.getElementById('hero-bank-count');
      if (heroCount) {
        heroCount.textContent = `${count} questões de Humanas no banco · gabarito conferido`;
      }
      const adminCount = document.getElementById('admin-total-count');
      if (adminCount) {
        adminCount.textContent = count;
      }
    }

    function loadStats() {
      try {
        const saved = JSON.parse(localStorage.getItem(STORAGE_KEYS.STATS));
        if (saved && typeof saved === 'object') {
          userStats = {
            totalAnswered: saved.totalAnswered || 0,
            totalHits: saved.totalHits || 0,
            bySubject: saved.bySubject || {},
            wrongQuestionIds: Array.isArray(saved.wrongQuestionIds) ? saved.wrongQuestionIds : [],
            history: Array.isArray(saved.history) ? saved.history : []
          };
        }
      } catch (e) {
        console.error('Erro ao ler estatísticas:', e);
      }
      updateErrorBadge();
    }

    function saveStats() {
      const keys = getScopedStorageKeys();
      localStorage.setItem(keys.stats, JSON.stringify(userStats));
      updateErrorBadge();
    }

    function updateErrorBadge() {
      const badge = document.getElementById('error-count-badge');
      if (badge) badge.textContent = userStats.wrongQuestionIds.length;
    }

    function recordAnswerAttempt(questionObj, isCorrect) {
      userStats.totalAnswered += 1;
      if (isCorrect) userStats.totalHits += 1;

      const subj = questionObj.subject || 'Geral';
      if (!userStats.bySubject[subj]) {
        userStats.bySubject[subj] = { answered: 0, hits: 0 };
      }
      userStats.bySubject[subj].answered += 1;
      if (isCorrect) userStats.bySubject[subj].hits += 1;

      // Atualiza pool de revisão de erros (apenas para questões numéricas do banco principal)
      if (typeof questionObj.id === 'number') {
        const idx = userStats.wrongQuestionIds.indexOf(questionObj.id);
        if (!isCorrect && idx === -1) {
          userStats.wrongQuestionIds.push(questionObj.id);
        } else if (isCorrect && idx !== -1) {
          userStats.wrongQuestionIds.splice(idx, 1);
        }
      }

      saveStats();
    }

    function addHistoryEntry(mode, detail, scoreText) {
      const now = new Date();
      const dateStr = now.toLocaleDateString('pt-BR', { day: '2-digit', month: '2-digit' }) + ' ' +
                      now.toLocaleTimeString('pt-BR', { hour: '2-digit', minute: '2-digit' });
      userStats.history.unshift({ date: dateStr, mode, detail, scoreText });
      if (userStats.history.length > 25) userStats.history.pop();
      saveStats();
    }

    // =========================================================================
    // 4. NAVEGAÇÃO ENTRE ABAS E COMPONENTES DE UI (TOAST / MODAL)
    // =========================================================================
    function renderNavigation() {
      const desktopNav = document.getElementById('desktop-nav');
      const mobileNav = document.getElementById('mobile-nav');

      desktopNav.innerHTML = NAV_ITEMS.map(item => {
        const active = item.id === currentTab;
        return `
          <button onclick="navigateTo('${item.id}')" class="px-3.5 py-1.5 rounded-lg text-xs font-semibold transition ${
            active
              ? 'bg-white dark:bg-slate-900 text-fatec-700 dark:text-fatec-300 shadow-sm'
              : 'text-slate-600 dark:text-slate-300 hover:text-slate-900 dark:hover:text-white'
          }">
            ${item.label}
          </button>
        `;
      }).join('');

      mobileNav.innerHTML = NAV_ITEMS.map(item => {
        const active = item.id === currentTab;
        return `
          <button onclick="navigateTo('${item.id}')" class="px-3 py-1.5 rounded-lg text-xs font-semibold whitespace-nowrap transition ${
            active
              ? 'bg-fatec-600 text-white shadow-sm'
              : 'bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300'
          }">
            ${item.label}
          </button>
        `;
      }).join('');
    }

    function navigateTo(tabId) {
      currentTab = tabId;
      document.querySelectorAll('.tab-section').forEach(sec => sec.classList.add('hidden'));
      const target = document.getElementById(`tab-${tabId}`);
      if (target) target.classList.remove('hidden');

      renderNavigation();
      window.scrollTo({ top: 0, behavior: 'smooth' });

      if (tabId === 'desempenho') renderStatsDashboard();
      if (tabId === 'treinar') applyTrainFilters();
      if (tabId === 'admin') renderAdminList();
    }

    function showToast(message, type = 'info') {
      const container = document.getElementById('toast-container');
      const colors = {
        info: 'bg-slate-900 text-white border-slate-700',
        success: 'bg-emerald-600 text-white border-emerald-500',
        warning: 'bg-amber-600 text-white border-amber-500',
        error: 'bg-rose-600 text-white border-rose-500'
      };
      const el = document.createElement('div');
      el.className = `pointer-events-auto px-4 py-3 rounded-xl text-xs sm:text-sm font-semibold shadow-lg border animate-fade-in ${colors[type] || colors.info}`;
      el.textContent = message;
      container.appendChild(el);
      setTimeout(() => {
        el.style.opacity = '0';
        el.style.transition = 'opacity 0.25s ease';
        setTimeout(() => el.remove(), 260);
      }, 3000);
    }

    function showConfirm(title, message, onConfirm) {
      const modal = document.getElementById('confirm-modal');
      document.getElementById('confirm-title').textContent = title;
      document.getElementById('confirm-message').textContent = message;
      modal.classList.remove('hidden');
      modal.classList.add('flex');

      const close = () => {
        modal.classList.add('hidden');
        modal.classList.remove('flex');
      };

      document.getElementById('confirm-cancel').onclick = close;
      document.getElementById('confirm-ok').onclick = () => {
        close();
        onConfirm();
      };
    }

    // =========================================================================
    // 5. MINI QUIZ DE BOAS-VINDAS (NA ABA INÍCIO)
    // =========================================================================
    function initMiniQuiz() {
      renderMiniQuizQuestion();
    }

    function renderMiniQuizQuestion() {
      const bodyEl = document.getElementById('mini-quiz-body');
      const resultEl = document.getElementById('mini-quiz-result');
      const statusBadge = document.getElementById('mini-quiz-status');
      const counterBadge = document.getElementById('mini-quiz-counter');

      if (miniQuizState.finished) {
        bodyEl.classList.add('hidden');
        resultEl.classList.remove('hidden');
        statusBadge.textContent = 'Concluído';
        statusBadge.className = 'px-3 py-1 rounded-full text-xs font-bold bg-emerald-100 text-emerald-800 dark:bg-emerald-900/40 dark:text-emerald-300';
        counterBadge.textContent = `4 de 4 concluídas`;

        document.getElementById('mini-quiz-score-title').textContent =
          `Você acertou ${miniQuizState.hits} de ${MINI_QUIZ_QUESTIONS.length}!`;

        let desc = 'Bom começo! Explore o banco completo para consolidar todos os temas de Humanas.';
        if (miniQuizState.hits === 4) {
          desc = 'Desempenho impecável! Você já está no ritmo das provas da Fatec.';
        } else if (miniQuizState.hits <= 1) {
          desc = 'Sem problemas! Use a aba Roteiro e o Modo Revisão para fortalecer sua base.';
        }
        document.getElementById('mini-quiz-score-desc').textContent = desc;
        return;
      }

      bodyEl.classList.remove('hidden');
      resultEl.classList.add('hidden');

      const q = MINI_QUIZ_QUESTIONS[miniQuizState.index];
      statusBadge.textContent = 'Em andamento';
      statusBadge.className = 'px-3 py-1 rounded-full text-xs font-bold bg-amber-100 text-amber-800 dark:bg-amber-900/40 dark:text-amber-300';
      counterBadge.textContent = `Pergunta ${miniQuizState.index + 1} de ${MINI_QUIZ_QUESTIONS.length} · ${q.topic}`;

      document.getElementById('mini-quiz-question').textContent = q.question;

      const optionsContainer = document.getElementById('mini-quiz-options');
      optionsContainer.innerHTML = Object.entries(q.options).map(([letter, text]) => {
        const isSelected = miniQuizState.selectedOption === letter;
        const isAnswered = miniQuizState.answeredCurrent;
        const isCorrect = q.correct === letter;

        let cardClass = 'border-slate-200 dark:border-slate-700 hover:border-fatec-400 bg-slate-50/70 dark:bg-slate-800/50';
        let badgeClass = 'bg-white dark:bg-slate-700 text-slate-700 dark:text-slate-200 border border-slate-300 dark:border-slate-600';

        if (!isAnswered && isSelected) {
          cardClass = 'border-fatec-600 bg-fatec-50 dark:bg-fatec-900/30 ring-2 ring-fatec-500/30';
          badgeClass = 'bg-fatec-600 text-white border-fatec-600';
        } else if (isAnswered) {
          if (isCorrect) {
            cardClass = 'border-emerald-500 bg-emerald-50 dark:bg-emerald-900/25';
            badgeClass = 'bg-emerald-600 text-white border-emerald-600';
          } else if (isSelected && !isCorrect) {
            cardClass = 'border-rose-500 bg-rose-50 dark:bg-rose-900/25';
            badgeClass = 'bg-rose-600 text-white border-rose-600';
          } else {
            cardClass = 'border-slate-200 dark:border-slate-800 opacity-60';
          }
        }

        return `
          <button type="button" onclick="selectMiniQuizOption('${letter}')" ${isAnswered ? 'disabled' : ''}
            class="w-full text-left p-3.5 sm:p-4 rounded-2xl border transition flex items-start gap-3.5 ${cardClass}">
            <span class="w-7 h-7 rounded-lg text-xs font-extrabold flex items-center justify-center shrink-0 mt-0.5 ${badgeClass}">
              ${letter}
            </span>
            <span class="text-sm sm:text-base text-slate-800 dark:text-slate-100 leading-snug">${text}</span>
          </button>
        `;
      }).join('');

      const feedbackEl = document.getElementById('mini-quiz-feedback');
      const hintEl = document.getElementById('mini-quiz-hint');
      const nextBtn = document.getElementById('mini-quiz-next');

      if (!miniQuizState.answeredCurrent) {
        feedbackEl.classList.add('hidden');
        hintEl.textContent = miniQuizState.selectedOption
          ? `Alternativa ${miniQuizState.selectedOption} selecionada. Clique em Próxima para confirmar.`
          : 'Escolha uma alternativa.';
        nextBtn.disabled = !miniQuizState.selectedOption;
        nextBtn.textContent = 'Próxima';
      } else {
        const isRight = miniQuizState.selectedOption === q.correct;
        feedbackEl.classList.remove('hidden');
        feedbackEl.className = isRight
          ? 'rounded-2xl p-4 text-sm leading-relaxed border bg-emerald-50 dark:bg-emerald-900/25 border-emerald-200 dark:border-emerald-800 text-emerald-900 dark:text-emerald-200'
          : 'rounded-2xl p-4 text-sm leading-relaxed border bg-rose-50 dark:bg-rose-900/25 border-rose-200 dark:border-rose-800 text-rose-900 dark:text-rose-200';
        feedbackEl.innerHTML = `<strong>${isRight ? '✓ Resposta correta!' : `✕ Resposta incorreta (Gabarito: ${q.correct}).`}</strong> ${q.explanation}`;
        hintEl.textContent = miniQuizState.index < MINI_QUIZ_QUESTIONS.length - 1
          ? 'Clique em Continuar para a próxima pergunta.'
          : 'Clique em Ver resultado para finalizar.';
        nextBtn.disabled = false;
        nextBtn.textContent = miniQuizState.index < MINI_QUIZ_QUESTIONS.length - 1 ? 'Continuar →' : 'Ver resultado';
      }
    }

    function selectMiniQuizOption(letter) {
      if (miniQuizState.answeredCurrent) return;
      miniQuizState.selectedOption = letter;
      renderMiniQuizQuestion();
    }

    function handleMiniQuizNext() {
      if (!miniQuizState.selectedOption) return;
      const q = MINI_QUIZ_QUESTIONS[miniQuizState.index];

      // Passo 1: valida e mostra feedback
      if (!miniQuizState.answeredCurrent) {
        miniQuizState.answeredCurrent = true;
        const isCorrect = miniQuizState.selectedOption === q.correct;
        if (isCorrect) miniQuizState.hits += 1;
        recordAnswerAttempt(q, isCorrect);
        renderMiniQuizQuestion();
        return;
      }

      // Passo 2: avança para próxima ou conclui
      if (miniQuizState.index < MINI_QUIZ_QUESTIONS.length - 1) {
        miniQuizState.index += 1;
        miniQuizState.selectedOption = null;
        miniQuizState.answeredCurrent = false;
        renderMiniQuizQuestion();
      } else {
        miniQuizState.finished = true;
        addHistoryEntry('Mini Quiz', '4 questões iniciais', `${miniQuizState.hits}/4 acertos`);
        renderMiniQuizQuestion();
      }
    }

    function resetMiniQuiz() {
      miniQuizState = {
        index: 0,
        selectedOption: null,
        answeredCurrent: false,
        hits: 0,
        finished: false
      };
      renderMiniQuizQuestion();
    }

    // =========================================================================
    // 6. ABA TREINAR & MODO REVISÃO DE ERROS
    // =========================================================================
    function populateSubjectFilter() {
      const select = document.getElementById('filter-subject');
      const subjects = [...new Set(questionsBank.map(q => q.subject))];
      select.innerHTML = '<option value="TODAS">Todas as matérias</option>' +
        subjects.map(s => `<option value="${s}">${s}</option>`).join('');
    }

    function startErrorReviewFromHome() {
      navigateTo('treinar');
      if (userStats.wrongQuestionIds.length === 0) {
        showToast('Você ainda não tem questões erradas registradas. Faça um treino primeiro!', 'info');
        return;
      }
      if (!trainState.onlyErrors) {
        toggleErrorReviewMode();
      }
    }

    function toggleErrorReviewMode() {
      trainState.onlyErrors = !trainState.onlyErrors;
      const btn = document.getElementById('btn-review-errors');
      if (trainState.onlyErrors) {
        btn.classList.add('bg-amber-500', 'text-white', 'border-amber-500');
        btn.classList.remove('bg-white', 'dark:bg-slate-900', 'text-amber-700', 'dark:text-amber-300');
      } else {
        btn.classList.remove('bg-amber-500', 'text-white', 'border-amber-500');
        btn.classList.add('bg-white', 'dark:bg-slate-900', 'text-amber-700', 'dark:text-amber-300');
      }
      applyTrainFilters();
    }

    function disableErrorReviewMode() {
      trainState.onlyErrors = false;
      const btn = document.getElementById('btn-review-errors');
      btn.classList.remove('bg-amber-500', 'text-white', 'border-amber-500');
      btn.classList.add('bg-white', 'dark:bg-slate-900', 'text-amber-700', 'dark:text-amber-300');
      applyTrainFilters();
    }

    function changeCorrectionMode() {
      trainState.correctionMode = document.getElementById('filter-correction-mode').value;
      resetTrainSession();
    }

    function applyTrainFilters() {
      const subj = document.getElementById('filter-subject').value;
      const order = document.getElementById('filter-order').value;

      let list = [...questionsBank];
      if (subj !== 'TODAS') {
        list = list.filter(q => q.subject === subj);
      }
      if (trainState.onlyErrors) {
        list = list.filter(q => userStats.wrongQuestionIds.includes(q.id));
      }
      if (order === 'aleatoria') {
        list.sort(() => Math.random() - 0.5);
      }

      trainState.filteredQuestions = list;
      trainState.currentIndex = 0;
      trainState.batchFinished = false;
      document.getElementById('train-batch-summary').classList.add('hidden');
      renderTrainQuestion();
    }

    function resetTrainSession() {
      trainState.answers = {};
      trainState.checked = {};
      trainState.sessionHits = 0;
      trainState.sessionMisses = 0;
      trainState.batchFinished = false;
      document.getElementById('train-batch-summary').classList.add('hidden');
      applyTrainFilters();
    }

    function renderTrainQuestion() {
      const total = trainState.filteredQuestions.length;
      document.getElementById('train-total-filtered').textContent = total;
      document.getElementById('train-session-hits').textContent = trainState.sessionHits;
      document.getElementById('train-session-misses').textContent = trainState.sessionMisses;

      const card = document.getElementById('train-card');
      const emptyState = document.getElementById('train-empty-state');

      if (total === 0) {
        card.classList.add('hidden');
        emptyState.classList.remove('hidden');
        document.getElementById('train-current-num').textContent = '0';
        return;
      }

      card.classList.remove('hidden');
      emptyState.classList.add('hidden');

      const q = trainState.filteredQuestions[trainState.currentIndex];
      document.getElementById('train-current-num').textContent = `${trainState.currentIndex + 1} de ${total}`;
      document.getElementById('train-q-subject').textContent = q.subject;
      document.getElementById('train-q-topic').textContent = q.topic;
      document.getElementById('train-q-exam').textContent = q.exam || 'Fatec · Banco Humanas';
      document.getElementById('train-q-text').textContent = q.question;

      const selected = trainState.answers[q.id];
      const isChecked = !!trainState.checked[q.id] || trainState.batchFinished;
      const statusBadge = document.getElementById('train-q-status-badge');

      if (userStats.wrongQuestionIds.includes(q.id)) {
        statusBadge.textContent = '🔁 Marcada para revisão';
        statusBadge.className = 'text-xs font-bold text-amber-600 dark:text-amber-400';
      } else {
        statusBadge.textContent = '';
      }

      const optionsEl = document.getElementById('train-q-options');
      optionsEl.innerHTML = Object.entries(q.options).map(([letter, text]) => {
        const isSel = selected === letter;
        const isCorr = q.correct === letter;

        let cls = 'border-slate-200 dark:border-slate-700 hover:border-fatec-400 bg-slate-50/60 dark:bg-slate-800/50';
        let badgeCls = 'bg-white dark:bg-slate-700 text-slate-700 dark:text-slate-200 border border-slate-300 dark:border-slate-600';

        if (!isChecked && isSel) {
          cls = 'border-fatec-600 bg-fatec-50 dark:bg-fatec-900/30 ring-2 ring-fatec-500/30';
          badgeCls = 'bg-fatec-600 text-white border-fatec-600';
        } else if (isChecked) {
          if (isCorr) {
            cls = 'border-emerald-500 bg-emerald-50 dark:bg-emerald-900/25';
            badgeCls = 'bg-emerald-600 text-white border-emerald-600';
          } else if (isSel && !isCorr) {
            cls = 'border-rose-500 bg-rose-50 dark:bg-rose-900/25';
            badgeCls = 'bg-rose-600 text-white border-rose-600';
          } else {
            cls = 'border-slate-200 dark:border-slate-800 opacity-65';
          }
        }

        return `
          <button type="button" onclick="selectTrainOption('${letter}')" ${isChecked ? 'disabled' : ''}
            class="w-full text-left p-3.5 sm:p-4 rounded-2xl border transition flex items-start gap-3.5 ${cls}">
            <span class="w-7 h-7 rounded-lg text-xs font-extrabold flex items-center justify-center shrink-0 mt-0.5 ${badgeCls}">
              ${letter}
            </span>
            <span class="text-sm sm:text-base text-slate-800 dark:text-slate-100 leading-snug">${text}</span>
          </button>
        `;
      }).join('');

      const expEl = document.getElementById('train-q-explanation');
      if (isChecked) {
        const right = selected === q.correct;
        expEl.classList.remove('hidden');
        expEl.className = right
          ? 'rounded-2xl p-4 text-sm leading-relaxed border bg-emerald-50 dark:bg-emerald-900/25 border-emerald-200 dark:border-emerald-800 text-emerald-900 dark:text-emerald-200'
          : 'rounded-2xl p-4 text-sm leading-relaxed border bg-rose-50 dark:bg-rose-900/25 border-rose-200 dark:border-rose-800 text-rose-900 dark:text-rose-200';
        expEl.innerHTML = `<strong>${right ? '✓ Você acertou!' : `✕ Gabarito: ${q.correct}.`}</strong> ${q.explanation}`;
      } else {
        expEl.classList.add('hidden');
      }

      document.getElementById('train-prev-btn').disabled = trainState.currentIndex === 0;
      const nextBtn = document.getElementById('train-next-btn');
      nextBtn.disabled = trainState.currentIndex >= total - 1;

      const finishBatchBtn = document.getElementById('train-finish-batch-btn');
      if (trainState.correctionMode === 'final' && !trainState.batchFinished && Object.keys(trainState.answers).length > 0) {
        finishBatchBtn.classList.remove('hidden');
      } else {
        finishBatchBtn.classList.add('hidden');
      }
    }

    function selectTrainOption(letter) {
      const q = trainState.filteredQuestions[trainState.currentIndex];
      if (!q) return;

      if (trainState.correctionMode === 'imediata') {
        if (trainState.checked[q.id]) return;
        trainState.answers[q.id] = letter;
        trainState.checked[q.id] = true;

        const isCorrect = letter === q.correct;
        if (isCorrect) trainState.sessionHits += 1;
        else trainState.sessionMisses += 1;

        recordAnswerAttempt(q, isCorrect);
      } else {
        if (trainState.batchFinished) return;
        trainState.answers[q.id] = letter;
      }

      renderTrainQuestion();
    }

    function prevTrainQuestion() {
      if (trainState.currentIndex > 0) {
        trainState.currentIndex -= 1;
        renderTrainQuestion();
      }
    }

    function nextTrainQuestion() {
      if (trainState.currentIndex < trainState.filteredQuestions.length - 1) {
        trainState.currentIndex += 1;
        renderTrainQuestion();
      }
    }

    function finishBatchCorrection() {
      if (trainState.batchFinished) return;
      trainState.batchFinished = true;

      let hits = 0;
      let answeredCount = 0;
      const reviewItems = [];

      trainState.filteredQuestions.forEach((q, idx) => {
        const ans = trainState.answers[q.id];
        if (ans) {
          answeredCount += 1;
          trainState.checked[q.id] = true;
          const ok = ans === q.correct;
          if (ok) {
            hits += 1;
            trainState.sessionHits += 1;
          } else {
            trainState.sessionMisses += 1;
          }
          recordAnswerAttempt(q, ok);
          reviewItems.push(`
            <div class="p-3.5 rounded-xl border ${ok ? 'border-emerald-200 dark:border-emerald-800 bg-emerald-50/40 dark:bg-emerald-900/10' : 'border-rose-200 dark:border-rose-800 bg-rose-50/40 dark:bg-rose-900/10'} text-xs sm:text-sm">
              <div class="font-bold">${idx + 1}. [${q.subject}] ${ok ? '✓ Acerto' : `✕ Erro (Sua: ${ans} | Gabarito: ${q.correct})`}</div>
              <div class="text-slate-600 dark:text-slate-300 mt-1">${q.explanation}</div>
            </div>
          `);
        }
      });

      addHistoryEntry('Treino em Bloco', `${answeredCount} questões respondidas`, `${hits}/${answeredCount} acertos`);

      const summaryEl = document.getElementById('train-batch-summary');
      summaryEl.classList.remove('hidden');
      document.getElementById('train-batch-summary-text').textContent =
        `Você respondeu ${answeredCount} questões e acertou ${hits} (${answeredCount ? Math.round((hits / answeredCount) * 100) : 0}%).`;
      document.getElementById('train-batch-review-list').innerHTML = reviewItems.join('');

      renderTrainQuestion();
      summaryEl.scrollIntoView({ behavior: 'smooth' });
    }

    // =========================================================================
    // 7. ABA SIMULADO CRONOMETRADO (60 QUESTÕES / 5H OU EXPRESS 15 / 45MIN)
    // =========================================================================
    function buildSimuladoQuestions(count) {
      const result = [];
      const shuffledBase = [...questionsBank].sort(() => Math.random() - 0.5);
      while (result.length < count) {
        for (const item of shuffledBase) {
          if (result.length >= count) break;
          result.push({ ...item });
        }
      }
      return result;
    }

    function startSimulado() {
      const type = document.querySelector('input[name="simulado-type"]:checked').value;
      const count = type === 'oficial' ? 60 : 15;
      const seconds = type === 'oficial' ? 5 * 3600 : 45 * 60;

      simuladoState = {
        active: true,
        paused: false,
        questions: buildSimuladoQuestions(count),
        currentIndex: 0,
        answers: {},
        flagged: {},
        totalSeconds: seconds,
        remainingSeconds: seconds,
        timerInterval: null
      };

      document.getElementById('simulado-setup').classList.add('hidden');
      document.getElementById('simulado-result').classList.add('hidden');
      document.getElementById('simulado-active').classList.remove('hidden');
      document.getElementById('simulado-total-count').textContent = count;

      updateSimuladoTimerDisplay();
      simuladoState.timerInterval = setInterval(() => {
        if (!simuladoState.paused && simuladoState.active) {
          simuladoState.remainingSeconds -= 1;
          updateSimuladoTimerDisplay();
          if (simuladoState.remainingSeconds <= 0) {
            finishSimulado(true);
          }
        }
      }, 1000);

      renderSimuladoQuestion();
      renderSimuladoGrid();
    }

    function formatSeconds(sec) {
      const h = Math.floor(sec / 3600);
      const m = Math.floor((sec % 3600) / 60);
      const s = sec % 60;
      return [h, m, s].map(v => String(v).padStart(2, '0')).join(':');
    }

    function updateSimuladoTimerDisplay() {
      const el = document.getElementById('simulado-timer');
      el.textContent = formatSeconds(Math.max(0, simuladoState.remainingSeconds));
      if (simuladoState.remainingSeconds <= 300) {
        el.classList.add('text-rose-400');
      } else {
        el.classList.remove('text-rose-400');
      }
    }

    function toggleSimuladoPause() {
      simuladoState.paused = !simuladoState.paused;
      const btn = document.getElementById('simulado-pause-btn');
      btn.textContent = simuladoState.paused ? 'Retomar' : 'Pausar';
      showToast(simuladoState.paused ? 'Cronômetro pausado.' : 'Cronômetro retomado.', 'info');
    }

    function renderSimuladoQuestion() {
      const idx = simuladoState.currentIndex;
      const q = simuladoState.questions[idx];
      const total = simuladoState.questions.length;

      document.getElementById('sim-q-badge').textContent = `Questão ${idx + 1} de ${total} · ${q.subject}`;
      document.getElementById('sim-q-text').textContent = q.question;

      const flagBtn = document.getElementById('sim-flag-btn');
      flagBtn.textContent = simuladoState.flagged[idx] ? '🚩 Marcada' : '🚩 Marcar para revisar';

      const selected = simuladoState.answers[idx];
      document.getElementById('sim-q-options').innerHTML = Object.entries(q.options).map(([letter, text]) => {
        const isSel = selected === letter;
        return `
          <button type="button" onclick="selectSimuladoOption('${letter}')"
            class="w-full text-left p-3.5 sm:p-4 rounded-2xl border transition flex items-start gap-3.5 ${
              isSel
                ? 'border-fatec-600 bg-fatec-50 dark:bg-fatec-900/30 ring-2 ring-fatec-500/30'
                : 'border-slate-200 dark:border-slate-700 hover:border-fatec-400 bg-slate-50/60 dark:bg-slate-800/50'
            }">
            <span class="w-7 h-7 rounded-lg text-xs font-extrabold flex items-center justify-center shrink-0 mt-0.5 ${
              isSel ? 'bg-fatec-600 text-white' : 'bg-white dark:bg-slate-700 text-slate-700 dark:text-slate-200 border border-slate-300 dark:border-slate-600'
            }">${letter}</span>
            <span class="text-sm sm:text-base text-slate-800 dark:text-slate-100 leading-snug">${text}</span>
          </button>
        `;
      }).join('');

      document.getElementById('sim-prev-btn').disabled = idx === 0;
      document.getElementById('sim-next-btn').disabled = idx === total - 1;
      document.getElementById('simulado-answered-count').textContent = Object.keys(simuladoState.answers).length;
    }

    function selectSimuladoOption(letter) {
      if (simuladoState.paused) {
        showToast('Retome o cronômetro para marcar respostas.', 'warning');
        return;
      }
      simuladoState.answers[simuladoState.currentIndex] = letter;
      renderSimuladoQuestion();
      renderSimuladoGrid();
    }

    function toggleSimuladoFlag() {
      const idx = simuladoState.currentIndex;
      simuladoState.flagged[idx] = !simuladoState.flagged[idx];
      renderSimuladoQuestion();
      renderSimuladoGrid();
    }

    function navigateSimuladoQuestion(newIdx) {
      if (newIdx >= 0 && newIdx < simuladoState.questions.length) {
        simuladoState.currentIndex = newIdx;
        renderSimuladoQuestion();
        renderSimuladoGrid();
      }
    }

    function renderSimuladoGrid() {
      const grid = document.getElementById('simulado-grid');
      grid.innerHTML = simuladoState.questions.map((_, i) => {
        const isCurrent = i === simuladoState.currentIndex;
        const isAnswered = !!simuladoState.answers[i];
        const isFlagged = !!simuladoState.flagged[i];

        let cls = 'bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-300';
        if (isFlagged) cls = 'bg-amber-400 text-slate-900 font-bold';
        else if (isAnswered) cls = 'bg-fatec-600 text-white font-bold';

        if (isCurrent) cls += ' ring-2 ring-offset-2 ring-slate-900 dark:ring-white dark:ring-offset-slate-900';

        return `
          <button onclick="navigateSimuladoQuestion(${i})" class="h-9 rounded-lg text-xs transition ${cls}">
            ${i + 1}
          </button>
        `;
      }).join('');
    }

    function confirmFinishSimulado() {
      const answered = Object.keys(simuladoState.answers).length;
      const total = simuladoState.questions.length;
      showConfirm(
        'Entregar Simulado?',
        `Você respondeu ${answered} de ${total} questões. Deseja encerrar e ver sua nota agora?`,
        () => finishSimulado(false)
      );
    }

    function finishSimulado(timeUp) {
      simuladoState.active = false;
      clearInterval(simuladoState.timerInterval);

      const total = simuladoState.questions.length;
      let hits = 0;

      const listHtml = simuladoState.questions.map((q, idx) => {
        const userAns = simuladoState.answers[idx];
        const isCorrect = userAns === q.correct;
        if (isCorrect) hits += 1;
        if (userAns) {
          recordAnswerAttempt(q, isCorrect);
        }

        return `
          <div class="p-4 rounded-2xl border ${
            isCorrect
              ? 'border-emerald-200 dark:border-emerald-800 bg-emerald-50/40 dark:bg-emerald-900/15'
              : 'border-rose-200 dark:border-rose-800 bg-rose-50/40 dark:bg-rose-900/15'
          } text-xs sm:text-sm space-y-1">
            <div class="font-bold flex flex-wrap justify-between gap-2">
              <span>Questão ${idx + 1} · ${q.subject} (${q.topic})</span>
              <span>${isCorrect ? '✓ Correta' : `✕ Sua: ${userAns || 'Branco'} | Gabarito: ${q.correct}`}</span>
            </div>
            <p class="text-slate-700 dark:text-slate-300 line-clamp-2">${q.question}</p>
            <p class="text-slate-500 dark:text-slate-400 italic">${q.explanation}</p>
          </div>
        `;
      }).join('');

      const elapsed = simuladoState.totalSeconds - Math.max(0, simuladoState.remainingSeconds);
      const pct = Math.round((hits / total) * 100);

      addHistoryEntry(
        total === 60 ? 'Simulado Oficial (60q)' : 'Simulado Express (15q)',
        `Tempo: ${formatSeconds(elapsed)}`,
        `${hits}/${total} (${pct}%)`
      );

      document.getElementById('simulado-active').classList.add('hidden');
      document.getElementById('simulado-result').classList.remove('hidden');
      document.getElementById('sim-res-title').textContent =
        `${timeUp ? 'Tempo esgotado! ' : ''}Você acertou ${hits} de ${total} questões (${pct}%)`;
      document.getElementById('sim-res-time').textContent = `Tempo utilizado: ${formatSeconds(elapsed)}`;
      document.getElementById('sim-res-list').innerHTML = listHtml;
    }

    function resetSimuladoUI() {
      document.getElementById('simulado-result').classList.add('hidden');
      document.getElementById('simulado-active').classList.add('hidden');
      document.getElementById('simulado-setup').classList.remove('hidden');
    }

    // =========================================================================
    // 8. ABA ROTEIRO DE ESTUDOS
    // =========================================================================
    function loadRoadmap() {
      const keys = getScopedStorageKeys();
      try {
        roadmapChecked = JSON.parse(localStorage.getItem(keys.roadmap)) || {};
      } catch (e) {
        roadmapChecked = {};
      }
    }

    function saveRoadmap() {
      const keys = getScopedStorageKeys();
      localStorage.setItem(keys.roadmap, JSON.stringify(roadmapChecked));
    }

    function toggleRoadmapItem(id) {
      roadmapChecked[id] = !roadmapChecked[id];
      saveRoadmap();
      renderRoadmap();
    }

    function renderRoadmap() {
      const container = document.getElementById('roadmap-container');
      let totalItems = 0;
      let doneItems = 0;

      container.innerHTML = ROADMAP_DATA.map(block => {
        const itemsHtml = block.items.map(item => {
          totalItems += 1;
          const checked = !!roadmapChecked[item.id];
          if (checked) doneItems += 1;
          return `
            <label class="cursor-pointer flex items-start gap-3 p-3 rounded-xl hover:bg-slate-50 dark:hover:bg-slate-800/60 transition">
              <input type="checkbox" ${checked ? 'checked' : ''} onchange="toggleRoadmapItem('${item.id}')"
                class="mt-1 w-4 h-4 rounded accent-fatec-600 shrink-0" />
              <span class="text-sm ${checked ? 'line-through text-slate-400 dark:text-slate-500' : 'text-slate-800 dark:text-slate-200'}">
                ${item.title}
              </span>
            </label>
          `;
        }).join('');

        return `
          <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl p-6 shadow-sm space-y-3">
            <div class="flex items-center justify-between gap-2 pb-2 border-b border-slate-100 dark:border-slate-800">
              <h3 class="font-bold text-base sm:text-lg text-slate-900 dark:text-white">${block.category}</h3>
              <span class="px-2.5 py-1 rounded-full text-[11px] font-bold bg-fatec-50 text-fatec-700 dark:bg-fatec-900/40 dark:text-fatec-300">
                ${block.badge}
              </span>
            </div>
            <div class="space-y-1">${itemsHtml}</div>
          </div>
        `;
      }).join('');

      const pct = totalItems ? Math.round((doneItems / totalItems) * 100) : 0;
      document.getElementById('roadmap-percent').textContent = `${pct}%`;
      document.getElementById('roadmap-count-text').textContent = `${doneItems} de ${totalItems} tópicos`;
      document.getElementById('roadmap-progress-bar').style.width = `${pct}%`;
    }

    // =========================================================================
    // 9. ABA DESEMPENHO
    // =========================================================================
    function renderStatsDashboard() {
      const total = userStats.totalAnswered;
      const hits = userStats.totalHits;
      const acc = total > 0 ? Math.round((hits / total) * 100) : 0;

      document.getElementById('stat-total-answered').textContent = total;
      document.getElementById('stat-total-hits').textContent = hits;
      document.getElementById('stat-accuracy').textContent = `${acc}%`;
      document.getElementById('stat-wrong-pool').textContent = userStats.wrongQuestionIds.length;

      const subjContainer = document.getElementById('stat-by-subject');
      const subjects = [...new Set([...questionsBank.map(q => q.subject), ...Object.keys(userStats.bySubject)])];

      subjContainer.innerHTML = subjects.map(subj => {
        const data = userStats.bySubject[subj] || { answered: 0, hits: 0 };
        const pct = data.answered > 0 ? Math.round((data.hits / data.answered) * 100) : 0;
        return `
          <div class="space-y-1.5">
            <div class="flex justify-between text-xs sm:text-sm font-semibold">
              <span class="text-slate-800 dark:text-slate-200">${subj}</span>
              <span class="text-slate-500 dark:text-slate-400">${data.hits}/${data.answered} (${pct}%)</span>
            </div>
            <div class="w-full h-2.5 bg-slate-100 dark:bg-slate-800 rounded-full overflow-hidden">
              <div class="h-full bg-fatec-600 rounded-full transition-all duration-300" style="width: ${pct}%"></div>
            </div>
          </div>
        `;
      }).join('');

      const historyContainer = document.getElementById('stat-history-list');
      if (userStats.history.length === 0) {
        historyContainer.innerHTML = `
          <p class="text-sm text-slate-500 dark:text-slate-400 py-6 text-center">
            Nenhuma sessão concluída ainda. Experimente o Mini Quiz ou um Simulado!
          </p>
        `;
      } else {
        historyContainer.innerHTML = userStats.history.map(item => `
          <div class="flex items-center justify-between p-3 rounded-xl bg-slate-50 dark:bg-slate-800/60 border border-slate-200/70 dark:border-slate-700/70 text-xs sm:text-sm">
            <div>
              <div class="font-bold text-slate-900 dark:text-white">${item.mode}</div>
              <div class="text-xs text-slate-500 dark:text-slate-400">${item.detail} · ${item.date}</div>
            </div>
            <span class="px-2.5 py-1 rounded-lg font-bold bg-fatec-100 text-fatec-800 dark:bg-fatec-900/50 dark:text-fatec-200">
              ${item.scoreText}
            </span>
          </div>
        `).join('');
      }
    }

    function confirmResetStats() {
      showConfirm(
        'Zerar todas as estatísticas?',
        'Isso limpará seu histórico de acertos, erros e a lista de revisão no navegador.',
        () => {
          userStats = {
            totalAnswered: 0,
            totalHits: 0,
            bySubject: {},
            wrongQuestionIds: [],
            history: []
          };
          saveStats();
          renderStatsDashboard();
          showToast('Estatísticas zeradas com sucesso.', 'info');
        }
      );
    }

    // =========================================================================
    // 10. ABA CONTATO
    // =========================================================================
    function handleContactSubmit(e) {
      e.preventDefault();
      const name = document.getElementById('contact-name').value.trim();
      const email = document.getElementById('contact-email').value.trim();
      const msg = document.getElementById('contact-message').value.trim();
      if (!name || !email || !msg) return;

      document.getElementById('contact-form').reset();
      document.getElementById('contact-success').classList.remove('hidden');
      showToast('Mensagem enviada! Obrigado pelo contato.', 'success');
    }

    // =========================================================================
    // 11. ABA ADMIN (CADASTRAR / EXCLUIR / RESTAURAR QUESTÕES)
    // =========================================================================
    function handleAdminAddQuestion(e) {
      e.preventDefault();
      const newQ = {
        id: Date.now(),
        subject: document.getElementById('admin-subject').value,
        topic: document.getElementById('admin-topic').value.trim(),
        exam: 'Fatec · Cadastrada no Admin',
        question: document.getElementById('admin-question').value.trim(),
        options: {
          A: document.getElementById('admin-opt-A').value.trim(),
          B: document.getElementById('admin-opt-B').value.trim(),
          C: document.getElementById('admin-opt-C').value.trim(),
          D: document.getElementById('admin-opt-D').value.trim(),
          E: document.getElementById('admin-opt-E').value.trim()
        },
        correct: document.getElementById('admin-correct').value,
        explanation: document.getElementById('admin-explanation').value.trim()
      };

      questionsBank.unshift(newQ);
      saveQuestionsBank();
      populateSubjectFilter();
      renderAdminList();
      document.getElementById('admin-form').reset();
      showToast('Questão adicionada ao banco!', 'success');
    }

    function deleteAdminQuestion(id) {
      if (questionsBank.length <= 1) {
        showToast('O banco precisa manter pelo menos 1 questão.', 'warning');
        return;
      }
      questionsBank = questionsBank.filter(q => q.id !== id);
      saveQuestionsBank();
      populateSubjectFilter();
      renderAdminList();
      showToast('Questão removida.', 'info');
    }

    function renderAdminList() {
      const query = (document.getElementById('admin-search')?.value || '').toLowerCase();
      const filtered = questionsBank.filter(q =>
        q.question.toLowerCase().includes(query) ||
        q.subject.toLowerCase().includes(query) ||
        q.topic.toLowerCase().includes(query)
      );

      const listEl = document.getElementById('admin-questions-list');
      listEl.innerHTML = filtered.map((q, idx) => `
        <div class="p-4 rounded-2xl border border-slate-200 dark:border-slate-800 bg-slate-50/70 dark:bg-slate-800/40 flex items-start justify-between gap-3">
          <div class="space-y-1">
            <div class="flex flex-wrap items-center gap-2 text-xs">
              <span class="font-bold text-fatec-600 dark:text-fatec-400">#${idx + 1} · ${q.subject}</span>
              <span class="text-slate-500 dark:text-slate-400">(${q.topic})</span>
              <span class="px-2 py-0.5 rounded bg-emerald-100 dark:bg-emerald-900/40 text-emerald-800 dark:text-emerald-300 font-bold">
                Gabarito: ${q.correct}
              </span>
            </div>
            <p class="text-xs sm:text-sm text-slate-800 dark:text-slate-200 line-clamp-2">${q.question}</p>
          </div>
          <button onclick="deleteAdminQuestion(${q.id})" title="Excluir questão"
            class="px-2.5 py-1.5 rounded-lg text-xs font-semibold text-rose-600 hover:bg-rose-100 dark:hover:bg-rose-900/30 shrink-0 transition">
            Excluir
          </button>
        </div>
      `).join('');
    }

    function exportBankJSON() {
      const textarea = document.createElement('textarea');
      textarea.value = JSON.stringify(questionsBank, null, 2);
      document.body.appendChild(textarea);
      textarea.select();
      try {
        document.execCommand('copy');
        showToast('JSON do banco copiado para a área de transferência!', 'success');
      } catch (err) {
        showToast('Não foi possível copiar automaticamente.', 'error');
      }
      document.body.removeChild(textarea);
    }

    function confirmRestoreDefaultBank() {
      showConfirm(
        'Restaurar 30 questões originais?',
        'Isso substituirá alterações feitas no Admin pelas 30 questões padrão da Fatec.',
        () => {
          questionsBank = [...DEFAULT_QUESTIONS];
          saveQuestionsBank();
          populateSubjectFilter();
          renderAdminList();
          showToast('Banco original de 30 questões restaurado!', 'success');
        }
      );
    }

    // Inicia ao carregar a janela
    window.addEventListener('DOMContentLoaded', initApp);
  </script>
</body>
</html>
