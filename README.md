<!DOCTYPE html>
<html lang="pt-PT">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Duelo a Dois — Arena de Corrida, Trabalho, Tarefas &amp; Vida Pessoal</title>

  <!-- Google Fonts: Space Grotesk (placar & números) + Plus Jakarta Sans (interface editorial) -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Space+Grotesk:wght@500;600;700&display=swap" rel="stylesheet" />

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>

  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['"Plus Jakarta Sans"', '-apple-system', 'BlinkMacSystemFont', 'sans-serif'],
            display: ['"Space Grotesk"', 'sans-serif'],
          },
          colors: {
            canvas: '#F8F7F4',
            cardBorder: '#E7E5E0',
            ink: '#1E1D1B',
            inkMuted: '#68645E',
            p1: '#E85D3F',
            p1Soft: '#FDF0ED',
            p1Border: '#F6C8BE',
            p2: '#16796F',
            p2Soft: '#EAF5F3',
            p2Border: '#B7DDD8',
            gold: '#D99B26',
            goldSoft: '#FCF6E8',
            goldBorder: '#F0D699',
          }
        }
      }
    };
  </script>

  <style>
    /* ==========================================================================
       ESTÉTICA EDITORIAL ENERGÉTICA — DUELO A DOIS
       ========================================================================== */
    :root {
      --bg-canvas: #F8F7F4;
      --border-soft: #E7E5E0;
      --p1-coral: #E85D3F;
      --p2-emerald: #16796F;
      --gold-leader: #D99B26;
    }

    body {
      background-color: var(--bg-canvas);
      color: #1E1D1B;
      font-family: 'Plus Jakarta Sans', sans-serif;
      overflow-x: hidden;
      background-image:
        radial-gradient(rgba(232, 93, 63, 0.045) 1px, transparent 0),
        radial-gradient(rgba(22, 121, 111, 0.045) 1px, transparent 0);
      background-size: 32px 32px;
      background-position: 0 0, 16px 16px;
    }

    .tabular-nums {
      font-variant-numeric: tabular-nums;
    }

    .arena-card {
      background-color: #FFFFFF;
      border: 1px solid #E7E5E0;
      box-shadow:
        0 1px 2px rgba(30, 29, 27, 0.03),
        0 8px 24px -8px rgba(30, 29, 27, 0.06);
    }

    /* Micro-animações para coroa do líder, salto de pontos e barras */
    @keyframes crownFloat {
      0%, 100% { transform: translateY(0) rotate(-3deg); }
      50% { transform: translateY(-4px) rotate(3deg); }
    }

    @keyframes scorePop {
      0% { transform: scale(1); }
      45% { transform: scale(1.14); }
      100% { transform: scale(1); }
    }

    @keyframes pulseRing {
      0% { box-shadow: 0 0 0 0 rgba(217, 155, 38, 0.45); }
      70% { box-shadow: 0 0 0 10px rgba(217, 155, 38, 0); }
      100% { box-shadow: 0 0 0 0 rgba(217, 155, 38, 0); }
    }

    .crown-float {
      display: inline-block;
      animation: crownFloat 2.4s ease-in-out infinite;
    }

    .score-pop {
      animation: scorePop 0.42s cubic-bezier(0.22, 1, 0.36, 1);
    }

    .leader-glow {
      box-shadow:
        0 0 0 2px rgba(217, 155, 38, 0.55),
        0 14px 30px -8px rgba(217, 155, 38, 0.22);
    }

    .timer-pulse {
      animation: pulseRing 2s infinite;
    }

    /* Barra de rolagem limpa */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #F8F7F4;
    }
    ::-webkit-scrollbar-thumb {
      background: #D6D2C8;
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #B5AFA2;
    }
  </style>
