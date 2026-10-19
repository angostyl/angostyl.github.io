<!DOCTYPE html>
<html lang="pt-PT">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gestão de Turnos da Barraca</title>
  <style>
    :root {
      --bg: #f8fafc;
      --surface: #ffffff;
      --border: #e2e8f0;
      --border-strong: #cbd5e1;
      --text: #0f172a;
      --text-muted: #475569;
      --primary: #2563eb;
      --primary-hover: #1d4ed8;
      --danger: #dc2626;
      --danger-hover: #b91c1c;
      --warning-bg: #fef3c7;
      --warning-border: #f59e0b;
      --warning-text: #92400e;

      /* Estados de Disponibilidade */
      --disp-bg: #dcfce7;
      --disp-text: #166534;
      --disp-border: #86efac;

      --pref-bg: #dbeafe;
      --pref-text: #1e40af;
      --pref-border: #93c5fd;

      --indisp-bg: #fee2e2;
      --indisp-text: #991b1b;
      --indisp-border: #fca5a5;

      /* Escala de Amizades */
      --amz-m2-bg: #fecaca;
      --amz-m2-text: #7f1d1d;
      --amz-m1-bg: #ffedd5;
      --amz-m1-text: #9a3412;
      --amz-0-bg: #f1f5f9;
      --amz-0-text: #334155;
      --amz-p1-bg: #dcfce7;
      --amz-p1-text: #166534;
      --amz-p2-bg: #bbf7d0;
      --amz-p2-text: #14532d;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      background-color: var(--bg);
      color: var(--text);
      line-height: 1.5;
      padding-bottom: 3rem;
    }

    /* Cabeçalho e Barra de Navegação */
    header {
      background-color: var(--surface);
      border-bottom: 1px solid var(--border);
      position: sticky;
      top: 0;
      z-index: 100;
      box-shadow: 0 1px 2px rgba(0, 0, 0, 0.03);
    }

    .header-top {
      max-width: 1200px;
      margin: 0 auto;
      padding: 0.85rem 1.25rem;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      gap: 1rem;
    }

    .app-title {
      font-size: 1.25rem;
      font-weight: 700;
      color: var(--text);
    }

    .data-actions {
      display: flex;
      gap: 0.5rem;
      align-items: center;
      flex-wrap: wrap;
    }

    nav {
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 1.25rem;
      display: flex;
      gap: 0.25rem;
      overflow-x: auto;
      border-top: 1px solid var(--border);
    }

    .nav-btn {
      background: none;
      border: none;
      border-bottom: 3px solid transparent;
      padding: 0.75rem 1.1rem;
      font-size: 0.95rem;
      font-weight: 600;
      color: var(--text-muted);
      cursor: pointer;
      white-space: nowrap;
      transition: color 0.15s, border-color 0.15s, background-color 0.15s;
    }

    .nav-btn:hover {
      color: var(--text);
      background-color: #f1f5f9;
    }

    .nav-btn.active {
      color: var(--primary);
      border-bottom-color: var(--primary);
    }

    /* Contentor Principal */
    main {
      max-width: 1200px;
      margin: 1.5rem auto;
      padding: 0 1.25rem;
    }

    .section {
      display: none;
    }

    .section.active {
      display: block;
    }

    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 1.25rem;
      margin-bottom: 1.25rem;
      box-shadow: 0 1px 2px rgba(0, 0, 0, 0.02);
    }

    .card-title {
      font-size: 1.1rem;
      font-weight: 700;
      margin-bottom: 0.35rem;
    }

    .card-desc {
      font-size: 0.9rem;
      color: var(--text-muted);
      margin-bottom: 1rem;
    }

    /* Formulários e Botões */
    .form-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(190px, 1fr));
      gap: 0.85rem;
      align-items: end;
    }

    .form-group {
      display: flex;
      flex-direction: column;
      gap: 0.35rem;
    }

    label {
      font-size: 0.85rem;
      font-weight: 600;
      color: var(--text);
    }

    input[type="text"],
    input[type="number"],
    input[type="date"],
    input[type="time"],
    select {
      width: 100%;
      padding: 0.5rem 0.65rem;
      border: 1px solid var(--border-strong);
      border-radius: 6px;
      font-size: 0.9rem;
      color: var(--text);
      background-color: #fff;
    }

    input:focus,
    select:focus {
      outline: 2px solid var(--primary);
      outline-offset: 1px;
    }

    .inline-num-input {
      width: 85px !important;
      padding: 0.35rem 0.5rem !important;
      font-weight: 700;
      text-align: center;
    }

    .btn {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 0.4rem;
      padding: 0.5rem 0.9rem;
      font-size: 0.9rem;
      font-weight: 600;
      border-radius: 6px;
      border: 1px solid transparent;
      cursor: pointer;
      transition: background-color 0.15s, border-color 0.15s;
      white-space: nowrap;
    }

    .btn-primary {
      background-color: var(--primary);
      color: #fff;
    }

    .btn-primary:hover {
      background-color: var(--primary-hover);
    }

    .btn-secondary {
      background-color: #fff;
      color: var(--text);
      border-color: var(--border-strong);
    }

    .btn-secondary:hover {
      background-color: #f1f5f9;
    }

    .btn-danger {
      background-color: var(--danger);
      color: #fff;
    }

    .btn-danger:hover {
      background-color: var(--danger-hover);
    }

    .btn-sm {
      padding: 0.3rem 0.6rem;
      font-size: 0.8rem;
    }

    /* Tabelas e Grelhas */
    .table-wrapper {
      overflow-x: auto;
      border: 1px solid var(--border);
      border-radius: 6px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.9rem;
      background-color: var(--surface);
    }

    th, td {
      padding: 0.6rem 0.75rem;
      border-bottom: 1px solid var(--border);
      border-right: 1px solid var(--border);
      text-align: left;
    }

    th:last-child, td:last-child {
      border-right: none;
    }

    tr:last-child td {
      border-bottom: none;
    }

    th {
      background-color: #f1f5f9;
      font-weight: 700;
      color: var(--text);
      white-space: nowrap;
    }

    .sticky-col {
      position: sticky;
      left: 0;
      background-color: var(--surface);
      z-index: 2;
      font-weight: 600;
      border-right: 2px solid var(--border-strong);
    }

    th.sticky-col {
      background-color: #e2e8f0;
      z-index: 3;
    }

    /* Passo 1: Seleção de Dias */
    .day-check-cell {
      text-align: center;
      min-width: 115px;
      padding: 0.45rem;
    }

    .day-toggle-label {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 0.45rem;
      padding: 0.35rem 0.65rem;
      border-radius: 6px;
      border: 1px solid var(--border-strong);
      cursor: pointer;
      font-size: 0.83rem;
      font-weight: 600;
      user-select: none;
      width: 100%;
      transition: all 0.15s;
    }

    .day-toggle-label.day-on {
      background-color: var(--disp-bg);
      color: var(--disp-text);
      border-color: var(--disp-border);
    }

    .day-toggle-label.day-off {
      background-color: #f8fafc;
      color: var(--text-muted);
      border-color: var(--border);
    }

    /* Passo 2: Horários por Pessoa e Dia Selecionado */
    .person-schedule-card {
      border: 1px solid var(--border-strong);
      border-radius: 8px;
      margin-bottom: 1rem;
      overflow: hidden;
      background-color: var(--surface);
    }

    .person-schedule-header {
      background-color: #f1f5f9;
      padding: 0.65rem 1rem;
      font-weight: 700;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 0.5rem;
      border-bottom: 1px solid var(--border);
    }

    .person-days-container {
      padding: 0.85rem 1rem;
      display: flex;
      flex-direction: column;
      gap: 0.85rem;
    }

    .selected-day-row {
      border: 1px solid var(--border);
      border-radius: 6px;
      padding: 0.75rem;
      background-color: #f8fafc;
    }

    .selected-day-title {
      font-weight: 700;
      font-size: 0.9rem;
      margin-bottom: 0.5rem;
      color: var(--text);
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 0.5rem;
    }

    .shifts-flex {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(210px, 1fr));
      gap: 0.6rem;
    }

    .shift-slot-box {
      background-color: var(--surface);
      border: 1px solid var(--border);
      border-radius: 6px;
      padding: 0.45rem 0.6rem;
      display: flex;
      flex-direction: column;
      gap: 0.3rem;
    }

    .shift-slot-time {
      font-size: 0.82rem;
      font-weight: 700;
      color: var(--text);
    }

    .disp-select {
      font-weight: 600;
      font-size: 0.82rem;
      padding: 0.35rem 0.45rem;
      border-radius: 5px;
      cursor: pointer;
    }

    .state-available {
      background-color: var(--disp-bg);
      color: var(--disp-text);
      border-color: var(--disp-border);
    }

    .state-preferred {
      background-color: var(--pref-bg);
      color: var(--pref-text);
      border-color: var(--pref-border);
    }

    .state-unavailable {
      background-color: var(--indisp-bg);
      color: var(--indisp-text);
      border-color: var(--indisp-border);
    }

    /* Células de Amizade */
    td.amz-cell {
      padding: 0.35rem;
      min-width: 110px;
      text-align: center;
    }

    td.amz-self {
      background-color: #e2e8f0;
      color: var(--text-muted);
      text-align: center;
      font-weight: 700;
    }

    .amz-select {
      font-weight: 700;
      font-size: 0.85rem;
      text-align: center;
      padding: 0.3rem 0.4rem;
      border-radius: 5px;
      cursor: pointer;
    }

    .amz-val--2 { background-color: var(--amz-m2-bg); color: var(--amz-m2-text); border-color: #f87171; }
    .amz-val--1 { background-color: var(--amz-m1-bg); color: var(--amz-m1-text); border-color: #fb923c; }
    .amz-val-0  { background-color: var(--amz-0-bg);  color: var(--amz-0-text);  border-color: #cbd5e1; }
    .amz-val-1  { background-color: var(--amz-p1-bg); color: var(--amz-p1-text); border-color: #86efac; }
    .amz-val-2  { background-color: var(--amz-p2-bg); color: var(--amz-p2-text); border-color: #4ade80; }

    /* Legendas e Avisos */
    .legend {
      display: flex;
      flex-wrap: wrap;
      gap: 0.75rem;
      margin-bottom: 1rem;
      font-size: 0.85rem;
    }

    .legend-item {
      display: inline-flex;
      align-items: center;
      gap: 0.4rem;
      padding: 0.25rem 0.6rem;
      border-radius: 5px;
      font-weight: 600;
      border: 1px solid transparent;
    }

    .alert {
      padding: 0.85rem 1rem;
      border-radius: 6px;
      margin-bottom: 1rem;
      font-size: 0.9rem;
    }

    .alert-warning {
      background-color: var(--warning-bg);
      border: 1px solid var(--warning-border);
      color: var(--warning-text);
    }

    .alert-warning ul {
      margin-top: 0.4rem;
      padding-left: 1.25rem;
    }

    .empty-state {
      text-align: center;
      padding: 1.5rem;
      color: var(--text-muted);
      font-size: 0.92rem;
    }

    .badge {
      display: inline-block;
      padding: 0.2rem 0.55rem;
      border-radius: 999px;
      font-size: 0.8rem;
      font-weight: 600;
      margin-right: 0.35rem;
      margin-bottom: 0.2rem;
    }

    .badge-person {
      background-color: #e0e7ff;
      color: #1e3a8a;
    }

    .badge-ok {
      background-color: var(--disp-bg);
      color: var(--disp-text);
    }

    .badge-incomplete {
      background-color: var(--indisp-bg);
      color: var(--indisp-text);
    }

    .day-block {
      margin-bottom: 1.25rem;
    }

    .day-header {
      background-color: #1e293b;
      color: #fff;
      padding: 0.55rem 0.85rem;
      font-weight: 700;
      border-radius: 6px 6px 0 0;
      font-size: 0.95rem;
    }

    .day-block .table-wrapper {
      border-top: none;
      border-radius: 0 0 6px 6px;
    }

    .actions-row {
      display: flex;
      gap: 0.5rem;
      flex-wrap: wrap;
      align-items: center;
    }
  </style>
</head>
<body>

  <header>
    <div class="header-top">
      <div class="app-title">Gestão de Horários da Barraca</div>
      <div class="data-actions">
        <button type="button" class="btn btn-secondary btn-sm" id="btn-export">Exportar Dados (.json)</button>
        <label for="input-import" class="btn btn-secondary btn-sm" style="margin:0; cursor:pointer;">Importar Dados (.json)</label>
        <input type="file" id="input-import" accept=".json" style="display:none;">
      </div>
    </div>
    <nav aria-label="Navegação principal">
      <button type="button" class="nav-btn active" data-target="sec-equipa">1. Equipa</button>
      <button type="button" class="nav-btn" data-target="sec-disponibilidades">2. Disponibilidades</button>
      <button type="button" class="nav-btn" data-target="sec-amizades">3. Amizades</button>
      <button type="button" class="nav-btn" data-target="sec-horarios">4. Horários</button>
    </nav>
  </header>

  <main>
    <!-- SECÇÃO 1: EQUIPA -->
    <section id="sec-equipa" class="section active">
      <div class="card">
        <h2 class="card-title" id="form-equipa-title">Adicionar Elemento à Equipa</h2>
        <p class="card-desc">Defina o nome, o total de turnos pretendidos no evento e o <strong>limite máximo de turnos por dia</strong> para cada pessoa.</p>
        <form id="form-equipa">
          <input type="hidden" id="pessoa-id-edit" value="">
          <div class="form-grid">
            <div class="form-group">
              <label for="pessoa-nome">Nome *</label>
              <input type="text" id="pessoa-nome" required placeholder="Ex.: João Silva">
            </div>
            <div class="form-group">
              <label for="pessoa-turnos">Turnos pretendidos (total) *</label>
              <input type="number" id="pessoa-turnos" min="0" max="100" value="3" required>
            </div>
            <div class="form-group">
              <label for="pessoa-max-dia">Limite máx. turnos por dia *</label>
              <input type="number" id="pessoa-max-dia" min="1" max="24" value="1" required>
            </div>
            <div class="form-group">
              <label for="pessoa-obs">Observações (opcional)</label>
              <input type="text" id="pessoa-obs" placeholder="Ex.: Prefere início da noite">
            </div>
            <div class="actions-row">
              <button type="submit" class="btn btn-primary" id="btn-salvar-pessoa">Adicionar Pessoa</button>
              <button type="button" class="btn btn-secondary" id="btn-cancelar-edicao" style="display:none;">Cancelar</button>
            </div>
          </div>
        </form>
      </div>

      <div class="card">
        <h2 class="card-title">Elementos da Equipa (<span id="count-equipa">0</span>)</h2>
        <p class="card-desc">Pode alterar o <strong>Limite Máx. Turnos / Dia</strong> ou os <strong>Turnos Pretendidos</strong> diretamente nas caixas numéricas da tabela abaixo.</p>
        <div class="table-wrapper" id="equipa-table-container"></div>
      </div>
    </section>

    <!-- SECÇÃO 2: DISPONIBILIDADES -->
    <section id="sec-disponibilidades" class="section">
      <div class="card">
        <h2 class="card-title">Definições de Datas e Turnos</h2>
        <p class="card-desc">Configure os dias do evento, o horário de funcionamento e a duração dos turnos.</p>
        <form id="form-definicoes">
          <div class="form-grid">
            <div class="form-group">
              <label for="cfg-data-inicio">Data de Início</label>
              <input type="date" id="cfg-data-inicio" required>
            </div>
            <div class="form-group">
              <label for="cfg-data-fim">Data de Fim</label>
              <input type="date" id="cfg-data-fim" required>
            </div>
            <div class="form-group">
              <label for="cfg-hora-inicio">Hora de Abertura</label>
              <input type="time" id="cfg-hora-inicio" required>
            </div>
            <div class="form-group">
              <label for="cfg-hora-fim">Hora de Fecho</label>
              <input type="time" id="cfg-hora-fim" required>
            </div>
            <div class="form-group">
              <label for="cfg-duracao">Duração do Turno (minutos)</label>
              <input type="number" id="cfg-duracao" min="15" max="720" step="15" required>
            </div>
            <div class="form-group">
              <label for="cfg-pessoas-turno">Pessoas por Turno</label>
              <input type="number" id="cfg-pessoas-turno" min="1" max="50" required>
            </div>
            <div class="actions-row">
              <button type="submit" class="btn btn-primary">Guardar Definições</button>
            </div>
          </div>
        </form>
      </div>

      <!-- PASSO 1: SELECIONAR OS DIAS -->
      <div class="card">
        <h2 class="card-title">Passo 1: Selecionar os Dias em que Cada Pessoa Pode Trabalhar</h2>
        <p class="card-desc">Assinale primeiro os dias em que cada pessoa está disponível. Só depois de assinalar o dia é que os horários desse dia aparecem no Passo 2 abaixo.</p>
        <div class="table-wrapper" id="dias-disponiveis-container"></div>
      </div>

      <!-- PASSO 2: HORÁRIOS DOS DIAS SELECIONADOS -->
      <div class="card">
        <h2 class="card-title">Passo 2: Disponibilidade de Horários (Apenas nos Dias Selecionados)</h2>
        <p class="card-desc">Para cada pessoa, configure os horários apenas para os dias que selecionou no Passo 1.</p>
        <div class="legend">
          <span class="legend-item state-available">Disponível</span>
          <span class="legend-item state-preferred">Preferencialmente disponível</span>
          <span class="legend-item state-unavailable">Indisponível</span>
        </div>
        <div class="form-grid" style="margin-bottom: 1rem; max-width: 360px;">
          <div class="form-group">
            <label for="select-filtro-disp-pessoa">Filtrar por Pessoa</label>
            <select id="select-filtro-disp-pessoa">
              <option value="todas">Mostrar todas as pessoas</option>
            </select>
          </div>
        </div>
        <div id="horarios-disponiveis-container"></div>
      </div>
    </section>

    <!-- SECÇÃO 3: AMIZADES -->
    <section id="sec-amizades" class="section">
      <div class="card">
        <h2 class="card-title">Matriz de Preferências de Trabalho (Amizades)</h2>
        <p class="card-desc">Registe a preferência da pessoa na <strong>linha</strong> em trabalhar com a pessoa na <strong>coluna</strong>. As preferências são independentes.</p>
        <div class="legend">
          <span class="legend-item amz-val--2">-2: Prefere muito evitar</span>
          <span class="legend-item amz-val--1">-1: Prefere evitar</span>
          <span class="legend-item amz-val-0">0: Indiferente</span>
          <span class="legend-item amz-val-1">+1: Gosta de trabalhar com</span>
          <span class="legend-item amz-val-2">+2: Prefere muito trabalhar com</span>
        </div>
        <div class="table-wrapper" id="amizades-container"></div>
      </div>
    </section>

    <!-- SECÇÃO 4: HORÁRIOS -->
    <section id="sec-horarios" class="section">
      <div class="card">
        <h2 class="card-title">Geração Automática de Horários</h2>
        <p class="card-desc">
          Regras aplicadas pelo gerador:
          <strong>1)</strong> Só escala pessoas nos dias e horários disponíveis;
          <strong>2)</strong> Dá <strong>prioridade a que cada pessoa disponível num dia tenha pelo menos 1 turno nesse dia</strong> antes de atribuir um 2.º turno no mesmo dia a alguém;
          <strong>3)</strong> Nunca ultrapassa o <strong>limite máximo de turnos por dia</strong> configurado para cada pessoa.
        </p>
        <div class="actions-row">
          <button type="button" class="btn btn-primary" id="btn-gerar-horario">Gerar Horários</button>
          <button type="button" class="btn btn-secondary" id="btn-regenerar-horario">Voltar a Gerar</button>
        </div>
      </div>

      <div id="avisos-horario"></div>

      <div class="card">
        <h2 class="card-title">Horários por Dia</h2>
        <div id="horarios-dia-container"></div>
      </div>

      <div class="card">
        <h2 class="card-title">Consultar Turnos por Pessoa</h2>
        <div class="form-grid" style="margin-bottom: 1rem; max-width: 360px;">
          <div class="form-group">
            <label for="select-consulta-pessoa">Selecionar Pessoa</label>
            <select id="select-consulta-pessoa">
              <option value="todas">Todas as pessoas</option>
            </select>
          </div>
        </div>
        <div id="horarios-pessoa-container"></div>
      </div>
    </section>
  </main>

  <script>
    (function () {
      "use strict";

      const STORAGE_KEY = "barraca_gestao_turnos_v2";
      const LEGACY_STORAGE_KEY = "barraca_gestao_turnos_v1";

      // Estado da aplicação
      let state = {
        equipa: [],
        definicoes: {
          dataInicio: "2026-05-03",
          dataFim: "2026-05-05",
          horaInicio: "21:00",
          horaFim: "03:00",
          duracaoMinutos: 120,
          pessoasPorTurno: 2
        },
        // diasDisponiveis[pessoaId][dataIso] = true | false (Passo 1)
        diasDisponiveis: {},
        // disponibilidades[pessoaId][turnoId] = "available" | "preferred" | "unavailable" (Passo 2)
        disponibilidades: {},
        // amizades[pessoaIdA][pessoaIdB] = -2 | -1 | 0 | 1 | 2
        amizades: {},
        // horarioGerado: { geradoEm: string, atribuicoes: { [turnoId]: [pessoaId, ...] }, avisos: [] }
        horarioGerado: null
      };

      // Utilitários
      function gerarId() {
        return "id_" + Date.now().toString(36) + "_" + Math.random().toString(36).substring(2, 8);
      }

      function escaparHtml(str) {
        if (!str) return "";
        return String(str)
          .replace(/&/g, "&amp;")
          .replace(/</g, "&lt;")
          .replace(/>/g, "&gt;")
          .replace(/"/g, "&quot;")
          .replace(/'/g, "&#039;");
      }

      function obterLimiteDiaPessoa(pessoa) {
        if (pessoa && typeof pessoa.maxTurnosDia !== "undefined" && pessoa.maxTurnosDia !== null && pessoa.maxTurnosDia !== "") {
          const val = parseInt(pessoa.maxTurnosDia, 10);
          if (!isNaN(val) && val >= 1) return val;
        }
        return 1;
      }

      function guardarEstado() {
        try {
          localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
        } catch (err) {
          console.error("Erro ao guardar no armazenamento local:", err);
        }
      }

      function carregarEstado() {
        try {
          let guardado = localStorage.getItem(STORAGE_KEY);
          if (!guardado) {
            guardado = localStorage.getItem(LEGACY_STORAGE_KEY);
          }
          if (guardado) {
            const parsed = JSON.parse(guardado);
            if (parsed && typeof parsed === "object") {
              if (parsed.definicoes && typeof parsed.definicoes === "object") {
                state.definicoes = Object.assign({}, state.definicoes, parsed.definicoes);
              }
              state.equipa = Array.isArray(parsed.equipa) ? parsed.equipa.map(function (p) {
                return Object.assign({}, p, {
                  maxTurnosDia: obterLimiteDiaPessoa(p)
                });
              }) : [];
              state.diasDisponiveis = parsed.diasDisponiveis && typeof parsed.diasDisponiveis === "object"
                ? parsed.diasDisponiveis
                : {};
              state.disponibilidades = parsed.disponibilidades && typeof parsed.disponibilidades === "object"
                ? parsed.disponibilidades
                : {};
              state.amizades = parsed.amizades && typeof parsed.amizades === "object"
                ? parsed.amizades
                : {};
              state.horarioGerado = parsed.horarioGerado || null;
            }
          }
        } catch (err) {
          console.error("Erro ao carregar dados do armazenamento local:", err);
        }
      }

      // Datas e Turnos
      function converterHoraParaMinutos(horaStr) {
        const partes = (horaStr || "00:00").split(":");
        return parseInt(partes[0], 10) * 60 + parseInt(partes[1], 10);
      }

      function formatarMinutosParaHora(totalMinutos) {
        const norm = ((totalMinutos % 1440) + 1440) % 1440;
        const h = Math.floor(norm / 60);
        const m = norm % 60;
        return String(h).padStart(2, "0") + ":" + String(m).padStart(2, "0");
      }

      function formatarDataPt(dataIso) {
        const partes = dataIso.split("-");
        if (partes.length !== 3) return dataIso;
        return partes[2] + "/" + partes[1] + "/" + partes[0];
      }

      function obterDiasConfigurados() {
        const cfg = state.definicoes;
        if (!cfg.dataInicio || !cfg.dataFim) return [];
        const dInicio = new Date(cfg.dataInicio + "T00:00:00");
        const dFim = new Date(cfg.dataFim + "T00:00:00");
        if (isNaN(dInicio.getTime()) || isNaN(dFim.getTime()) || dFim < dInicio) return [];

        const dias = [];
        const maxDias = 60;
        let contador = 0;
        for (let d = new Date(dInicio); d <= dFim && contador < maxDias; d.setDate(d.getDate() + 1), contador++) {
          const ano = d.getFullYear();
          const mes = String(d.getMonth() + 1).padStart(2, "0");
          const dia = String(d.getDate()).padStart(2, "0");
          const dataIso = ano + "-" + mes + "-" + dia;
          dias.push({
            dataIso: dataIso,
            dataFormatada: formatarDataPt(dataIso)
          });
        }
        return dias;
      }

      function obterTurnosConfigurados() {
        const cfg = state.definicoes;
        const dias = obterDiasConfigurados();
        if (dias.length === 0 || !cfg.horaInicio || !cfg.horaFim) return [];

        const minInicio = converterHoraParaMinutos(cfg.horaInicio);
        let minFim = converterHoraParaMinutos(cfg.horaFim);
        if (minFim <= minInicio) {
          minFim += 1440;
        }

        const duracao = Math.max(15, parseInt(cfg.duracaoMinutos, 10) || 60);
        const turnos = [];

        dias.forEach(function (dObj) {
          for (let t = minInicio; t < minFim; t += duracao) {
            const tFim = Math.min(t + duracao, minFim);
            const hIniStr = formatarMinutosParaHora(t);
            const hFimStr = formatarMinutosParaHora(tFim);
            const idTurno = dObj.dataIso + "_" + hIniStr + "-" + hFimStr;

            turnos.push({
              id: idTurno,
              dataIso: dObj.dataIso,
              dataFormatada: dObj.dataFormatada,
              horaInicio: hIniStr,
              horaFim: hFimStr,
              duracaoMin: tFim - t
            });
          }
        });
        return turnos;
      }

      // Gestão de Disponibilidade de Dia (Passo 1) e de Horário (Passo 2)
      function podeTrabalharNoDia(pessoaId, dataIso) {
        if (state.diasDisponiveis[pessoaId] && typeof state.diasDisponiveis[pessoaId][dataIso] === "boolean") {
          return state.diasDisponiveis[pessoaId][dataIso];
        }
        return false;
      }

      function definirDiaDisponivel(pessoaId, dataIso, ativo) {
        if (!state.diasDisponiveis[pessoaId]) {
          state.diasDisponiveis[pessoaId] = {};
        }
        state.diasDisponiveis[pessoaId][dataIso] = Boolean(ativo);

        // Se acabou de ativar o dia, garantir que os turnos desse dia ficam "available" por defeito
        if (ativo) {
          if (!state.disponibilidades[pessoaId]) {
            state.disponibilidades[pessoaId] = {};
          }
          const turnosDoDia = obterTurnosConfigurados().filter(function (t) {
            return t.dataIso === dataIso;
          });
          turnosDoDia.forEach(function (t) {
            if (!state.disponibilidades[pessoaId][t.id]) {
              state.disponibilidades[pessoaId][t.id] = "available";
            }
          });
        }
        guardarEstado();
      }

      function obterDisponibilidadeHorario(pessoaId, turno) {
        if (!podeTrabalharNoDia(pessoaId, turno.dataIso)) {
          return "unavailable";
        }
        if (state.disponibilidades[pessoaId] && state.disponibilidades[pessoaId][turno.id]) {
          return state.disponibilidades[pessoaId][turno.id];
        }
        return "available";
      }

      function definirDisponibilidadeHorario(pessoaId, turnoId, valor) {
        if (!state.disponibilidades[pessoaId]) {
          state.disponibilidades[pessoaId] = {};
        }
        state.disponibilidades[pessoaId][turnoId] = valor;
        guardarEstado();
      }

      function obterAmizade(idA, idB) {
        if (idA === idB) return 0;
        if (state.amizades[idA] && typeof state.amizades[idA][idB] === "number") {
          return state.amizades[idA][idB];
        }
        return 0;
      }

      function definirAmizade(idA, idB, valor) {
        if (!state.amizades[idA]) {
          state.amizades[idA] = {};
        }
        state.amizades[idA][idB] = parseInt(valor, 10) || 0;
        guardarEstado();
      }

      // =========================================================================
      // SECÇÃO 1: EQUIPA
      // =========================================================================
      const formEquipa = document.getElementById("form-equipa");
      const inputPessoaIdEdit = document.getElementById("pessoa-id-edit");
      const inputPessoaNome = document.getElementById("pessoa-nome");
      const inputPessoaTurnos = document.getElementById("pessoa-turnos");
      const inputPessoaMaxDia = document.getElementById("pessoa-max-dia");
      const inputPessoaObs = document.getElementById("pessoa-obs");
      const btnSalvarPessoa = document.getElementById("btn-salvar-pessoa");
      const btnCancelarEdicao = document.getElementById("btn-cancelar-edicao");
      const formEquipaTitle = document.getElementById("form-equipa-title");

      function renderizarEquipa() {
        const container = document.getElementById("equipa-table-container");
        const countSpan = document.getElementById("count-equipa");
        countSpan.textContent = state.equipa.length;

        if (state.equipa.length === 0) {
          container.innerHTML = '<div class="empty-state">Ainda não foram adicionadas pessoas à equipa. Utilize o formulário acima para adicionar elementos.</div>';
          return;
        }

        let html = `
          <table>
            <thead>
              <tr>
                <th>Nome</th>
                <th style="width: 190px;">Turnos Pretendidos (Evento)</th>
                <th style="width: 190px;">Limite Máx. Turnos / Dia</th>
                <th>Observações</th>
                <th style="width: 170px;">Ações</th>
              </tr>
            </thead>
            <tbody>
        `;

        state.equipa.forEach(function (p) {
          const limDia = obterLimiteDiaPessoa(p);
          html += `
            <tr>
              <td><strong>${escaparHtml(p.nome)}</strong></td>
              <td>
                <input type="number" class="inline-num-input" min="0" max="100" value="${Number(p.turnosPretendidos)}" data-inline-field="turnosPretendidos" data-id="${p.id}" aria-label="Turnos pretendidos de ${escaparHtml(p.nome)}">
              </td>
              <td>
                <input type="number" class="inline-num-input" min="1" max="24" value="${limDia}" data-inline-field="maxTurnosDia" data-id="${p.id}" aria-label="Limite máximo de turnos por dia de ${escaparHtml(p.nome)}">
              </td>
              <td>${p.observacoes ? escaparHtml(p.observacoes) : '<span style="color:var(--text-muted)">—</span>'}</td>
              <td>
                <div class="actions-row">
                  <button type="button" class="btn btn-secondary btn-sm" data-action="edit-pessoa" data-id="${p.id}">Editar</button>
                  <button type="button" class="btn btn-danger btn-sm" data-action="del-pessoa" data-id="${p.id}">Remover</button>
                </div>
              </td>
            </tr>
          `;
        });

        html += "</tbody></table>";
        container.innerHTML = html;
      }

      function limparFormularioEquipa() {
        inputPessoaIdEdit.value = "";
        inputPessoaNome.value = "";
        inputPessoaTurnos.value = "3";
        inputPessoaMaxDia.value = "1";
        inputPessoaObs.value = "";
        formEquipaTitle.textContent = "Adicionar Elemento à Equipa";
        btnSalvarPessoa.textContent = "Adicionar Pessoa";
        btnCancelarEdicao.style.display = "none";
      }

      formEquipa.addEventListener("submit", function (e) {
        e.preventDefault();
        const nome = inputPessoaNome.value.trim();
        const turnosPretendidos = Math.max(0, parseInt(inputPessoaTurnos.value, 10) || 0);
        const maxTurnosDia = Math.max(1, parseInt(inputPessoaMaxDia.value, 10) || 1);
        const observacoes = inputPessoaObs.value.trim();
        const editId = inputPessoaIdEdit.value;

        if (!nome) return;

        if (editId) {
          const pessoa = state.equipa.find(function (p) { return p.id === editId; });
          if (pessoa) {
            pessoa.nome = nome;
            pessoa.turnosPretendidos = turnosPretendidos;
            pessoa.maxTurnosDia = maxTurnosDia;
            pessoa.observacoes = observacoes;
          }
        } else {
          state.equipa.push({
            id: gerarId(),
            nome: nome,
            turnosPretendidos: turnosPretendidos,
            maxTurnosDia: maxTurnosDia,
            observacoes: observacoes
          });
        }

        guardarEstado();
        limparFormularioEquipa();
        atualizarTodasVistas();
      });

      btnCancelarEdicao.addEventListener("click", function () {
        limparFormularioEquipa();
      });

      // Edição inline de limites na tabela da equipa
      document.getElementById("equipa-table-container").addEventListener("change", function (e) {
        const input = e.target.closest("input[data-inline-field]");
        if (!input) return;
        const id = input.getAttribute("data-id");
        const field = input.getAttribute("data-inline-field");
        const pessoa = state.equipa.find(function (p) { return p.id === id; });
        if (!pessoa) return;

        if (field === "turnosPretendidos") {
          pessoa.turnosPretendidos = Math.max(0, parseInt(input.value, 10) || 0);
          input.value = pessoa.turnosPretendidos;
        } else if (field === "maxTurnosDia") {
          pessoa.maxTurnosDia = Math.max(1, parseInt(input.value, 10) || 1);
          input.value = pessoa.maxTurnosDia;
        }
        guardarEstado();
        renderizarHorarios();
      });

      document.getElementById("equipa-table-container").addEventListener("click", function (e) {
        const btn = e.target.closest("button[data-action]");
        if (!btn) return;
        const id = btn.getAttribute("data-id");
        const action = btn.getAttribute("data-action");

        if (action === "edit-pessoa") {
          const pessoa = state.equipa.find(function (p) { return p.id === id; });
          if (!pessoa) return;
          inputPessoaIdEdit.value = pessoa.id;
          inputPessoaNome.value = pessoa.nome;
          inputPessoaTurnos.value = pessoa.turnosPretendidos;
          inputPessoaMaxDia.value = obterLimiteDiaPessoa(pessoa);
          inputPessoaObs.value = pessoa.observacoes || "";
          formEquipaTitle.textContent = "Editar Elemento da Equipa";
          btnSalvarPessoa.textContent = "Guardar Alterações";
          btnCancelarEdicao.style.display = "inline-flex";
          inputPessoaNome.focus();
        } else if (action === "del-pessoa") {
          const pessoa = state.equipa.find(function (p) { return p.id === id; });
          if (!pessoa) return;
          if (!confirm('Remover "' + pessoa.nome + '" da equipa?')) return;

          state.equipa = state.equipa.filter(function (p) { return p.id !== id; });
          delete state.diasDisponiveis[id];
          delete state.disponibilidades[id];
          delete state.amizades[id];
          Object.keys(state.amizades).forEach(function (outroId) {
            delete state.amizades[outroId][id];
          });

          if (state.horarioGerado && state.horarioGerado.atribuicoes) {
            Object.keys(state.horarioGerado.atribuicoes).forEach(function (tId) {
              state.horarioGerado.atribuicoes[tId] = state.horarioGerado.atribuicoes[tId].filter(function (pId) {
                return pId !== id;
              });
            });
          }

          if (inputPessoaIdEdit.value === id) {
            limparFormularioEquipa();
          }

          guardarEstado();
          atualizarTodasVistas();
        }
      });

      // =========================================================================
      // SECÇÃO 2: DISPONIBILIDADES (EM 2 PASSOS)
      // =========================================================================
      const formDefinicoes = document.getElementById("form-definicoes");

      function preencherFormularioDefinicoes() {
        document.getElementById("cfg-data-inicio").value = state.definicoes.dataInicio;
        document.getElementById("cfg-data-fim").value = state.definicoes.dataFim;
        document.getElementById("cfg-hora-inicio").value = state.definicoes.horaInicio;
        document.getElementById("cfg-hora-fim").value = state.definicoes.horaFim;
        document.getElementById("cfg-duracao").value = state.definicoes.duracaoMinutos;
        document.getElementById("cfg-pessoas-turno").value = state.definicoes.pessoasPorTurno;
      }

      formDefinicoes.addEventListener("submit", function (e) {
        e.preventDefault();
        const dIni = document.getElementById("cfg-data-inicio").value;
        const dFim = document.getElementById("cfg-data-fim").value;
        if (dFim < dIni) {
          alert("A data de fim não pode ser anterior à data de início.");
          return;
        }

        state.definicoes.dataInicio = dIni;
        state.definicoes.dataFim = dFim;
        state.definicoes.horaInicio = document.getElementById("cfg-hora-inicio").value;
        state.definicoes.horaFim = document.getElementById("cfg-hora-fim").value;
        state.definicoes.duracaoMinutos = Math.max(15, parseInt(document.getElementById("cfg-duracao").value, 10) || 60);
        state.definicoes.pessoasPorTurno = Math.max(1, parseInt(document.getElementById("cfg-pessoas-turno").value, 10) || 2);

        guardarEstado();
        atualizarTodasVistas();
      });

      function renderizarDisponibilidades() {
        const diasContainer = document.getElementById("dias-disponiveis-container");
        const horariosContainer = document.getElementById("horarios-disponiveis-container");
        const selectFiltroPessoa = document.getElementById("select-filtro-disp-pessoa");

        const selecaoAtual = selectFiltroPessoa.value || "todas";
        let optionsHtml = '<option value="todas">Mostrar todas as pessoas</option>';
        state.equipa.forEach(function (p) {
          optionsHtml += `<option value="${p.id}" ${selecaoAtual === p.id ? "selected" : ""}>${escaparHtml(p.nome)}</option>`;
        });
        selectFiltroPessoa.innerHTML = optionsHtml;

        if (state.equipa.length === 0) {
          diasContainer.innerHTML = '<div class="empty-state">Adicione pessoas na secção "1. Equipa" primeiro.</div>';
          horariosContainer.innerHTML = '<div class="empty-state">Adicione pessoas na secção "1. Equipa" primeiro.</div>';
          return;
        }

        const dias = obterDiasConfigurados();
        const turnos = obterTurnosConfigurados();

        if (dias.length === 0 || turnos.length === 0) {
          diasContainer.innerHTML = '<div class="empty-state">Verifique as definições de datas e horários acima.</div>';
          horariosContainer.innerHTML = '<div class="empty-state">Verifique as definições de datas e horários acima.</div>';
          return;
        }

        // Renderizar Passo 1: Tabela de seleção de dias
        let htmlDias = "<table><thead><tr><th class='sticky-col'>Pessoa \\ Dia do Evento</th>";
        dias.forEach(function (d) {
          htmlDias += `<th style="text-align:center;">${escaparHtml(d.dataFormatada)}</th>`;
        });
        htmlDias += "<th style='text-align:center; width:170px;'>Seleção Rápida</th></tr></thead><tbody>";

        state.equipa.forEach(function (p) {
          htmlDias += `<tr><td class="sticky-col">${escaparHtml(p.nome)}</td>`;
          dias.forEach(function (d) {
            const ativo = podeTrabalharNoDia(p.id, d.dataIso);
            const cls = ativo ? "day-on" : "day-off";
            const texto = ativo ? "Pode ir" : "Não pode";
            htmlDias += `
              <td class="day-check-cell">
                <label class="day-toggle-label ${cls}">
                  <input type="checkbox" class="chk-dia-disp" data-pessoa="${p.id}" data-dia="${d.dataIso}" ${ativo ? "checked" : ""}>
                  <span>${texto}</span>
                </label>
              </td>
            `;
          });
          htmlDias += `
            <td style="text-align:center;">
              <div class="actions-row" style="justify-content:center;">
                <button type="button" class="btn btn-secondary btn-sm" data-quick-days="all" data-pessoa="${p.id}">Todos</button>
                <button type="button" class="btn btn-secondary btn-sm" data-quick-days="none" data-pessoa="${p.id}">Nenhum</button>
              </div>
            </td>
          `;
          htmlDias += "</tr>";
        });

        htmlDias += "</tbody></table>";
        diasContainer.innerHTML = htmlDias;

        // Renderizar Passo 2: Horários apenas dos dias selecionados
        renderizarHorariosDosDiasSelecionados(dias, turnos);
      }

      function renderizarHorariosDosDiasSelecionados(dias, turnos) {
        const horariosContainer = document.getElementById("horarios-disponiveis-container");
        const filtroPessoa = document.getElementById("select-filtro-disp-pessoa").value || "todas";

        const pessoasLista = (filtroPessoa === "todas")
          ? state.equipa
          : state.equipa.filter(function (p) { return p.id === filtroPessoa; });

        let htmlHorarios = "";

        pessoasLista.forEach(function (p) {
          const diasSelecionados = dias.filter(function (d) {
            return podeTrabalharNoDia(p.id, d.dataIso);
          });

          htmlHorarios += `
            <div class="person-schedule-card">
              <div class="person-schedule-header">
                <span>${escaparHtml(p.nome)} (Máx. ${obterLimiteDiaPessoa(p)} turno(s)/dia)</span>
                <span style="font-size:0.85rem; color:var(--text-muted);">${diasSelecionados.length} dia(s) selecionado(s)</span>
              </div>
              <div class="person-days-container">
          `;

          if (diasSelecionados.length === 0) {
            htmlHorarios += `
              <div style="color:var(--text-muted); font-size:0.88rem;">
                Nenhum dia selecionado no Passo 1 para <strong>${escaparHtml(p.nome)}</strong>. Assinale pelo menos um dia acima para escolher os horários.
              </div>
            `;
          } else {
            diasSelecionados.forEach(function (d) {
              const turnosDesseDia = turnos.filter(function (t) {
                return t.dataIso === d.dataIso;
              });

              htmlHorarios += `
                <div class="selected-day-row">
                  <div class="selected-day-title">
                    <span>Dia ${escaparHtml(d.dataFormatada)}</span>
                  </div>
                  <div class="shifts-flex">
              `;

              turnosDesseDia.forEach(function (t) {
                const val = obterDisponibilidadeHorario(p.id, t);
                htmlHorarios += `
                  <div class="shift-slot-box">
                    <div class="shift-slot-time">${escaparHtml(t.horaInicio)} - ${escaparHtml(t.horaFim)}</div>
                    <select class="disp-select state-${val}" data-pessoa="${p.id}" data-turno="${t.id}">
                      <option value="available" ${val === "available" ? "selected" : ""}>Disponível</option>
                      <option value="preferred" ${val === "preferred" ? "selected" : ""}>Preferencialmente disponível</option>
                      <option value="unavailable" ${val === "unavailable" ? "selected" : ""}>Indisponível</option>
                    </select>
                  </div>
                `;
              });

              htmlHorarios += `
                  </div>
                </div>
              `;
            });
          }

          htmlHorarios += `
              </div>
            </div>
          `;
        });

        horariosContainer.innerHTML = htmlHorarios;
      }

      // Eventos Passo 1 (Seleção de Dias)
      document.getElementById("dias-disponiveis-container").addEventListener("change", function (e) {
        const chk = e.target.closest("input.chk-dia-disp");
        if (!chk) return;
        const pId = chk.getAttribute("data-pessoa");
        const dataIso = chk.getAttribute("data-dia");
        definirDiaDisponivel(pId, dataIso, chk.checked);
        renderizarDisponibilidades();
      });

      document.getElementById("dias-disponiveis-container").addEventListener("click", function (e) {
        const btn = e.target.closest("button[data-quick-days]");
        if (!btn) return;
        const pId = btn.getAttribute("data-pessoa");
        const modo = btn.getAttribute("data-quick-days");
        const ativo = (modo === "all");
        const dias = obterDiasConfigurados();
        dias.forEach(function (d) {
          definirDiaDisponivel(pId, d.dataIso, ativo);
        });
        renderizarDisponibilidades();
      });

      // Eventos Passo 2 (Horários dentro dos dias selecionados)
      document.getElementById("horarios-disponiveis-container").addEventListener("change", function (e) {
        const sel = e.target.closest("select.disp-select");
        if (!sel) return;
        const pId = sel.getAttribute("data-pessoa");
        const tId = sel.getAttribute("data-turno");
        const val = sel.value;

        sel.className = "disp-select state-" + val;
        definirDisponibilidadeHorario(pId, tId, val);
      });

      document.getElementById("select-filtro-disp-pessoa").addEventListener("change", function () {
        const dias = obterDiasConfigurados();
        const turnos = obterTurnosConfigurados();
        renderizarHorariosDosDiasSelecionados(dias, turnos);
      });

      // =========================================================================
      // SECÇÃO 3: AMIZADES
      // =========================================================================
      function renderizarAmizades() {
        const container = document.getElementById("amizades-container");
        if (state.equipa.length < 2) {
          container.innerHTML = '<div class="empty-state">Adicione pelo menos 2 pessoas na secção "Equipa" para preencher a matriz de preferências.</div>';
          return;
        }

        let html = "<table><thead><tr><th class='sticky-col'>Pessoa \\ Em relação a</th>";
        state.equipa.forEach(function (pCol) {
          html += `<th style="text-align:center;">${escaparHtml(pCol.nome)}</th>`;
        });
        html += "</tr></thead><tbody>";

        const opcoes = [
          { v: -2, label: "-2 (Evitar muito)" },
          { v: -1, label: "-1 (Evitar)" },
          { v: 0,  label: "0 (Indiferente)" },
          { v: 1,  label: "+1 (Gosta)" },
          { v: 2,  label: "+2 (Prefere muito)" }
        ];

        state.equipa.forEach(function (pLinha) {
          html += `<tr><td class="sticky-col">${escaparHtml(pLinha.nome)}</td>`;
          state.equipa.forEach(function (pCol) {
            if (pLinha.id === pCol.id) {
              html += `<td class="amz-self">—</td>`;
            } else {
              const val = obterAmizade(pLinha.id, pCol.id);
              html += `<td class="amz-cell"><select class="amz-select amz-val-${val}" data-origem="${pLinha.id}" data-alvo="${pCol.id}">`;
              opcoes.forEach(function (op) {
                html += `<option value="${op.v}" ${val === op.v ? "selected" : ""}>${op.label}</option>`;
              });
              html += `</select></td>`;
            }
          });
          html += "</tr>";
        });

        html += "</tbody></table>";
        container.innerHTML = html;
      }

      document.getElementById("amizades-container").addEventListener("change", function (e) {
        const sel = e.target.closest("select.amz-select");
        if (!sel) return;
        const origem = sel.getAttribute("data-origem");
        const alvo = sel.getAttribute("data-alvo");
        const val = parseInt(sel.value, 10) || 0;

        sel.className = "amz-select amz-val-" + val;
        definirAmizade(origem, alvo, val);
      });

      // =========================================================================
      // SECÇÃO 4: GERAÇÃO E CONSULTA DE HORÁRIOS
      // =========================================================================
      function calcularAfinidadePar(idA, idB) {
        return obterAmizade(idA, idB) + obterAmizade(idB, idA);
      }

      function temPreferenciaNegativaPar(idA, idB) {
        return obterAmizade(idA, idB) < 0 || obterAmizade(idB, idA) < 0;
      }

      function gerarHorariosAutomaticos() {
        const turnos = obterTurnosConfigurados();
        const necessarios = Math.max(1, parseInt(state.definicoes.pessoasPorTurno, 10) || 2);

        if (state.equipa.length === 0 || turnos.length === 0) {
          alert("Certifique-se de que existem pessoas na equipa e turnos configurados antes de gerar o horário.");
          return;
        }

        const contagemAtribuidos = {};
        const contagemAtribuidosPorDia = {};
        const ultimoIndiceTurno = {};

        state.equipa.forEach(function (p) {
          contagemAtribuidos[p.id] = 0;
          contagemAtribuidosPorDia[p.id] = {};
          ultimoIndiceTurno[p.id] = -999;
        });

        const atribuicoes = {};
        turnos.forEach(function (t) {
          atribuicoes[t.id] = [];
        });

        // Contar quantos turnos cada pessoa tem disponíveis em cada dia (para priorizar quem tem menos opções no dia)
        const opcoesNoDiaPorPessoa = {};
        state.equipa.forEach(function (p) {
          opcoesNoDiaPorPessoa[p.id] = {};
          turnos.forEach(function (t) {
            const disp = obterDisponibilidadeHorario(p.id, t);
            if (disp === "available" || disp === "preferred") {
              opcoesNoDiaPorPessoa[p.id][t.dataIso] = (opcoesNoDiaPorPessoa[p.id][t.dataIso] || 0) + 1;
            }
          });
        });

        // Ordenar turnos pela escassez de candidatos disponíveis
        const turnosOrdenados = turnos.map(function (t, idxOriginal) {
          const dispCount = state.equipa.filter(function (p) {
            return obterDisponibilidadeHorario(p.id, t) !== "unavailable";
          }).length;
          return {
            turno: t,
            idxOriginal: idxOriginal,
            dispCount: dispCount,
            tieBreaker: Math.random()
          };
        }).sort(function (a, b) {
          if (a.dispCount !== b.dispCount) return a.dispCount - b.dispCount;
          return a.tieBreaker - b.tieBreaker;
        });

        const avisos = [];

        turnosOrdenados.forEach(function (item) {
          const t = item.turno;
          const idxTurno = item.idxOriginal;
          const escaladosNoTurno = atribuicoes[t.id];

          for (let vaga = 0; vaga < necessarios; vaga++) {
            // Filtrar candidatos estritamente elegíveis:
            // 1. Selecionaram este dia no Passo 1 e não estão indisponíveis neste horário no Passo 2
            // 2. Não estão já escalados neste mesmo turno
            // 3. Não atingiram o seu limite máximo de turnos por dia (maxTurnosDia)
            const candidatos = state.equipa.filter(function (p) {
              if (escaladosNoTurno.indexOf(p.id) !== -1) return false;
              const disp = obterDisponibilidadeHorario(p.id, t);
              if (disp !== "available" && disp !== "preferred") return false;

              const feitosNoDia = contagemAtribuidosPorDia[p.id][t.dataIso] || 0;
              const limiteDia = obterLimiteDiaPessoa(p);
              if (feitosNoDia >= limiteDia) return false;

              return true;
            });

            if (candidatos.length === 0) {
              break;
            }

            const candidatosPontuados = candidatos.map(function (cand) {
              const atribuidosTotal = contagemAtribuidos[cand.id];
              const feitosNoDia = contagemAtribuidosPorDia[cand.id][t.dataIso] || 0;
              const pretendidos = Number(cand.turnosPretendidos);
              const disp = obterDisponibilidadeHorario(cand.id, t);
              const opcoesDia = opcoesNoDiaPorPessoa[cand.id][t.dataIso] || 1;

              let pontuacao = 0;

              // PRIORIDADE N.º 1: Garantir que cada pessoa disponível no dia tem PELO MENOS 1 turno nesse dia
              // antes de qualquer pessoa receber o 2.º turno nesse mesmo dia.
              if (feitosNoDia === 0) {
                pontuacao += 10000;
                // Entre os que ainda têm 0 turnos hoje, dar prioridade a quem tem menos horários disponíveis hoje
                pontuacao += Math.max(0, (20 - opcoesDia) * 40);
              } else {
                // Penalizar fortemente quem já tem pelo menos 1 turno neste dia
                pontuacao -= (feitosNoDia * 3000);
              }

              // PRIORIDADE N.º 2: Equilíbrio face ao número total de turnos pretendidos no evento
              if (pretendidos > 0 && atribuidosTotal < pretendidos) {
                pontuacao += 1000;
              } else if (pretendidos === 0) {
                pontuacao -= 600;
              }
              const racioCarga = pretendidos > 0 ? (atribuidosTotal / pretendidos) : (atribuidosTotal + 1);
              const folgaMeta = pretendidos - atribuidosTotal;
              pontuacao += (folgaMeta * 120) - (racioCarga * 250) - (atribuidosTotal * 45);

              // Evitar turnos seguidos se houver alternativas
              if (Math.abs(idxTurno - ultimoIndiceTurno[cand.id]) === 1) {
                pontuacao -= 90;
              }

              // PRIORIDADE N.º 3: Preferencialmente disponível no horário
              if (disp === "preferred") {
                pontuacao += 160;
              }

              // PRIORIDADE N.º 4: Amizades
              let somaAmizade = 0;
              let temConflitoNegativo = false;

              if (escaladosNoTurno.length > 0) {
                escaladosNoTurno.forEach(function (colegaId) {
                  somaAmizade += calcularAfinidadePar(cand.id, colegaId);
                  if (temPreferenciaNegativaPar(cand.id, colegaId)) {
                    temConflitoNegativo = true;
                  }
                });
              } else {
                candidatos.forEach(function (outroCand) {
                  if (outroCand.id !== cand.id) {
                    const af = calcularAfinidadePar(cand.id, outroCand.id);
                    if (af > 0) somaAmizade += af * 0.25;
                  }
                });
              }

              pontuacao += (somaAmizade * 18);
              if (temConflitoNegativo) {
                pontuacao -= 75;
              }

              const desempate = Math.random() * 6;

              return {
                pessoa: cand,
                scoreTotal: pontuacao + desempate
              };
            });

            candidatosPontuados.sort(function (a, b) {
              return b.scoreTotal - a.scoreTotal;
            });

            const escolhido = candidatosPontuados[0].pessoa;
            escaladosNoTurno.push(escolhido.id);
            contagemAtribuidos[escolhido.id] += 1;
            contagemAtribuidosPorDia[escolhido.id][t.dataIso] = (contagemAtribuidosPorDia[escolhido.id][t.dataIso] || 0) + 1;
            ultimoIndiceTurno[escolhido.id] = idxTurno;
          }
        });

        turnos.forEach(function (t) {
          const totalEscalados = atribuicoes[t.id].length;
          if (totalEscalados < necessarios) {
            avisos.push(
              "Dia " + t.dataFormatada + " (" + t.horaInicio + " - " + t.horaFim + "): apenas " +
              totalEscalados + " de " + necessarios + " pessoa(s) atribuída(s) (verifique se há pessoas suficientes com este dia selecionado e abaixo do limite diário)."
            );
          }
        });

        state.horarioGerado = {
          geradoEm: new Date().toISOString(),
          necessariosPorTurno: necessarios,
          atribuicoes: atribuicoes,
          avisos: avisos
        };

        guardarEstado();
        renderizarHorarios();
      }

      function obterMapaPessoas() {
        const mapa = {};
        state.equipa.forEach(function (p) {
          mapa[p.id] = p;
        });
        return mapa;
      }

      function renderizarHorarios() {
        const avisosContainer = document.getElementById("avisos-horario");
        const diaContainer = document.getElementById("horarios-dia-container");
        const pessoaContainer = document.getElementById("horarios-pessoa-container");
        const selectPessoa = document.getElementById("select-consulta-pessoa");

        const selecaoAtual = selectPessoa.value || "todas";
        let optionsHtml = '<option value="todas">Todas as pessoas</option>';
        state.equipa.forEach(function (p) {
          optionsHtml += `<option value="${p.id}" ${selecaoAtual === p.id ? "selected" : ""}>${escaparHtml(p.nome)}</option>`;
        });
        selectPessoa.innerHTML = optionsHtml;

        if (!state.horarioGerado || !state.horarioGerado.atribuicoes) {
          avisosContainer.innerHTML = "";
          diaContainer.innerHTML = '<div class="empty-state">Ainda não foi gerado nenhum horário. Clique em "Gerar Horários" acima.</div>';
          pessoaContainer.innerHTML = '<div class="empty-state">Gere primeiro os horários para consultar os turnos atribuídos a cada pessoa.</div>';
          return;
        }

        const turnos = obterTurnosConfigurados();
        const mapaPessoas = obterMapaPessoas();
        const necessarios = state.definicoes.pessoasPorTurno;

        if (state.horarioGerado.avisos && state.horarioGerado.avisos.length > 0) {
          let avisoHtml = `
            <div class="alert alert-warning">
              <strong>Atenção — Turnos incompletos:</strong>
              <ul>
          `;
          state.horarioGerado.avisos.forEach(function (msg) {
            avisoHtml += `<li>${escaparHtml(msg)}</li>`;
          });
          avisoHtml += "</ul></div>";
          avisosContainer.innerHTML = avisoHtml;
        } else {
          avisosContainer.innerHTML = "";
        }

        const turnosPorDia = {};
        turnos.forEach(function (t) {
          if (!turnosPorDia[t.dataIso]) {
            turnosPorDia[t.dataIso] = {
              dataFormatada: t.dataFormatada,
              lista: []
            };
          }
          turnosPorDia[t.dataIso].lista.push(t);
        });

        const datasChave = Object.keys(turnosPorDia);
        if (datasChave.length === 0) {
          diaContainer.innerHTML = '<div class="empty-state">Não existem turnos configurados para apresentar.</div>';
        } else {
          let htmlDias = "";
          datasChave.forEach(function (dataIso) {
            const grupo = turnosPorDia[dataIso];
            htmlDias += `
              <div class="day-block">
                <div class="day-header">Dia ${escaparHtml(grupo.dataFormatada)}</div>
                <div class="table-wrapper">
                  <table>
                    <thead>
                      <tr>
                        <th style="width: 180px;">Hora de Início e Fim</th>
                        <th>Pessoas Escaladas</th>
                        <th style="width: 210px;">Pessoas Atribuídas</th>
                      </tr>
                    </thead>
                    <tbody>
            `;

            grupo.lista.forEach(function (t) {
              const idsEscalados = (state.horarioGerado.atribuicoes[t.id] || []).filter(function (id) {
                return Boolean(mapaPessoas[id]);
              });

              let badgesHtml = "";
              if (idsEscalados.length === 0) {
                badgesHtml = '<span style="color:var(--danger); font-weight:600;">Ninguém escalado</span>';
              } else {
                idsEscalados.forEach(function (id) {
                  badgesHtml += `<span class="badge badge-person">${escaparHtml(mapaPessoas[id].nome)}</span>`;
                });
              }

              const completo = idsEscalados.length >= necessarios;
              const statusClass = completo ? "badge-ok" : "badge-incomplete";
              const statusTexto = idsEscalados.length + " / " + necessarios + (completo ? " (Completo)" : " (Incompleto)");

              htmlDias += `
                <tr>
                  <td><strong>${escaparHtml(t.horaInicio)} - ${escaparHtml(t.horaFim)}</strong></td>
                  <td>${badgesHtml}</td>
                  <td><span class="badge ${statusClass}">${statusTexto}</span></td>
                </tr>
              `;
            });

            htmlDias += "</tbody></table></div></div>";
          });
          diaContainer.innerHTML = htmlDias;
        }

        renderizarConsultaPorPessoa(turnos, mapaPessoas);
      }

      function renderizarConsultaPorPessoa(turnos, mapaPessoas) {
        const pessoaContainer = document.getElementById("horarios-pessoa-container");
        const filtroPessoaId = document.getElementById("select-consulta-pessoa").value;

        if (!state.horarioGerado || !state.horarioGerado.atribuicoes || state.equipa.length === 0) {
          pessoaContainer.innerHTML = '<div class="empty-state">Sem dados de horário para apresentar.</div>';
          return;
        }

        const pessoasParaMostrar = (filtroPessoaId === "todas")
          ? state.equipa
          : state.equipa.filter(function (p) { return p.id === filtroPessoaId; });

        let html = `
          <div class="table-wrapper">
            <table>
              <thead>
                <tr>
                  <th style="width: 180px;">Pessoa</th>
                  <th style="width: 150px;">Total Atribuídos</th>
                  <th style="width: 150px;">Limite Máx. / Dia</th>
                  <th>Turnos Escalados</th>
                </tr>
              </thead>
              <tbody>
        `;

        pessoasParaMostrar.forEach(function (p) {
          const turnosDaPessoa = [];
          turnos.forEach(function (t) {
            const listaIds = state.horarioGerado.atribuicoes[t.id] || [];
            if (listaIds.indexOf(p.id) !== -1) {
              const colegas = listaIds
                .filter(function (cId) { return cId !== p.id && mapaPessoas[cId]; })
                .map(function (cId) { return mapaPessoas[cId].nome; });
              turnosDaPessoa.push({
                turno: t,
                colegas: colegas
              });
            }
          });

          let listaTurnosHtml = "";
          if (turnosDaPessoa.length === 0) {
            listaTurnosHtml = '<span style="color:var(--text-muted)">Nenhum turno atribuído</span>';
          } else {
            turnosDaPessoa.forEach(function (item) {
              const colegasStr = item.colegas.length > 0
                ? " (com " + item.colegas.map(escaparHtml).join(", ") + ")"
                : "";
              listaTurnosHtml += `
                <div style="margin-bottom: 0.25rem;">
                  <strong>${escaparHtml(item.turno.dataFormatada)}</strong>: ${escaparHtml(item.turno.horaInicio)} - ${escaparHtml(item.turno.horaFim)}${colegasStr}
                </div>
              `;
            });
          }

          html += `
            <tr>
              <td><strong>${escaparHtml(p.nome)}</strong></td>
              <td><strong>${turnosDaPessoa.length}</strong> / ${Number(p.turnosPretendidos)}</td>
              <td>Máx. <strong>${obterLimiteDiaPessoa(p)}</strong> / dia</td>
              <td>${listaTurnosHtml}</td>
            </tr>
          `;
        });

        html += "</tbody></table></div>";
        pessoaContainer.innerHTML = html;
      }

      document.getElementById("btn-gerar-horario").addEventListener("click", function () {
        gerarHorariosAutomaticos();
      });

      document.getElementById("btn-regenerar-horario").addEventListener("click", function () {
        gerarHorariosAutomaticos();
      });

      document.getElementById("select-consulta-pessoa").addEventListener("change", function () {
        const turnos = obterTurnosConfigurados();
        const mapaPessoas = obterMapaPessoas();
        renderizarConsultaPorPessoa(turnos, mapaPessoas);
      });

      // =========================================================================
      // NAVEGAÇÃO ENTRE SECÇÕES E EXPORTAÇÃO / IMPORTAÇÃO
      // =========================================================================
      const navButtons = document.querySelectorAll(".nav-btn");
      const sections = document.querySelectorAll(".section");

      navButtons.forEach(function (btn) {
        btn.addEventListener("click", function () {
          const targetId = btn.getAttribute("data-target");
          navButtons.forEach(function (b) { b.classList.remove("active"); });
          sections.forEach(function (s) { s.classList.remove("active"); });

          btn.classList.add("active");
          const sec = document.getElementById(targetId);
          if (sec) sec.classList.add("active");
        });
      });

      document.getElementById("btn-export").addEventListener("click", function () {
        const dataStr = JSON.stringify(state, null, 2);
        const blob = new Blob([dataStr], { type: "application/json" });
        const url = URL.createObjectURL(blob);
        const a = document.createElement("a");
        a.href = url;
        a.download = "dados_barraca_turnos.json";
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
        URL.revokeObjectURL(url);
      });

      document.getElementById("input-import").addEventListener("change", function (e) {
        const file = e.target.files && e.target.files[0];
        if (!file) return;
        const reader = new FileReader();
        reader.onload = function (evt) {
          try {
            const imported = JSON.parse(evt.target.result);
            if (!imported || typeof imported !== "object" || !Array.isArray(imported.equipa)) {
              alert("O ficheiro selecionado não tem um formato válido.");
              return;
            }
            if (imported.definicoes) {
              state.definicoes = Object.assign({}, state.definicoes, imported.definicoes);
            }
            state.equipa = imported.equipa.map(function (p) {
              return Object.assign({}, p, {
                maxTurnosDia: obterLimiteDiaPessoa(p)
              });
            });
            state.diasDisponiveis = imported.diasDisponiveis || {};
            state.disponibilidades = imported.disponibilidades || {};
            state.amizades = imported.amizades || {};
            state.horarioGerado = imported.horarioGerado || null;

            guardarEstado();
            preencherFormularioDefinicoes();
            limparFormularioEquipa();
            atualizarTodasVistas();
            alert("Dados importados com sucesso.");
          } catch (err) {
            alert("Erro ao ler o ficheiro JSON.");
          } finally {
            e.target.value = "";
          }
        };
        reader.readAsText(file);
      });

      function atualizarTodasVistas() {
        renderizarEquipa();
        renderizarDisponibilidades();
        renderizarAmizades();
        renderizarHorarios();
      }

      // Inicialização
      carregarEstado();
      preencherFormularioDefinicoes();
      limparFormularioEquipa();
      atualizarTodasVistas();
    })();
  </script>
</body>
</html>
