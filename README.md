<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Checklist Cocina</title>
<link href="https://fonts.googleapis.com/css2?family=Google+Sans:wght@400;500;700&family=Roboto:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --purple: #673ab7;
    --purple-light: #ede7f6;
    --blue: #1a73e8;
    --green: #0f9d58;
    --orange: #e65100;
    --red: #d93025;
    --text: #202124;
    --text2: #5f6368;
    --border: #dadce0;
    --bg: #f0ebf8;
    --card: #ffffff;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg); font-family: 'Roboto', sans-serif; color: var(--text); min-height: 100vh; padding-bottom: 60px; }
  .top-bar { background: var(--purple); height: 10px; position: fixed; top: 0; left: 0; right: 0; z-index: 200; }
  .progress-track { position: fixed; top: 10px; left: 0; right: 0; height: 4px; background: rgba(103,58,183,0.2); z-index: 200; }
  .progress-fill { height: 100%; background: var(--purple); transition: width 0.5s ease; width: 0%; }
  .step-label { position: fixed; top: 18px; right: 16px; z-index: 200; background: var(--purple); color: white; font-size: 11px; padding: 3px 10px; border-radius: 12px; font-family: 'Roboto', sans-serif; display: none; }
  .wrapper { max-width: 640px; margin: 0 auto; padding: 30px 16px 16px; }
  .header-card { background: var(--card); border-radius: 8px; border-top: 10px solid var(--purple); padding: 24px 24px 20px; margin-bottom: 12px; box-shadow: 0 1px 3px rgba(0,0,0,0.12); }
  .header-card h1 { font-family: 'Google Sans', sans-serif; font-size: 26px; font-weight: 400; margin-bottom: 6px; }
  .header-card p { font-size: 14px; color: var(--text2); line-height: 1.5; }
  .page { display: none; }
  .page.active { display: block; }
  .section-card { background: var(--card); border-radius: 8px; margin-bottom: 12px; box-shadow: 0 1px 3px rgba(0,0,0,0.12); overflow: hidden; animation: fadeIn 0.3s ease; }
  @keyframes fadeIn { from { opacity:0; transform:translateY(8px); } to { opacity:1; transform:translateY(0); } }
  .section-header-bar { padding: 16px 20px; border-left: 4px solid var(--purple); }
  .section-header-bar.apertura { background: #fff8e1; border-color: #f9a825; }
  .section-header-bar.cierre { background: #fce4ec; border-color: #c62828; }
  .section-header-bar.info { background: var(--purple-light); border-color: var(--purple); }
  .section-header-bar h2 { font-family: 'Google Sans', sans-serif; font-size: 16px; font-weight: 500; }
  .section-header-bar.apertura h2 { color: #e65100; }
  .section-header-bar.cierre h2 { color: #c62828; }
  .section-header-bar.info h2 { color: var(--purple); }
  .sec-meta { font-size: 12px; color: var(--text2); margin-top: 4px; }
  .section-body { padding: 4px 0; }
  .check-item { display: flex; align-items: center; gap: 14px; padding: 12px 20px; cursor: pointer; border-bottom: 1px solid #f1f3f4; transition: background 0.15s; }
  .check-item:last-child { border-bottom: none; }
  .check-item:hover { background: #f8f9fa; }
  .check-item.checked { background: #f0fdf4; }
  .gcheckbox { width: 20px; height: 20px; min-width: 20px; border: 2px solid #9e9e9e; border-radius: 3px; display: flex; align-items: center; justify-content: center; transition: all 0.15s; background: white; }
  .check-item.checked .gcheckbox { background: var(--green); border-color: var(--green); }
  .gcheckbox svg { display: none; }
  .check-item.checked .gcheckbox svg { display: block; }
  .item-label { font-size: 14px; color: var(--text); line-height: 1.4; user-select: none; }
  .check-item.checked .item-label { color: var(--text2); }
  .field-group { padding: 14px 20px; border-bottom: 1px solid #f1f3f4; }
  .field-group:last-child { border-bottom: none; }
  .field-label { font-size: 14px; color: var(--text); margin-bottom: 8px; display: block; }
  .field-label .req { color: var(--red); margin-left: 2px; }
  .gfield { width: 100%; border: none; border-bottom: 1px solid var(--border); padding: 6px 0; font-size: 14px; color: var(--text); font-family: 'Roboto', sans-serif; outline: none; background: transparent; transition: border-color 0.2s; }
  .gfield:focus { border-bottom: 2px solid var(--purple); }
  .gselect { width: 100%; border: 1px solid var(--border); border-radius: 4px; padding: 9px 10px; font-size: 14px; font-family: 'Roboto', sans-serif; color: var(--text); outline: none; background: white; transition: border-color 0.2s; }
  .gselect:focus { border-color: var(--purple); }
  textarea.gfield { resize: vertical; min-height: 80px; border: 1px solid var(--border); border-radius: 4px; padding: 8px 10px; }
  .mode-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-top: 8px; }
  .mode-btn { border: 2px solid var(--border); border-radius: 12px; padding: 24px 16px; text-align: center; cursor: pointer; transition: all 0.2s; background: white; }
  .mode-btn.mode-ap.selected { border-color: #f9a825; background: #fffde7; }
  .mode-btn.mode-cl.selected { border-color: #c62828; background: #fce4ec; }
  .mode-icon { font-size: 36px; margin-bottom: 10px; }
  .mode-title { font-family: 'Google Sans', sans-serif; font-size: 16px; font-weight: 500; color: var(--text); margin-bottom: 4px; }
  .mode-desc { font-size: 12px; color: var(--text2); }
  .start-btn { width: 100%; padding: 13px; border-radius: 6px; border: none; background: var(--purple); color: white; font-size: 15px; font-family: 'Google Sans', sans-serif; font-weight: 500; cursor: pointer; margin-top: 16px; transition: background 0.2s; }
  .start-btn:disabled { background: #bdbdbd; cursor: not-allowed; }
  .nav-row { display: flex; justify-content: space-between; align-items: center; margin-top: 16px; flex-wrap: wrap; gap: 10px; }
  .btn-next { background: var(--purple); color: white; border: none; padding: 10px 24px; border-radius: 4px; font-size: 14px; font-family: 'Google Sans', sans-serif; font-weight: 500; cursor: pointer; }
  .btn-back { background: transparent; color: var(--purple); border: none; padding: 10px 16px; border-radius: 4px; font-size: 14px; font-family: 'Google Sans', sans-serif; font-weight: 500; cursor: pointer; }
  .btn-clear { font-size: 13px; color: var(--text2); background: none; border: none; cursor: pointer; padding: 4px 8px; }
  .done-card { background: var(--card); border-radius: 8px; padding: 32px 24px; text-align: center; box-shadow: 0 1px 3px rgba(0,0,0,0.12); }
  .done-icon { font-size: 52px; margin-bottom: 14px; }
  .done-card h2 { font-family: 'Google Sans', sans-serif; font-size: 22px; font-weight: 400; margin-bottom: 8px; }
  .done-card p { font-size: 14px; color: var(--text2); margin-bottom: 20px; }
  .done-email-lbl { font-size: 13px; color: var(--text2); margin-bottom: 6px; text-align: left; }
  .done-email-display { background: #f1f3f4; border-radius: 6px; padding: 10px 14px; font-size: 14px; color: var(--text); margin-bottom: 16px; text-align: left; word-break: break-all; }
  .btn-send { background: var(--green); color: white; border: none; padding: 12px 28px; border-radius: 6px; font-size: 15px; font-family: 'Google Sans', sans-serif; font-weight: 500; cursor: pointer; }
  .send-msg { font-size: 13px; margin-top: 10px; min-height: 20px; }
  .send-msg.ok { color: var(--green); }
  .send-msg.err { color: var(--red); }
  .summary-mini { margin: 16px 0; background: #f8f9fa; border-radius: 8px; padding: 14px 16px; text-align: left; }
  .summary-row { display: flex; justify-content: space-between; padding: 5px 0; font-size: 13px; border-bottom: 1px solid #e8eaed; }
  .summary-row:last-child { border-bottom: none; }
  .summary-row .lbl { color: var(--text2); }
  .summary-row .val { font-weight: 500; }
  .val.ok { color: var(--green); }
  .val.warn { color: #f29900; }
  .btn-new { background: none; border: 1px solid var(--border); color: var(--text2); padding: 9px 20px; border-radius: 4px; font-size: 13px; cursor: pointer; margin-top: 14px; font-family: 'Roboto', sans-serif; }
  .count-chip { display: inline-block; background: #e8eaed; border-radius: 10px; padding: 1px 8px; font-size: 11px; color: var(--text2); margin-left: 6px; }
  .count-chip.done { background: #e6f4ea; color: var(--green); }
</style>
</head>
<body>
<div class="top-bar"></div>
<div class="progress-track"><div class="progress-fill" id="prog"></div></div>
<div class="step-label" id="step-lbl"></div>
<div class="wrapper">
  <div class="header-card">
    <h1>🍽️ Checklist de Cocina</h1>
    <p>Sistema de control para apertura y cierre de turno.</p>
  </div>
  <div class="page active" id="page-inicio">
    <div class="section-card">
      <div class="section-header-bar info"><h2>Configuración del turno</h2><div class="sec-meta">Completa estos datos antes de comenzar</div></div>
      <div class="section-body">
        <div class="field-group"><label class="field-label">Correo para recibir el reporte <span class="req">*</span></label><input type="email" class="gfield" id="inicio-email" placeholder="gerente@restaurante.com"></div>
        <div class="field-group"><label class="field-label">Fecha <span class="req">*</span></label><input type="date" class="gfield" id="inicio-fecha"></div>
        <div class="field-group"><label class="field-label">Turno <span class="req">*</span></label><select class="gselect" id="inicio-turno"><option value="">Selecciona una opción</option><option>Mañana</option><option>Tarde</option><option>Noche</option></select></div>
        <div class="field-group"><label class="field-label">Tu nombre (responsable) <span class="req">*</span></label><input type="text" class="gfield" id="inicio-nombre" placeholder="Nombre completo"></div>
        <div class="field-group">
          <label class="field-label">¿Qué turno vas a registrar?</label>
          <div class="mode-grid">
            <div class="mode-btn mode-ap" id="btn-mode-ap" onclick="selectMode('apertura')"><div class="mode-icon">🌅</div><div class="mode-title">Apertura</div><div class="mode-desc">Inicio de turno · 9 secciones</div></div>
            <div class="mode-btn mode-cl" id="btn-mode-cl" onclick="selectMode('cierre')"><div class="mode-icon">🌙</div><div class="mode-title">Cierre</div><div class="mode-desc">Fin de turno · 4 secciones</div></div>
          </div>
          <button class="start-btn" id="start-btn" onclick="startChecklist()" disabled>Comenzar →</button>
        </div>
      </div>
    </div>
  </div>
  <div class="page" id="page-apertura">
    <div class="section-card" id="ap-sec-0"><div class="section-header-bar info"><h2>📋 Información de Apertura</h2><div class="sec-meta">Datos adicionales del turno</div></div><div class="section-body"><div class="field-group"><label class="field-label">Áreas activas hoy</label><select class="gselect" id="ap-areas"><option value="">Selecciona</option><option>Cocina principal</option><option>Pizzería</option><option>Ambas</option><option>Otra</option></select></div></div></div>
    <div class="section-card" id="ap-sec-1" style="display:none"><div class="section-header-bar apertura"><h2>🔹 Seguridad y Servicios Básicos</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="ap-body-1"></div></div>
    <div class="section-card" id="ap-sec-2" style="display:none"><div class="section-header-bar apertura"><h2>✅ Equipo Común</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="ap-body-2"></div></div>
    <div class="section-card" id="ap-sec-3" style="display:none"><div class="section-header-bar apertura"><h2>🔥 Estufa y Parrilla</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="ap-body-3"></div></div>
    <div class="section-card" id="ap-sec-4" style="display:none"><div class="section-header-bar apertura"><h2>💧 Freidoras</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="ap-body-4"></div></div>
    <div class="section-card" id="ap-sec-5" style="display:none"><div class="section-header-bar apertura"><h2>❄️ Línea Fría</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="ap-body-5"></div></div>
    <div class="section-card" id="ap-sec-6" style="display:none"><div class="section-header-bar apertura"><h2>🍕 Pizzería</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="ap-body-6"></div></div>
    <div class="section-card" id="ap-sec-7" style="display:none"><div class="section-header-bar apertura"><h2>🔹 Almacenamiento y Alimentos</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="ap-body-7"></div></div>
    <div class="section-card" id="ap-sec-8" style="display:none"><div class="section-header-bar apertura"><h2>🔹 Estaciones, Higiene y Personal</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="ap-body-8"></div></div>
    <div class="section-card" id="ap-sec-9" style="display:none"><div class="section-header-bar info"><h2>📝 Observaciones Finales</h2><div class="sec-meta">Registra notas, excepciones o pendientes</div></div><div class="section-body"><div class="field-group"><label class="field-label">Observaciones</label><textarea class="gfield" id="ap-obs" placeholder="Equipo con falla, tarea pendiente..."></textarea></div><div class="field-group"><label class="field-label">Firma</label><input type="text" class="gfield" id="ap-firma" placeholder="Escribe tu nombre como firma"></div></div></div>
    <div class="nav-row" id="ap-nav"><button class="btn-back" id="ap-btn-back" onclick="apBack()">← Atrás</button><div style="display:flex;gap:10px;align-items:center"><button class="btn-clear" onclick="clearCurrentAp()">Borrar sección</button><button class="btn-next" id="ap-btn-next" onclick="apNext()">Siguiente →</button></div></div>
  </div>
  <div class="page" id="page-cierre">
    <div class="section-card" id="cl-sec-0"><div class="section-header-bar info"><h2>📋 Información de Cierre</h2><div class="sec-meta">Datos del turno de cierre</div></div><div class="section-body"><div class="field-group"><label class="field-label">Nombre del responsable de cierre</label><input type="text" class="gfield" id="cl-resp" placeholder="Puede ser distinto al de apertura"></div></div></div>
    <div class="section-card" id="cl-sec-1" style="display:none"><div class="section-header-bar cierre"><h2>🔹 Seguridad y Apagado de Servicios</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="cl-body-1"></div></div>
    <div class="section-card" id="cl-sec-2" style="display:none"><div class="section-header-bar cierre"><h2>🧹 Limpieza de Equipos</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="cl-body-2"></div></div>
    <div class="section-card" id="cl-sec-3" style="display:none"><div class="section-header-bar cierre"><h2>📦 Almacenamiento, Residuos y Limpieza General</h2><div class="sec-meta">Verifica cada punto antes de marcar</div></div><div class="section-body" id="cl-body-3"></div></div>
    <div class="section-card" id="cl-sec-4" style="display:none"><div class="section-header-bar info"><h2>📝 Informe y Observaciones Finales</h2><div class="sec-meta">Cierra el turno con notas y firma</div></div><div class="section-body"><div class="field-group"><label class="field-label">Observaciones y excepciones</label><textarea class="gfield" id="cl-obs" placeholder="Equipo con falla, tarea pendiente..."></textarea></div><div class="field-group"><label class="field-label">Firma del responsable de cierre</label><input type="text" class="gfield" id="cl-firma" placeholder="Escribe tu nombre como firma"></div></div></div>
    <div class="nav-row" id="cl-nav"><button class="btn-back" id="cl-btn-back" onclick="clBack()">← Atrás</button><div style="display:flex;gap:10px;align-items:center"><button class="btn-clear" onclick="clearCurrentCl()">Borrar sección</button><button class="btn-next" id="cl-btn-next" onclick="clNext()">Siguiente →</button></div></div>
  </div>
  <div class="page" id="page-done">
    <div class="done-card">
      <div class="done-icon" id="done-icon">✅</div>
      <h2 id="done-title">¡Turno completado!</h2>
      <p id="done-desc">Revisa el resumen y envía el reporte.</p>
      <div class="done-email-lbl">Correo destino:</div>
      <div class="done-email-display" id="done-email-display">—</div>
      <div class="summary-mini" id="done-summary"></div>
      <button class="btn-send" onclick="sendReport()">📧 Enviar Reporte</button>
      <div class="send-msg" id="send-msg"></div><br>
      <button class="btn-new" onclick="goInicio()">↩ Registrar otro turno</button>
    </div>
  </div>
</div>
<script>
const AP_SECTIONS=[{id:1,items:["Salidas de emergencia despejadas","Detectores de humo y extintores revisados","Suministro de agua fría/caliente en buen estado","Luces eléctricas y de emergencia funcionando","Conexión de gas verificada (válvulas cerradas si no se usa)","Pisos y paredes limpios, sin mojaduras","Sistema de extracción en marcha"]},{id:2,items:["Lavavajillas probado (temperatura y funcionamiento)","Utensilios limpios, afilados y organizados","Ollas, sartenes y recipientes disponibles","Balanzas calibradas"]},{id:3,items:["Hornallas y hornos encendidos y probados","Parrilla encendida y temperatura ajustada","Rejillas y bandejas de goteo limpias"]},{id:4,items:["Nivel y calidad de aceite verificados","Temperatura de calentamiento probada","Filtros limpios y en su lugar"]},{id:5,items:["Refrigeradores (0°C a 5°C) verificados","Congeladores (≤-18°C) verificados","Exhibidores y mesas refrigeradas en marcha","Sellos de puertas en buen estado"]},{id:6,items:["Horno de pizza encendido y ajustado (250°C-350°C)","Mesas de amasado desinfectadas","Máquina de amasar en funcionamiento","Piedras/bandejas limpias"]},{id:7,items:["Alimentos revisados (sin descomposición)","Rotación FIFO aplicada","Alimentos etiquetados (nombre/fecha)","Insumos básicos completos","Productos frescos revisados"]},{id:8,items:["Superficies desinfectadas y organizadas","Utensilios específicos por estación disponibles","Preparaciones base listas","Vajilla y cubiertos en stock","Uniformes limpios y calzado de seguridad","Jabón, toallas y gel antibacterial disponibles","Chequeo de higiene personal realizado","Personal asignado confirmado"]}];
const CL_SECTIONS=[{id:1,items:["Todos los equipos eléctricos apagados y desconectados (excepto línea fría)","Válvulas de gas cerradas completamente","Grifos de agua cerrados","Luces no necesarias apagadas","Extintores y detectores revisados","Salidas de emergencia despejadas","Puertas y ventanas aseguradas"]},{id:2,items:["Lavavajillas vacío y limpio (filtros y tanques limpios)","Utensilios lavados, secados y guardados","Superficies de trabajo desinfectadas","Estufa y parrilla apagadas, rejillas y bandejas limpiadas","Interior de hornos limpiado","Aceite de freidoras filtrado o cambiado","Freidoras tapadas y área desinfectada","Alimentos en línea fría almacenados correctamente y etiquetados","Puertas de línea fría cerradas herméticamente","Exteriores de línea fría limpiados","Horno de pizza apagado y enfriado, interior limpiado","Mesas de amasado desinfectadas","Ingredientes sobrantes de pizzería almacenados"]},{id:3,items:["Todos los alimentos almacenados en lugar adecuado","Rotación FIFO actualizada","Residuos separados y sacados del establecimiento","Contenedores de residuos limpios y cerrados","Inventario básico registrado","Pisos barridos y fregados","Paredes y azulejos limpiados (manchas visibles)","Ventiladores y extractores limpiados (filtros)","Cajones y armarios organizados y limpios","Productos de limpieza guardados en lugar seguro"]}];
let mode=null,apStep=0,clStep=0;
let apState=AP_SECTIONS.map(s=>s.items.map(()=>false));
let clState=CL_SECTIONS.map(s=>s.items.map(()=>false));
function buildCheck(bodyId,sid,stateArr,prefix){const body=document.getElementById(bodyId);if(!body)return;body.innerHTML='';const allSecs=AP_SECTIONS.concat(CL_SECTIONS);stateArr.forEach((checked,ii)=>{const div=document.createElement('div');div.className='check-item'+(checked?' checked':'');div.id=`${prefix}-ci-${sid}-${ii}`;div.onclick=()=>toggleCI(prefix,sid,ii);div.innerHTML=`<div class="gcheckbox"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5L4.5 8.5L11 1.5" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div><span class="item-label">${allSecs.find(s=>s.id===sid)?.items[ii]||''}</span>`;body.appendChild(div);});}
function toggleCI(prefix,sid,ii){let stateArr=prefix==='ap'?apState:clState;let sections=prefix==='ap'?AP_SECTIONS:CL_SECTIONS;const secIdx=sections.findIndex(s=>s.id===sid);stateArr[secIdx][ii]=!stateArr[secIdx][ii];document.getElementById(`${prefix}-ci-${sid}-${ii}`).classList.toggle('checked',stateArr[secIdx][ii]);const done=stateArr[secIdx].filter(Boolean).length;const total=sections[secIdx].items.length;const hdr=document.querySelector(`#${prefix}-sec-${sid} .sec-meta`);if(hdr)hdr.innerHTML=`<span class="count-chip ${done===total?'done':''}">${done===total?'✓ Todo listo':`${done}/${total} marcados`}</span>`;}
function selectMode(m){mode=m;document.getElementById('btn-mode-ap').classList.toggle('selected',m==='apertura');document.getElementById('btn-mode-cl').classList.toggle('selected',m==='cierre');document.getElementById('start-btn').disabled=false;}
function startChecklist(){const email=document.getElementById('inicio-email').value.trim();const fecha=document.getElementById('inicio-fecha').value;const turno=document.getElementById('inicio-turno').value;const nombre=document.getElementById('inicio-nombre').value.trim();if(!email||!email.includes('@')){alert('Por favor ingresa un correo válido.');return;}if(!fecha){alert('Por favor ingresa la fecha.');return;}if(!turno){alert('Por favor selecciona el turno.');return;}if(!nombre){alert('Por favor ingresa tu nombre.');return;}if(!mode){alert('Por favor selecciona si es Apertura o Cierre.');return;}if(mode==='apertura'){apStep=0;AP_SECTIONS.forEach((s,i)=>{buildCheck('ap-body-'+(i+1),s.id,apState[i],'ap');});showApStep(0);showPage('apertura');}else{clStep=0;CL_SECTIONS.forEach((s,i)=>{buildCheck('cl-body-'+(i+1),s.id,clState[i],'cl');});showClStep(0);showPage('cierre');}}
function showApStep(step){for(let i=0;i<=9;i++){const el=document.getElementById('ap-sec-'+i);if(el)el.style.display=(i===step)?'':'none';}document.getElementById('ap-btn-back').style.display=step===0?'none':'';document.getElementById('ap-btn-next').textContent=step===9?'Finalizar ✓':'Siguiente →';document.getElementById('prog').style.width=Math.round((step/9)*100)+'%';document.getElementById('step-lbl').textContent=`Apertura ${step+1}/10`;document.getElementById('step-lbl').style.display='';window.scrollTo({top:0,behavior:'smooth'});}
function apNext(){if(apStep<9){apStep++;showApStep(apStep);}else{showDone();}}
function apBack(){if(apStep>0){apStep--;showApStep(apStep);}else{showPage('inicio');document.getElementById('step-lbl').style.display='none';document.getElementById('prog').style.width='0%';}}
function clearCurrentAp(){if(apStep===0||apStep===9)return;const sid=apStep;const secIdx=AP_SECTIONS.findIndex(s=>s.id===sid);if(secIdx<0)return;apState[secIdx]=apState[secIdx].map(()=>false);AP_SECTIONS[secIdx].items.forEach((_,ii)=>{const el=document.getElementById(`ap-ci-${sid}-${ii}`);if(el)el.classList.remove('checked');});}
function showClStep(step){for(let i=0;i<=4;i++){const el=document.getElementById('cl-sec-'+i);if(el)el.style.display=(i===step)?'':'none';}document.getElementById('cl-btn-back').style.display=step===0?'none':'';document.getElementById('cl-btn-next').textContent=step===4?'Finalizar ✓':'Siguiente →';document.getElementById('prog').style.width=Math.round((step/4)*100)+'%';document.getElementById('step-lbl').textContent=`Cierre ${step+1}/5`;document.getElementById('step-lbl').style.display='';window.scrollTo({top:0,behavior:'smooth'});}
function clNext(){if(clStep<4){clStep++;showClStep(clStep);}else{showDone();}}
function clBack(){if(clStep>0){clStep--;showClStep(clStep);}else{showPage('inicio');document.getElementById('step-lbl').style.display='none';document.getElementById('prog').style.width='0%';}}
function clearCurrentCl(){if(clStep===0||clStep===4)return;const sid=clStep;const secIdx=CL_SECTIONS.findIndex(s=>s.id===sid);if(secIdx<0)return;clState[secIdx]=clState[secIdx].map(()=>false);CL_SECTIONS[secIdx].items.forEach((_,ii)=>{const el=document.getElementById(`cl-ci-${sid}-${ii}`);if(el)el.classList.remove('checked');});}
function showDone(){document.getElementById('prog').style.width='100%';document.getElementById('step-lbl').style.display='none';document.getElementById('done-email-display').textContent=document.getElementById('inicio-email').value.trim();const isAp=mode==='apertura';document.getElementById('done-icon').textContent=isAp?'🌅':'🌙';document.getElementById('done-title').textContent=isAp?'¡Apertura completada!':'¡Cierre completado!';let html='';html+=row('Fecha',document.getElementById('inicio-fecha').value||'—');html+=row('Turno',document.getElementById('inicio-turno').value||'—');html+=row('Responsable',document.getElementById('inicio-nombre').value||'—');html+=row('Tipo',isAp?'🌅 Apertura':'🌙 Cierre');const st=isAp?apState:clState;const total=st.reduce((a,s)=>a+s.length,0);const done=st.reduce((a,s)=>a+s.filter(Boolean).length,0);html+=row('Ítems completados',`${done}/${total}`,done===total);document.getElementById('done-summary').innerHTML=html;showPage('done');}
function row(label,val,isOk){const cls=isOk===true?'ok':isOk===false?'warn':'';return`<div class="summary-row"><span class="lbl">${label}</span><span class="val ${cls}">${val}</span></div>`;}
function buildReport(){const fecha=document.getElementById('inicio-fecha').value||'—';const turno=document.getElementById('inicio-turno').value||'—';const nombre=document.getElementById('inicio-nombre').value||'—';const isAp=mode==='apertura';const apNames=["Seguridad y Servicios Básicos","Equipo Común","Estufa y Parrilla","Freidoras","Línea Fría","Pizzería","Almacenamiento y Alimentos","Estaciones, Higiene y Personal"];const clNames=["Seguridad y Apagado","Limpieza de Equipos","Almacenamiento, Residuos y Limpieza General"];let txt=`${isAp?'🌅 APERTURA':'🌙 CIERRE'} — REPORTE DE TURNO\n${'='.repeat(45)}\n`;txt+=`Fecha: ${fecha}  |  Turno: ${turno}\nResponsable: ${nombre}\n`;if(isAp){txt+=`Áreas: ${document.getElementById('ap-areas').value||'—'}\n\n`;const total=apState.reduce((a,s)=>a+s.length,0);const done=apState.reduce((a,s)=>a+s.filter(Boolean).length,0);txt+=`COMPLETADO: ${done}/${total} ítems\n\n${'─'.repeat(45)}\n`;AP_SECTIONS.forEach((sec,i)=>{const d=apState[i].filter(Boolean).length;txt+=`\n${apNames[i]} (${d}/${sec.items.length})\n`;sec.items.forEach((item,ii)=>{txt+=`  ${apState[i][ii]?'[✓]':'[ ]'} ${item}\n`;});});txt+=`\n${'─'.repeat(45)}\nOBSERVACIONES:\n${document.getElementById('ap-obs').value||'Ninguna'}\n\nFIRMA: ${document.getElementById('ap-firma').value||nombre}`;}else{txt+=`Resp. cierre: ${document.getElementById('cl-resp').value||'—'}\n\n`;const total=clState.reduce((a,s)=>a+s.length,0);const done=clState.reduce((a,s)=>a+s.filter(Boolean).length,0);txt+=`COMPLETADO: ${done}/${total} ítems\n\n${'─'.repeat(45)}\n`;CL_SECTIONS.forEach((sec,i)=>{const d=clState[i].filter(Boolean).length;txt+=`\n${clNames[i]} (${d}/${sec.items.length})\n`;sec.items.forEach((item,ii)=>{txt+=`  ${clState[i][ii]?'[✓]':'[ ]'} ${item}\n`;});});txt+=`\n${'─'.repeat(45)}\nOBSERVACIONES:\n${document.getElementById('cl-obs').value||'Ninguna'}\n\nFIRMA: ${document.getElementById('cl-firma').value||nombre}`;}return txt;}
function sendReport(){const email=document.getElementById('inicio-email').value.trim();const msg=document.getElementById('send-msg');const isAp=mode==='apertura';const subject=encodeURIComponent(`${isAp?'Apertura':'Cierre'} Cocina – ${document.getElementById('inicio-fecha').value||''} – Turno ${document.getElementById('inicio-turno').value||''}`);window.location.href=`mailto:${email}?subject=${subject}&body=${encodeURIComponent(buildReport())}`;msg.className='send-msg ok';msg.textContent='✓ Se abrió tu cliente de correo con el reporte listo.';}
function showPage(name){['inicio','apertura','cierre','done'].forEach(p=>{document.getElementById('page-'+p).classList.toggle('active',p===name);});}
function goInicio(){apStep=0;clStep=0;mode=null;apState=AP_SECTIONS.map(s=>s.items.map(()=>false));clState=CL_SECTIONS.map(s=>s.items.map(()=>false));document.getElementById('btn-mode-ap').classList.remove('selected');document.getElementById('btn-mode-cl').classList.remove('selected');document.getElementById('start-btn').disabled=true;document.getElementById('send-msg').textContent='';document.getElementById('prog').style.width='0%';document.getElementById('step-lbl').style.display='none';showPage('inicio');window.scrollTo({top:0,behavior:'smooth'});}
document.getElementById('inicio-fecha').valueAsDate=new Date();
</script>
</body>
</html>
