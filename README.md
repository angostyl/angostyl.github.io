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

    /* Células de Disponibilidade */
    td.disp-cell {
      padding: 0.35rem;
      min-width: 155px;
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

    .shift-col-hdr {
      text-align: center;
      font-size: 0.8rem;
      line-height: 1.3;
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
        <p class="card-desc">Registe os elementos da barraca, o número total de turnos pretendidos durante o evento, o limite máximo de turnos por dia para cada pessoa e observações opcionais.</p>
        <form id="form-equipa">
          <input type="hidden" id="pessoa-id-edit" value="">
          <div class="form-grid">
            <div class="form-group">
              <label for="pessoa-nome">Nome *</label>
              <input type="text" id="pessoa-nome" required placeholder="Ex.: João Silva">
            </div>
            <div class="form-group">
              <label for="pessoa-turnos">Turnos pretendidos (evento) *</label>
              <input type="number" id="pessoa-turnos" min="0" max="100" value="3" required>
            </div>
            <div class="form-group">
              <label for="pessoa-max-dia">Máx. turnos por dia *</label>
              <input type="number" id="pessoa-max-dia" min="1" max="24" value="1" required>
            </div>
            <div class="form-group">
              <label for="pessoa-obs">Observações (opcional)</label>
              <input type="text" id="pessoa-obs" placeholder="Ex.: Prefere turnos ao início da noite">
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
        <div class="table-wrapper" id="equipa-table-container"></div>
      </div>
    </section>

    <!-- SECÇÃO 2: DISPONIBILIDADES -->
    <section id="sec-disponibilidades" class="section">
      <div class="card">
        <h2 class="card-title">Definições de Datas e Turnos</h2>
        <p class="card-desc">Configure o período do evento, o horário diário de funcionamento, a duração de cada turno e o limite padrão de turnos por dia por pessoa. Se a hora de fecho for igual ou inferior à de abertura, considera-se que o fecho ocorre na madrugada seguinte.</p>
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
            <div class="form-group">
              <label for="cfg-max-turnos-dia">Máx. Turnos / Dia por Pessoa (Geral)</label>
              <input type="number" id="cfg-max-turnos-dia" min="1" max="24" required>
            </div>
            <div class="actions-row">
              <button type="submit" class="btn btn-primary">Guardar e Atualizar Grelha</button>
              <button type="button" class="btn btn-secondary" id="btn-aplicar-max-todos" title="Aplica este limite diário a todas as pessoas já registadas na equipa">Aplicar Limite Diário a Todos</button>
            </div>
          </div>
        </form>
      </div>

      <div class="card">
        <h2 class="card-title">Grelha de Disponibilidades</h2>
        <p class="card-desc">Indique a disponibilidade de cada elemento para cada turno.</p>
        <div class="legend">
          <span class="legend-item state-available">Disponível</span>
          <span class="legend-item state-preferred">Preferencialmente disponível</span>
          <span class="legend-item state-unavailable">Indisponível</span>
        </div>
        <div class="table-wrapper" id="disponibilidades-container"></div>
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
        <p class="card-desc">Distribui a equipa pelos turnos respeitando disponibilidades, o limite máximo de turnos por dia de cada pessoa, equilíbrio de carga e preferências de amizade.</p>
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

      const STORAGE_KEY = "barraca_gestao_turnos_v1";

      // Estado da aplicação
      let state = {
        equipa: [],
        definicoes: {
          dataInicio: "2026-05-03",
          dataFim: "2026-05-05",
          horaInicio: "21:00",
          horaFim: "03:00",
          duracaoMinutos: 120,
          pessoasPorTurno: 2,
          maxTurnosDia: 1
        },
        // disponibilidades[pessoaId][turnoId] = "available" | "preferred" | "unavailable"
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
        const limPadrao = Math.max(1, parseInt(state.definicoes.maxTurnosDia, 10) || 1);
        if (pessoa && typeof pessoa.maxTurnosDia !== "undefined" && pessoa.maxTurnosDia !== null && pessoa.maxTurnosDia !== "") {
          const val = parseInt(pessoa.maxTurnosDia, 10);
          if (!isNaN(val) && val >= 1) return val;
        }
        return limPadrao;
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
          const guardado = localStorage.getItem(STORAGE_KEY);
          if (guardado) {
            const parsed = JSON.parse(guardado);
            if (parsed && typeof parsed === "object") {
              if (parsed.definicoes && typeof parsed.definicoes === "object") {
                state.definicoes = Object.assign({}, state.definicoes, parsed.definicoes);
              }
              state.definicoes.maxTurnosDia = Math.max(1, parseInt(state.definicoes.maxTurnosDia, 10) || 1);
              state.equipa = Array.isArray(parsed.equipa) ? parsed.equipa.map(function (p) {
                return Object.assign({}, p, {
                  maxTurnosDia: obterLimiteDiaPessoa(p)
                });
              }) : [];
              state.disponibilidades = parsed.disponibilidades && typeof parsed.disponibilidades === "object" ? parsed.disponibilidades : {};
              state.amizades = parsed.amizades && typeof parsed.amizades === "object" ? parsed.amizades : {};
              state.horarioGerado = parsed.horarioGerado || null;
            }
          }
        } catch (err) {
          console.error("Erro ao carregar dados do armazenamento local:", err);
        }
      }

      // Cálculo da lista de turnos com base nas definições
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

      function obterTurnosConfigurados() {
        const cfg = state.definicoes;
        if (!cfg.dataInicio || !cfg.dataFim || !cfg.horaInicio || !cfg.horaFim) return [];

        const dInicio = new Date(cfg.dataInicio + "T00:00:00");
        const dFim = new Date(cfg.dataFim + "T00:00:00");
        if (isNaN(dInicio.getTime()) || isNaN(dFim.getTime()) || dFim < dInicio) return [];

        const minInicio = converterHoraParaMinutos(cfg.horaInicio);
        let minFim = converterHoraParaMinutos(cfg.horaFim);
        // Se hora de fecho <= hora de abertura, atravessa a meia-noite
        if (minFim <= minInicio) {
          minFim += 1440;
        }

        const duracao = Math.max(15, parseInt(cfg.duracaoMinutos, 10) || 60);
        const turnos = [];
        const maxDias = 60; // Proteção contra intervalos excessivos
        let contadorDias = 0;

        for (let d = new Date(dInicio); d <= dFim && contadorDias < maxDias; d.setDate(d.getDate() + 1), contadorDias++) {
          const ano = d.getFullYear();
          const mes = String(d.getMonth() + 1).padStart(2, "0");
          const dia = String(d.getDate()).padStart(2, "0");
          const dataIso = ano + "-" + mes + "-" + dia;

          for (let t = minInicio; t < minFim; t += duracao) {
            const tFim = Math.min(t + duracao, minFim);
            const hIniStr = formatarMinutosParaHora(t);
            const hFimStr = formatarMinutosParaHora(tFim);
            const idTurno = dataIso + "_" + hIniStr + "-" + hFimStr;

            turnos.push({
              id: idTurno,
              dataIso: dataIso,
              dataFormatada: formatarDataPt(dataIso),
              horaInicio: hIniStr,
              horaFim: hFimStr,
              duracaoMin: tFim - t
            });
          }
        }
        return turnos;
      }

      function obterDisponibilidade(pessoaId, turnoId) {
        if (state.disponibilidades[pessoaId] && state.disponibilidades[pessoaId][turnoId]) {
          return state.disponibilidades[pessoaId][turnoId];
        }
        return "available"; // Estado predefinido: Disponível
      }

      function definirDisponibilidade(pessoaId, turnoId, valor) {
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
                <th>Turnos Pretendidos</th>
                <th>Máx. Turnos / Dia</th>
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
              <td>${Number(p.turnosPretendidos)}</td>
              <td>${limDia}</td>
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
        inputPessoaMaxDia.value = String(Math.max(1, parseInt(state.definicoes.maxTurnosDia, 10) || 1));
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
      // SECÇÃO 2: DISPONIBILIDADES E DEFINIÇÕES
      // =========================================================================
      const formDefinicoes = document.getElementById("form-definicoes");

      function preencherFormularioDefinicoes() {
        document.getElementById("cfg-data-inicio").value = state.definicoes.dataInicio;
        document.getElementById("cfg-data-fim").value = state.definicoes.dataFim;
        document.getElementById("cfg-hora-inicio").value = state.definicoes.horaInicio;
        document.getElementById("cfg-hora-fim").value = state.definicoes.horaFim;
        document.getElementById("cfg-duracao").value = state.definicoes.duracaoMinutos;
        document.getElementById("cfg-pessoas-turno").value = state.definicoes.pessoasPorTurno;
        document.getElementById("cfg-max-turnos-dia").value = Math.max(1, parseInt(state.definicoes.maxTurnosDia, 10) || 1);
      }

      formDefinicoes.addEventListener("submit", function (e) {
        e.preventDefault();
        const dIni = document.getElementById("cfg-data-inicio").value;
        const dFim = document.getElementById("cfg-data-fim").value;
        if (dFim < dIni) {
          alert("A data de fim não pode ser anterior à data de início.");
          return;
        }

        const antigoMaxDia = Math.max(1, parseInt(state.definicoes.maxTurnosDia, 10) || 1);
        const novoMaxDia = Math.max(1, parseInt(document.getElementById("cfg-max-turnos-dia").value, 10) || 1);

        state.definicoes.dataInicio = dIni;
        state.definicoes.dataFim = dFim;
        state.definicoes.horaInicio = document.getElementById("cfg-hora-inicio").value;
        state.definicoes.horaFim = document.getElementById("cfg-hora-fim").value;
        state.definicoes.duracaoMinutos = Math.max(15, parseInt(document.getElementById("cfg-duracao").value, 10) || 60);
        state.definicoes.pessoasPorTurno = Math.max(1, parseInt(document.getElementById("cfg-pessoas-turno").value, 10) || 2);
        state.definicoes.maxTurnosDia = novoMaxDia;

        // Atualizar as pessoas que tinham o valor padrão anterior
        if (novoMaxDia !== antigoMaxDia) {
          state.equipa.forEach(function (p) {
            if (!p.maxTurnosDia || p.maxTurnosDia === antigoMaxDia) {
              p.maxTurnosDia = novoMaxDia;
            }
          });
        }

        if (!inputPessoaIdEdit.value) {
          inputPessoaMaxDia.value = String(novoMaxDia);
        }

        guardarEstado();
        atualizarTodasVistas();
      });

      document.getElementById("btn-aplicar-max-todos").addEventListener("click", function () {
        const novoMaxDia = Math.max(1, parseInt(document.getElementById("cfg-max-turnos-dia").value, 10) || 1);
        state.definicoes.maxTurnosDia = novoMaxDia;
        state.equipa.forEach(function (p) {
          p.maxTurnosDia = novoMaxDia;
        });
        if (!inputPessoaIdEdit.value) {
          inputPessoaMaxDia.value = String(novoMaxDia);
        }
        guardarEstado();
        atualizarTodasVistas();
        alert("O limite máximo de " + novoMaxDia + " turno(s) por dia foi aplicado a todos os elementos da equipa.");
      });

      function renderizarDisponibilidades() {
        const container = document.getElementById("disponibilidades-container");
        if (state.equipa.length === 0) {
          container.innerHTML = '<div class="empty-state">Adicione pessoas na secção "Equipa" para configurar as respetivas disponibilidades.</div>';
          return;
        }

        const turnos = obterTurnosConfigurados();
        if (turnos.length === 0) {
          container.innerHTML = '<div class="empty-state">Verifique as definições de datas e horários acima para gerar as colunas de turnos.</div>';
          return;
        }

        let html = "<table><thead><tr><th class='sticky-col'>Pessoa \\ Turno</th>";
        turnos.forEach(function (t) {
          html += `
            <th class="shift-col-hdr">
              <div>${escaparHtml(t.dataFormatada)}</div>
              <div>${escaparHtml(t.horaInicio)} - ${escaparHtml(t.horaFim)}</div>
            </th>
          `;
        });
        html += "</tr></thead><tbody>";

        state.equipa.forEach(function (p) {
          html += `<tr><td class="sticky-col">${escaparHtml(p.nome)}</td>`;
          turnos.forEach(function (t) {
            const val = obterDisponibilidade(p.id, t.id);
            html += `
              <td class="disp-cell">
                <select class="disp-select state-${val}" data-pessoa="${p.id}" data-turno="${t.id}" aria-label="Disponibilidade de ${escaparHtml(p.nome)} em ${t.dataFormatada} ${t.horaInicio}">
                  <option value="available" ${val === "available" ? "selected" : ""}>Disponível</option>
                  <option value="preferred" ${val === "preferred" ? "selected" : ""}>Pref. disponível</option>
                  <option value="unavailable" ${val === "unavailable" ? "selected" : ""}>Indisponível</option>
                </select>
              </td>
            `;
          });
          html += "</tr>";
        });

        html += "</tbody></table>";
        container.innerHTML = html;
      }

      document.getElementById("disponibilidades-container").addEventListener("change", function (e) {
        const sel = e.target.closest("select.disp-select");
        if (!sel) return;
        const pId = sel.getAttribute("data-pessoa");
        const tId = sel.getAttribute("data-turno");
        const val = sel.value;

        sel.className = "disp-select state-" + val;
        definirDisponibilidade(pId, tId, val);
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
              html += `<td class="amz-cell"><select class="amz-select amz-val-${val}" data-origem="${pLinha.id}" data-alvo="${pCol.id}" aria-label="Preferência de ${escaparHtml(pLinha.nome)} em relação a ${escaparHtml(pCol.nome)}">`;
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

        // Contagem de turnos atribuídos por pessoa (no total e por cada dia)
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

        // Ordenar turnos pela escassez de pessoas disponíveis (turnos mais restritos primeiro),
        // mantendo um ligeiro fator aleatório de desempate para permitir "Voltar a gerar"
        const turnosOrdenados = turnos.map(function (t, idxOriginal) {
          const dispCount = state.equipa.filter(function (p) {
            return obterDisponibilidade(p.id, t.id) !== "unavailable";
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

        // Preencher cada turno
        turnosOrdenados.forEach(function (item) {
          const t = item.turno;
          const idxTurno = item.idxOriginal;
          const escaladosNoTurno = atribuicoes[t.id];

          for (let vaga = 0; vaga < necessarios; vaga++) {
            // Regras estritas de elegibilidade:
            // 1. Nunca atribuir alguém indisponível ou já escalado neste turno
            // 2. Respeitar o limite máximo de turnos por dia de cada pessoa
            const candidatos = state.equipa.filter(function (p) {
              if (escaladosNoTurno.indexOf(p.id) !== -1) return false;
              const disp = obterDisponibilidade(p.id, t.id);
              if (disp !== "available" && disp !== "preferred") return false;

              const feitosNoDia = contagemAtribuidosPorDia[p.id][t.dataIso] || 0;
              const limiteDia = obterLimiteDiaPessoa(p);
              if (feitosNoDia >= limiteDia) return false;

              return true;
            });

            if (candidatos.length === 0) {
              break; // Não há mais candidatos elegíveis para este turno
            }

            // Avaliar cada candidato respeitando a hierarquia de prioridades:
            // 1. Disponibilidade e distribuição equilibrada (prioridade máxima)
            // 2. Preferencialmente disponível
            // 3. Juntar preferências positivas e evitar negativas quando existam alternativas
            const candidatosPontuados = candidatos.map(function (cand) {
              const atribuidos = contagemAtribuidos[cand.id];
              const feitosNoDia = contagemAtribuidosPorDia[cand.id][t.dataIso] || 0;
              const pretendidos = Number(cand.turnosPretendidos);
              const disp = obterDisponibilidade(cand.id, t.id);

              // Equilíbrio de turnos:
              // Pessoas ainda abaixo da meta de turnos pretendidos têm prioridade clara
              const abaixoDaMeta = (pretendidos > 0 && atribuidos < pretendidos) ? 1 : 0;
              const racioCarga = pretendidos > 0 ? (atribuidos / pretendidos) : (atribuidos + 1);
              const folgaMeta = pretendidos - atribuidos;

              let pontuacaoEquilibrio = 0;
              if (abaixoDaMeta) {
                pontuacaoEquilibrio += 1000;
              } else if (pretendidos === 0) {
                pontuacaoEquilibrio -= 600;
              }
              pontuacaoEquilibrio += (folgaMeta * 120) - (racioCarga * 250) - (atribuidos * 45);

              // Dar preferência a quem ainda não fez nenhum turno nesse mesmo dia
              pontuacaoEquilibrio -= (feitosNoDia * 60);

              // Evitar turnos imediatamente consecutivos quando existam alternativas equilibradas
              if (Math.abs(idxTurno - ultimoIndiceTurno[cand.id]) === 1) {
                pontuacaoEquilibrio -= 90;
              }

              // Regra 4: Preferência a pessoas marcadas como "Preferencialmente disponível"
              const pontuacaoDisponibilidade = (disp === "preferred") ? 160 : 0;

              // Regras 5 e 6: Amizades com elementos já escalados neste turno
              let somaAmizade = 0;
              let temConflitoNegativo = false;

              if (escaladosNoTurno.length > 0) {
                escaladosNoTurno.forEach(function (colegaId) {
                  const afinidade = calcularAfinidadePar(cand.id, colegaId);
                  somaAmizade += afinidade;
                  if (temPreferenciaNegativaPar(cand.id, colegaId)) {
                    temConflitoNegativo = true;
                  }
                });
              } else {
                // Para a primeira vaga do turno, valorizar ligeiramente quem tem afinidade positiva
                // com outros candidatos disponíveis para este mesmo turno
                candidatos.forEach(function (outroCand) {
                  if (outroCand.id !== cand.id) {
                    const af = calcularAfinidadePar(cand.id, outroCand.id);
                    if (af > 0) somaAmizade += af * 0.25;
                  }
                });
              }

              let pontuacaoAmizade = somaAmizade * 18;
              if (temConflitoNegativo) {
                // Penalizar relações negativas para que alternativas sem conflito sejam preferidas
                pontuacaoAmizade -= 75;
              }

              // Desempate aleatório suave para o botão "Voltar a Gerar"
              const desempate = Math.random() * 6;

              return {
                pessoa: cand,
                scoreTotal: pontuacaoEquilibrio + pontuacaoDisponibilidade + pontuacaoAmizade + desempate
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

        // Verificar turnos incompletos pela ordem cronológica
        turnos.forEach(function (t) {
          const totalEscalados = atribuicoes[t.id].length;
          if (totalEscalados < necessarios) {
            avisos.push(
              "Dia " + t.dataFormatada + " (" + t.horaInicio + " - " + t.horaFim + "): apenas " +
              totalEscalados + " de " + necessarios + " pessoa(s) atribuída(s) (sem mais pessoas disponíveis ou dentro do limite diário de turnos)."
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

        // Atualizar opções do seletor de pessoas
        const selecaoAtual = selectPessoa.value || "todas";
        let optionsHtml = '<option value="todas">Todas as pessoas (Resumo e Turnos)</option>';
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

        // Mostrar avisos se existirem turnos incompletos
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

        // Agrupar turnos por dia
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
                badgesHtml = '<span style="color:var(--danger); font-weight:600;">Ninguém escalado (sem candidatos elegíveis)</span>';
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

        // Renderizar consulta por pessoa
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
                  <th style="width: 190px;">Pessoa</th>
                  <th style="width: 160px;">Atribuídos / Pretendidos</th>
                  <th style="width: 140px;">Máx. Turnos / Dia</th>
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
              <td>${obterLimiteDiaPessoa(p)} / dia</td>
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
            state.definicoes.maxTurnosDia = Math.max(1, parseInt(state.definicoes.maxTurnosDia, 10) || 1);
            state.equipa = imported.equipa.map(function (p) {
              return Object.assign({}, p, {
                maxTurnosDia: obterLimiteDiaPessoa(p)
              });
            });
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