</head>
<body class="min-h-screen flex flex-col selection:bg-[#D99B26]/20 selection:text-[#1E1D1B]">

  <!-- Canvas fixo para celebração de confetes -->
  <canvas id="confetti-canvas" class="fixed inset-0 pointer-events-none z-50"></canvas>

  <!-- ========================================================================
       1. CABEÇALHO EDITORIAL & CONTROLOS DE SALA
       ======================================================================== -->
  <header class="sticky top-0 z-30 bg-[#F8F7F4]/92 backdrop-blur-md border-b border-[#E7E5E0]">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3.5">
      <div class="flex flex-col lg:flex-row lg:items-center lg:justify-between gap-3.5">
        <!-- Logótipo & Identidade -->
        <div class="flex items-center justify-between gap-3">
          <div class="flex items-center gap-3">
            <div class="w-11 h-11 rounded-2xl bg-white border border-[#E7E5E0] flex items-center justify-center shadow-xs shrink-0">
              <!-- Ícone Vetorial Duelo a Dois -->
              <svg width="26" height="26" viewBox="0 0 28 28" fill="none" xmlns="http://www.w3.org/2000/svg">
                <circle cx="9.5" cy="14" r="6.5" fill="#FDF0ED" stroke="#E85D3F" stroke-width="2" />
                <circle cx="18.5" cy="14" r="6.5" fill="#EAF5F3" stroke="#16796F" stroke-width="2" />
                <path d="M14 5.5L15.4 8.5L18.5 9L16.2 11.2L16.8 14.3L14 12.8L11.2 14.3L11.8 11.2L9.5 9L12.6 8.5L14 5.5Z" fill="#D99B26" />
              </svg>
            </div>
            <div>
              <div class="flex items-center gap-2">
                <h1 class="font-display text-xl sm:text-2xl font-bold tracking-tight text-[#1E1D1B] leading-none">
                  Duelo a Dois
                </h1>
                <span class="px-2 py-0.5 rounded-full text-[10px] font-bold uppercase tracking-wider bg-[#FCF6E8] text-[#B37B12] border border-[#F0D699]">
                  Arena Semanal
                </span>
              </div>
              <p class="text-xs text-[#68645E] mt-1">
                Corrida, Trabalho, Tarefas &amp; Vida Pessoal — quem faz mais esta semana?
              </p>
            </div>
          </div>

          <!-- Indicador compacto mobile da sala -->
          <button
            id="btn-sync-mobile"
            type="button"
            class="lg:hidden px-3 py-2 rounded-xl bg-white border border-[#E7E5E0] text-xs font-semibold text-[#1E1D1B] flex items-center gap-1.5 shadow-2xs cursor-pointer"
          >
            <span id="sync-dot-mobile" class="w-2 h-2 rounded-full bg-emerald-500"></span>
            <span id="room-code-mobile" class="font-display">DUELO</span>
          </button>
        </div>

        <!-- Barra de Ações Rápidas -->
        <div class="flex flex-wrap items-center gap-2">
          <!-- Botão Sincronizar a Dois -->
          <button
            id="btn-open-sync"
            type="button"
            class="hidden lg:flex items-center gap-2 px-3.5 py-2.5 rounded-xl bg-white hover:bg-[#FCF6E8]/60 border border-[#E7E5E0] text-xs font-semibold text-[#1E1D1B] transition-all shadow-2xs cursor-pointer"
            title="Partilhar código da sala para sincronizar nos dois telemóveis"
          >
            <span id="sync-dot-desktop" class="w-2 h-2 rounded-full bg-emerald-500"></span>
            <span class="text-[#68645E]">Sala:</span>
            <span id="room-code-desktop" class="font-display font-bold tracking-wider text-[#1E1D1B]">DUELO-26</span>
            <span class="px-2 py-0.5 rounded-md bg-[#F8F7F4] border border-[#E7E5E0] text-[11px] text-[#1E1D1B]">
              Sincronizar a Dois
            </span>
          </button>

          <!-- Botão Nova Tarefa / Registo -->
          <button
            id="btn-open-new-modal"
            type="button"
            class="px-4 py-2.5 rounded-xl bg-[#1E1D1B] hover:bg-[#32302C] text-white text-xs sm:text-sm font-semibold flex items-center gap-2 transition-all shadow-xs cursor-pointer"
          >
            <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round">
              <path d="M12 5v14M5 12h14" />
            </svg>
            <span>Nova Tarefa / Hábito</span>
          </button>

          <!-- Botão Encerrar Semana & Coroar Vencedor -->
          <button
            id="btn-crown-week"
            type="button"
            class="px-3.5 py-2.5 rounded-xl bg-[#FCF6E8] hover:bg-[#F9ECCB] border border-[#F0D699] text-[#9A670A] text-xs sm:text-sm font-semibold flex items-center gap-1.5 transition-all cursor-pointer"
            title="Atribuir troféu ao líder atual e iniciar nova ronda semanal"
          >
            <span>🏆 Encerrar Semana</span>
          </button>

          <!-- Botão Reiniciar Dados de Demonstração -->
          <button
            id="btn-reset-demo"
            type="button"
            class="px-3 py-2.5 rounded-xl bg-transparent hover:bg-[#EFECE6] text-[#68645E] hover:text-[#1E1D1B] text-xs font-medium transition-colors cursor-pointer"
            title="Reiniciar semana ou repor dados iniciais"
          >
            Reiniciar
          </button>
        </div>
      </div>
    </div>
  </header>

  <!-- ========================================================================
       CONTEÚDO PRINCIPAL
       ======================================================================== -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">

    <!-- ======================================================================
         2. APOSTA / PRÉMIO DA SEMANA (EDITÁVEL)
         ====================================================================== -->
    <section class="arena-card rounded-2xl p-4 sm:px-6 sm:py-4 bg-gradient-to-r from-[#FCF6E8]/85 via-white to-[#FCF6E8]/60 border-[#F0D699]">
      <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
        <div class="flex items-start sm:items-center gap-3.5 flex-1">
          <div class="w-10 h-10 rounded-xl bg-[#FCF6E8] border border-[#F0D699] flex items-center justify-center text-xl shrink-0">
            🎁
          </div>
          <div class="flex-1 min-w-0">
            <div class="flex items-center gap-2">
              <span class="text-[11px] font-bold uppercase tracking-[0.16em] text-[#B37B12]">
                Aposta &amp; Prémio da Semana
              </span>
            </div>

            <!-- Texto da Aposta (Modo Leitura) -->
            <p id="bet-display-text" class="font-display font-semibold text-base sm:text-lg text-[#1E1D1B] mt-0.5 leading-snug">
              Quem ganhar escolhe o jantar de sábado e não arruma a cozinha!
            </p>

            <!-- Input da Aposta (Modo Edição) -->
            <div id="bet-edit-row" class="hidden mt-1.5 flex flex-col sm:flex-row items-stretch sm:items-center gap-2">
              <input
                id="bet-input"
                type="text"
                maxlength="110"
                class="flex-1 px-3 py-1.5 rounded-xl bg-white border border-[#D99B26] text-sm font-medium text-[#1E1D1B] focus:outline-none"
                placeholder="Escreve o prémio ou aposta desta semana..."
              />
              <div class="flex items-center gap-1.5">
                <button
                  id="btn-save-bet"
                  type="button"
                  class="px-3.5 py-1.5 rounded-xl bg-[#D99B26] text-white text-xs font-semibold hover:bg-[#C2881D] cursor-pointer"
                >
                  Guardar
                </button>
                <button
                  id="btn-cancel-bet"
                  type="button"
                  class="px-3 py-1.5 rounded-xl bg-[#F8F7F4] border border-[#E7E5E0] text-xs font-medium text-[#68645E] cursor-pointer"
                >
                  Cancelar
                </button>
              </div>
            </div>
          </div>
        </div>

        <button
          id="btn-edit-bet"
          type="button"
          class="self-start sm:self-center px-3 py-2 rounded-xl bg-white hover:bg-[#FCF6E8] border border-[#E7E5E0] text-xs font-semibold text-[#68645E] hover:text-[#1E1D1B] flex items-center gap-1.5 transition-colors cursor-pointer shrink-0"
        >
          <span>✏️ Editar Aposta</span>
        </button>
      </div>
    </section>

    <!-- ======================================================================
         3. PLACAR CABEÇA-A-CABEÇA & CABO DE GUERRA
         ====================================================================== -->
    <section class="grid grid-cols-1 lg:grid-cols-12 gap-5 items-stretch">
      <!-- CARTÃO JOGADOR 1 (CORAL #E85D3F) -->
      <div
        id="card-player-1"
        class="lg:col-span-4 arena-card rounded-3xl p-5 sm:p-6 relative transition-all duration-300 border-t-4"
        style="border-top-color: #E85D3F;"
      >
        <div class="flex items-start justify-between gap-3">
          <div class="flex items-center gap-3.5">
            <!-- Avatar Clicável para trocar emoji -->
            <button
              type="button"
              data-action="cycle-avatar"
              data-player="p1"
              title="Clica para mudar o avatar"
              class="w-14 h-14 rounded-2xl bg-[#FDF0ED] border border-[#F6C8BE] flex items-center justify-center text-3xl hover:scale-105 transition-transform cursor-pointer shrink-0"
            >
              <span id="p1-avatar">🏃‍♀️</span>
            </button>

            <div>
              <div class="flex items-center gap-1.5">
                <span class="text-[11px] font-bold uppercase tracking-wider text-[#E85D3F]">Jogador 1</span>
                <span id="p1-leader-badge" class="hidden px-2 py-0.5 rounded-full bg-[#FCF6E8] border border-[#F0D699] text-[#B37B12] text-[10px] font-bold items-center gap-1">
                  <span class="crown-float">👑</span> Líder
                </span>
              </div>

              <div class="flex items-center gap-1.5 mt-0.5">
                <h2 id="p1-name-display" class="font-display text-xl sm:text-2xl font-bold text-[#1E1D1B]">
                  Sofia
                </h2>
                <button
                  type="button"
                  data-action="edit-player"
                  data-player="p1"
                  class="p-1 rounded-lg text-[#68645E] hover:text-[#E85D3F] hover:bg-[#FDF0ED] transition-colors cursor-pointer"
                  title="Editar nome e avatar"
                >
                  <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M12 20h9" />
                    <path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z" />
                  </svg>
                </button>
              </div>
            </div>
          </div>

          <!-- Pontuação Total Jogador 1 -->
          <div class="text-right">
            <div id="p1-score" class="font-display text-3xl sm:text-4xl font-bold tabular-nums text-[#E85D3F] leading-none">
              245
            </div>
            <span class="text-[11px] font-semibold uppercase tracking-wider text-[#68645E]">pontos</span>
          </div>
        </div>

        <!-- Métricas de Sequência & Vitórias -->
        <div class="mt-5 pt-4 border-t border-[#E7E5E0] grid grid-cols-2 gap-2.5">
          <div class="px-3 py-2 rounded-xl bg-[#FDF0ED]/65 border border-[#F6C8BE]/60 flex items-center justify-between">
            <span class="text-xs text-[#68645E]">Sequência</span>
            <span id="p1-streak" class="font-display text-xs font-bold text-[#E85D3F]">🔥 5 dias</span>
          </div>
          <div class="px-3 py-2 rounded-xl bg-[#F8F7F4] border border-[#E7E5E0] flex items-center justify-between">
            <span class="text-xs text-[#68645E]">Vitórias</span>
            <span id="p1-wins" class="font-display text-xs font-bold text-[#1E1D1B]">🏆 3 sem.</span>
          </div>
        </div>
      </div>

      <!-- CENTRO: CABO DE GUERRA & DIFERENÇA DE PONTOS -->
      <div class="lg:col-span-4 arena-card rounded-3xl p-5 sm:p-6 flex flex-col justify-between bg-gradient-to-b from-white to-[#F8F7F4]/70">
        <div class="text-center">
          <span class="text-[11px] font-bold uppercase tracking-[0.18em] text-[#68645E]">
            Cabo de Guerra Semanal
          </span>
          <div id="lead-status-headline" class="font-display text-lg sm:text-xl font-bold text-[#1E1D1B] mt-1">
            Sofia lidera por +25 pts
          </div>
          <p id="lead-status-sub" class="text-xs text-[#68645E] mt-0.5">
            Cada tarefa, corrida ou sessão de foco muda o equilíbrio!
          </p>
        </div>

        <!-- Barra Visual Cabo de Guerra -->
        <div class="my-5">
          <div class="flex items-center justify-between text-xs font-display font-bold mb-2">
            <span id="tug-p1-pct" class="text-[#E85D3F]">53%</span>
            <span class="text-[11px] font-sans font-medium text-[#68645E]">Equilíbrio da Arena</span>
            <span id="tug-p2-pct" class="text-[#16796F]">47%</span>
          </div>

          <div class="relative h-6 w-full rounded-full bg-[#EFECE6] p-1 overflow-hidden border border-[#E7E5E0] flex items-center">
            <!-- Barra Esquerda (Jogador 1) -->
            <div
              id="tug-bar-p1"
              class="h-full rounded-l-full transition-all duration-500 ease-out"
              style="width: 53%; background: linear-gradient(90deg, #E85D3F 0%, #F27A60 100%);"
            ></div>
            <!-- Barra Direita (Jogador 2) -->
            <div
              id="tug-bar-p2"
              class="h-full rounded-r-full transition-all duration-500 ease-out"
              style="width: 47%; background: linear-gradient(90deg, #1E988C 0%, #16796F 100%);"
            ></div>

            <!-- Marcador Central / Ponteiro Dinâmico -->
            <div
              id="tug-marker"
              class="absolute top-1/2 -translate-y-1/2 -translate-x-1/2 w-7 h-7 rounded-full bg-white border-2 border-[#D99B26] shadow-md flex items-center justify-center text-xs transition-all duration-500 ease-out"
              style="left: 53%;"
            >
              ⚡
            </div>
          </div>
        </div>

        <!-- Atalho para Secção "Quem Faz Mais" -->
        <div class="flex items-center justify-between gap-2 pt-3 border-t border-[#E7E5E0] text-xs">
          <span class="text-[#68645E]">Total combinado: <strong id="combined-points" class="font-display text-[#1E1D1B]">465 pts</strong></span>
          <a href="#sec-quem-faz-mais" class="font-semibold text-[#B37B12] hover:underline">
            Ver estatísticas ↓
          </a>
        </div>
      </div>

      <!-- CARTÃO JOGADOR 2 (VERDE-ESMERALDA #16796F) -->
      <div
        id="card-player-2"
        class="lg:col-span-4 arena-card rounded-3xl p-5 sm:p-6 relative transition-all duration-300 border-t-4"
        style="border-top-color: #16796F;"
      >
        <div class="flex items-start justify-between gap-3">
          <div class="flex items-center gap-3.5">
            <!-- Avatar Clicável -->
            <button
              type="button"
              data-action="cycle-avatar"
              data-player="p2"
              title="Clica para mudar o avatar"
              class="w-14 h-14 rounded-2xl bg-[#EAF5F3] border border-[#B7DDD8] flex items-center justify-center text-3xl hover:scale-105 transition-transform cursor-pointer shrink-0"
            >
              <span id="p2-avatar">⚡</span>
            </button>

            <div>
              <div class="flex items-center gap-1.5">
                <span class="text-[11px] font-bold uppercase tracking-wider text-[#16796F]">Jogador 2</span>
                <span id="p2-leader-badge" class="hidden px-2 py-0.5 rounded-full bg-[#FCF6E8] border border-[#F0D699] text-[#B37B12] text-[10px] font-bold items-center gap-1">
                  <span class="crown-float">👑</span> Líder
                </span>
              </div>

              <div class="flex items-center gap-1.5 mt-0.5">
                <h2 id="p2-name-display" class="font-display text-xl sm:text-2xl font-bold text-[#1E1D1B]">
                  Tiago
                </h2>
                <button
                  type="button"
                  data-action="edit-player"
                  data-player="p2"
                  class="p-1 rounded-lg text-[#68645E] hover:text-[#16796F] hover:bg-[#EAF5F3] transition-colors cursor-pointer"
                  title="Editar nome e avatar"
                >
                  <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M12 20h9" />
                    <path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z" />
                  </svg>
                </button>
              </div>
            </div>
          </div>

          <!-- Pontuação Total Jogador 2 -->
          <div class="text-right">
            <div id="p2-score" class="font-display text-3xl sm:text-4xl font-bold tabular-nums text-[#16796F] leading-none">
              220
            </div>
            <span class="text-[11px] font-semibold uppercase tracking-wider text-[#68645E]">pontos</span>
          </div>
        </div>

        <!-- Métricas de Sequência & Vitórias -->
        <div class="mt-5 pt-4 border-t border-[#E7E5E0] grid grid-cols-2 gap-2.5">
          <div class="px-3 py-2 rounded-xl bg-[#EAF5F3]/65 border border-[#B7DDD8]/60 flex items-center justify-between">
            <span class="text-xs text-[#68645E]">Sequência</span>
            <span id="p2-streak" class="font-display text-xs font-bold text-[#16796F]">🔥 4 dias</span>
          </div>
          <div class="px-3 py-2 rounded-xl bg-[#F8F7F4] border border-[#E7E5E0] flex items-center justify-between">
            <span class="text-xs text-[#68645E]">Vitórias</span>
            <span id="p2-wins" class="font-display text-xs font-bold text-[#1E1D1B]">🏆 2 sem.</span>
          </div>
        </div>
      </div>
    </section>

    <!-- ======================================================================
         4. FILTROS DAS 4 CATEGORIAS + FERRAMENTAS DEDICADAS
         ====================================================================== -->
    <section class="space-y-5">
      <!-- Barra de Navegação por Categoria -->
      <div class="arena-card rounded-2xl p-2 flex flex-wrap items-center justify-between gap-2">
        <div class="flex flex-wrap items-center gap-1.5" role="tablist">
          <button
            type="button"
            data-cat-tab="all"
            class="cat-tab-btn px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold bg-[#1E1D1B] text-white transition-all cursor-pointer"
          >
            Todas as Categorias
          </button>
          <button
            type="button"
            data-cat-tab="running"
            class="cat-tab-btn px-3.5 py-2 rounded-xl text-xs sm:text-sm font-semibold text-[#68645E] hover:text-[#1E1D1B] hover:bg-[#F8F7F4] transition-all cursor-pointer"
          >
            🏃 Corrida &amp; Treino
          </button>
          <button
            type="button"
            data-cat-tab="work"
            class="cat-tab-btn px-3.5 py-2 rounded-xl text-xs sm:text-sm font-semibold text-[#68645E] hover:text-[#1E1D1B] hover:bg-[#F8F7F4] transition-all cursor-pointer"
          >
            💼 Trabalho &amp; Foco
          </button>
          <button
            type="button"
            data-cat-tab="chores"
            class="cat-tab-btn px-3.5 py-2 rounded-xl text-xs sm:text-sm font-semibold text-[#68645E] hover:text-[#1E1D1B] hover:bg-[#F8F7F4] transition-all cursor-pointer"
          >
            ✅ Tarefas &amp; Casa
          </button>
          <button
            type="button"
            data-cat-tab="personal"
            class="cat-tab-btn px-3.5 py-2 rounded-xl text-xs sm:text-sm font-semibold text-[#68645E] hover:text-[#1E1D1B] hover:bg-[#F8F7F4] transition-all cursor-pointer"
          >
            ✨ Pessoal &amp; Hábitos
          </button>
        </div>

        <button
          id="btn-quick-add-inline"
          type="button"
          class="px-3.5 py-2 rounded-xl bg-[#F8F7F4] hover:bg-[#EFECE6] border border-[#E7E5E0] text-xs font-semibold text-[#1E1D1B] flex items-center gap-1.5 transition-colors cursor-pointer"
        >
          <span>+ Adicionar Item</span>
        </button>
      </div>

      <!-- GRELHA DAS 4 CATEGORIAS -->
      <div id="categories-grid" class="grid grid-cols-1 lg:grid-cols-2 gap-6">

        <!-- ==================================================================
             CATEGORIA 1: 🏃 CORRIDA & TREINO
             ================================================================== -->
        <article data-category-panel="running" class="arena-card rounded-3xl p-5 sm:p-6 flex flex-col justify-between">
          <div>
            <!-- Cabeçalho da Categoria -->
            <div class="flex items-center justify-between gap-2 pb-4 border-b border-[#E7E5E0]">
              <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl bg-[#FDF0ED] border border-[#F6C8BE] flex items-center justify-center text-xl">
                  🏃
                </div>
                <div>
                  <h3 class="font-display text-lg font-bold text-[#1E1D1B]">Corrida &amp; Treino</h3>
                  <p class="text-xs text-[#68645E]">15 pts/km + 0,5 pts/min de treino</p>
                </div>
              </div>

              <!-- Resumo Rápido Km -->
              <div class="text-right text-xs">
                <div class="font-display font-bold">
                  <span id="run-km-p1" class="text-[#E85D3F]">11.2 km</span>
                  <span class="text-[#68645E] mx-1">vs</span>
                  <span id="run-km-p2" class="text-[#16796F]">9.5 km</span>
                </div>
                <span class="text-[11px] text-[#68645E]">acumulado semanal</span>
              </div>
            </div>

            <!-- Formulário Dedicado de Registo de Corrida / Treino -->
            <form id="form-log-run" class="mt-4 space-y-3.5">
              <!-- Seletor de Competidor -->
              <div>
                <label class="block text-[11px] font-bold uppercase tracking-wider text-[#68645E] mb-1.5">
                  Quem treinou?
                </label>
                <div class="grid grid-cols-2 gap-2">
                  <button
                    type="button"
                    data-run-player="p1"
                    class="run-player-btn py-2 px-3 rounded-xl border-2 border-[#E85D3F] bg-[#FDF0ED] text-[#E85D3F] text-xs font-bold flex items-center justify-center gap-1.5 transition-all cursor-pointer"
                  >
                    <span id="run-btn-p1-label">🏃‍♀️ Sofia</span>
                  </button>
                  <button
                    type="button"
                    data-run-player="p2"
                    class="run-player-btn py-2 px-3 rounded-xl border border-[#E7E5E0] bg-white text-[#68645E] text-xs font-semibold flex items-center justify-center gap-1.5 transition-all cursor-pointer"
                  >
                    <span id="run-btn-p2-label">⚡ Tiago</span>
                  </button>
                </div>
              </div>

              <!-- Tipo de Atividade -->
              <div>
                <label for="run-type" class="block text-[11px] font-bold uppercase tracking-wider text-[#68645E] mb-1">
                  Modalidade
                </label>
                <select
                  id="run-type"
                  class="w-full px-3.5 py-2.5 rounded-xl bg-[#F8F7F4] border border-[#E7E5E0] text-sm font-medium text-[#1E1D1B] focus:outline-none focus:border-[#1E1D1B]"
                >
                  <option value="Corrida de Rua">🏃 Corrida de Rua</option>
                  <option value="Passadeira">👟 Passadeira</option>
                  <option value="Caminhada Rápida">🚶 Caminhada Rápida</option>
                  <option value="Ginásio">🏋️ Ginásio / Força</option>
                </select>
              </div>

              <!-- Distância (km) e Tempo (minutos) -->
              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="run-km" class="block text-[11px] font-bold uppercase tracking-wider text-[#68645E] mb-1">
                    Distância (km)
                  </label>
                  <input
                    id="run-km"
                    type="number"
                    step="0.1"
                    min="0"
                    max="100"
                    value="5.0"
                    class="w-full px-3.5 py-2 rounded-xl bg-[#F8F7F4] border border-[#E7E5E0] font-display text-base font-bold text-[#1E1D1B] focus:outline-none focus:border-[#1E1D1B]"
                  />
                </div>
                <div>
                  <label for="run-mins" class="block text-[11px] font-bold uppercase tracking-wider text-[#68645E] mb-1">
                    Tempo (min)
                  </label>
                  <input
                    id="run-mins"
                    type="number"
                    step="1"
                    min="1"
                    max="600"
                    value="28"
                    class="w-full px-3.5 py-2 rounded-xl bg-[#F8F7F4] border border-[#E7E5E0] font-display text-base font-bold text-[#1E1D1B] focus:outline-none focus:border-[#1E1D1B]"
                  />
                </div>
              </div>

              <!-- Botão Submeter com Prévia de Pontos -->
              <button
                id="btn-submit-run"
                type="submit"
                class="w-full py-3 px-4 rounded-xl bg-[#E85D3F] hover:opacity-95 text-white font-display text-sm font-bold flex items-center justify-center gap-2 transition-all shadow-xs cursor-pointer"
              >
                <span>Registar Treino</span>
                <span id="run-points-preview" class="px-2 py-0.5 rounded-lg bg-white/20 text-xs">
                  +89 pts
                </span>
              </button>
            </form>
          </div>

          <!-- Lista de Desafios Rápidos de Treino -->
          <div class="mt-5 pt-4 border-t border-[#E7E5E0]">
            <span class="block text-[11px] font-bold uppercase tracking-wider text-[#68645E] mb-2.5">
              Desafios Rápidos de Movimento
            </span>
            <div id="list-running" class="space-y-2">
              <!-- Itens de corrida/treino injetados via JS -->
            </div>
          </div>
        </article>

        <!-- ==================================================================
             CATEGORIA 2: 💼 TRABALHO & FOCO (CRONÓMETRO INTEGRADO + METAS)
             ================================================================== -->
        <article data-category-panel="work" class="arena-card rounded-3xl p-5 sm:p-6 flex flex-col justify-between">
          <div>
            <!-- Cabeçalho -->
            <div class="flex items-center justify-between gap-2 pb-4 border-b border-[#E7E5E0]">
              <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl bg-[#FCF6E8] border border-[#F0D699] flex items-center justify-center text-xl">
                  💼
                </div>
                <div>
                  <h3 class="font-display text-lg font-bold text-[#1E1D1B]">Trabalho &amp; Foco</h3>
                  <p class="text-xs text-[#68645E]">Sessões de concentração &amp; metas profissionais</p>
                </div>
              </div>

              <div class="text-right text-xs">
                <div class="font-display font-bold">
                  <span id="work-sessions-p1" class="text-[#E85D3F]">3 sessões</span>
                  <span class="text-[#68645E] mx-1">vs</span>
                  <span id="work-sessions-p2" class="text-[#16796F]">4 sessões</span>
                </div>
                <span class="text-[11px] text-[#68645E]">foco concluído</span>
              </div>
            </div>

            <!-- Cronómetro de Foco Integrado -->
            <div class="mt-4 p-4 rounded-2xl bg-[#F8F7F4] border border-[#E7E5E0] space-y-3">
              <div class="flex flex-wrap items-center justify-between gap-2">
                <span class="text-[11px] font-bold uppercase tracking-wider text-[#68645E]">
                  Cronómetro de Foco
                </span>

                <!-- Escolha de quem está a focar -->
                <div class="inline-flex rounded-xl bg-white p-0.5 border border-[#E7E5E0]">
                  <button
                    type="button"
                    data-timer-player="p1"
                    class="timer-player-btn px-2.5 py-1 rounded-lg text-xs font-bold bg-[#FDF0ED] text-[#E85D3F] transition-all cursor-pointer"
                  >
                    <span id="timer-p1-name">Sofia</span>
                  </button>
                  <button
                    type="button"
                    data-timer-player="p2"
                    class="timer-player-btn px-2.5 py-1 rounded-lg text-xs font-semibold text-[#68645E] transition-all cursor-pointer"
                  >
                    <span id="timer-p2-name">Tiago</span>
                  </button>
                </div>
              </div>

              <!-- Predefinições de Tempo -->
              <div class="grid grid-cols-3 gap-1.5">
                <button
                  type="button"
                  data-timer-preset="25"
                  class="timer-preset-btn py-1.5 px-2 rounded-xl bg-[#1E1D1B] text-white text-xs font-semibold transition-all cursor-pointer"
                >
                  Pomodoro 25m
                </button>
                <button
                  type="button"
                  data-timer-preset="50"
                  class="timer-preset-btn py-1.5 px-2 rounded-xl bg-white border border-[#E7E5E0] text-[#68645E] hover:text-[#1E1D1B] text-xs font-semibold transition-all cursor-pointer"
                >
                  Deep Work 50m
                </button>
                <button
                  type="button"
                  data-timer-preset="0"
                  class="timer-preset-btn py-1.5 px-2 rounded-xl bg-white border border-[#E7E5E0] text-[#68645E] hover:text-[#1E1D1B] text-xs font-semibold transition-all cursor-pointer"
                >
                  Cronómetro Livre
                </button>
              </div>

              <!-- Mostrador Digital -->
              <div class="flex items-center justify-between gap-3 py-1">
                <div>
                  <div id="timer-display" class="font-display text-3xl sm:text-4xl font-bold tabular-nums text-[#1E1D1B] tracking-tight">
                    25:00
                  </div>
                  <span id="timer-reward-label" class="text-[11px] font-semibold text-[#B37B12]">
                    Recompensa: +30 pts ao concluir
                  </span>
                </div>

                <div class="flex flex-wrap items-center gap-1.5">
                  <button
                    id="btn-timer-toggle"
                    type="button"
                    class="px-3.5 py-2 rounded-xl bg-[#1E1D1B] hover:bg-[#33312D] text-white text-xs font-bold transition-all cursor-pointer"
                  >
                    ▶ Iniciar
                  </button>
                  <button
                    id="btn-timer-complete"
                    type="button"
                    class="px-3 py-2 rounded-xl bg-[#D99B26] hover:bg-[#C4891C] text-white text-xs font-bold transition-all cursor-pointer"
                    title="Concluir sessão e atribuir pontos agora"
                  >
                    ✓ Concluir
                  </button>
                  <button
                    id="btn-timer-reset"
                    type="button"
                    class="px-2.5 py-2 rounded-xl bg-white border border-[#E7E5E0] text-[#68645E] hover:text-[#1E1D1B] text-xs font-semibold cursor-pointer"
                    title="Repor relógio"
                  >
                    ↺
                  </button>
                </div>
              </div>
            </div>
          </div>

          <!-- Lista de Metas Profissionais -->
          <div class="mt-5 pt-4 border-t border-[#E7E5E0]">
            <span class="block text-[11px] font-bold uppercase tracking-wider text-[#68645E] mb-2.5">
              Metas de Trabalho &amp; Produtividade
            </span>
            <div id="list-work" class="space-y-2">
              <!-- Itens de trabalho injetados via JS -->
            </div>
          </div>
        </article>

        <!-- ==================================================================
             CATEGORIA 3: ✅ TAREFAS & CASA
             ================================================================== -->
        <article data-category-panel="chores" class="arena-card rounded-3xl p-5 sm:p-6 flex flex-col justify-between">
          <div>
            <div class="flex items-center justify-between gap-2 pb-4 border-b border-[#E7E5E0]">
              <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl bg-[#EAF5F3] border border-[#B7DDD8] flex items-center justify-center text-xl">
                  ✅
                </div>
                <div>
                  <h3 class="font-display text-lg font-bold text-[#1E1D1B]">Tarefas &amp; Casa</h3>
                  <p class="text-xs text-[#68645E]">Clica no teu botão assim que tratares da tarefa</p>
                </div>
              </div>

              <div class="text-right text-xs">
                <div class="font-display font-bold">
                  <span id="chores-count-p1" class="text-[#E85D3F]">4 feitas</span>
                  <span class="text-[#68645E] mx-1">vs</span>
                  <span id="chores-count-p2" class="text-[#16796F]">5 feitas</span>
                </div>
                <span class="text-[11px] text-[#68645E]">esta semana</span>
              </div>
            </div>

            <div id="list-chores" class="mt-4 space-y-2.5">
              <!-- Tarefas de casa injetadas via JS -->
            </div>
          </div>

          <div class="mt-4 pt-3 border-t border-[#E7E5E0] flex items-center justify-between text-xs text-[#68645E]">
            <span>Podes concluir tarefas recorrentes várias vezes na semana.</span>
            <button
              type="button"
              data-open-cat="chores"
              class="font-semibold text-[#16796F] hover:underline cursor-pointer"
            >
              + Nova tarefa
            </button>
          </div>
        </article>

        <!-- ==================================================================
             CATEGORIA 4: ✨ PESSOAL & HÁBITOS
             ================================================================== -->
        <article data-category-panel="personal" class="arena-card rounded-3xl p-5 sm:p-6 flex flex-col justify-between">
          <div>
            <div class="flex items-center justify-between gap-2 pb-4 border-b border-[#E7E5E0]">
              <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl bg-[#F4F0FA] border border-[#DDD2F0] flex items-center justify-center text-xl">
                  ✨
                </div>
                <div>
                  <h3 class="font-display text-lg font-bold text-[#1E1D1B]">Pessoal &amp; Hábitos</h3>
                  <p class="text-xs text-[#68645E]">Registo diário "+1" para bem-estar &amp; evolução</p>
                </div>
              </div>

              <div class="text-right text-xs">
                <div class="font-display font-bold">
                  <span id="personal-count-p1" class="text-[#E85D3F]">6 hábitos</span>
                  <span class="text-[#68645E] mx-1">vs</span>
                  <span id="personal-count-p2" class="text-[#16796F]">5 hábitos</span>
                </div>
                <span class="text-[11px] text-[#68645E]">esta semana</span>
              </div>
            </div>

            <div id="list-personal" class="mt-4 space-y-2.5">
              <!-- Hábitos pessoais injetados via JS -->
            </div>
          </div>

          <div class="mt-4 pt-3 border-t border-[#E7E5E0] flex items-center justify-between text-xs text-[#68645E]">
            <span>Pequenos hábitos diários somam pontos decisivos!</span>
            <button
              type="button"
              data-open-cat="personal"
              class="font-semibold text-[#1E1D1B] hover:underline cursor-pointer"
            >
              + Novo hábito
            </button>
          </div>
        </article>

      </div>
    </section>

    <!-- ======================================================================
         5. PAINEL "QUEM FAZ MAIS" (ESTATÍSTICAS) & MURAL DE ATIVIDADE RECENTE
         ====================================================================== -->
    <section id="sec-quem-faz-mais" class="grid grid-cols-1 lg:grid-cols-12 gap-6 pt-2">
      <!-- Coluna Esquerda (7 cols): Estatísticas Comparativas por Categoria -->
      <div class="lg:col-span-7 arena-card rounded-3xl p-5 sm:p-6 space-y-5">
        <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2 pb-4 border-b border-[#E7E5E0]">
          <div>
            <span class="text-[11px] font-bold uppercase tracking-[0.18em] text-[#B37B12]">
              Análise Cabeça-a-Cabeça
            </span>
            <h3 class="font-display text-xl font-bold text-[#1E1D1B] mt-0.5">
              Quem Faz Mais por Categoria
            </h3>
          </div>
          <div class="flex items-center gap-3 text-xs font-semibold">
            <span class="flex items-center gap-1.5 text-[#E85D3F]">
              <span class="w-2.5 h-2.5 rounded-full bg-[#E85D3F]"></span>
              <span id="stats-legend-p1">Sofia</span>
            </span>
            <span class="flex items-center gap-1.5 text-[#16796F]">
              <span class="w-2.5 h-2.5 rounded-full bg-[#16796F]"></span>
              <span id="stats-legend-p2">Tiago</span>
            </span>
          </div>
        </div>

        <!-- 4 Barras Comparativas Detalhadas -->
        <div id="category-stats-container" class="space-y-4">
          <!-- Gerado dinamicamente via JS -->
        </div>

        <!-- Histórico de Campeões Semanais -->
        <div class="pt-4 border-t border-[#E7E5E0]">
          <div class="flex items-center justify-between mb-3">
            <h4 class="font-display text-sm font-bold text-[#1E1D1B] flex items-center gap-1.5">
              <span>🏆 Mural de Campeões Semanais</span>
            </h4>
            <span class="text-[11px] text-[#68645E]">Rondas anteriores</span>
          </div>
          <div id="champions-history-list" class="space-y-2">
            <!-- Histórico de vencedores injetado via JS -->
          </div>
        </div>
      </div>

      <!-- Coluna Direita (5 cols): Mural de Atividade Recente (Feed Cronológico) -->
      <div class="lg:col-span-5 arena-card rounded-3xl p-5 sm:p-6 flex flex-col">
        <div class="flex items-center justify-between pb-4 border-b border-[#E7E5E0]">
          <div>
            <span class="text-[11px] font-bold uppercase tracking-[0.18em] text-[#68645E]">
              Tempo Real
            </span>
            <h3 class="font-display text-xl font-bold text-[#1E1D1B] mt-0.5">
              Mural de Atividade
            </h3>
          </div>
          <span id="feed-count-badge" class="px-2.5 py-1 rounded-full bg-[#F8F7F4] border border-[#E7E5E0] text-xs font-semibold text-[#68645E]">
            0 registos
          </span>
        </div>

        <div id="activity-feed-list" class="mt-4 space-y-2.5 max-h-[540px] overflow-y-auto pr-1 flex-1">
          <!-- Entradas cronológicas injetadas via JS -->
        </div>
      </div>
    </section>

  </main>

  <!-- ========================================================================
       RODAPÉ
       ======================================================================== -->
  <footer class="mt-10 border-t border-[#E7E5E0] bg-white/60 py-5">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col sm:flex-row items-center justify-between gap-2 text-xs text-[#68645E]">
      <p class="font-display font-semibold text-[#1E1D1B]">
        Duelo a Dois — Competição saudável que transforma a rotina em equipa.
      </p>
      <p>
        Usa <strong class="text-[#1E1D1B]">Sincronizar a Dois</strong> para partilhar a mesma sala entre dois dispositivos.
      </p>
    </div>
  </footer>

  <!-- ========================================================================
       MODAL 1: ADICIONAR NOVA TAREFA, HÁBITO OU ATIVIDADE PERSONALIZADA
       ======================================================================== -->
  <div
    id="modal-new-item"
    class="fixed inset-0 z-40 hidden items-center justify-center bg-[#1E1D1B]/45 backdrop-blur-xs p-4 overflow-y-auto"
    role="dialog"
    aria-modal="true"
  >
    <div class="bg-white border border-[#E7E5E0] rounded-3xl max-w-lg w-full shadow-2xl overflow-hidden my-auto">
      <div class="px-6 py-4 border-b border-[#E7E5E0] flex items-center justify-between bg-[#F8F7F4]">
        <div>
          <span class="text-[11px] font-bold uppercase tracking-widest text-[#B37B12]">Personalizar Arena</span>
          <h3 class="font-display text-xl font-bold text-[#1E1D1B]">Nova Tarefa, Hábito ou Registo</h3>
        </div>
        <button
          type="button"
          data-close-modal="modal-new-item"
          class="w-9 h-9 rounded-full bg-white border border-[#E7E5E0] flex items-center justify-center text-[#68645E] hover:text-[#1E1D1B] cursor-pointer"
        >
          ✕
        </button>
      </div>

      <form id="form-new-item" class="p-6 space-y-4">
        <!-- Categoria -->
        <div>
          <label class="block text-xs font-bold uppercase tracking-wider text-[#68645E] mb-1.5">
            Categoria
          </label>
          <div class="grid grid-cols-2 gap-2">
            <button type="button" data-new-cat="running" class="new-cat-btn py-2 px-3 rounded-xl border border-[#E7E5E0] text-xs font-semibold text-left flex items-center gap-2 cursor-pointer">
              <span>🏃</span><span>Corrida &amp; Treino</span>
            </button>
            <button type="button" data-new-cat="work" class="new-cat-btn py-2 px-3 rounded-xl border border-[#E7E5E0] text-xs font-semibold text-left flex items-center gap-2 cursor-pointer">
              <span>💼</span><span>Trabalho &amp; Foco</span>
            </button>
            <button type="button" data-new-cat="chores" class="new-cat-btn py-2 px-3 rounded-xl border-2 border-[#1E1D1B] bg-[#F8F7F4] text-xs font-bold text-left flex items-center gap-2 cursor-pointer">
              <span>✅</span><span>Tarefas &amp; Casa</span>
            </button>
            <button type="button" data-new-cat="personal" class="new-cat-btn py-2 px-3 rounded-xl border border-[#E7E5E0] text-xs font-semibold text-left flex items-center gap-2 cursor-pointer">
              <span>✨</span><span>Pessoal &amp; Hábitos</span>
            </button>
          </div>
        </div>

        <!-- Nome da Tarefa / Hábito -->
        <div>
          <label for="new-item-title" class="block text-xs font-bold uppercase tracking-wider text-[#68645E] mb-1.5">
            Descrição da Tarefa ou Hábito
          </label>
          <input
            id="new-item-title"
            type="text"
            maxlength="60"
            required
            placeholder="Ex: Passear o cão, Limpar a varanda, Ler 30 min..."
            class="w-full px-3.5 py-2.5 rounded-xl bg-[#F8F7F4] border border-[#E7E5E0] text-sm font-medium text-[#1E1D1B] focus:outline-none focus:border-[#1E1D1B]"
          />
        </div>

        <!-- Valor em Pontos -->
        <div>
          <label class="block text-xs font-bold uppercase tracking-wider text-[#68645E] mb-1.5">
            Valor em Pontos
          </label>
          <div class="grid grid-cols-4 gap-2">
            <button type="button" data-new-pts="10" class="new-pts-btn py-2 rounded-xl border border-[#E7E5E0] font-display text-sm font-bold text-[#68645E] cursor-pointer">
              10 pts
            </button>
            <button type="button" data-new-pts="20" class="new-pts-btn py-2 rounded-xl border-2 border-[#D99B26] bg-[#FCF6E8] font-display text-sm font-bold text-[#B37B12] cursor-pointer">
              20 pts
            </button>
            <button type="button" data-new-pts="35" class="new-pts-btn py-2 rounded-xl border border-[#E7E5E0] font-display text-sm font-bold text-[#68645E] cursor-pointer">
              35 pts
            </button>
            <button type="button" data-new-pts="50" class="new-pts-btn py-2 rounded-xl border border-[#E7E5E0] font-display text-sm font-bold text-[#68645E] cursor-pointer">
              50 pts
            </button>
          </div>
        </div>

        <!-- Modo: Adicionar à lista partilhada OU Registar já concluído -->
        <div class="pt-2 border-t border-[#E7E5E0] space-y-2.5">
          <button
            type="submit"
            class="w-full py-2.5 px-4 rounded-xl bg-[#1E1D1B] hover:bg-[#33312D] text-white text-sm font-semibold cursor-pointer"
          >
            + Adicionar à Lista Partilhada
          </button>

          <div class="grid grid-cols-2 gap-2">
            <button
              id="btn-instant-log-p1"
              type="button"
              class="py-2 px-3 rounded-xl bg-[#FDF0ED] hover:bg-[#FADFD9] border border-[#F6C8BE] text-[#E85D3F] text-xs font-bold cursor-pointer"
            >
              ✓ Já Fiz (<span id="modal-p1-name">Sofia</span>)
            </button>
            <button
              id="btn-instant-log-p2"
              type="button"
              class="py-2 px-3 rounded-xl bg-[#EAF5F3] hover:bg-[#D7ECE8] border border-[#B7DDD8] text-[#16796F] text-xs font-bold cursor-pointer"
            >
              ✓ Já Fiz (<span id="modal-p2-name">Tiago</span>)
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>

  <!-- ========================================================================
       MODAL 2: EDITAR PERFIL DO JOGADOR (NOME & AVATAR)
       ======================================================================== -->
  <div
    id="modal-edit-player"
    class="fixed inset-0 z-40 hidden items-center justify-center bg-[#1E1D1B]/45 backdrop-blur-xs p-4"
    role="dialog"
    aria-modal="true"
  >
    <div class="bg-white border border-[#E7E5E0] rounded-3xl max-w-md w-full shadow-2xl overflow-hidden">
      <div class="px-6 py-4 border-b border-[#E7E5E0] flex items-center justify-between bg-[#F8F7F4]">
        <h3 id="edit-player-modal-title" class="font-display text-lg font-bold text-[#1E1D1B]">
          Editar Competidor
        </h3>
        <button
          type="button"
          data-close-modal="modal-edit-player"
          class="w-8 h-8 rounded-full bg-white border border-[#E7E5E0] flex items-center justify-center text-[#68645E] cursor-pointer"
        >
          ✕
        </button>
      </div>

      <form id="form-edit-player" class="p-6 space-y-4">
        <div>
          <label for="edit-player-name-input" class="block text-xs font-bold uppercase tracking-wider text-[#68645E] mb-1.5">
            Nome ou Alcunha
          </label>
          <input
            id="edit-player-name-input"
            type="text"
            maxlength="22"
            required
            class="w-full px-3.5 py-2.5 rounded-xl bg-[#F8F7F4] border border-[#E7E5E0] font-display text-base font-bold text-[#1E1D1B] focus:outline-none focus:border-[#1E1D1B]"
          />
        </div>

        <div>
          <label class="block text-xs font-bold uppercase tracking-wider text-[#68645E] mb-1.5">
            Escolher Avatar
          </label>
          <div id="avatar-picker-grid" class="grid grid-cols-5 gap-2">
            <!-- Avatares injetados via JS -->
          </div>
        </div>

        <div class="pt-2 flex justify-end gap-2">
          <button
            type="button"
            data-close-modal="modal-edit-player"
            class="px-4 py-2 rounded-xl bg-[#F8F7F4] border border-[#E7E5E0] text-xs font-semibold text-[#68645E] cursor-pointer"
          >
            Cancelar
          </button>
          <button
            type="submit"
            class="px-5 py-2 rounded-xl bg-[#1E1D1B] text-white text-xs font-bold cursor-pointer"
          >
            Guardar Perfil
          </button>
        </div>
      </form>
    </div>
  </div>

  <!-- ========================================================================
       MODAL 3: SINCRONIZAR A DOIS (CÓDIGO DE SALA PARTILHÁVEL)
       ======================================================================== -->
  <div
    id="modal-sync"
    class="fixed inset-0 z-40 hidden items-center justify-center bg-[#1E1D1B]/45 backdrop-blur-xs p-4"
    role="dialog"
    aria-modal="true"
  >
    <div class="bg-white border border-[#E7E5E0] rounded-3xl max-w-md w-full shadow-2xl overflow-hidden">
      <div class="px-6 py-4 border-b border-[#E7E5E0] flex items-center justify-between bg-[#F8F7F4]">
        <div>
          <span class="text-[11px] font-bold uppercase tracking-widest text-[#16796F]">Tempo Real</span>
          <h3 class="font-display text-xl font-bold text-[#1E1D1B]">Sincronizar a Dois</h3>
        </div>
        <button
          type="button"
          data-close-modal="modal-sync"
          class="w-9 h-9 rounded-full bg-white border border-[#E7E5E0] flex items-center justify-center text-[#68645E] cursor-pointer"
        >
          ✕
        </button>
      </div>

      <div class="p-6 space-y-5">
        <p class="text-xs sm:text-sm text-[#68645E] leading-relaxed">
          Partilha o <strong class="text-[#1E1D1B]">Código da Sala</strong> abaixo com a outra pessoa. Quando ambos estiverem ligados à mesma sala, todos os pontos, corridas e tarefas atualizam ao segundo nos dois ecrãs!
        </p>

        <!-- Código Atual da Sala -->
        <div class="p-4 rounded-2xl bg-[#F8F7F4] border border-[#E7E5E0] text-center space-y-2.5">
          <div class="flex items-center justify-center gap-1.5 text-[11px] font-bold uppercase tracking-widest text-[#68645E]">
            <span id="modal-sync-status-dot" class="w-2 h-2 rounded-full bg-emerald-500"></span>
            <span id="modal-sync-status-text">Sala Ativa &amp; Pronta</span>
          </div>

          <div id="modal-room-code-display" class="font-display text-3xl font-bold tracking-widest text-[#1E1D1B] select-all">
            DUELO-26
          </div>

          <div class="flex flex-wrap items-center justify-center gap-2 pt-1">
            <button
              id="btn-copy-room-code"
              type="button"
              class="px-3.5 py-2 rounded-xl bg-[#1E1D1B] hover:bg-[#33312D] text-white text-xs font-semibold cursor-pointer"
            >
              📋 Copiar Código
            </button>
            <button
              id="btn-new-random-room"
              type="button"
              class="px-3.5 py-2 rounded-xl bg-white hover:bg-[#FCF6E8] border border-[#E7E5E0] text-xs font-semibold text-[#1E1D1B] cursor-pointer"
            >
              🎲 Nova Sala
            </button>
          </div>
        </div>

        <!-- Entrar numa Sala Existente -->
        <form id="form-join-room" class="pt-3 border-t border-[#E7E5E0] space-y-2.5">
          <label for="input-join-room" class="block text-xs font-bold uppercase tracking-wider text-[#68645E]">
            Ou insere o código da outra pessoa:
          </label>
          <div class="flex items-center gap-2">
            <input
              id="input-join-room"
              type="text"
              maxlength="16"
              placeholder="Ex: DUELO-482"
              class="flex-1 px-3.5 py-2.5 rounded-xl bg-[#F8F7F4] border border-[#E7E5E0] font-display text-sm font-bold uppercase tracking-wider text-[#1E1D1B] focus:outline-none focus:border-[#16796F]"
            />
            <button
              type="submit"
              class="px-4 py-2.5 rounded-xl bg-[#16796F] hover:opacity-95 text-white text-xs font-bold cursor-pointer shrink-0"
            >
              Ligar à Sala
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>

  <!-- ========================================================================
       NOTIFICAÇÃO TOAST
       ======================================================================== -->
  <div
    id="toast-container"
    class="fixed bottom-5 right-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 pointer-events-none"
  >
    <div class="bg-[#1E1D1B] text-white border border-[#D99B26]/50 px-4 py-3 rounded-2xl shadow-xl flex items-center gap-2.5 text-xs sm:text-sm font-medium">
      <span class="text-[#D99B26]">✦</span>
      <span id="toast-message">Pontuação atualizada!</span>
    </div>
  </div>

  <!-- ========================================================================
       SCRIPT PRINCIPAL (ESTADO + LÓGICA + FIREBASE FIRESTORE SYNC)
       ======================================================================== -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getAuth, signInAnonymously, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
    import { getFirestore, doc, setDoc, getDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

    (function () {
      'use strict';

      /* ----------------------------------------------------------------------
         1. CONSTANTES & DADOS INICIAIS DE DEMONSTRAÇÃO
         ---------------------------------------------------------------------- */
      const AVATAR_OPTIONS = ['🏃‍♀️', '⚡', '🦊', '🔥', '🦁', '🚀', '🌿', '🎯', '🏃‍♂️', '🐺'];

      const CATEGORY_META = {
        running: { label: 'Corrida & Treino', icon: '🏃', color: '#E85D3F', bg: '#FDF0ED' },
        work: { label: 'Trabalho & Foco', icon: '💼', color: '#B37B12', bg: '#FCF6E8' },
        chores: { label: 'Tarefas & Casa', icon: '✅', color: '#16796F', bg: '#EAF5F3' },
        personal: { label: 'Pessoal & Hábitos', icon: '✨', color: '#6E49A6', bg: '#F4F0FA' },
      };

      function createInitialState() {
        return {
          bet: 'Quem ganhar escolhe o jantar de sábado e não arruma a cozinha!',
          players: {
            p1: { name: 'Sofia', avatar: '🏃‍♀️', streak: 5, wins: 3 },
            p2: { name: 'Tiago', avatar: '⚡', streak: 4, wins: 2 },
          },
          tasks: [
            // Corrida & Treino
            { id: 't-run-1', category: 'running', title: 'Treino de Alongamento & Mobilidade (20 min)', points: 20, p1Count: 1, p2Count: 0 },
            { id: 't-run-2', category: 'running', title: '10.000 Passos no Dia', points: 25, p1Count: 1, p2Count: 1 },
            // Trabalho & Foco
            { id: 't-work-1', category: 'work', title: 'Caixa de Entrada de E-mails a Zero', points: 20, p1Count: 1, p2Count: 1 },
            { id: 't-work-2', category: 'work', title: 'Concluir Tarefa Prioritária do Dia', points: 35, p1Count: 1, p2Count: 2 },
            { id: 't-work-3', category: 'work', title: 'Organizar Planeamento Semanal', points: 20, p1Count: 1, p2Count: 1 },
            // Tarefas & Casa
            { id: 't-chore-1', category: 'chores', title: 'Lavar e arrumar a loiça', points: 15, p1Count: 2, p2Count: 1 },
            { id: 't-chore-2', category: 'chores', title: 'Ir ao supermercado', points: 35, p1Count: 0, p2Count: 1 },
            { id: 't-chore-3', category: 'chores', title: 'Aspirar a sala e os quartos', points: 25, p1Count: 1, p2Count: 1 },
            { id: 't-chore-4', category: 'chores', title: 'Tratar da roupa (lavar e estender)', points: 20, p1Count: 1, p2Count: 1 },
            { id: 't-chore-5', category: 'chores', title: 'Preparar o jantar', points: 30, p1Count: 0, p2Count: 1 },
            // Pessoal & Hábitos
            { id: 't-pers-1', category: 'personal', title: 'Ler 20 páginas', points: 15, p1Count: 2, p2Count: 1 },
            { id: 't-pers-2', category: 'personal', title: 'Beber 2L de água', points: 10, p1Count: 2, p2Count: 2 },
            { id: 't-pers-3', category: 'personal', title: 'Meditar / Alongar 15 min', points: 15, p1Count: 1, p2Count: 0 },
            { id: 't-pers-4', category: 'personal', title: 'Dormir antes das 23h30', points: 20, p1Count: 1, p2Count: 1 },
            { id: 't-pers-5', category: 'personal', title: 'Estudar idioma / curso', points: 20, p1Count: 0, p2Count: 1 },
          ],
          logs: [
            {
              id: 'log-1',
              player: 'p1',
              category: 'running',
              title: 'Corrida de Rua — 6.2 km (34 min)',
              points: 110,
              km: 6.2,
              minutes: 34,
              timeLabel: 'Hoje, 08:30',
            },
            {
              id: 'log-2',
              player: 'p2',
              category: 'running',
              title: 'Passadeira — 5.0 km (28 min)',
              points: 89,
              km: 5.0,
              minutes: 28,
              timeLabel: 'Hoje, 09:15',
            },
            {
              id: 'log-3',
              player: 'p2',
              category: 'chores',
              title: 'Ir ao supermercado',
              points: 35,
              km: 0,
              minutes: 0,
              timeLabel: 'Hoje, 10:40',
            },
            {
              id: 'log-4',
              player: 'p1',
              category: 'work',
              title: 'Sessão Deep Work (50 min)',
              points: 65,
              km: 0,
              minutes: 50,
              timeLabel: 'Hoje, 11:20',
            },
            {
              id: 'log-5',
              player: 'p2',
              category: 'work',
              title: 'Sessão Pomodoro (25 min)',
              points: 30,
              km: 0,
              minutes: 25,
              timeLabel: 'Hoje, 11:50',
            },
            {
              id: 'log-6',
              player: 'p1',
              category: 'chores',
              title: 'Lavar e arrumar a loiça',
              points: 15,
              km: 0,
              minutes: 0,
              timeLabel: 'Hoje, 12:15',
            },
            {
              id: 'log-7',
              player: 'p1',
              category: 'personal',
              title: 'Ler 20 páginas (+1)',
              points: 15,
              km: 0,
              minutes: 0,
              timeLabel: 'Hoje, 12:45',
            },
            {
              id: 'log-8',
              player: 'p2',
              category: 'personal',
              title: 'Estudar idioma / curso (+1)',
              points: 20,
              km: 0,
              minutes: 0,
              timeLabel: 'Hoje, 13:05',
            },
            {
              id: 'log-9',
              player: 'p1',
              category: 'running',
              title: 'Corrida de Rua — 5.0 km (26 min)',
              points: 88,
              km: 5.0,
              minutes: 26,
              timeLabel: 'Ontem, 18:40',
            },
            {
              id: 'log-10',
              player: 'p2',
              category: 'running',
              title: 'Corrida de Rua — 4.5 km (24 min)',
              points: 80,
              km: 4.5,
              minutes: 24,
              timeLabel: 'Ontem, 19:10',
            },
          ],
          champions: [
            {
              id: 'champ-1',
              weekLabel: 'Semana Anterior',
              winnerName: 'Sofia',
              winnerAvatar: '🏃‍♀️',
              winnerColor: '#E85D3F',
              scoreSummary: '485 vs 440 pts',
              bet: 'Escolheu cinema e gelados no domingo',
            },
            {
              id: 'champ-2',
              weekLabel: 'Há 2 Semanas',
              winnerName: 'Tiago',
              winnerAvatar: '⚡',
              winnerColor: '#16796F',
              scoreSummary: '510 vs 475 pts',
              bet: 'Pequeno-almoço servido na cama',
            },
          ],
        };
      }

      /* ----------------------------------------------------------------------
         2. ESTADO EM MEMÓRIA & SINCRONIZAÇÃO FIREBASE FIRESTORE
         ---------------------------------------------------------------------- */
      let state = createInitialState();
      let activeCategoryTab = 'all';
      let selectedRunPlayer = 'p1';
      let previousLeader = null; // 'p1' | 'p2' | 'tie'

      // Estado do Cronómetro de Foco
      let timerState = {
        player: 'p1',
        presetMinutes: 25, // 25, 50, ou 0 (Livre)
        secondsLeft: 25 * 60,
        elapsedFreeSeconds: 0,
        running: false,
        intervalId: null,
      };

      // Estado do Modal Nova Tarefa
      let newItemCategory = 'chores';
      let newItemPoints = 20;

      // Estado do Modal Editar Jogador
      let editingPlayerKey = 'p1';
      let editingPlayerAvatar = '🏃‍♀️';

      // Estado de Sincronização Firebase
      let db = null;
      let auth = null;
      let currentUser = null;
      let appId = typeof __app_id !== 'undefined' ? __app_id : 'duelo-a-dois-app';
      let roomCode = extractInitialRoomCode();
      let unsubscribeRoom = null;
      let isCloudConnected = false;
      let ignoreNextRemoteSnapshot = false;

      function extractInitialRoomCode() {
        try {
          const hash = window.location.hash || '';
          const match = hash.match(/sala=([A-Za-z0-9-]+)/i);
          if (match && match[1]) {
            return match[1].toUpperCase();
          }
        } catch (e) {
          // Ignorar erro de hash
        }
        return 'DUELO-' + Math.floor(100 + Math.random() * 900);
      }

      function updateSyncIndicators(connected, statusText) {
        isCloudConnected = connected;
        const dotDesktop = document.getElementById('sync-dot-desktop');
        const dotMobile = document.getElementById('sync-dot-mobile');
        const modalDot = document.getElementById('modal-sync-status-dot');
        const modalText = document.getElementById('modal-sync-status-text');

        const colorClass = connected ? 'bg-emerald-500' : 'bg-amber-500';
        [dotDesktop, dotMobile, modalDot].forEach((el) => {
          if (!el) return;
          el.classList.remove('bg-emerald-500', 'bg-amber-500');
          el.classList.add(colorClass);
        });

        if (modalText) {
          modalText.textContent = statusText || (connected ? 'Sincronizado em Tempo Real' : 'Modo Sessão Rápida');
        }

        const codeDesk = document.getElementById('room-code-desktop');
        const codeMob = document.getElementById('room-code-mobile');
        const modalCode = document.getElementById('modal-room-code-display');
        if (codeDesk) codeDesk.textContent = roomCode;
        if (codeMob) codeMob.textContent = roomCode;
        if (modalCode) modalCode.textContent = roomCode;
      }

      async function initFirebaseSync() {
        updateSyncIndicators(false, 'A ligar à sala...');

        if (typeof __firebase_config === 'undefined' || !__firebase_config) {
          updateSyncIndicators(false, 'Modo Local Ativo (Sala Pronto)');
          return;
        }

        try {
          const firebaseConfig = JSON.parse(__firebase_config);
          const app = initializeApp(firebaseConfig);
          auth = getAuth(app);
          db = getFirestore(app);

          // REGRA 3: Autenticar ANTES de qualquer operação no Firestore
          if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
            await signInWithCustomToken(auth, __initial_auth_token);
          } else {
            await signInAnonymously(auth);
          }

          currentUser = auth.currentUser;
          if (!currentUser) return;

          await connectToRoom(roomCode, false);
        } catch (err) {
          console.warn('Sincronização em nuvem indisponível, a continuar em memória:', err);
          updateSyncIndicators(false, 'Modo Sessão Local Ativo');
        }
      }

      async function connectToRoom(targetCode, isJoiningExisting = false) {
        const cleanCode = String(targetCode || '')
          .trim()
          .toUpperCase()
          .replace(/[^A-Z0-9-]/g, '');

        if (!cleanCode) return;
        roomCode = cleanCode;

        try {
          window.history.replaceState(null, '', `#sala=${roomCode}`);
        } catch (e) {
          // Ignorar se o iframe bloquear replaceState
        }

        updateSyncIndicators(isCloudConnected, `Sala ${roomCode}`);

        // REGRA 3: Guardar operações Firestore com verificação de currentUser
        if (!db || !currentUser) {
          renderAll();
          return;
        }

        if (unsubscribeRoom) {
          unsubscribeRoom();
          unsubscribeRoom = null;
        }

        // REGRA 1: Caminho estrito /artifacts/{appId}/public/data/{collectionName}/{docId}
        const roomRef = doc(db, 'artifacts', appId, 'public', 'data', 'dueloRooms', roomCode);

        try {
          const snap = await getDoc(roomRef);
          if (snap.exists() && snap.data() && snap.data().payload) {
            state = snap.data().payload;
            renderAll();
            if (isJoiningExisting) {
              showToast(`Ligado à sala ${roomCode} com sucesso!`);
            }
          } else {
            // Criar sala com o estado atual
            await pushStateToCloud();
            if (isJoiningExisting) {
              showToast(`Nova sala ${roomCode} criada e sincronizada!`);
            }
          }

          unsubscribeRoom = onSnapshot(
            roomRef,
            (docSnap) => {
              updateSyncIndicators(true, 'Sincronizado em Tempo Real');
              if (ignoreNextRemoteSnapshot) {
                ignoreNextRemoteSnapshot = false;
                return;
              }
              if (docSnap.exists() && docSnap.data() && docSnap.data().payload) {
                state = docSnap.data().payload;
                renderAll();
              }
            },
            (error) => {
              console.warn('Erro no listener onSnapshot:', error);
              updateSyncIndicators(false, 'Ligação interrompida (Modo Local)');
            }
          );
        } catch (err) {
          console.warn('Erro ao ligar à sala Firestore:', err);
          updateSyncIndicators(false, 'Modo Sessão Local Ativo');
        }
      }

      async function pushStateToCloud() {
        if (!db || !currentUser || !roomCode) return;
        try {
          ignoreNextRemoteSnapshot = true;
          const roomRef = doc(db, 'artifacts', appId, 'public', 'data', 'dueloRooms', roomCode);
          await setDoc(roomRef, {
            payload: state,
            updatedAt: Date.now(),
            updatedBy: currentUser.uid,
          });
        } catch (err) {
          ignoreNextRemoteSnapshot = false;
          console.warn('Não foi possível sincronizar alteração:', err);
        }
      }

      function commitStateChange(celebrate = false) {
        const beforeLeader = previousLeader;
        renderAll();
        pushStateToCloud();

        // Verifica se houve mudança de liderança para lançar confetes!
        const totals = computeTotals();
        const afterLeader =
          totals.p1.points > totals.p2.points
            ? 'p1'
            : totals.p2.points > totals.p1.points
            ? 'p2'
            : 'tie';

        if (celebrate || (beforeLeader && afterLeader !== 'tie' && beforeLeader !== afterLeader)) {
          launchConfetti();
        }
        previousLeader = afterLeader;
      }

      /* ----------------------------------------------------------------------
         3. CÁLCULO DE PONTUAÇÕES & ESTATÍSTICAS POR CATEGORIA
         ---------------------------------------------------------------------- */
      function computeTotals() {
        const stats = {
          p1: { points: 0, km: 0, workSessions: 0, workMinutes: 0, choresCount: 0, personalCount: 0 },
          p2: { points: 0, km: 0, workSessions: 0, workMinutes: 0, choresCount: 0, personalCount: 0 },
        };

        (state.logs || []).forEach((log) => {
          const p = log.player === 'p2' ? 'p2' : 'p1';
          stats[p].points += Number(log.points) || 0;

          if (log.category === 'running') {
            stats[p].km += Number(log.km) || 0;
          } else if (log.category === 'work') {
            stats[p].workSessions += 1;
            stats[p].workMinutes += Number(log.minutes) || 25;
          } else if (log.category === 'chores') {
            stats[p].choresCount += 1;
          } else if (log.category === 'personal') {
            stats[p].personalCount += 1;
          }
        });

        stats.p1.km = Math.round(stats.p1.km * 10) / 10;
        stats.p2.km = Math.round(stats.p2.km * 10) / 10;

        return stats;
      }

      function formatCurrentTimeLabel() {
        const now = new Date();
        const hh = String(now.getHours()).padStart(2, '0');
        const mm = String(now.getMinutes()).padStart(2, '0');
        return `Hoje, ${hh}:${mm}`;
      }

      /* ----------------------------------------------------------------------
         4. UTILITÁRIOS DE UI (ESCAPE HTML, TOAST & CONFETES)
         ---------------------------------------------------------------------- */
      function escapeHTML(str) {
        if (!str) return '';
        return String(str)
          .replace(/&/g, '&amp;')
          .replace(/</g, '&lt;')
          .replace(/>/g, '&gt;')
          .replace(/"/g, '&quot;')
          .replace(/'/g, '&#39;');
      }

      let toastTimer = null;
      function showToast(msg) {
        const box = document.getElementById('toast-container');
        const text = document.getElementById('toast-message');
        if (!box || !text) return;

        text.textContent = msg;
        box.classList.remove('translate-y-20', 'opacity-0');
        box.classList.add('translate-y-0', 'opacity-100');

        clearTimeout(toastTimer);
        toastTimer = setTimeout(() => {
          box.classList.add('translate-y-20', 'opacity-0');
          box.classList.remove('translate-y-0', 'opacity-100');
        }, 3000);
      }

      // Motor leve de confetes em Canvas HTML5
      let confettiParticles = [];
      let confettiAnimId = null;

      function launchConfetti() {
        const canvas = document.getElementById('confetti-canvas');
        if (!canvas) return;
        const ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        const palette = ['#E85D3F', '#16796F', '#D99B26', '#F4A261', '#2A9D8F'];
        for (let i = 0; i < 85; i++) {
          confettiParticles.push({
            x: canvas.width * (0.25 + Math.random() * 0.5),
            y: canvas.height * 0.32,
            vx: (Math.random() - 0.5) * 14,
            vy: -Math.random() * 11 - 4,
            size: Math.random() * 7 + 4,
            color: palette[Math.floor(Math.random() * palette.length)],
            rot: Math.random() * 360,
            vRot: (Math.random() - 0.5) * 12,
            life: 110 + Math.random() * 40,
          });
        }

        if (confettiAnimId) return;

        function animateConfetti() {
          ctx.clearRect(0, 0, canvas.width, canvas.height);
          confettiParticles.forEach((p) => {
            p.x += p.vx;
            p.y += p.vy;
            p.vy += 0.35; // gravidade
            p.vx *= 0.99;
            p.rot += p.vRot;
            p.life -= 1;

            ctx.save();
            ctx.translate(p.x, p.y);
            ctx.rotate((p.rot * Math.PI) / 180);
            ctx.fillStyle = p.color;
            ctx.globalAlpha = Math.max(0, Math.min(1, p.life / 40));
            ctx.fillRect(-p.size / 2, -p.size / 2, p.size, p.size * 0.65);
            ctx.restore();
          });

          confettiParticles = confettiParticles.filter((p) => p.life > 0 && p.y < canvas.height + 30);

          if (confettiParticles.length > 0) {
            confettiAnimId = requestAnimationFrame(animateConfetti);
          } else {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            confettiAnimId = null;
          }
        }

        animateConfetti();
      }

      /* ----------------------------------------------------------------------
         5. RENDERIZAÇÃO COMPLETA DA INTERFACE
         ---------------------------------------------------------------------- */
      function renderAll() {
        const totals = computeTotals();
        const p1 = state.players.p1;
        const p2 = state.players.p2;

        // 1. Aposta da Semana
        document.getElementById('bet-display-text').textContent = state.bet;

        // 2. Cartões dos Jogadores
        document.getElementById('p1-avatar').textContent = p1.avatar;
        document.getElementById('p1-name-display').textContent = p1.name;
        document.getElementById('p1-score').textContent = totals.p1.points;
        document.getElementById('p1-streak').textContent = `🔥 ${p1.streak} dias`;
        document.getElementById('p1-wins').textContent = `🏆 ${p1.wins} sem.`;

        document.getElementById('p2-avatar').textContent = p2.avatar;
        document.getElementById('p2-name-display').textContent = p2.name;
        document.getElementById('p2-score').textContent = totals.p2.points;
        document.getElementById('p2-streak').textContent = `🔥 ${p2.streak} dias`;
        document.getElementById('p2-wins').textContent = `🏆 ${p2.wins} sem.`;

        // Sincronizar nomes nos botões das ferramentas
        document.getElementById('run-btn-p1-label').textContent = `${p1.avatar} ${p1.name}`;
        document.getElementById('run-btn-p2-label').textContent = `${p2.avatar} ${p2.name}`;
        document.getElementById('timer-p1-name').textContent = p1.name;
        document.getElementById('timer-p2-name').textContent = p2.name;
        document.getElementById('stats-legend-p1').textContent = p1.name;
        document.getElementById('stats-legend-p2').textContent = p2.name;
        document.getElementById('modal-p1-name').textContent = p1.name;
        document.getElementById('modal-p2-name').textContent = p2.name;

        // 3. Liderança & Cabo de Guerra
        const cardP1 = document.getElementById('card-player-1');
        const cardP2 = document.getElementById('card-player-2');
        const badgeP1 = document.getElementById('p1-leader-badge');
        const badgeP2 = document.getElementById('p2-leader-badge');

        const diff = Math.abs(totals.p1.points - totals.p2.points);
        const totalSum = totals.p1.points + totals.p2.points;
        const p1Pct = totalSum > 0 ? Math.max(12, Math.min(88, Math.round((totals.p1.points / totalSum) * 100))) : 50;
        const p2Pct = 100 - p1Pct;

        document.getElementById('tug-p1-pct').textContent = `${p1Pct}%`;
        document.getElementById('tug-p2-pct').textContent = `${p2Pct}%`;
        document.getElementById('tug-bar-p1').style.width = `${p1Pct}%`;
        document.getElementById('tug-bar-p2').style.width = `${p2Pct}%`;
        document.getElementById('tug-marker').style.left = `${p1Pct}%`;
        document.getElementById('combined-points').textContent = `${totalSum} pts`;

        cardP1.classList.remove('leader-glow');
        cardP2.classList.remove('leader-glow');
        badgeP1.classList.add('hidden');
        badgeP1.classList.remove('inline-flex');
        badgeP2.classList.add('hidden');
        badgeP2.classList.remove('inline-flex');

        if (totals.p1.points > totals.p2.points) {
          cardP1.classList.add('leader-glow');
          badgeP1.classList.remove('hidden');
          badgeP1.classList.add('inline-flex');
          document.getElementById('lead-status-headline').textContent = `👑 ${p1.name} lidera por +${diff} pts`;
          document.getElementById('lead-status-sub').textContent = `${p2.name} precisa de ${diff + 1} pts para assumir a frente!`;
        } else if (totals.p2.points > totals.p1.points) {
          cardP2.classList.add('leader-glow');
          badgeP2.classList.remove('hidden');
          badgeP2.classList.add('inline-flex');
          document.getElementById('lead-status-headline').textContent = `👑 ${p2.name} lidera por +${diff} pts`;
          document.getElementById('lead-status-sub').textContent = `${p1.name} precisa de ${diff + 1} pts para assumir a frente!`;
        } else {
          document.getElementById('lead-status-headline').textContent = '⚖️ Empate Técnico na Arena!';
          document.getElementById('lead-status-sub').textContent = 'O próximo ponto desempata e conquista a coroa!';
        }

        // 4. Resumos por Cabeçalho de Categoria
        document.getElementById('run-km-p1').textContent = `${totals.p1.km} km`;
        document.getElementById('run-km-p2').textContent = `${totals.p2.km} km`;
        document.getElementById('work-sessions-p1').textContent = `${totals.p1.workSessions} sessões`;
        document.getElementById('work-sessions-p2').textContent = `${totals.p2.workSessions} sessões`;
        document.getElementById('chores-count-p1').textContent = `${totals.p1.choresCount} feitas`;
        document.getElementById('chores-count-p2').textContent = `${totals.p2.choresCount} feitas`;
        document.getElementById('personal-count-p1').textContent = `${totals.p1.personalCount} hábitos`;
        document.getElementById('personal-count-p2').textContent = `${totals.p2.personalCount} hábitos`;

        // 5. Renderizar Listas das 4 Categorias
        renderCategoryTasks('running', 'list-running');
        renderCategoryTasks('work', 'list-work');
        renderCategoryTasks('chores', 'list-chores');
        renderCategoryTasks('personal', 'list-personal');

        // 6. Renderizar Painel "Quem Faz Mais" & Feed & Campeões
        renderCategoryStats(totals);
        renderActivityFeed();
        renderChampionsHistory();
      }

      function renderCategoryTasks(catKey, containerId) {
        const container = document.getElementById(containerId);
        if (!container) return;

        const p1 = state.players.p1;
        const p2 = state.players.p2;
        const items = (state.tasks || []).filter((t) => t.category === catKey);

        if (items.length === 0) {
          container.innerHTML = `<p class="text-xs text-[#68645E] italic py-2">Ainda sem itens nesta lista.</p>`;
          return;
        }

        const isHabit = catKey === 'personal';

        container.innerHTML = items
          .map((item) => {
            return `
              <div class="group p-3 rounded-2xl bg-[#F8F7F4]/80 hover:bg-[#F8F7F4] border border-[#E7E5E0] flex flex-col sm:flex-row sm:items-center justify-between gap-2.5 transition-colors">
                <div class="min-w-0 flex-1">
                  <div class="flex items-center gap-2">
                    <span class="font-medium text-sm text-[#1E1D1B] truncate">${escapeHTML(item.title)}</span>
                    <span class="px-2 py-0.5 rounded-md bg-white border border-[#E7E5E0] font-display text-[11px] font-bold text-[#B37B12] shrink-0">
                      +${item.points} pts
                    </span>
                  </div>
                  <div class="flex items-center gap-2 mt-1 text-[11px] text-[#68645E]">
                    <span><strong class="text-[#E85D3F]">${escapeHTML(p1.name)}:</strong> ${item.p1Count || 0}x</span>
                    <span>•</span>
                    <span><strong class="text-[#16796F]">${escapeHTML(p2.name)}:</strong> ${item.p2Count || 0}x</span>
                  </div>
                </div>

                <div class="flex items-center gap-1.5 shrink-0">
                  <button
                    type="button"
                    data-action="claim-task"
                    data-task-id="${item.id}"
                    data-player="p1"
                    class="px-2.5 py-1.5 rounded-xl bg-[#FDF0ED] hover:bg-[#E85D3F] text-[#E85D3F] hover:text-white border border-[#F6C8BE] text-xs font-bold transition-all cursor-pointer"
                    title="Registar para ${escapeHTML(p1.name)}"
                  >
                    ${isHabit ? `+1 ${escapeHTML(p1.name)}` : `${p1.avatar} +${item.points}`}
                  </button>

                  <button
                    type="button"
                    data-action="claim-task"
                    data-task-id="${item.id}"
                    data-player="p2"
                    class="px-2.5 py-1.5 rounded-xl bg-[#EAF5F3] hover:bg-[#16796F] text-[#16796F] hover:text-white border border-[#B7DDD8] text-xs font-bold transition-all cursor-pointer"
                    title="Registar para ${escapeHTML(p2.name)}"
                  >
                    ${isHabit ? `+1 ${escapeHTML(p2.name)}` : `${p2.avatar} +${item.points}`}
                  </button>

                  <button
                    type="button"
                    data-action="delete-task"
                    data-task-id="${item.id}"
                    class="p-1.5 rounded-lg text-[#68645E]/60 hover:text-red-600 hover:bg-red-50 transition-colors cursor-pointer"
                    title="Remover item da lista"
                  >
                    ✕
                  </button>
                </div>
              </div>
            `;
          })
          .join('');
      }

      function renderCategoryStats(totals) {
        const container = document.getElementById('category-stats-container');
        if (!container) return;

        const p1 = state.players.p1;
        const p2 = state.players.p2;

        const rows = [
          {
            icon: '🏃',
            title: 'Corrida & Distância',
            v1: totals.p1.km,
            v2: totals.p2.km,
            label1: `${totals.p1.km} km`,
            label2: `${totals.p2.km} km`,
          },
          {
            icon: '💼',
            title: 'Sessões de Trabalho & Foco',
            v1: totals.p1.workSessions,
            v2: totals.p2.workSessions,
            label1: `${totals.p1.workSessions} sessões (${totals.p1.workMinutes}m)`,
            label2: `${totals.p2.workSessions} sessões (${totals.p2.workMinutes}m)`,
          },
          {
            icon: '✅',
            title: 'Tarefas & Casa Concluídas',
            v1: totals.p1.choresCount,
            v2: totals.p2.choresCount,
            label1: `${totals.p1.choresCount} tarefas`,
            label2: `${totals.p2.choresCount} tarefas`,
          },
          {
            icon: '✨',
            title: 'Hábitos Pessoais Cumpridos',
            v1: totals.p1.personalCount,
            v2: totals.p2.personalCount,
            label1: `${totals.p1.personalCount} hábitos`,
            label2: `${totals.p2.personalCount} hábitos`,
          },
        ];

        container.innerHTML = rows
          .map((row) => {
            const sum = row.v1 + row.v2;
            const pct1 = sum > 0 ? Math.max(8, Math.min(92, Math.round((row.v1 / sum) * 100))) : 50;
            const pct2 = 100 - pct1;
            const leaderTag =
              row.v1 > row.v2
                ? `<span class="text-[11px] font-bold text-[#E85D3F]">👑 ${escapeHTML(p1.name)}</span>`
                : row.v2 > row.v1
                ? `<span class="text-[11px] font-bold text-[#16796F]">👑 ${escapeHTML(p2.name)}</span>`
                : `<span class="text-[11px] font-semibold text-[#68645E]">Empate</span>`;

            return `
              <div class="p-3.5 rounded-2xl bg-[#F8F7F4] border border-[#E7E5E0]">
                <div class="flex items-center justify-between gap-2 mb-1.5">
                  <span class="text-xs sm:text-sm font-bold text-[#1E1D1B]">${row.icon} ${row.title}</span>
                  ${leaderTag}
                </div>

                <div class="flex items-center justify-between text-xs font-display font-bold mb-1.5">
                  <span class="text-[#E85D3F]">${row.label1}</span>
                  <span class="text-[#16796F]">${row.label2}</span>
                </div>

                <div class="h-2.5 w-full rounded-full bg-[#E7E5E0] overflow-hidden flex">
                  <div class="h-full bg-[#E85D3F] transition-all duration-500" style="width: ${pct1}%"></div>
                  <div class="h-full bg-[#16796F] transition-all duration-500" style="width: ${pct2}%"></div>
                </div>
              </div>
            `;
          })
          .join('');
      }

      function renderActivityFeed() {
        const container = document.getElementById('activity-feed-list');
        const badge = document.getElementById('feed-count-badge');
        const logs = state.logs || [];

        if (badge) badge.textContent = `${logs.length} registos`;
        if (!container) return;

        if (logs.length === 0) {
          container.innerHTML = `
            <div class="text-center py-10 text-xs text-[#68645E]">
              Ainda não há registos nesta ronda. Começa por concluir uma tarefa ou corrida!
            </div>
          `;
          return;
        }

        container.innerHTML = logs
          .map((log) => {
            const isP1 = log.player === 'p1';
            const playerObj = isP1 ? state.players.p1 : state.players.p2;
            const catMeta = CATEGORY_META[log.category] || CATEGORY_META.chores;
            const accentColor = isP1 ? '#E85D3F' : '#16796F';
            const softBg = isP1 ? '#FDF0ED' : '#EAF5F3';

            return `
              <div class="p-3 rounded-2xl bg-[#F8F7F4] border border-[#E7E5E0] flex items-start justify-between gap-2.5">
                <div class="flex items-start gap-2.5 min-w-0">
                  <div
                    class="w-9 h-9 rounded-xl flex items-center justify-center text-lg shrink-0 border"
                    style="background-color: ${softBg}; border-color: ${accentColor}33;"
                  >
                    ${playerObj.avatar}
                  </div>
                  <div class="min-w-0">
                    <div class="flex flex-wrap items-center gap-1.5">
                      <span class="text-xs font-bold" style="color: ${accentColor};">
                        ${escapeHTML(playerObj.name)}
                      </span>
                      <span class="text-[10px] px-1.5 py-0.5 rounded-md bg-white border border-[#E7E5E0] text-[#68645E]">
                        ${catMeta.icon} ${catMeta.label}
                      </span>
                    </div>
                    <p class="text-xs font-medium text-[#1E1D1B] mt-0.5 leading-snug">
                      ${escapeHTML(log.title)}
                    </p>
                    <span class="text-[10px] text-[#68645E]">${escapeHTML(log.timeLabel)}</span>
                  </div>
                </div>

                <div class="flex flex-col items-end gap-1 shrink-0">
                  <span class="font-display text-xs font-bold px-2 py-0.5 rounded-lg bg-white border border-[#E7E5E0] text-[#B37B12]">
                    +${log.points} pts
                  </span>
                  <button
                    type="button"
                    data-action="undo-log"
                    data-log-id="${log.id}"
                    class="text-[10px] font-semibold text-[#68645E] hover:text-red-600 transition-colors cursor-pointer"
                    title="Anular este registo"
                  >
                    Anular
                  </button>
                </div>
              </div>
            `;
          })
          .join('');
      }

      function renderChampionsHistory() {
        const container = document.getElementById('champions-history-list');
        if (!container) return;

        const list = state.champions || [];
        if (list.length === 0) {
          container.innerHTML = `<p class="text-xs text-[#68645E] italic">Encerra a primeira semana para registar o campeão!</p>`;
          return;
        }

        container.innerHTML = list
          .map((item) => {
            return `
              <div class="px-3.5 py-2.5 rounded-2xl bg-[#FCF6E8]/60 border border-[#F0D699] flex items-center justify-between gap-2 text-xs">
                <div class="flex items-center gap-2.5 min-w-0">
                  <span class="text-lg">${item.winnerAvatar}</span>
                  <div class="min-w-0">
                    <div class="font-bold text-[#1E1D1B] truncate">
                      <span style="color: ${item.winnerColor};">${escapeHTML(item.winnerName)}</span> venceu (${escapeHTML(item.weekLabel)})
                    </div>
                    <div class="text-[11px] text-[#68645E] truncate">Prémio: ${escapeHTML(item.bet)}</div>
                  </div>
                </div>
                <span class="font-display font-bold text-[#B37B12] shrink-0">${escapeHTML(item.scoreSummary)}</span>
              </div>
            `;
          })
          .join('');
      }

      /* ----------------------------------------------------------------------
         6. LÓGICA DO CRONÓMETRO DE FOCO INTEGRADO
         ---------------------------------------------------------------------- */
      function formatSecondsMMSS(totalSec) {
        const mins = Math.floor(totalSec / 60);
        const secs = totalSec % 60;
        return `${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
      }

      function updateTimerUI() {
        const display = document.getElementById('timer-display');
        const rewardLabel = document.getElementById('timer-reward-label');
        const toggleBtn = document.getElementById('btn-timer-toggle');

        if (timerState.presetMinutes > 0) {
          display.textContent = formatSecondsMMSS(timerState.secondsLeft);
          const pts = timerState.presetMinutes === 50 ? 65 : 30;
          rewardLabel.textContent = `Recompensa: +${pts} pts ao concluir`;
        } else {
          display.textContent = formatSecondsMMSS(timerState.elapsedFreeSeconds);
          const livePts = Math.max(10, Math.round(timerState.elapsedFreeSeconds / 60));
          rewardLabel.textContent = `Modo Livre: +${livePts} pts acumulados`;
        }

        if (timerState.running) {
          toggleBtn.textContent = '⏸ Pausar';
          display.classList.add('text-[#B37B12]');
        } else {
          toggleBtn.textContent = '▶ Iniciar';
          display.classList.remove('text-[#B37B12]');
        }
      }

      function completeFocusSession() {
        stopFocusTimer();
        const pKey = timerState.player;
        const pObj = state.players[pKey];

        let earnedPts = 30;
        let workedMins = 25;
        let label = 'Sessão Pomodoro (25 min)';

        if (timerState.presetMinutes === 50) {
          earnedPts = 65;
          workedMins = 50;
          label = 'Sessão Deep Work (50 min)';
        } else if (timerState.presetMinutes === 0) {
          workedMins = Math.max(10, Math.round(timerState.elapsedFreeSeconds / 60));
          earnedPts = Math.max(15, workedMins);
          label = `Sessão de Foco Livre (${workedMins} min)`;
        }

        state.logs.unshift({
          id: 'log-' + Date.now(),
          player: pKey,
          category: 'work',
          title: label,
          points: earnedPts,
          km: 0,
          minutes: workedMins,
          timeLabel: formatCurrentTimeLabel(),
        });

        resetFocusTimer();
        commitStateChange(true);
        showToast(`💼 ${pObj.name} concluiu foco e ganhou +${earnedPts} pts!`);
      }

      function stopFocusTimer() {
        timerState.running = false;
        if (timerState.intervalId) {
          clearInterval(timerState.intervalId);
          timerState.intervalId = null;
        }
        updateTimerUI();
      }

      function resetFocusTimer() {
        stopFocusTimer();
        timerState.secondsLeft = timerState.presetMinutes * 60;
        timerState.elapsedFreeSeconds = 0;
        updateTimerUI();
      }

      /* ----------------------------------------------------------------------
         7. CÁLCULO DA PRÉVIA DE PONTOS NA CORRIDA
         ---------------------------------------------------------------------- */
      function calculateRunPoints() {
        const km = Math.max(0, parseFloat(document.getElementById('run-km').value) || 0);
        const mins = Math.max(0, parseInt(document.getElementById('run-mins').value, 10) || 0);
        return Math.max(10, Math.round(km * 15 + mins * 0.5));
      }

      function updateRunPreviewUI() {
        const pts = calculateRunPoints();
        const badge = document.getElementById('run-points-preview');
        if (badge) badge.textContent = `+${pts} pts`;
      }

      /* ----------------------------------------------------------------------
         8. GESTÃO DE MODAIS & EVENTOS DE INTERAÇÃO
         ---------------------------------------------------------------------- */
      function openModal(id) {
        const el = document.getElementById(id);
        if (!el) return;
        el.classList.remove('hidden');
        el.classList.add('flex');
      }

      function closeModal(id) {
        const el = document.getElementById(id);
        if (!el) return;
        el.classList.add('hidden');
        el.classList.remove('flex');
      }

      function copyToClipboardSafe(text) {
        const temp = document.createElement('textarea');
        temp.value = text;
        temp.style.position = 'fixed';
        temp.style.left = '-9999px';
        document.body.appendChild(temp);
        temp.select();
        try {
          document.execCommand('copy');
          showToast(`Código "${text}" copiado!`);
        } catch (err) {
          showToast(`Código da sala: ${text}`);
        }
        document.body.removeChild(temp);
      }

      function bindEvents() {
        // Fechar modais via botão X ou clique no fundo escuro
        document.querySelectorAll('[data-close-modal]').forEach((btn) => {
          btn.addEventListener('click', () => {
            closeModal(btn.getAttribute('data-close-modal'));
          });
        });

        ['modal-new-item', 'modal-edit-player', 'modal-sync'].forEach((modalId) => {
          const backdrop = document.getElementById(modalId);
          backdrop.addEventListener('click', (e) => {
            if (e.target.id === modalId) closeModal(modalId);
          });
        });

        // Editar Aposta / Prémio da Semana
        document.getElementById('btn-edit-bet').addEventListener('click', () => {
          document.getElementById('bet-display-text').classList.add('hidden');
          document.getElementById('bet-edit-row').classList.remove('hidden');
          const input = document.getElementById('bet-input');
          input.value = state.bet;
          input.focus();
        });

        document.getElementById('btn-cancel-bet').addEventListener('click', () => {
          document.getElementById('bet-display-text').classList.remove('hidden');
          document.getElementById('bet-edit-row').classList.add('hidden');
        });

        document.getElementById('btn-save-bet').addEventListener('click', () => {
          const val = document.getElementById('bet-input').value.trim();
          if (val) {
            state.bet = val;
            commitStateChange(false);
            showToast('Aposta da semana atualizada!');
          }
          document.getElementById('bet-display-text').classList.remove('hidden');
          document.getElementById('bet-edit-row').classList.add('hidden');
        });

        // Trocar Avatar rápido ao clicar no avatar ou abrir modal de edição de perfil
        document.body.addEventListener('click', (e) => {
          const actionEl = e.target.closest('[data-action]');
          if (!actionEl) return;

          const action = actionEl.getAttribute('data-action');

          if (action === 'cycle-avatar') {
            const pKey = actionEl.getAttribute('data-player');
            const curr = state.players[pKey].avatar;
            const nextIdx = (AVATAR_OPTIONS.indexOf(curr) + 1) % AVATAR_OPTIONS.length;
            state.players[pKey].avatar = AVATAR_OPTIONS[nextIdx];
            commitStateChange(false);
          } else if (action === 'edit-player') {
            editingPlayerKey = actionEl.getAttribute('data-player');
            const pObj = state.players[editingPlayerKey];
            editingPlayerAvatar = pObj.avatar;
            document.getElementById('edit-player-modal-title').textContent =
              editingPlayerKey === 'p1' ? 'Editar Jogador 1' : 'Editar Jogador 2';
            document.getElementById('edit-player-name-input').value = pObj.name;
            renderAvatarPicker();
            openModal('modal-edit-player');
          } else if (action === 'claim-task') {
            const taskId = actionEl.getAttribute('data-task-id');
            const pKey = actionEl.getAttribute('data-player');
            const task = state.tasks.find((t) => t.id === taskId);
            if (!task) return;

            if (pKey === 'p1') task.p1Count = (task.p1Count || 0) + 1;
            else task.p2Count = (task.p2Count || 0) + 1;

            const isHabit = task.category === 'personal';
            state.logs.unshift({
              id: 'log-' + Date.now(),
              player: pKey,
              category: task.category,
              title: isHabit ? `${task.title} (+1)` : task.title,
              points: task.points,
              km: 0,
              minutes: 0,
              timeLabel: formatCurrentTimeLabel(),
            });

            // Animação pop no placar
            const scoreEl = document.getElementById(`${pKey}-score`);
            if (scoreEl) {
              scoreEl.classList.remove('score-pop');
              void scoreEl.offsetWidth;
              scoreEl.classList.add('score-pop');
            }

            commitStateChange(false);
            showToast(`${state.players[pKey].avatar} ${state.players[pKey].name} ganhou +${task.points} pts!`);
          } else if (action === 'delete-task') {
            const taskId = actionEl.getAttribute('data-task-id');
            state.tasks = state.tasks.filter((t) => t.id !== taskId);
            commitStateChange(false);
            showToast('Item removido da lista.');
          } else if (action === 'undo-log') {
            const logId = actionEl.getAttribute('data-log-id');
            state.logs = state.logs.filter((l) => l.id !== logId);
            commitStateChange(false);
            showToast('Registo anulado e pontos ajustados.');
          }
        });

        // Submeter formulário de edição de perfil
        document.getElementById('form-edit-player').addEventListener('submit', (e) => {
          e.preventDefault();
          const newName = document.getElementById('edit-player-name-input').value.trim();
          if (newName) {
            state.players[editingPlayerKey].name = newName;
            state.players[editingPlayerKey].avatar = editingPlayerAvatar;
            commitStateChange(false);
            showToast('Perfil atualizado!');
          }
          closeModal('modal-edit-player');
        });

        // Tabs de filtro por categoria
        document.querySelectorAll('.cat-tab-btn').forEach((btn) => {
          btn.addEventListener('click', () => {
            activeCategoryTab = btn.getAttribute('data-cat-tab');

            document.querySelectorAll('.cat-tab-btn').forEach((b) => {
              const isSelected = b.getAttribute('data-cat-tab') === activeCategoryTab;
              b.className = isSelected
                ? 'cat-tab-btn px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold bg-[#1E1D1B] text-white transition-all cursor-pointer'
                : 'cat-tab-btn px-3.5 py-2 rounded-xl text-xs sm:text-sm font-semibold text-[#68645E] hover:text-[#1E1D1B] hover:bg-[#F8F7F4] transition-all cursor-pointer';
            });

            const grid = document.getElementById('categories-grid');
            grid.className =
              activeCategoryTab === 'all'
                ? 'grid grid-cols-1 lg:grid-cols-2 gap-6'
                : 'grid grid-cols-1 max-w-2xl mx-auto gap-6';

            document.querySelectorAll('[data-category-panel]').forEach((panel) => {
              const cat = panel.getAttribute('data-category-panel');
              panel.classList.toggle('hidden', activeCategoryTab !== 'all' && activeCategoryTab !== cat);
            });
          });
        });

        // Seletor de Jogador no formulário de Corrida
        document.querySelectorAll('.run-player-btn').forEach((btn) => {
          btn.addEventListener('click', () => {
            selectedRunPlayer = btn.getAttribute('data-run-player');
            const submitBtn = document.getElementById('btn-submit-run');

            document.querySelectorAll('.run-player-btn').forEach((b) => {
              const p = b.getAttribute('data-run-player');
              if (p === selectedRunPlayer && p === 'p1') {
                b.className = 'run-player-btn py-2 px-3 rounded-xl border-2 border-[#E85D3F] bg-[#FDF0ED] text-[#E85D3F] text-xs font-bold flex items-center justify-center gap-1.5 transition-all cursor-pointer';
              } else if (p === selectedRunPlayer && p === 'p2') {
                b.className = 'run-player-btn py-2 px-3 rounded-xl border-2 border-[#16796F] bg-[#EAF5F3] text-[#16796F] text-xs font-bold flex items-center justify-center gap-1.5 transition-all cursor-pointer';
              } else {
                b.className = 'run-player-btn py-2 px-3 rounded-xl border border-[#E7E5E0] bg-white text-[#68645E] text-xs font-semibold flex items-center justify-center gap-1.5 transition-all cursor-pointer';
              }
            });

            submitBtn.style.backgroundColor = selectedRunPlayer === 'p1' ? '#E85D3F' : '#16796F';
          });
        });

        document.getElementById('run-km').addEventListener('input', updateRunPreviewUI);
        document.getElementById('run-mins').addEventListener('input', updateRunPreviewUI);

        document.getElementById('form-log-run').addEventListener('submit', (e) => {
          e.preventDefault();
          const type = document.getElementById('run-type').value;
          const km = Math.max(0, parseFloat(document.getElementById('run-km').value) || 0);
          const mins = Math.max(1, parseInt(document.getElementById('run-mins').value, 10) || 1);
          const pts = calculateRunPoints();
          const pObj = state.players[selectedRunPlayer];

          const desc = km > 0 ? `${type} — ${km.toFixed(1)} km (${mins} min)` : `${type} (${mins} min)`;

          state.logs.unshift({
            id: 'log-' + Date.now(),
            player: selectedRunPlayer,
            category: 'running',
            title: desc,
            points: pts,
            km: km,
            minutes: mins,
            timeLabel: formatCurrentTimeLabel(),
          });

          commitStateChange(true);
          showToast(`🏃 ${pObj.name} registou ${desc} (+${pts} pts)!`);
        });

        // Controlos do Cronómetro de Foco
        document.querySelectorAll('.timer-player-btn').forEach((btn) => {
          btn.addEventListener('click', () => {
            timerState.player = btn.getAttribute('data-timer-player');
            document.querySelectorAll('.timer-player-btn').forEach((b) => {
              const p = b.getAttribute('data-timer-player');
              if (p === timerState.player && p === 'p1') {
                b.className = 'timer-player-btn px-2.5 py-1 rounded-lg text-xs font-bold bg-[#FDF0ED] text-[#E85D3F] transition-all cursor-pointer';
              } else if (p === timerState.player && p === 'p2') {
                b.className = 'timer-player-btn px-2.5 py-1 rounded-lg text-xs font-bold bg-[#EAF5F3] text-[#16796F] transition-all cursor-pointer';
              } else {
                b.className = 'timer-player-btn px-2.5 py-1 rounded-lg text-xs font-semibold text-[#68645E] transition-all cursor-pointer';
              }
            });
          });
        });

        document.querySelectorAll('.timer-preset-btn').forEach((btn) => {
          btn.addEventListener('click', () => {
            timerState.presetMinutes = Number(btn.getAttribute('data-timer-preset'));
            document.querySelectorAll('.timer-preset-btn').forEach((b) => {
              const active = Number(b.getAttribute('data-timer-preset')) === timerState.presetMinutes;
              b.className = active
                ? 'timer-preset-btn py-1.5 px-2 rounded-xl bg-[#1E1D1B] text-white text-xs font-semibold transition-all cursor-pointer'
                : 'timer-preset-btn py-1.5 px-2 rounded-xl bg-white border border-[#E7E5E0] text-[#68645E] hover:text-[#1E1D1B] text-xs font-semibold transition-all cursor-pointer';
            });
            resetFocusTimer();
          });
        });

        document.getElementById('btn-timer-toggle').addEventListener('click', () => {
          if (timerState.running) {
            stopFocusTimer();
          } else {
            timerState.running = true;
            updateTimerUI();
            timerState.intervalId = setInterval(() => {
              if (timerState.presetMinutes > 0) {
                if (timerState.secondsLeft > 0) {
                  timerState.secondsLeft -= 1;
                  updateTimerUI();
                } else {
                  completeFocusSession();
                }
              } else {
                timerState.elapsedFreeSeconds += 1;
                updateTimerUI();
              }
            }, 1000);
          }
        });

        document.getElementById('btn-timer-complete').addEventListener('click', completeFocusSession);
        document.getElementById('btn-timer-reset').addEventListener('click', resetFocusTimer);

        // Modal Nova Tarefa / Hábito
        const openNewItemModalWithCat = (cat = 'chores') => {
          newItemCategory = cat;
          document.getElementById('new-item-title').value = '';
          updateNewItemModalSelectors();
          openModal('modal-new-item');
        };

        document.getElementById('btn-open-new-modal').addEventListener('click', () => openNewItemModalWithCat('chores'));
        document.getElementById('btn-quick-add-inline').addEventListener('click', () => openNewItemModalWithCat('chores'));

        document.querySelectorAll('[data-open-cat]').forEach((btn) => {
          btn.addEventListener('click', () => openNewItemModalWithCat(btn.getAttribute('data-open-cat')));
        });

        document.querySelectorAll('.new-cat-btn').forEach((btn) => {
          btn.addEventListener('click', () => {
            newItemCategory = btn.getAttribute('data-new-cat');
            updateNewItemModalSelectors();
          });
        });

        document.querySelectorAll('.new-pts-btn').forEach((btn) => {
          btn.addEventListener('click', () => {
            newItemPoints = Number(btn.getAttribute('data-new-pts'));
            updateNewItemModalSelectors();
          });
        });

        document.getElementById('form-new-item').addEventListener('submit', (e) => {
          e.preventDefault();
          const title = document.getElementById('new-item-title').value.trim();
          if (!title) return;

          state.tasks.unshift({
            id: 'task-' + Date.now(),
            category: newItemCategory,
            title,
            points: newItemPoints,
            p1Count: 0,
            p2Count: 0,
          });

          commitStateChange(false);
          closeModal('modal-new-item');
          showToast(`"${title}" adicionado à lista!`);
        });

        // Botões "Já Fiz" no Modal Nova Tarefa
        const handleInstantLog = (pKey) => {
          const title = document.getElementById('new-item-title').value.trim();
          if (!title) {
            document.getElementById('new-item-title').focus();
            return;
          }

          state.logs.unshift({
            id: 'log-' + Date.now(),
            player: pKey,
            category: newItemCategory,
            title,
            points: newItemPoints,
            km: 0,
            minutes: 0,
            timeLabel: formatCurrentTimeLabel(),
          });

          commitStateChange(true);
          closeModal('modal-new-item');
          showToast(`${state.players[pKey].name} ganhou +${newItemPoints} pts por "${title}"!`);
        };

        document.getElementById('btn-instant-log-p1').addEventListener('click', () => handleInstantLog('p1'));
        document.getElementById('btn-instant-log-p2').addEventListener('click', () => handleInstantLog('p2'));

        // Encerrar Semana & Coroar Vencedor (confirmação segura em 2 cliques)
        let crownPending = false;
        let crownTimeout = null;
        const crownBtn = document.getElementById('btn-crown-week');

        crownBtn.addEventListener('click', () => {
          const totals = computeTotals();
          if (totals.p1.points === totals.p2.points) {
            showToast('Está empatado! Desempata antes de encerrar a semana.');
            return;
          }

          if (!crownPending) {
            crownPending = true;
            const leaderName = totals.p1.points > totals.p2.points ? state.players.p1.name : state.players.p2.name;
            crownBtn.innerHTML = `<span>👑 Coroar ${escapeHTML(leaderName)}?</span>`;
            crownTimeout = setTimeout(() => {
              crownPending = false;
              crownBtn.innerHTML = '<span>🏆 Encerrar Semana</span>';
            }, 3500);
          } else {
            clearTimeout(crownTimeout);
            crownPending = false;
            crownBtn.innerHTML = '<span>🏆 Encerrar Semana</span>';

            const p1Won = totals.p1.points > totals.p2.points;
            const winnerKey = p1Won ? 'p1' : 'p2';
            const winner = state.players[winnerKey];

            winner.wins = (winner.wins || 0) + 1;
            winner.streak = (winner.streak || 0) + 1;

            state.champions.unshift({
              id: 'champ-' + Date.now(),
              weekLabel: `Ronda #${(state.champions.length || 0) + 1}`,
              winnerName: winner.name,
              winnerAvatar: winner.avatar,
              winnerColor: p1Won ? '#E85D3F' : '#16796F',
              scoreSummary: `${Math.max(totals.p1.points, totals.p2.points)} vs ${Math.min(totals.p1.points, totals.p2.points)} pts`,
              bet: state.bet,
            });

            // Inicia nova ronda semanal mantendo tarefas e hábitos
            state.logs = [];
            state.tasks.forEach((t) => {
              t.p1Count = 0;
              t.p2Count = 0;
            });

            commitStateChange(true);
            showToast(`🏆 ${winner.name} foi coroado(a) campeão(ã) da semana!`);
          }
        });

        // Reiniciar Dados de Demonstração (confirmação segura em 2 cliques)
        let resetPending = false;
        let resetTimeout = null;
        const resetBtn = document.getElementById('btn-reset-demo');

        resetBtn.addEventListener('click', () => {
          if (!resetPending) {
            resetPending = true;
            resetBtn.textContent = 'Confirmar?';
            resetBtn.classList.add('text-red-600', 'bg-red-50');
            resetTimeout = setTimeout(() => {
              resetPending = false;
              resetBtn.textContent = 'Reiniciar';
              resetBtn.classList.remove('text-red-600', 'bg-red-50');
            }, 3500);
          } else {
            clearTimeout(resetTimeout);
            resetPending = false;
            resetBtn.textContent = 'Reiniciar';
            resetBtn.classList.remove('text-red-600', 'bg-red-50');

            state = createInitialState();
            commitStateChange(false);
            showToast('Arena reiniciada com dados iniciais!');
          }
        });

        // Modal Sincronizar a Dois
        document.getElementById('btn-open-sync').addEventListener('click', () => openModal('modal-sync'));
        document.getElementById('btn-sync-mobile').addEventListener('click', () => openModal('modal-sync'));

        document.getElementById('btn-copy-room-code').addEventListener('click', () => {
          copyToClipboardSafe(roomCode);
        });

        document.getElementById('btn-new-random-room').addEventListener('click', () => {
          const freshCode = 'DUELO-' + Math.floor(100 + Math.random() * 900);
          connectToRoom(freshCode, true);
        });

        document.getElementById('form-join-room').addEventListener('submit', (e) => {
          e.preventDefault();
          const code = document.getElementById('input-join-room').value.trim();
          if (!code) return;
          document.getElementById('input-join-room').value = '';
          connectToRoom(code, true);
          closeModal('modal-sync');
        });
      }

      function renderAvatarPicker() {
        const grid = document.getElementById('avatar-picker-grid');
        if (!grid) return;

        grid.innerHTML = AVATAR_OPTIONS.map((emoji) => {
          const selected = emoji === editingPlayerAvatar;
          return `
            <button
              type="button"
              data-pick-avatar="${emoji}"
              class="h-11 rounded-xl text-2xl flex items-center justify-center border transition-all cursor-pointer ${
                selected
                  ? 'border-2 border-[#1E1D1B] bg-[#FCF6E8] scale-105'
                  : 'border-[#E7E5E0] bg-[#F8F7F4] hover:bg-white'
              }"
            >
              ${emoji}
            </button>
          `;
        }).join('');

        grid.querySelectorAll('[data-pick-avatar]').forEach((btn) => {
          btn.addEventListener('click', () => {
            editingPlayerAvatar = btn.getAttribute('data-pick-avatar');
            renderAvatarPicker();
          });
        });
      }

      function updateNewItemModalSelectors() {
        document.querySelectorAll('.new-cat-btn').forEach((btn) => {
          const active = btn.getAttribute('data-new-cat') === newItemCategory;
          btn.className = active
            ? 'new-cat-btn py-2 px-3 rounded-xl border-2 border-[#1E1D1B] bg-[#F8F7F4] text-xs font-bold text-left flex items-center gap-2 cursor-pointer'
            : 'new-cat-btn py-2 px-3 rounded-xl border border-[#E7E5E0] text-xs font-semibold text-left flex items-center gap-2 cursor-pointer';
        });

        document.querySelectorAll('.new-pts-btn').forEach((btn) => {
          const active = Number(btn.getAttribute('data-new-pts')) === newItemPoints;
          btn.className = active
            ? 'new-pts-btn py-2 rounded-xl border-2 border-[#D99B26] bg-[#FCF6E8] font-display text-sm font-bold text-[#B37B12] cursor-pointer'
            : 'new-pts-btn py-2 rounded-xl border border-[#E7E5E0] font-display text-sm font-bold text-[#68645E] cursor-pointer';
        });
      }

      /* ----------------------------------------------------------------------
         9. INICIALIZAÇÃO IMEDIATA
         ---------------------------------------------------------------------- */
      window.addEventListener('DOMContentLoaded', () => {
        const totals = computeTotals();
        previousLeader =
          totals.p1.points > totals.p2.points
            ? 'p1'
            : totals.p2.points > totals.p1.points
            ? 'p2'
            : 'tie';

        bindEvents();
        updateRunPreviewUI();
        updateTimerUI();
        renderAll();
        initFirebaseSync();
      });
    })();
  </script>
</body>
</html>