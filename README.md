<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Medicina UNCo · Malla + Planificador</title>

  <!-- Fuentes -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&family=Shadows+Into+Light&display=swap" rel="stylesheet">

  <style>
    :root{
      --aprobada:#4b6043;
      --habilitada:#a6b98b;
      --bloqueada:#E4f1d7;
      --cursando:#658354;
      --eliminar:#5c7c58;

      --card-radius:12px;
      --gap:12px;
      --shadow: 0 6px 18px rgba(24,24,24,0.08);

      --ink:#0f2522;
      --ink-soft:#203a34;
      --glass:rgba(255,255,255,.68);
      --glass-border:rgba(18,35,50,.10);
    }

    body{
      margin:0;
      position:relative;
      background:
        linear-gradient(180deg, rgba(255,255,255,.12), rgba(255,255,255,.3)),
        url('https://i.pinimg.com/736x/7a/82/12/7a82124299edd13937dd5b15432d3909.jpg');
      background-size: cover;
      background-attachment: fixed;
      background-position: center;
      padding: 20px;
      color: var(--ink);
      font-family: 'Poppins', sans-serif;
      font-weight: 400;
      min-height: 100vh;
    }

    .top-fade{
      position: fixed;
      inset: 0 0 auto 0;
      height: 160px;
      background: linear-gradient(
        to bottom,
        rgba(255,255,255,.92) 0%,
        rgba(255,255,255,.70) 55%,
        rgba(255,255,255,0) 100%
      );
      pointer-events: none;
      z-index: 0;
    }

    header{
      position: sticky;
      top: 10px;
      z-index: 2;
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
      flex-wrap:wrap;

      background: var(--glass);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      border:1px solid var(--glass-border);
      border-radius: 14px;
      padding: 12px 14px;
      box-shadow: var(--shadow);
    }

    h1{
      font-size: 2rem;
      margin: 0;
      font-family: 'Shadows Into Light', cursive;
      font-weight: 600;
      text-align: center;
      color: var(--ink-soft);
    }

    .view-toggle {
      display: flex;
      gap: 8px;
      margin-left: auto;
    }

    .view-toggle button {
      background: transparent;
      color: var(--ink);
      border: 1px solid rgba(18,35,50,0.18);
      padding: 8px 16px;
      border-radius: 30px;
      font-weight: 500;
    }

    .view-toggle button.active {
      background: var(--ink);
      color: #C7DDB5;
      border-color: var(--ink);
    }

    .controls{display:flex;gap:8px;align-items:center;flex-wrap:wrap}

    button{
      background: var(--ink);
      color:#C7DDB5;
      border:none;
      padding:10px 12px;
      border-radius:10px;
      cursor:pointer;
      box-shadow:var(--shadow);
      transition: all 0.2s ease;
    }
    button:hover{
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(24,24,24,0.12);
    }
    button.secondary{
      background:transparent;
      color:var(--ink);
      border:1px solid rgba(18,35,50,0.18);
    }
    button.eliminar{
      background: var(--eliminar);
      color: white;
      padding: 6px 10px;
      font-size: 0.85rem;
    }

    .small{font-size:0.85rem;color:var(--ink-soft);font-weight: 500}

    .legend{display:flex;gap:10px;flex-wrap:wrap;align-items:center}
    .legend .item{display:flex;gap:8px;align-items:center;font-size:.9rem}
    .swatch{width:16px;height:12px;border-radius:4px;display:inline-block;box-shadow: inset 0 0 0 1px rgba(0,0,0,.06)}

    /* --- Vistas --- */
    .view {
      display: none;
      margin-top: 18px;
    }
    .view.active {
      display: block;
    }

    /* --- Malla grid --- */
    .grid{display:grid;gap:var(--gap);}
    @media(min-width:1100px){.grid{grid-template-columns:repeat(4,1fr)}}
    @media(min-width:720px) and (max-width:1099px){.grid{grid-template-columns:repeat(2,1fr)}}
    @media(max-width:719px){.grid{grid-template-columns:1fr}}

    .year-col{
      background:rgba(255,255,255,0.85);
      padding:12px;
      border-radius:14px;
      box-shadow:var(--shadow);
      backdrop-filter: blur(2px);
      border:1px solid var(--glass-border);
    }
    .year-title{font-weight:700;margin:0 0 8px 0}

    .subject{
      padding:10px;border-radius:var(--card-radius);margin-bottom:8px;
      display:flex;align-items:center;justify-content:space-between;gap:8px;
      cursor:pointer;
      transition:transform .18s ease, box-shadow .18s ease, background-color .35s ease,color .35s ease;
      text-decoration:none;
      position: relative;
      overflow: hidden;
    }
    .subject:hover{
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(24,24,24,0.12);
    }
    .subject:active{transform:translateY(1px)}
    .subject.locked{background:var(--bloqueada);color:#073b30;}
    .subject.unlocked{background:var(--habilitada);color:#2a2119;}
    .subject.cursando{background:var(--cursando);color:#183b12;font-style:italic;}
    .subject.aprobada{background:var(--aprobada);color:#fff;font-style:italic;text-decoration:line-through}
    .subject .meta{font-size:0.75rem;opacity:0.85}

    .status-indicator {
      position: absolute;
      top: 0;
      right: 0;
      width: 8px;
      height: 100%;
      transition: all 0.3s ease;
    }
    .subject.aprobada .status-indicator { background: rgba(255,255,255,0.3); }
    .subject.cursando .status-indicator { background: rgba(255,255,255,0.5); }

    /* --- Planificador --- */
    .planificador-container {
      background: rgba(255,255,255,0.85);
      padding: 20px;
      border-radius: 14px;
      box-shadow: var(--shadow);
      backdrop-filter: blur(2px);
      border: 1px solid var(--glass-border);
      margin-top: 20px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 15px;
      background: rgba(255,255,255,0.2);
      border-radius: 10px;
      overflow: hidden;
      box-shadow: var(--shadow);
      backdrop-filter: blur(6px);
    }

    th {
      background: rgba(101,131,84,0.95);
      color: white;
      text-align: center;
      font-weight: 600;
    }

    th, td {
      padding: 12px;
      text-align: left;
      border: 1px solid #ddd;
    }

    tr:nth-child(even) {
      background-color: #f9f9f9;
    }

    .toast{
      position:fixed;left:50%;transform:translateX(-50%);bottom:24px;
      background:rgba(18,18,18,0.88);color:#fff;padding:8px 14px;border-radius:8px;display:none;
      z-index: 10;
      backdrop-filter: blur(4px);
    }

    .estado-materia {
      display: inline-block;
      padding: 4px 8px;
      border-radius: 4px;
      font-size: 0.8rem;
      margin-left: 10px;
      font-weight: bold;
    }

    .estado-aprobada { background: var(--aprobada); color: white; }
    .estado-cursando { background: var(--cursando); color: white; }
    .estado-habilitada { background: var(--habilitada); color: var(--ink); }
    .estado-bloqueada { background: var(--bloqueada); color: var(--ink); }
    
    .checkbox-container {
      display: inline-block;
      position: relative;
      cursor: pointer;
    }
    
    .checkbox-container input {
      position: absolute;
      opacity: 0;
      cursor: pointer;
      height: 0;
      width: 0;
    }
    
    .checkmark {
      display: inline-block;
      width: 20px;
      height: 20px;
      background-color: #fff;
      border: 2px solid #4B6043;
      border-radius: 50%;
      position: relative;
    }
    
    .checkbox-container input:checked ~ .checkmark {
      background-color: #4B6043;
    }
    
    .checkbox-container input:checked ~ .checkmark:after {
      content: "•";
      position: absolute;
      color: white;
      font-size: 14px;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
    
    .checkbox-container input:disabled ~ .checkmark {
      border-color: #ccc;
      background-color: #f5f5f5;
    }
    
    .checkbox-container input:disabled:checked ~ .checkmark {
      background-color: #ccc;
      border-color: #999;
    }

    .completado-icono {
      display: inline-block;
      width: 24px;
      height: 24px;
      margin-left: 10px;
      background-image: url('https://i.pinimg.com/236x/0f/ea/b4/0feab49f253f445a24e948e3cbb21d54.jpg');
      background-size: contain;
      background-repeat: no-repeat;
      background-position: center;
      animation: bounce 0.5s ease-in-out;
    }
    
    @keyframes bounce {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.2); }
    }

    .tema-row {
      position: relative;
    }
    
    .subtema-container {
      background: #f8f9fa;
      border-left: 4px solid var(--habilitada);
      margin: 5px 0;
      padding: 0;
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.3s ease, padding 0.3s ease;
    }
    
    .subtema-container.abierto {
      max-height: 500px;
      padding: 15px;
    }
    
    .subtema-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      padding: 8px 0;
      font-weight: 500;
    }
    
    .subtema-flecha {
      font-size: 12px;
      transition: transform 0.3s;
    }
    
    .subtema-container.abierto .subtema-flecha {
      transform: rotate(180deg);
    }
    
    .checklist-horizontal {
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
      margin-top: 10px;
    }
    
    .check-item-horizontal {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 5px 10px;
      background: white;
      border-radius: 20px;
      border: 1px solid #ddd;
    }
    
    .check-label-horizontal {
      font-size: 0.85rem;
      color: var(--ink-soft);
    }
    
    .check-item-horizontal.completado {
      background: var(--habilitada);
      border-color: var(--aprobada);
    }
    
    .check-item-horizontal.bloqueado {
      opacity: 0.5;
      cursor: not-allowed;
    }
    
    .acciones-tema {
      display: flex;
      gap: 8px;
      justify-content: flex-end;
    }
    
    .sin-temas {
      text-align: center;
      padding: 40px;
      color: var(--ink-soft);
      font-style: italic;
      background: white;
      border-radius: 10px;
      margin-top: 15px;
    }
    
    .progreso-tema {
      font-size: 0.8rem;
      color: var(--ink-soft);
      margin-top: 5px;
    }
    
    .agregar-subtema {
      margin-top: 10px;
    }
    
    .subtema-item {
      background: white;
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 10px;
      margin-bottom: 10px;
    }
    
    .subtema-item-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
    }
    
    .subtema-nombre {
      font-weight: 500;
    }
    
    .acciones-subtema {
      display: flex;
      gap: 5px;
    }
    
    .estadisticas {
      margin-top: 15px;
      padding: 15px;
      background: var(--glass);
      border-radius: 10px;
      border: 1px solid var(--glass-border);
    }
    
    .barra-progreso {
      width: 100%;
      height: 12px;
      background: #f0f0f0;
      border-radius: 6px;
      overflow: hidden;
      margin-top: 8px;
    }
    
    .progreso-llenado {
      height: 100%;
      background: var(--aprobada);
      transition: width 0.3s ease;
    }

    footer{margin-top:14px;font-size:0.9rem;color:var(--ink-soft);text-shadow: 0 1px 0 rgba(255,255,255,.3)}
  </style>
</head>
<body>
  <div class="top-fade"></div>

  <header>
    <div>
      <h1>Medicina (UNComa)</h1>
      <div class="small" id="viewDescription">Malla curricular · haz clic en una materia para cambiar su estado</div>
    </div>
    <div class="view-toggle">
      <button id="viewMallaBtn" class="active">Malla</button>
      <button id="viewPlanificadorBtn">Planificador</button>
    </div>
    <div class="controls" id="mallaControls">
      <div class="legend">
        <div class="item"><span class="swatch" style="background:var(--aprobada)"></span> Aprobada</div>
        <div class="item"><span class="swatch" style="background:var(--cursando)"></span> Cursando</div>
        <div class="item"><span class="swatch" style="background:var(--habilitada)"></span> Habilitada</div>
        <div class="item"><span class="swatch" style="background:var(--bloqueada)"></span> Bloqueada</div>
      </div>
      <button id="resetBtn" title="Reiniciar progreso de la malla">Reiniciar malla</button>
    </div>
  </header>

  <!-- Vista Malla -->
  <div id="mallaView" class="view active">
    <main id="mainGrid" class="grid"></main>
  </div>

  <!-- Vista Planificador -->
  <div id="planificadorView" class="view">
    <div class="planificador-container">
      <div style="margin-bottom: 15px;">
        <label for="materiaSelect">Seleccionar materia: </label>
        <select id="materiaSelect" style="padding: 8px; border-radius: 6px; border: 1px solid #ccc; width: 300px;">
          <option value="">-- Elegir materia --</option>
        </select>
        <div id="estadoMateria" style="margin-top: 8px;"></div>
      </div>
      
      <div id="planificadorContenido" style="display: none;">
        <div style="margin-bottom: 20px;">
          <button id="agregarTema">+ Agregar Tema</button>
          <button id="expandirTodo" class="secondary">Expandir Todo</button>
          <button id="colapsarTodo" class="secondary">Colapsar Todo</button>
          <button id="exportarJson" class="secondary">Exportar JSON</button>
          <button id="importarJson" class="secondary">Importar JSON</button>
        </div>
        
        <table id="tablaSeguimiento">
          <thead>
            <tr>
              <th style="text-align: left; width: 30%;">Tema</th>
              <th style="width: 45%;">Progreso</th>
              <th style="width: 25%;">Acciones</th>
            </tr>
          </thead>
          <tbody id="tablaBody"></tbody>
        </table>
      </div>
    </div>
  </div>

  <div class="toast" id="toast"></div>
  <footer>
    <div class="info">Datos importados de la Ordenanza N° 1483. Créditos: Lighu.exe y Kevi (Sion) uwu</div>
  </footer>

  <script>
    (function() {
      // ---------- DATOS COMPARTIDOS ----------
      const subjects = [
        {id:'IBFCa', name:'IBFCa (Ciclo Introductorio)', year:'1° Año', prereqs:[], state:'pendiente'},
        {id:'IBH', name:'IBH (Ciclo Introductorio)', year:'1° Año', prereqs:[], state:'pendiente'},
        {id:'IEM', name:'IEM (Ciclo Introductorio)', year:'1° Año', prereqs:[], state:'pendiente'},
        {id:'MYS', name:'MYS (Ciclo Introductorio)', year:'1° Año', prereqs:[], state:'pendiente'},
        {id:'IQSB', name:'IQSB (Ciclo Introductorio)', year:'1° Año', prereqs:[], state:'pendiente'},
        {id:'TallerA', name:'Taller A', year:'2° Año', prereqs:['IBFCa','IBH','IEM','MYS','IQSB'], state:'pendiente'},
        {id:'Anatomia', name:'Anatomía', year:'2° Año', prereqs:['IBFCa','IBH','IEM','MYS','IQSB'], state:'pendiente'},
        {id:'Histologia', name:'Histología', year:'2° Año', prereqs:['IBFCa','IBH','IEM','MYS','IQSB'], state:'pendiente'},
        {id:'Bioquimica', name:'Bioquímica', year:'2° Año', prereqs:['IBFCa','IBH','IEM','MYS','IQSB'], state:'pendiente'},
        {id:'APSI', name:'APS I', year:'2° Año', prereqs:['IBFCa','IBH','IEM','MYS','IQSB'], state:'pendiente'},
        {id:'Fisiologia', name:'Fisiología', year:'2° Año', prereqs:['Bioquimica'], state:'pendiente'},
        {id:'Bioetica', name:'Bioética', year:'3° Año', prereqs:['IBFCa','IBH','IEM','MYS','IQSB'], state:'pendiente'},
        {id:'Ingles', name:'Inglés', year:'3° Año', prereqs:['IBFCa','IBH','IEM','MYS','IQSB'], state:'pendiente'},
        {id:'Patologia', name:'Patología', year:'3° Año', prereqs:['Anatomia','Histologia'], state:'pendiente'},
        {id:'RMP', name:'RMP', year:'3° Año', prereqs:['APSI'], state:'pendiente'},
        {id:'Microbiologia', name:'Microbiología', year:'3° Año', prereqs:['Anatomia','Histologia','Bioquimica'], state:'pendiente'},
        {id:'APSII', name:'APS II', year:'3° Año', prereqs:['APSI'], state:'pendiente'},
        {id:'TallerB', name:'Taller B', year:'3° Año', prereqs:['TallerA'], state:'pendiente'},
        {id:'MedicinaI', name:'Medicina I', year:'4° Año', prereqs:['TallerA','Anatomia','Histologia','Bioquimica','APSI','Fisiologia','Bioetica','Ingles','Patologia','RMP','Microbiologia','APSII','TallerB'], state:'pendiente'},
        {id:'Farmacologia', name:'Farmacología', year:'4° Año', prereqs:['TallerA','Anatomia','Histologia','Bioquimica','APSI','Fisiologia','Bioetica','Ingles','Patologia','RMP','Microbiologia','APSII','TallerB'], state:'pendiente'},
        {id:'TallerC', name:'Taller C', year:'4° Año', prereqs:['TallerA','Anatomia','Histologia','Bioquimica','APSI','Fisiologia','Bioetica','Ingles','Patologia','RMP','Microbiologia','APSII','TallerB'], state:'pendiente'},
        {id:'FarmacologiaEsp', name:'Farmacología Especial', year:'5° Año', prereqs:['Farmacologia'], state:'pendiente'},
        {id:'MedYCirugia', name:'Medicina y Cirugía', year:'5° Año', prereqs:['MedicinaI','Farmacologia','TallerC'], state:'pendiente'},
        {id:'Psiquiatria', name:'Psiquiatría', year:'5° Año', prereqs:['MedicinaI','Farmacologia','TallerC'], state:'pendiente'},
        {id:'MedLegal', name:'Medicina Legal', year:'6° Año', prereqs:['MedicinaI','Farmacologia','TallerC'], state:'pendiente'},
        {id:'Ginecologia', name:'Ginecología y Obstetricia', year:'6° Año', prereqs:['MedicinaI','Farmacologia','TallerC'], state:'pendiente'},
        {id:'MedInfantil', name:'Medicina Infantil', year:'6° Año', prereqs:['MedicinaI','Farmacologia','TallerC'], state:'pendiente'},
        {id:'PFO', name:'PFO (Ciclo de Síntesis)', year:'7° Año', prereqs:['MedicinaI','Farmacologia','TallerC','MedYCirugia','Psiquiatria','MedLegal','Ginecologia','MedInfantil'], state:'pendiente'}
      ];

      const STATES = {LOCKED:'locked',UNLOCKED:'unlocked',CURSANDO:'cursando',APROBADA:'aprobada'};
      const storageKey = 'malla_uncomahue_v1';
      const etapas = ['leido', 'resumido', 'estudiado', 'repaso1', 'repaso2', 'repaso3'];
      const etapasNombres = {
        'leido': 'Leído', 
        'resumido': 'Resumido', 
        'estudiado': 'Estudiado', 
        'repaso1': 'Repaso 1', 
        'repaso2': 'Repaso 2', 
        'repaso3': 'Repaso 3'
      };

      // ---------- FUNCIONES COMUNES ----------
      function showToast(msg, ms = 1800) {
        const t = document.getElementById('toast');
        t.textContent = msg;
        t.style.display = 'block';
        setTimeout(() => { t.style.display = 'none'; }, ms);
      }

      // ---------- LÓGICA DE MALLA (estados de materias) ----------
      const saved = JSON.parse(localStorage.getItem(storageKey) || 'null');
      const subjMap = {};

      subjects.forEach(s => {
        subjMap[s.id] = Object.assign({}, s);
        if (saved && saved[s.id]) {
          subjMap[s.id].state = saved[s.id];
        } else {
          const token = (s.state || 'pendiente').toLowerCase();
          if (token.includes('aprob')) subjMap[s.id].state = STATES.APROBADA;
          else if (token.includes('curs')) subjMap[s.id].state = STATES.CURSANDO;
          else subjMap[s.id].state = STATES.LOCKED;
        }
      });

      function computeLockedStates() {
        let changed = false;
        subjects.forEach(s => {
          const curr = subjMap[s.id];
          if (curr.state === STATES.APROBADA) return;
          const prereqs = curr.prereqs || [];
          let allApproved = true;
          for (const p of prereqs) {
            const ps = subjMap[p];
            if (!ps || ps.state !== STATES.APROBADA) {
              allApproved = false;
              break;
            }
          }
          const newState = allApproved ? (curr.state === STATES.CURSANDO ? STATES.CURSANDO : STATES.UNLOCKED) : STATES.LOCKED;
          if (newState !== curr.state) {
            curr.state = newState;
            changed = true;
          }
        });
        return changed;
      }

      function stabilize() {
        let iter = 0;
        while (computeLockedStates() && iter < 20) iter++;
      }
      stabilize();

      function saveMalla() {
        const out = {};
        for (const k in subjMap) out[k] = subjMap[k].state;
        localStorage.setItem(storageKey, JSON.stringify(out));
      }

      // ---------- RENDERIZADO DE MALLA ----------
      const years = [...new Set(subjects.map(s => s.year))];
      const mainGrid = document.getElementById('mainGrid');

      function renderMalla() {
        mainGrid.innerHTML = '';
        years.forEach(year => {
          const col = document.createElement('section');
          col.className = 'year-col';
          const title = document.createElement('div');
          title.className = 'year-title';
          title.textContent = year;
          col.appendChild(title);

          subjects.filter(s => s.year === year).forEach(s => {
            const data = subjMap[s.id];
            const displayName = s.name.replace(/\s*\(.*\)/, '');
            const div = document.createElement('div');
            div.className = 'subject ' + data.state;
            div.dataset.id = s.id;
            div.innerHTML = `<div><strong>${displayName}</strong></div><div class="status-indicator"></div>`;
            div.addEventListener('click', onSubjectClick);
            col.appendChild(div);
          });

          mainGrid.appendChild(col);
        });
        saveMalla();
        // Actualizar el select del planificador con los nuevos estados
        poblarSelectMaterias();
      }

      function onSubjectClick(e) {
        const id = e.currentTarget.dataset.id;
        const s = subjMap[id];

        if (s.state === STATES.LOCKED) {
          const prereqsFaltantes = s.prereqs.filter(prereq => !subjMap[prereq] || subjMap[prereq].state !== STATES.APROBADA);
          if (prereqsFaltantes.length > 0) {
            const nombresFaltantes = prereqsFaltantes.map(prereqId => {
              const materia = subjects.find(subj => subj.id === prereqId);
              return materia ? materia.name.replace(/\s*\(.*\)/, '') : prereqId;
            });
            showToast(`Materia bloqueada. Faltan aprobar: ${nombresFaltantes.join(', ')}`);
          } else {
            showToast('Materia bloqueada: faltan correlativas.');
          }
          return;
        }

        let nextState;
        if (s.state === STATES.UNLOCKED) nextState = STATES.CURSANDO;
        else if (s.state === STATES.CURSANDO) nextState = STATES.APROBADA;
        else if (s.state === STATES.APROBADA) nextState = STATES.UNLOCKED;

        // Validaciones especiales
        if (s.id === 'Bioetica' && nextState === STATES.APROBADA) {
          if (!subjMap['TallerA'] || subjMap['TallerA'].state !== STATES.APROBADA) {
            showToast('Para aprobar Bioética necesitás tener aprobada Taller A.');
            return;
          }
        }
        if (s.id === 'APSII' && nextState === STATES.CURSANDO) {
          if (!subjMap['RMP'] || subjMap['RMP'].state === STATES.LOCKED || subjMap['RMP'].state === STATES.UNLOCKED) {
            showToast('Para cursar APS II necesitás tener cursada RMP (debe estar en estado Cursando o Aprobada).');
            return;
          }
        }
        if (s.id === 'APSII' && nextState === STATES.APROBADA) {
          if (!subjMap['RMP'] || subjMap['RMP'].state !== STATES.APROBADA) {
            showToast('Para aprobar APS II necesitás tener aprobada RMP.');
            return;
          }
          if (!subjMap['TallerA'] || subjMap['TallerA'].state !== STATES.APROBADA) {
            showToast('Para aprobar APS II necesitás tener aprobada Taller A.');
            return;
          }
        }

        s.state = nextState;
        stabilize();
        renderMalla();
      }

      document.getElementById('resetBtn').addEventListener('click', () => {
        if (!confirm('¿Reiniciar todo el progreso de la malla? Esto restaurará los estados iniciales.')) return;
        localStorage.removeItem(storageKey);
        subjects.forEach(s => {
          const token = (s.state || 'pendiente').toLowerCase();
          if (token.includes('aprob')) subjMap[s.id].state = STATES.APROBADA;
          else if (token.includes('curs')) subjMap[s.id].state = STATES.CURSANDO;
          else subjMap[s.id].state = STATES.LOCKED;
        });
        stabilize();
        renderMalla();
      });

      // ---------- LÓGICA DEL PLANIFICADOR ----------
      function obtenerEstadoMaterias() {
        // Reconstruye subjMap desde localStorage (igual que arriba, pero lo usamos en planner)
        const savedNow = JSON.parse(localStorage.getItem(storageKey) || '{}');
        const map = {};
        subjects.forEach(s => {
          map[s.id] = Object.assign({}, s);
          if (savedNow[s.id]) map[s.id].state = savedNow[s.id];
          else {
            const token = (s.state || 'pendiente').toLowerCase();
            if (token.includes('aprob')) map[s.id].state = STATES.APROBADA;
            else if (token.includes('curs')) map[s.id].state = STATES.CURSANDO;
            else map[s.id].state = STATES.LOCKED;
          }
        });
        // Recalcular locks (simplificado, no modificamos el original)
        return map;
      }

      function poblarSelectMaterias() {
        const select = document.getElementById('materiaSelect');
        if (!select) return;
        const estadoActual = obtenerEstadoMaterias();
        select.innerHTML = '<option value="">-- Elegir materia --</option>';
        subjects.forEach(subject => {
          const option = document.createElement('option');
          option.value = subject.id;
          const materiaEstado = estadoActual[subject.id].state;
          let estadoTexto = '';
          switch(materiaEstado) {
            case STATES.APROBADA: estadoTexto = ' (✓ Aprobada)'; break;
            case STATES.CURSANDO: estadoTexto = ' (▶ Cursando)'; break;
            case STATES.UNLOCKED: estadoTexto = ' (○ Habilitada)'; break;
            case STATES.LOCKED: estadoTexto = ' (✗ Bloqueada)'; break;
          }
          option.textContent = subject.name.replace(/\s*\(.*\)/, '') + estadoTexto;
          if (materiaEstado === STATES.LOCKED) option.disabled = true;
          select.appendChild(option);
        });
      }

      // Funciones del planificador (copiadas de planificador.html)
      function puedeMarcarEtapa(tema, etapaIndex) {
        if (etapaIndex === 0) return true;
        for (let i = 0; i < etapaIndex; i++) {
          if (!tema[etapas[i]]) return false;
        }
        return true;
      }

      function calcularProgreso(tema) {
        const completadas = etapas.filter(etapa => tema[etapa]).length;
        return { completadas, total: etapas.length, porcentaje: Math.round((completadas / etapas.length) * 100) };
      }

      function calcularProgresoGeneral(temas) {
        if (temas.length === 0) return { porcentaje: 0, completadas: 0, total: 0 };
        let totalEtapas = 0, etapasCompletadas = 0;
        temas.forEach(tema => {
          etapas.forEach(etapa => { totalEtapas++; if (tema[etapa]) etapasCompletadas++; });
          if (tema.subtemas) {
            tema.subtemas.forEach(subtema => {
              etapas.forEach(etapa => { totalEtapas++; if (subtema[etapa]) etapasCompletadas++; });
            });
          }
        });
        return { porcentaje: Math.round((etapasCompletadas / totalEtapas) * 100), completadas: etapasCompletadas, total: totalEtapas };
      }

      function sincronizarCheckboxes(temaIndex, etapa, temas, materiaId) {
        const tema = temas[temaIndex];
        if (!etapa.startsWith('subtema_') && tema[etapa]) {
          if (tema.subtemas) tema.subtemas.forEach(subtema => subtema[etapa] = true);
        }
        if (!etapa.startsWith('subtema_') && !tema[etapa]) {
          if (tema.subtemas) tema.subtemas.forEach(subtema => subtema[etapa] = false);
          const etapaIndex = etapas.indexOf(etapa);
          for (let i = etapaIndex + 1; i < etapas.length; i++) {
            tema[etapas[i]] = false;
            if (tema.subtemas) tema.subtemas.forEach(subtema => subtema[etapas[i]] = false);
          }
        }
        if (etapa.startsWith('subtema_')) {
          const etapaPrincipal = etapa.replace('subtema_', '');
          if (tema.subtemas && tema.subtemas.length > 0) {
            tema[etapaPrincipal] = tema.subtemas.every(subtema => subtema[etapaPrincipal]);
          }
        }
        localStorage.setItem(`planificador_${materiaId}`, JSON.stringify(temas));
      }

      function cargarTemas(materiaId, materiaEstado) {
        const temas = JSON.parse(localStorage.getItem(`planificador_${materiaId}`) || '[]');
        const tablaBody = document.getElementById('tablaBody');
        tablaBody.innerHTML = '';

        if (temas.length === 0) {
          tablaBody.innerHTML = '<tr><td colspan="3" class="sin-temas">No hay temas agregados aún. Usa el botón "Agregar Tema" para comenzar.</td></tr>';
        } else {
          temas.forEach((tema, index) => {
            const progreso = calcularProgreso(tema);
            const todasCompletadas = progreso.completadas === etapas.length;
            const fila = document.createElement('tr');
            fila.className = 'tema-row';
            fila.innerHTML = `
              <td>
                <div style="display: flex; align-items: center;">
                  <strong>${tema.nombre}</strong>
                  ${todasCompletadas ? '<span class="completado-icono"></span>' : ''}
                </div>
                <div class="progreso-tema">Progreso: ${progreso.completadas}/${progreso.total} (${progreso.porcentaje}%)</div>
              </td>
              <td>
                <div class="checklist-horizontal">
                  ${etapas.map((etapa, i) => {
                    const puedeMarcar = puedeMarcarEtapa(tema, i);
                    const estaCompletada = tema[etapa];
                    return `<label class="check-item-horizontal ${estaCompletada ? 'completado' : ''} ${!puedeMarcar ? 'bloqueado' : ''}">
                      <div class="checkbox-container">
                        <input type="checkbox" data-tema-index="${index}" data-etapa="${etapa}" ${estaCompletada ? 'checked' : ''} ${!puedeMarcar ? 'disabled' : ''}>
                        <span class="checkmark"></span>
                      </div>
                      <span class="check-label-horizontal">${etapasNombres[etapa]}</span>
                    </label>`;
                  }).join('')}
                </div>
              </td>
              <td><div class="acciones-tema"><button class="eliminar" data-tema-index="${index}">Eliminar</button></div></td>
            `;

            const filaSubtema = document.createElement('tr');
            filaSubtema.innerHTML = `
              <td colspan="3" style="padding: 0; border: none;">
                <div class="subtema-container ${tema.subtemasAbierto ? 'abierto' : ''}">
                  <div class="subtema-header">
                    <span>📝 Subtemas (${tema.subtemas ? tema.subtemas.length : 0})</span>
                    <span class="subtema-flecha">▼</span>
                  </div>
                  <div class="agregar-subtema">
                    <input type="text" id="nuevoSubtema-${index}" placeholder="Nombre del nuevo subtema" style="padding: 8px; width: 200px; border-radius: 6px; border: 1px solid #ddd;">
                    <button class="secondary" data-tema-index="${index}" id="agregarSubtemaBtn-${index}">Agregar Subtema</button>
                  </div>
                  <div id="subtemas-container-${index}">
                    ${tema.subtemas ? tema.subtemas.map((subtema, subtemaIndex) => `
                      <div class="subtema-item">
                        <div class="subtema-item-header">
                          <span class="subtema-nombre">${subtema.nombre}</span>
                          <div class="acciones-subtema">
                            <button class="eliminar" data-tema-index="${index}" data-subtema-index="${subtemaIndex}">Eliminar</button>
                          </div>
                        </div>
                        <div class="checklist-horizontal">
                          ${etapas.map((etapa, i) => {
                            const puedeMarcar = puedeMarcarEtapa(tema, i);
                            const estaCompletada = subtema[etapa] || false;
                            return `<label class="check-item-horizontal ${estaCompletada ? 'completado' : ''} ${!puedeMarcar ? 'bloqueado' : ''}">
                              <div class="checkbox-container">
                                <input type="checkbox" data-tema-index="${index}" data-subtema-index="${subtemaIndex}" data-etapa="${etapa}" ${estaCompletada ? 'checked' : ''} ${!puedeMarcar ? 'disabled' : ''}>
                                <span class="checkmark"></span>
                              </div>
                              <span class="check-label-horizontal">${etapasNombres[etapa]}</span>
                            </label>`;
                          }).join('')}
                        </div>
                      </div>
                    `).join('') : ''}
                  </div>
                </div>
              </td>
            `;
            tablaBody.appendChild(fila);
            tablaBody.appendChild(filaSubtema);
          });

          agregarEventosPlanificador(materiaId, temas, materiaEstado);
          mostrarEstadisticas(materiaId);
        }
      }

      function agregarEventosPlanificador(materiaId, temas, materiaEstado) {
        if (materiaEstado === STATES.LOCKED) return;
        document.querySelectorAll('input[type="checkbox"][data-etapa]').forEach(checkbox => {
          if (!checkbox.hasAttribute('data-subtema-index')) {
            checkbox.addEventListener('change', function() {
              const temaIndex = parseInt(this.getAttribute('data-tema-index'));
              const etapa = this.getAttribute('data-etapa');
              temas[temaIndex][etapa] = this.checked;
              sincronizarCheckboxes(temaIndex, etapa, temas, materiaId);
              cargarTemas(materiaId, materiaEstado);
              showToast('Progreso actualizado');
            });
          }
        });
        document.querySelectorAll('input[type="checkbox"][data-etapa][data-subtema-index]').forEach(checkbox => {
          checkbox.addEventListener('change', function() {
            const temaIndex = parseInt(this.getAttribute('data-tema-index'));
            const subtemaIndex = parseInt(this.getAttribute('data-subtema-index'));
            const etapa = this.getAttribute('data-etapa');
            if (temas[temaIndex].subtemas && temas[temaIndex].subtemas[subtemaIndex]) {
              temas[temaIndex].subtemas[subtemaIndex][etapa] = this.checked;
              sincronizarCheckboxes(temaIndex, etapa, temas, materiaId);
              mostrarEstadisticas(materiaId);
              showToast('Progreso del subtema actualizado');
            }
          });
        });
        document.querySelectorAll('button.eliminar[data-tema-index]').forEach(button => {
          if (!button.hasAttribute('data-subtema-index')) {
            button.addEventListener('click', function() {
              const temaIndex = parseInt(this.getAttribute('data-tema-index'));
              if (confirm('¿Eliminar este tema y todos sus subtemas?')) {
                temas.splice(temaIndex, 1);
                localStorage.setItem(`planificador_${materiaId}`, JSON.stringify(temas));
                cargarTemas(materiaId, materiaEstado);
                showToast('Tema eliminado');
              }
            });
          }
        });
        document.querySelectorAll('button.eliminar[data-subtema-index]').forEach(button => {
          button.addEventListener('click', function() {
            const temaIndex = parseInt(this.getAttribute('data-tema-index'));
            const subtemaIndex = parseInt(this.getAttribute('data-subtema-index'));
            if (confirm('¿Eliminar este subtema?')) {
              if (temas[temaIndex].subtemas) {
                temas[temaIndex].subtemas.splice(subtemaIndex, 1);
                localStorage.setItem(`planificador_${materiaId}`, JSON.stringify(temas));
                cargarTemas(materiaId, materiaEstado);
                showToast('Subtema eliminado');
              }
            }
          });
        });
        document.querySelectorAll('.subtema-header').forEach(header => {
          header.addEventListener('click', function() {
            this.parentElement.classList.toggle('abierto');
          });
        });
        document.querySelectorAll('button[id^="agregarSubtemaBtn-"]').forEach(button => {
          button.addEventListener('click', function() {
            const temaIndex = parseInt(this.getAttribute('data-tema-index'));
            const input = document.getElementById(`nuevoSubtema-${temaIndex}`);
            const nombreSubtema = input.value.trim();
            if (nombreSubtema === '') {
              showToast('Ingresa un nombre para el subtema');
              return;
            }
            if (!temas[temaIndex].subtemas) temas[temaIndex].subtemas = [];
            const nuevoSubtema = { nombre: nombreSubtema };
            etapas.forEach(etapa => nuevoSubtema[etapa] = false);
            temas[temaIndex].subtemas.push(nuevoSubtema);
            localStorage.setItem(`planificador_${materiaId}`, JSON.stringify(temas));
            input.value = '';
            cargarTemas(materiaId, materiaEstado);
            showToast('Subtema agregado');
          });
        });
      }

      function mostrarEstadisticas(materiaId) {
        const temas = JSON.parse(localStorage.getItem(`planificador_${materiaId}`) || '[]');
        const progresoGeneral = calcularProgresoGeneral(temas);
        const oldStats = document.querySelector('.estadisticas');
        if (oldStats) oldStats.remove();
        if (temas.length > 0) {
          const statsElement = document.createElement('div');
          statsElement.className = 'estadisticas';
          statsElement.innerHTML = `
            <div style="margin-top: 15px; padding: 15px; background: var(--glass); border-radius: 10px; border: 1px solid var(--glass-border);">
              <strong>Progreso General de la Materia:</strong> 
              <div style="margin-top: 8px; font-size: 1.1rem;">${progresoGeneral.completadas}/${progresoGeneral.total} etapas completadas</div>
              <div class="barra-progreso"><div class="progreso-llenado" style="width: ${progresoGeneral.porcentaje}%"></div></div>
              <div style="text-align: center; margin-top: 5px; font-weight: 500;">${progresoGeneral.porcentaje}% completado</div>
            </div>
          `;
          document.getElementById('estadoMateria').parentNode.insertBefore(statsElement, document.getElementById('estadoMateria').nextSibling);
        }
      }

      function agregarTemaPlanificador(materiaId, nombreTema) {
        const temas = JSON.parse(localStorage.getItem(`planificador_${materiaId}`) || '[]');
        const nuevoTema = { nombre: nombreTema, subtemas: [], subtemasAbierto: false };
        etapas.forEach(etapa => nuevoTema[etapa] = false);
        temas.push(nuevoTema);
        localStorage.setItem(`planificador_${materiaId}`, JSON.stringify(temas));
        const estado = obtenerEstadoMaterias()[materiaId].state;
        cargarTemas(materiaId, estado);
        showToast('Tema agregado');
      }

      // Inicializar planificador (eventos del select y botones)
      function initPlanificador() {
        const select = document.getElementById('materiaSelect');
        const planificadorContenido = document.getElementById('planificadorContenido');
        const estadoMateriaDiv = document.getElementById('estadoMateria');

        select.addEventListener('change', function() {
          const materiaId = this.value;
          if (materiaId) {
            const subjMapActual = obtenerEstadoMaterias();
            const materiaEstado = subjMapActual[materiaId].state;
            planificadorContenido.style.display = 'block';
            let estadoTexto = '', estadoClase = '';
            switch(materiaEstado) {
              case STATES.APROBADA: estadoTexto = 'Aprobada'; estadoClase = 'estado-aprobada'; break;
              case STATES.CURSANDO: estadoTexto = 'Cursando'; estadoClase = 'estado-cursando'; break;
              case STATES.UNLOCKED: estadoTexto = 'Habilitada'; estadoClase = 'estado-habilitada'; break;
              case STATES.LOCKED: estadoTexto = 'Bloqueada'; estadoClase = 'estado-bloqueada'; break;
            }
            estadoMateriaDiv.innerHTML = '<strong>Estado:</strong> <span class="estado-materia ' + estadoClase + '">' + estadoTexto + '</span>';
            document.getElementById('agregarTema').disabled = (materiaEstado === STATES.LOCKED);
            document.getElementById('agregarTema').style.opacity = (materiaEstado === STATES.LOCKED) ? '0.6' : '1';
            document.getElementById('agregarTema').style.cursor = (materiaEstado === STATES.LOCKED) ? 'not-allowed' : 'pointer';
            cargarTemas(materiaId, materiaEstado);
          } else {
            planificadorContenido.style.display = 'none';
          }
        });

        document.getElementById('agregarTema').addEventListener('click', function() {
          const materiaId = select.value;
          if (!materiaId) return;
          const subjMapActual = obtenerEstadoMaterias();
          if (subjMapActual[materiaId].state === STATES.LOCKED) {
            showToast('No puedes agregar temas a una materia bloqueada.');
            return;
          }
          const nombreTema = prompt('Ingresa el nombre del tema:');
          if (nombreTema && nombreTema.trim() !== '') agregarTemaPlanificador(materiaId, nombreTema.trim());
        });

        document.getElementById('expandirTodo').addEventListener('click', function() {
          const materiaId = select.value;
          if (!materiaId) return;
          const temas = JSON.parse(localStorage.getItem(`planificador_${materiaId}`) || '[]');
          temas.forEach(t => t.subtemasAbierto = true);
          localStorage.setItem(`planificador_${materiaId}`, JSON.stringify(temas));
          cargarTemas(materiaId, obtenerEstadoMaterias()[materiaId].state);
          showToast('Todos los subtemas expandidos');
        });

        document.getElementById('colapsarTodo').addEventListener('click', function() {
          const materiaId = select.value;
          if (!materiaId) return;
          const temas = JSON.parse(localStorage.getItem(`planificador_${materiaId}`) || '[]');
          temas.forEach(t => t.subtemasAbierto = false);
          localStorage.setItem(`planificador_${materiaId}`, JSON.stringify(temas));
          cargarTemas(materiaId, obtenerEstadoMaterias()[materiaId].state);
          showToast('Todos los subtemas colapsados');
        });

        document.getElementById('exportarJson').addEventListener('click', function() {
          const materiaId = select.value;
          if (!materiaId) return;
          const temas = JSON.parse(localStorage.getItem(`planificador_${materiaId}`) || '[]');
          const dataStr = JSON.stringify(temas, null, 2);
          const dataUri = 'data:application/json;charset=utf-8,'+ encodeURIComponent(dataStr);
          const link = document.createElement('a');
          link.setAttribute('href', dataUri);
          link.setAttribute('download', `planificador_${materiaId}.json`);
          link.click();
          showToast('Datos exportados');
        });

        document.getElementById('importarJson').addEventListener('click', function() {
          const materiaId = select.value;
          if (!materiaId) return;
          const input = document.createElement('input');
          input.type = 'file';
          input.accept = '.json';
          input.onchange = e => {
            const file = e.target.files[0];
            const reader = new FileReader();
            reader.onload = function(event) {
              try {
                const temasImportados = JSON.parse(event.target.result);
                if (Array.isArray(temasImportados)) {
                  temasImportados.forEach(tema => {
                    if (!tema.subtemas) tema.subtemas = [];
                    etapas.forEach(etapa => { if (tema[etapa] === undefined) tema[etapa] = false; });
                    if (tema.subtemasAbierto === undefined) tema.subtemasAbierto = false;
                  });
                  localStorage.setItem(`planificador_${materiaId}`, JSON.stringify(temasImportados));
                  cargarTemas(materiaId, obtenerEstadoMaterias()[materiaId].state);
                  showToast('Datos importados');
                } else showToast('Error: formato incorrecto');
              } catch { showToast('Error al importar'); }
            };
            reader.readAsText(file);
          };
          input.click();
        });
      }

      // ---------- CAMBIO DE VISTA ----------
      const mallaView = document.getElementById('mallaView');
      const planificadorView = document.getElementById('planificadorView');
      const viewMallaBtn = document.getElementById('viewMallaBtn');
      const viewPlanificadorBtn = document.getElementById('viewPlanificadorBtn');
      const viewDescription = document.getElementById('viewDescription');

      function setView(view) {
        if (view === 'malla') {
          mallaView.classList.add('active');
          planificadorView.classList.remove('active');
          viewMallaBtn.classList.add('active');
          viewPlanificadorBtn.classList.remove('active');
          viewDescription.textContent = 'Malla curricular · haz clic en una materia para cambiar su estado';
        } else {
          mallaView.classList.remove('active');
          planificadorView.classList.add('active');
          viewMallaBtn.classList.remove('active');
          viewPlanificadorBtn.classList.add('active');
          viewDescription.textContent = 'Planificador · gestiona temas y subtemas por materia';
          poblarSelectMaterias(); // refrescar opciones
          // Si hay una materia seleccionada previamente, recargar su contenido
          const select = document.getElementById('materiaSelect');
          if (select.value) {
            const event = new Event('change');
            select.dispatchEvent(event);
          }
        }
      }

      viewMallaBtn.addEventListener('click', () => setView('malla'));
      viewPlanificadorBtn.addEventListener('click', () => setView('planificador'));

      // ---------- INICIALIZACIÓN ----------
      renderMalla();
      poblarSelectMaterias();
      initPlanificador();

      // Teclado para malla (opcional)
      document.addEventListener('keydown', (e) => {
        if (e.key === 'Enter' && document.activeElement && document.activeElement.dataset && document.activeElement.dataset.id) {
          document.activeElement.click();
        }
      });
    })();
  </script>
</body>
</html>
