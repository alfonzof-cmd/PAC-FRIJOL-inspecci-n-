<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>PAC Frijol Maturín - Inspección de Campo</title>
<style>
  :root {
    --primary: #1b5e20;
    --primary-light: #4c8c4a;
    --primary-bg: #e8f5e9;
    --accent: #ef6c00;
    --bg: #f4f6f4;
    --card: #ffffff;
    --text: #1f2937;
    --text-muted: #6b7280;
    --border: #d1d5db;
    --danger: #c62828;
    --success: #2e7d32;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; }
  body { background: var(--bg); color: var(--text); padding-bottom: 85px; }
  
  header {
    background: linear-gradient(135deg, #1b5e20, #2e7d32);
    color: white;
    padding: 14px 16px;
    position: sticky;
    top: 0;
    z-index: 100;
    box-shadow: 0 2px 8px rgba(0,0,0,0.15);
  }
  header h1 { font-size: 1.15rem; font-weight: 700; display: flex; align-items: center; gap: 8px; }
  header p { font-size: 0.78rem; opacity: 0.9; margin-top: 2px; }

  #warn-content-uri {
    display: none;
    background: #fff3cd;
    color: #664d03;
    border-bottom: 2px solid #ffecb5;
    padding: 10px 14px;
    font-size: 0.8rem;
    line-height: 1.4;
  }

  .stats-bar {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
    padding: 10px 14px;
    background: #fff;
    border-bottom: 1px solid var(--border);
  }
  .stat-box {
    background: var(--bg);
    border-radius: 8px;
    padding: 8px;
    text-align: center;
  }
  .stat-num { font-size: 1.15rem; font-weight: 800; color: var(--primary); }
  .stat-num.orange { color: var(--accent); }
  .stat-label { font-size: 0.68rem; color: var(--text-muted); text-transform: uppercase; font-weight: 600; }

  .nav-tabs {
    display: flex;
    background: #fff;
    border-bottom: 1px solid var(--border);
    position: sticky;
    top: 62px;
    z-index: 90;
  }
  .nav-tab {
    flex: 1;
    padding: 11px 6px;
    text-align: center;
    font-size: 0.82rem;
    font-weight: 700;
    color: var(--text-muted);
    border-bottom: 3px solid transparent;
    cursor: pointer;
    background: none;
    border-top: none; border-left: none; border-right: none;
  }
  .nav-tab.active {
    color: var(--primary);
    border-bottom-color: var(--primary);
    background: var(--primary-bg);
  }

  .container { padding: 14px; max-width: 720px; margin: 0 auto; }
  .section { display: none; }
  .section.active { display: block; }

  .card {
    background: var(--card);
    border-radius: 12px;
    padding: 14px;
    margin-bottom: 14px;
    box-shadow: 0 1px 4px rgba(0,0,0,0.06);
    border: 1px solid #e5e7eb;
  }
  .card-title {
    font-size: 0.9rem;
    font-weight: 700;
    color: var(--primary);
    margin-bottom: 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid #f0f0f0;
    padding-bottom: 6px;
  }

  .mode-switch {
    display: flex;
    background: #e5e7eb;
    border-radius: 8px;
    padding: 3px;
    margin-bottom: 12px;
  }
  .mode-btn {
    flex: 1;
    padding: 8px;
    border: none;
    border-radius: 6px;
    font-size: 0.8rem;
    font-weight: 700;
    cursor: pointer;
    background: transparent;
    color: var(--text-muted);
  }
  .mode-btn.active {
    background: #fff;
    color: var(--primary);
    box-shadow: 0 1px 3px rgba(0,0,0,0.12);
  }

  .form-group { margin-bottom: 11px; }
  label { display: block; font-size: 0.78rem; font-weight: 700; color: #374151; margin-bottom: 4px; }
  select, input[type="text"], input[type="number"], input[type="date"], textarea {
    width: 100%;
    padding: 10px 11px;
    border: 1.5px solid var(--border);
    border-radius: 8px;
    font-size: 0.92rem;
    background: #fff;
    color: var(--text);
  }
  select:focus, input:focus, textarea:focus {
    outline: none;
    border-color: var(--primary);
    box-shadow: 0 0 0 3px rgba(27, 94, 32, 0.12);
  }
  .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; }

  .info-banner {
    background: var(--primary-bg);
    border-left: 4px solid var(--primary);
    padding: 10px 12px;
    border-radius: 8px;
    margin-bottom: 12px;
    font-size: 0.82rem;
    line-height: 1.45;
  }
  .info-banner strong { color: var(--primary); }
  .badge {
    display: inline-block;
    padding: 2px 8px;
    border-radius: 12px;
    font-size: 0.7rem;
    font-weight: 700;
  }
  .badge-visited { background: #dcfce7; color: #166534; }
  .badge-pending { background: #fef3c7; color: #92400e; }

  .chips { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 8px; }
  .chip {
    background: #f3f4f6;
    border: 1px solid #d1d5db;
    border-radius: 16px;
    padding: 4px 10px;
    font-size: 0.73rem;
    cursor: pointer;
    color: #374151;
  }

  .btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    gap: 6px;
    width: 100%;
    padding: 12px 16px;
    border: none;
    border-radius: 10px;
    font-size: 0.92rem;
    font-weight: 700;
    cursor: pointer;
  }
  .btn-primary { background: var(--primary); color: #fff; }
  .btn-accent { background: var(--accent); color: #fff; }
  .btn-outline { background: #fff; border: 1.5px solid var(--primary); color: var(--primary); }
  .btn-danger { background: #fee2e2; color: var(--danger); border: 1px solid #fecaca; }
  .btn-sm { padding: 7px 12px; font-size: 0.78rem; width: auto; }

  .visit-item {
    border: 1px solid #e5e7eb;
    border-radius: 10px;
    padding: 11px;
    margin-bottom: 9px;
    background: #fff;
  }
  .visit-item.visited { border-left: 4px solid var(--success); }
  .visit-item-header { display: flex; justify-content: space-between; align-items: flex-start; gap: 8px; }
  .visit-name { font-weight: 700; font-size: 0.9rem; color: #111827; }
  .visit-meta { font-size: 0.75rem; color: var(--text-muted); margin-top: 3px; }
  .visit-obs {
    margin-top: 7px;
    background: #f9fafb;
    padding: 7px 9px;
    border-radius: 6px;
    font-size: 0.78rem;
    color: #374151;
    border: 1px dashed #d1d5db;
  }

  #toast {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    background: #111827;
    color: #fff;
    padding: 10px 18px;
    border-radius: 24px;
    font-size: 0.84rem;
    font-weight: 600;
    z-index: 999;
    box-shadow: 0 4px 12px rgba(0,0,0,0.25);
    display: none;
    max-width: 90%;
    text-align: center;
  }

  .file-drop {
    border: 2px dashed var(--primary-light);
    background: var(--primary-bg);
    border-radius: 10px;
    padding: 16px;
    text-align: center;
    cursor: pointer;
    margin-bottom: 10px;
    display: block;
  }
  .file-drop input { display: none; }
</style>
</head>
<body>

<header>
  <h1>🌱 PAC Frijol - Maturín</h1>
  <p>Control Técnico de Inspección en Campo (Sin Conexión)</p>
</header>

<div id="warn-content-uri">
  ⚠️ <strong>Modo vista previa de Android detectado:</strong> Estás viendo el archivo desde un enlace temporal (<code>content://</code>). Para que Chrome guarde los datos permanentemente en tu teléfono, ábrelo desde tu carpeta <strong>Descargas</strong> o súbelo a tu GitHub Pages.
</div>

<div class="stats-bar">
  <div class="stat-box">
    <div class="stat-num" id="stat-total">43</div>
    <div class="stat-label">Productores</div>
  </div>
  <div class="stat-box">
    <div class="stat-num orange" id="stat-visitados">0</div>
    <div class="stat-label">Visitados Hoy</div>
  </div>
  <div class="stat-box">
    <div class="stat-num" id="stat-ha-sembradas">0.0</div>
    <div class="stat-label">Ha Sembradas</div>
  </div>
</div>

<div class="nav-tabs">
  <button type="button" class="nav-tab active" id="nav-inspeccion">📋 Inspección</button>
  <button type="button" class="nav-tab" id="nav-visitados">✅ Visitados (<span id="tab-count">0</span>)</button>
  <button type="button" class="nav-tab" id="nav-sincronizar">🔄 Excel / Equipo</button>
</div>

<div class="container">

  <!-- TAB 1: INSPECCIÓN -->
  <div id="tab-inspeccion" class="section active">
    <div class="card">
      <div class="card-title">1. Seleccionar Productor</div>
      
      <div class="mode-switch">
        <button type="button" class="mode-btn active" id="btn-mode-territorio">Por Comuna / C. Comunal</button>
        <button type="button" class="mode-btn" id="btn-mode-lista">Lista Completa (A-Z)</button>
      </div>

      <!-- MODO TERRITORIO -->
      <div id="filter-territorio">
        <div class="form-group">
          <label for="sel-comuna">Comuna</label>
          <select id="sel-comuna">
            <option value="">-- Todas las Comunas --</option>
<option value="BENDECIDO DE LA PATRIA">BENDECIDO DE LA PATRIA</option>
<option value="COCUIZA BOLIVARIANA">COCUIZA BOLIVARIANA</option>
<option value="COMBATIENTES DEL FURRIAL SEMBRANDO FUTURO EN REVOLUCION">COMBATIENTES DEL FURRIAL SEMBRANDO FUTURO EN REVOLUCION</option>
<option value="JUSEPIN DESPERTO CON CHAVEZ">JUSEPIN DESPERTO CON CHAVEZ</option>
<option value="PRODUCTIVA COROZO CORAZÓN DULCE Y REVOLUCIONARIO">PRODUCTIVA COROZO CORAZÓN DULCE Y REVOLUCIONARIO</option>
<option value="VENEZUELA HEROICA">VENEZUELA HEROICA</option>
          </select>
        </div>
        <div class="form-group">
          <label for="sel-consejo">Consejo Comunal (<span id="count-consejos">10</span>)</label>
          <select id="sel-consejo">
            <option value="">-- Todos los Consejos Comunales --</option>
<option value="AMARILIS">AMARILIS</option>
<option value="ANDRES ELOY BLANCO">ANDRES ELOY BLANCO</option>
<option value="COROZO CENTRO">COROZO CENTRO</option>
<option value="EL SAGRADO CORAZÓN DE JESÚS">EL SAGRADO CORAZÓN DE JESÚS</option>
<option value="KUWAIT BRISAS DEL GUARAPICHE">KUWAIT BRISAS DEL GUARAPICHE</option>
<option value="LAS MARGARITAS">LAS MARGARITAS</option>
<option value="LUCHADORES BICENTENARIO">LUCHADORES BICENTENARIO</option>
<option value="MADRICERA I">MADRICERA I</option>
<option value="MAKEDONIA">MAKEDONIA</option>
<option value="SAN JAIME">SAN JAIME</option>
          </select>
        </div>
      </div>

      <!-- MODO LISTA COMPLETA / BÚSQUEDA RÁPIDA -->
      <div id="filter-lista" style="display:none;">
        <div class="form-group">
          <label for="txt-buscar">Filtro rápido por nombre o cédula (opcional)</label>
          <input type="text" id="txt-buscar" placeholder="Ej: Douglas, Abreu, 14859...">
        </div>
      </div>

      <div class="form-group" style="margin-bottom:0;">
        <label for="sel-productor">Nombre del Productor (<span id="count-filtered">43</span> disponibles)</label>
        <select id="sel-productor">
          <option value="">-- Seleccione un productor (43) --</option>
<option value="1">DOUGLAS MATA - CI: 9894065</option>
<option value="2">JOSÉ GAMBOA - CI: 18212120</option>
<option value="3">RICHARD ABREU - CI: 14859887</option>
<option value="4">AUDORINA CANELON - CI: 8374815</option>
<option value="5">JULIAN SILVEIRA - CI: 17934699</option>
<option value="6">RICHARD ABREU - CI: 31418761</option>
<option value="7">REICHEL ABREU - CI: 32053140</option>
<option value="8">GUSTAVO VILLARROEL - CI: 14441584</option>
<option value="9">FREDDY MORENO - CI: 5396523</option>
<option value="10">MARIENNIS DURAN - CI: 17241708</option>
<option value="11">NORELYS SILVEIRA - CI: 16174803</option>
<option value="12">LUIS ALCIDES CASTILLO - CI: 11779771</option>
<option value="13">DIXON BETANCOURT - CI: 3191219</option>
<option value="14">ANTONIO DURAN - CI: 20446529</option>
<option value="15">JUNIOR RAMIREZ - CI: 25355102</option>
<option value="16">CARLOS BETANCOURT - CI: 8368326</option>
<option value="17">PABLO RUIZ - CI: 10837483</option>
<option value="18">GUSTAVO DIAZ - CI: 16375722</option>
<option value="19">ABEL LOPEZ - CI: 23414996</option>
<option value="20">JOSÉ SUÁREZ - CI: 12156846</option>
<option value="21">SIMÓN MANUEL MIEREZ - CI: 14716033</option>
<option value="22">JUAN BAUTISTA ESPINOZA RIVAS - CI: 11778461</option>
<option value="23">JOSE GREGORIO ROMERO - CI: 9896050</option>
<option value="24">JULIO RAMÓN CAMINO - CI: 11778729</option>
<option value="25">JOSE PALACIOS - CI: 14111544</option>
<option value="26">MARÍA ALEJANDRA BETANCOURT - CI: 9898403</option>
<option value="27">ÁLVARO MARTÍNEZ - CI: 14620620</option>
<option value="28">JESSICA CHALO - CI: 16518014</option>
<option value="29">JESÚS MOTA - CI: 11778783</option>
<option value="30">LUÍS ARTURO ROQUE - CI: 8224657</option>
<option value="31">RENNY ABZUETA MARTINEZ - CI: 15278022</option>
<option value="32">HERNAN PADRON - CI: 19257449</option>
<option value="33">ETAR DIR.ª GABRIELA RENGEL - CI: 18272536</option>
<option value="34">YOIDE GARCÍA - CI: 13250606</option>
<option value="35">GUSMARX PEREIRA - CI: 15633796</option>
<option value="36">DIORLAN ORTÍZ - CI: 20646651</option>
<option value="37">LUIS ESTAVAS - CI: 11085997</option>
<option value="38">MARÍA ROJAS - CI: 12151940</option>
<option value="39">MARIELIS CAMPOS - CI: 16174435</option>
<option value="40">DANNY JIMÉNEZ - CI: 19092307</option>
<option value="41">CRUZ ESTANGA - CI: 9295504</option>
<option value="42">CESAR GONZÁLEZ - CI: 12151617</option>
<option value="43">JORGE RODRÍGUEZ - CI: 17405560</option>
        </select>
      </div>
    </div>

    <!-- FORMULARIO DEL PRODUCTOR -->
    <div id="producer-form-container" style="display:none;">
      <div class="info-banner" id="producer-summary"></div>

      <div class="card">
        <div class="card-title">
          <span>2. Datos Técnicos y Avance</span>
          <span id="status-badge" class="badge badge-pending">Pendiente</span>
        </div>

        <div class="grid-2">
          <div class="form-group">
            <label>Fecha de Visita / Insp.</label>
            <input type="date" id="f-fecha-insp">
          </div>
          <div class="form-group">
            <label>Técnico Responsable</label>
            <input type="text" id="f-tecnico" placeholder="Ej: Ing. F. Alfonzo">
          </div>
        </div>

        <div class="grid-2">
          <div class="form-group">
            <label>Sup. Financiada (ha)</label>
            <input type="number" step="0.1" id="f-sup-fin">
          </div>
          <div class="form-group">
            <label>Retiro de Insumos</label>
            <select id="f-insumos">
              <option value="SI">SI</option>
              <option value="NO">NO</option>
              <option value="PARCIAL">PARCIAL</option>
            </select>
          </div>
        </div>

        <div class="form-group">
          <label>Combustible Recibido / Situación (L)</label>
          <input type="text" id="f-combustible" placeholder="Ej: 150 litros por cuenta propia / No ha comprado">
        </div>

        <div class="form-group">
          <label>Tipo de Siembra / Semilla</label>
          <input type="text" id="f-tipo-siembra" placeholder="Ej: FRIJOL ya compro">
        </div>

        <div class="grid-3">
          <div class="form-group">
            <label>Prep. (ha)</label>
            <input type="number" step="0.1" id="f-preparadas" placeholder="0.0">
          </div>
          <div class="form-group">
            <label>1er Pase (ha)</label>
            <input type="number" step="0.1" id="f-pase1" placeholder="0.0">
          </div>
          <div class="form-group">
            <label>2do Pase (ha)</label>
            <input type="number" step="0.1" id="f-pase2" placeholder="0.0">
          </div>
        </div>

        <div class="grid-2">
          <div class="form-group">
            <label>Fecha / Estado Siembra</label>
            <input type="text" id="f-fecha-siembra" placeholder="Ej: 2026-11-27 o NO HA INICIADO">
          </div>
          <div class="form-group">
            <label>Hectáreas Sembradas</label>
            <input type="number" step="0.1" id="f-ha-sembradas" placeholder="0.0">
          </div>
        </div>

        <div class="form-group" id="box-obs-previa" style="display:none;">
          <label>Nota Anterior en Archivo Base</label>
          <input type="text" id="f-obs-previa" readonly style="background:#f3f4f6; color:#4b5563;">
        </div>

        <div class="form-group">
          <label for="f-obs-campo">3. Observación de la Inspección del Día *</label>
          <div class="chips" id="chips-container">
            <span class="chip" data-text="Cultivo en buen desarrollo vegetativo.">+ Buen desarrollo</span>
            <span class="chip" data-text="En espera de combustible para rastreo.">+ Espera gasoil</span>
            <span class="chip" data-text="Terreno preparado, próximo a sembrar.">+ Listo p/ sembrar</span>
            <span class="chip" data-text="Incidencia leve de plagas, se dio recomendación.">+ Presencia plagas</span>
            <span class="chip" data-text="Productor ausente en la unidad de producción.">+ Ausente</span>
          </div>
          <textarea id="f-obs-campo" rows="3" placeholder="Escriba aquí lo observado hoy en la parcela del productor..."></textarea>
        </div>

        <button type="button" class="btn btn-primary" id="btn-guardar">💾 Guardar Inspección del Productor</button>
      </div>
    </div>
  </div>

  <!-- TAB 2: VISITADOS Y LISTADO -->
  <div id="tab-visitados" class="section">
    <div class="card">
      <div class="card-title">
        <span>Registro de Visitas Realizadas</span>
        <button type="button" class="btn btn-outline btn-sm" id="btn-toggle-all">Ver todos (43)</button>
      </div>
      <p style="font-size:0.78rem; color:var(--text-muted); margin-bottom:10px;">
        Toque cualquier productor para editar su inspección.
      </p>
      <div id="visit-list-container"></div>
    </div>
  </div>

  <!-- TAB 3: EXPORTAR E IMPORTAR EXCEL -->
  <div id="tab-sincronizar" class="section">
    <div class="card">
      <div class="card-title">📤 Exportar Reporte Excel</div>
      <p style="font-size:0.82rem; color:#374151; margin-bottom:12px; line-height:1.4;">
        Genere el archivo Excel compatible con el formato oficial <strong>PAC FRIJOL MATURÍN</strong> más la columna de <strong>Observación de Campo</strong> para compartir por WhatsApp con el equipo.
      </p>
      <div class="grid-2">
        <button type="button" class="btn btn-primary" id="btn-export-vis">📊 Solo Visitados (<span id="btn-exp-vis">0</span>)</button>
        <button type="button" class="btn btn-accent" id="btn-export-all">📑 Base Completa (43)</button>
      </div>
    </div>

    <div class="card">
      <div class="card-title">📥 Cargar / Fusionar Excel de Compañero</div>
      <p style="font-size:0.82rem; color:#374151; margin-bottom:12px; line-height:1.4;">
        Seleccione el archivo <strong>.xls</strong> enviado por otro técnico. La app actualizará automáticamente los productores visitados por su compañero sin borrar los suyos.
      </p>
      <label class="file-drop" for="file-import">
        <div style="font-size:1.5rem; margin-bottom:4px;">📂</div>
        <div style="font-weight:700; color:var(--primary); font-size:0.88rem;">Tocar aquí para seleccionar archivo Excel</div>
        <div style="font-size:0.74rem; color:var(--text-muted); margin-top:2px;">Soporta archivos exportados por esta app</div>
        <input type="file" id="file-import" accept=".xls,.xlsx,.xml,.csv">
      </label>
      <div id="import-log" style="font-size:0.8rem; color:var(--primary); font-weight:600; margin-top:6px;"></div>
    </div>

    <div class="card">
      <div class="card-title">⚙️ Configuración y Respaldo</div>
      <div class="form-group">
        <label>Nombre del Técnico por Defecto (se guarda en el teléfono)</label>
        <input type="text" id="cfg-tecnico" placeholder="Ej: Ing. Francisco Alfonzo">
      </div>
      <button type="button" class="btn btn-danger btn-sm" id="btn-reset">🗑️ Reiniciar datos