[calendario mauro.html](https://github.com/user-attachments/files/28397813/calendario.mauro.html)
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<title>Mauro · Planning</title>
<link href="https://fonts.googleapis.com/css2?family=Figtree:wght@400;600;700;800;900&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
* { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }

body {
  font-family: 'Figtree', sans-serif;
  background: #F5F5F5;
  color: #111;
  min-height: 100%;
}

html { min-height: 100%; }

.screen { display: none; flex-direction: column; background: #F5F5F5; min-height: 100vh; width: 100%; }
.screen.active { display: flex; }

/* ─── CABECERA CALENDARIO ─── */
.cal-head {
  background: #fff;
  padding: 50px 16px 12px;
  border-bottom: 2px solid #E0E0E0;
  flex-shrink: 0;
}

.cal-head-top { display: flex; align-items: center; justify-content: space-between; margin-bottom: 10px; }
.logo { font-size: 20px; font-weight: 900; color: #111; }
.logo span { color: #2563EB; }
.hoy-chip { background: #F0F0F0; border: 1px solid #DDD; border-radius: 20px; padding: 4px 12px; font-family: 'DM Mono', monospace; font-size: 10px; color: #666; }

.legend { display: flex; gap: 6px 14px; flex-wrap: wrap; }
.leg { display: flex; align-items: center; gap: 5px; font-size: 11px; font-weight: 700; color: #333; }
.leg-dot { width: 11px; height: 11px; border-radius: 3px; }

/* ─── GRID ─── */
.cal-body { flex: 1; overflow-y: auto; padding: 12px 10px 40px; }

.wd-row { display: grid; grid-template-columns: repeat(7,1fr); margin-bottom: 4px; }
.wd { text-align: center; font-size: 9px; font-family: 'DM Mono', monospace; color: #999; text-transform: uppercase; padding: 3px 0; }

.cal-grid { display: grid; grid-template-columns: repeat(7,1fr); gap: 5px; }

/* ─── CELDA ─── */
.cell {
  border-radius: 11px;
  min-height: 68px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  cursor: pointer;
  border: 2px solid transparent;
  transition: transform .1s;
  position: relative;
  overflow: hidden;
}
.cell:active { transform: scale(.87); }
.cell.empty { background: transparent !important; border: none; pointer-events: none; }

/* Número — siempre negro, grande, legible */
.cn { font-size: 16px; font-weight: 900; color: #111; line-height: 1; }
/* Tag turno */
.ct { font-family: 'DM Mono', monospace; font-size: 9px; font-weight: 800; color: #333; }
/* Porcentaje */
.cp {
  font-family: 'DM Mono', monospace; font-size: 8px; font-weight: 800;
  background: rgba(0,0,0,.12); color: #111;
  border-radius: 4px; padding: 1px 5px;
}

.cell.today { border-color: #111 !important; }
.cell.past { opacity: .28; filter: grayscale(40%); pointer-events: none; }
.cell.past .cn { text-decoration: line-through; color: #555; }

/* ─── PANTALLA DÍA ─── */
#screen-day { background: #F5F5F5; }

.day-head {
  padding: 50px 16px 12px;
  border-bottom: 2px solid rgba(0,0,0,.1);
  flex-shrink: 0;
}

.day-head-top { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; }

.back {
  width: 36px; height: 36px; border-radius: 10px;
  background: rgba(0,0,0,.15); border: 1.5px solid rgba(0,0,0,.2);
  display: flex; align-items: center; justify-content: center;
  font-size: 18px; font-weight: 800; color: #111; cursor: pointer; flex-shrink: 0;
}

.day-title-wrap { flex: 1; }
.day-title { font-size: 17px; font-weight: 900; color: #111; line-height: 1.1; }
.day-sub { font-size: 10px; font-family: 'DM Mono', monospace; color: #444; margin-top: 2px; }

.day-tag {
  font-size: 10px; font-weight: 800; padding: 5px 11px; border-radius: 20px;
  font-family: 'DM Mono', monospace; background: rgba(0,0,0,.12);
  color: #111; border: 1.5px solid rgba(0,0,0,.15); flex-shrink: 0;
}

/* Barra progreso */
.prog-row { display: flex; align-items: center; gap: 8px; }
.prog-track { flex: 1; height: 7px; background: rgba(0,0,0,.15); border-radius: 10px; overflow: hidden; }
.prog-fill { height: 100%; border-radius: 10px; background: #111; transition: width .3s; width: 0%; }
.prog-pct { font-family: 'DM Mono', monospace; font-size: 13px; font-weight: 800; color: #111; min-width: 36px; text-align: right; }

/* ─── CHECKLIST ─── */
.chk-scroll { flex: 1; overflow-y: auto; padding: 10px 12px 40px; background: #F5F5F5; }

/* Item — fondo BLANCO, texto NEGRO siempre */
.chk-item {
  display: flex; align-items: center; gap: 10px;
  background: #FFFFFF;
  border: 1.5px solid #E0E0E0;
  border-radius: 12px;
  padding: 12px 12px 12px 0;
  margin-bottom: 7px;
  cursor: pointer;
  transition: opacity .2s;
  -webkit-user-select: none; user-select: none;
  overflow: hidden;
  box-shadow: 0 1px 4px rgba(0,0,0,.06);
}
.chk-item:active { opacity: .6; }
.chk-item.done { opacity: .4; background: #F9F9F9; }

.chk-bar { width: 5px; align-self: stretch; min-height: 40px; flex-shrink: 0; border-radius: 0 3px 3px 0; }
.chk-icon { font-size: 22px; flex-shrink: 0; }
.chk-body { flex: 1; min-width: 0; }
.chk-time { font-family: 'DM Mono', monospace; font-size: 10px; color: #888; margin-bottom: 2px; }
.chk-name { font-size: 14px; font-weight: 800; color: #111; line-height: 1.2; }
.chk-detail { font-size: 12px; color: #555; margin-top: 3px; line-height: 1.35; }

.chk-circle {
  width: 26px; height: 26px; border-radius: 50%;
  border: 2px solid #CCC;
  display: flex; align-items: center; justify-content: center;
  font-size: 14px; font-weight: 900;
  flex-shrink: 0; margin-right: 2px;
  color: transparent; transition: all .18s;
}
.chk-item.done .chk-circle { background: #16A34A; border-color: #16A34A; color: white; }

/* ─── URGENTE ─── */
.chk-item.urgente {
  background: linear-gradient(135deg, #FFFBEB 0%, #FEF3C7 100%);
  border: 2px solid #F59E0B;
  border-radius: 14px;
  box-shadow: 0 2px 8px rgba(245,158,11,0.25);
  padding: 13px 10px;
}
.chk-item.urgente .chk-name { font-size: 15px; color: #92400E; }
.chk-item.urgente .chk-time { color: #B45309; font-weight: 700; }
.chk-item.urgente .chk-bar { background: #F59E0B !important; width: 6px; }
.urgente-badge {
  display:inline-block; background:#F59E0B; color:#fff;
  font-size:9px; font-weight:900; letter-spacing:.08em;
  padding:2px 7px; border-radius:20px; margin-bottom:4px;
  text-transform:uppercase;
}
.b-urgente { background: #F59E0B; }

/* Barras laterales colores */
.b-opo     { background: #7C3AED; }
.b-gym     { background: #EA580C; }
.b-trabajo { background: #2563EB; }
.b-dieta   { background: #0891B2; }
.b-clinica { background: #059669; }
.b-master  { background: #DB2777; }
.b-desc    { background: #CBD5E1; }
.b-danger  { background: #DC2626; }

/* Celebración */
.cel { display:none; background:#fff; border:2px solid #86EFAC; border-radius:14px; padding:18px; margin-bottom:10px; text-align:center; box-shadow:0 2px 8px rgba(0,0,0,.08); }
.cel.show { display:block; }
.cel-e { font-size:34px; }
.cel-t { font-size:15px; font-weight:900; color:#16A34A; margin-top:6px; }
.cel-s { font-size:12px; color:#666; margin-top:3px; }

.reset-btn { width:100%; margin-top:6px; padding:12px; background:#fff; border:1.5px solid #DDD; border-radius:10px; font-family:'DM Mono',monospace; font-size:11px; color:#888; cursor:pointer; text-align:center; }

::-webkit-scrollbar { width:0; }

/* ─── PANEL AÑADIR ACTIVIDAD ─── */
.add-overlay {
  display:none; position:fixed; inset:0; background:rgba(0,0,0,.55);
  z-index:100; align-items:flex-end; justify-content:center;
}
.add-overlay.open { display:flex; }

.add-panel {
  background:#fff; border-radius:22px 22px 0 0; width:100%; max-width:480px;
  padding:20px 16px 40px; max-height:85vh; overflow-y:auto;
}
.add-panel-title { font-size:16px; font-weight:900; color:#111; margin-bottom:14px; }

.pre-grid {
  display:grid; grid-template-columns:repeat(3,1fr); gap:8px; margin-bottom:16px;
}
.pre-btn {
  background:#F5F5F5; border:1.5px solid #E0E0E0; border-radius:12px;
  padding:10px 6px; text-align:center; cursor:pointer; transition:all .15s;
  -webkit-user-select:none; user-select:none;
}
.pre-btn:active { transform:scale(.93); }
.pre-btn.sel { background:#2563EB; border-color:#2563EB; }
.pre-btn.sel .pre-icon, .pre-btn.sel .pre-name { color:#fff; }
.pre-icon { font-size:22px; display:block; margin-bottom:3px; }
.pre-name { font-size:10px; font-weight:800; color:#333; line-height:1.2; }

.add-sep { font-size:11px; font-weight:800; color:#999; margin-bottom:8px; text-transform:uppercase; letter-spacing:.05em; }

.add-input {
  width:100%; border:1.5px solid #E0E0E0; border-radius:10px;
  padding:10px 12px; font-family:'Figtree',sans-serif; font-size:14px;
  font-weight:700; color:#111; background:#F9F9F9; margin-bottom:12px;
  outline:none;
}
.add-input:focus { border-color:#2563EB; background:#fff; }

.time-row { display:flex; gap:8px; margin-bottom:16px; align-items:center; }
.time-row label { font-size:11px; font-weight:800; color:#666; display:block; margin-bottom:4px; }
.time-row input[type=time] {
  border:1.5px solid #E0E0E0; border-radius:10px; padding:9px 10px;
  font-family:'DM Mono',monospace; font-size:13px; color:#111; background:#F9F9F9;
  width:100%; outline:none;
}
.time-row input[type=time]:focus { border-color:#2563EB; }
.time-sep { font-size:18px; color:#999; padding-top:20px; }

.cat-row { display:flex; gap:6px; flex-wrap:wrap; margin-bottom:16px; }
.cat-chip {
  padding:5px 11px; border-radius:20px; border:1.5px solid #E0E0E0;
  font-size:11px; font-weight:800; cursor:pointer; background:#F5F5F5; color:#333;
  transition:all .15s;
}
.cat-chip.sel { color:#fff; border-color:transparent; }

.add-confirm {
  width:100%; padding:14px; background:#111; color:#fff; border:none;
  border-radius:12px; font-family:'Figtree',sans-serif; font-size:15px; font-weight:900;
  cursor:pointer; margin-bottom:8px;
}
.add-cancel {
  width:100%; padding:11px; background:transparent; color:#888; border:none;
  font-family:'Figtree',sans-serif; font-size:14px; cursor:pointer;
}
</style>
</head>
<body>

<div class="screen active" id="screen-cal">
  <div class="cal-head">
    <div class="cal-head-top">
      <div class="logo"><span id="mes-titulo">Mayo</span></div>
      <div class="hoy-chip" id="chip">–</div>
    </div>
    <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:5px;margin-bottom:8px;">
      <div onclick="switchMes(0)" id="tab-mayo"  style="text-align:center;padding:6px 2px;border-radius:8px;font-weight:800;font-size:11px;cursor:pointer;background:#2563EB;color:white;">Mayo</div>
      <div onclick="switchMes(1)" id="tab-junio" style="text-align:center;padding:6px 2px;border-radius:8px;font-weight:800;font-size:11px;cursor:pointer;background:#F0F0F0;color:#666;border:1.5px solid #DDD;">Jun</div>
      <div onclick="switchMes(2)" id="tab-julio" style="text-align:center;padding:6px 2px;border-radius:8px;font-weight:800;font-size:11px;cursor:pointer;background:#F0F0F0;color:#666;border:1.5px solid #DDD;">Jul</div>
      <div onclick="switchMes(3)" id="tab-agosto" style="text-align:center;padding:6px 2px;border-radius:8px;font-weight:800;font-size:11px;cursor:pointer;background:#F0F0F0;color:#666;border:1.5px solid #DDD;">Ago</div>
    </div>
    <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:5px;margin-bottom:8px;">
      <div onclick="switchMes(4)" id="tab-septiembre" style="text-align:center;padding:6px 2px;border-radius:8px;font-weight:800;font-size:11px;cursor:pointer;background:#F0F0F0;color:#666;border:1.5px solid #DDD;">Sep</div>
      <div onclick="switchMes(5)" id="tab-octubre"    style="text-align:center;padding:6px 2px;border-radius:8px;font-weight:800;font-size:11px;cursor:pointer;background:#F0F0F0;color:#666;border:1.5px solid #DDD;">Oct</div>
      <div onclick="switchMes(6)" id="tab-noviembre"  style="text-align:center;padding:6px 2px;border-radius:8px;font-weight:800;font-size:11px;cursor:pointer;background:#F0F0F0;color:#666;border:1.5px solid #DDD;">Nov</div>
    </div>
    <div class="legend">
      <div class="leg"><div class="leg-dot" style="background:#2563EB"></div>Mañana</div>
      <div class="leg"><div class="leg-dot" style="background:#16A34A"></div>Tarde</div>
      <div class="leg"><div class="leg-dot" style="background:#D97706"></div>Noche</div>
      <div class="leg"><div class="leg-dot" style="background:#DC2626"></div>Libre</div>
    </div>
  </div>
  <div class="cal-body">
    <div class="wd-row">
      <div class="wd">L</div><div class="wd">M</div><div class="wd">X</div>
      <div class="wd">J</div><div class="wd">V</div><div class="wd">S</div><div class="wd">D</div>
    </div>
    <div class="cal-grid" id="cal-grid"></div>
  </div>
</div>

<div class="screen" id="screen-day">
  <div class="day-head" id="day-head">
    <div class="day-head-top">
      <div class="back" onclick="goBack()">←</div>
      <div class="day-title-wrap">
        <div class="day-title" id="d-title">–</div>
        <div class="day-sub" id="d-sub">–</div>
      </div>
      <div class="day-tag" id="d-tag">–</div>
    </div>
    <div class="prog-row">
      <div class="prog-track"><div class="prog-fill" id="prog-fill"></div></div>
      <div class="prog-pct" id="prog-pct">0%</div>
    </div>
  </div>
  <div class="chk-scroll">
    <div style="height:8px"></div>
    <div class="cel" id="cel">
      <div class="cel-e">🔥</div>
      <div class="cel-t">¡Día completado!</div>
      <div class="cel-s">Eso es constancia. Mañana más.</div>
    </div>
    <div id="chk-list"></div>
    <button class="add-act-btn" onclick="openAddPanel()" style="width:100%;margin-top:6px;padding:13px;background:#2563EB;color:#fff;border:none;border-radius:10px;font-family:'Figtree',sans-serif;font-size:14px;font-weight:900;cursor:pointer;text-align:center;">+ Añadir actividad</button>
    <button class="reset-btn" onclick="resetDay()">↺ Reiniciar día</button>
  </div>
</div>

<!-- PANEL AÑADIR ACTIVIDAD -->
<div class="add-overlay" id="add-overlay" onclick="closeAddPanel(event)">
  <div class="add-panel" id="add-panel">
    <div class="add-panel-title">➕ Añadir actividad</div>
    <div class="pre-grid" id="pre-grid"></div>
    <div class="add-sep">O escribe la tuya</div>
    <input class="add-input" id="add-name-input" type="text" placeholder="Nombre de la actividad..." />
    <div class="add-sep">Horario</div>
    <div class="time-row">
      <div style="flex:1"><label>Desde</label><input type="time" id="add-time-start" /></div>
      <div class="time-sep">–</div>
      <div style="flex:1"><label>Hasta</label><input type="time" id="add-time-end" /></div>
    </div>
    <div class="add-sep">Categoría (color)</div>
    <div class="cat-row" id="cat-row"></div>
    <button class="add-confirm" onclick="confirmAddAct()">Añadir al día</button>
    <button class="add-cancel" onclick="closeAddPanel()">Cancelar</button>
  </div>
</div>

<script>
const MESES_DATA = {
  mayo: {
    nombre: 'Mayo', dias: 31, offset: 4, mesNum: 5,
    turnos: {
      1:'M',2:'M',3:'T',4:'T',5:'N1',6:'NB',7:'L',
      8:'L',9:'L',10:'L',11:'M',12:'M',13:'T',14:'T',
      15:'N1',16:'NB',17:'L',18:'L',19:'L',20:'L',21:'M',
      22:'M',23:'T',24:'T',25:'N1',26:'NB',27:'L',28:'L',
      29:'L',30:'L',31:'M'
    }
  },
  junio: {
    nombre: 'Junio', dias: 30, offset: 0, mesNum: 6,
    turnos: {
      1:'M',2:'T',3:'T',4:'N1',5:'NB',6:'L',7:'L',
      8:'L',9:'L',10:'M',11:'M',12:'T',13:'T',14:'N1',
      15:'NB',16:'L',17:'L',18:'L',19:'L',20:'M',21:'M',
      22:'T',23:'T',24:'N1',25:'NB',26:'L',27:'L',28:'L',
      29:'L',30:'M'
    }
  },
  julio: {
    nombre: 'Julio', dias: 31, offset: 2, mesNum: 7,
    turnos: {
      1:'M',2:'T',3:'T',4:'N1',5:'NB',6:'L',7:'L',
      8:'L',9:'L',10:'M',11:'M',12:'T',13:'T',14:'N1',
      15:'NB',16:'L',17:'L',18:'L',19:'L',20:'M',21:'M',
      22:'T',23:'T',24:'N1',25:'NB',26:'L',27:'L',28:'L',
      29:'L',30:'M',31:'M'
    }
  },
  agosto: {
    nombre: 'Agosto', dias: 31, offset: 5, mesNum: 8,
    turnos: {
      1:'T',2:'T',3:'N1',4:'NB',5:'L',6:'L',7:'L',
      8:'L',9:'M',10:'M',11:'T',12:'T',13:'N1',14:'NB',
      15:'L',16:'L',17:'L',18:'L',19:'M',20:'M',21:'T',
      22:'T',23:'N1',24:'NB',25:'L',26:'L',27:'L',28:'L',
      29:'M',30:'M',31:'T'
    }
  },
  septiembre: {
    nombre: 'Septiembre', dias: 30, offset: 1, mesNum: 9,
    turnos: {
      1:'T',2:'N1',3:'NB',4:'L',5:'L',6:'L',7:'L',
      8:'M',9:'M',10:'T',11:'T',12:'N1',13:'NB',14:'L',
      15:'L',16:'L',17:'L',18:'M',19:'M',20:'T',21:'T',
      22:'N1',23:'NB',24:'L',25:'L',26:'L',27:'L',28:'M',
      29:'M',30:'T'
    }
  },
  octubre: {
    nombre: 'Octubre', dias: 31, offset: 3, mesNum: 10,
    turnos: {
      1:'T',2:'N1',3:'NB',4:'L',5:'L',6:'L',7:'L',
      8:'M',9:'M',10:'T',11:'T',12:'N1',13:'NB',14:'L',
      15:'L',16:'L',17:'L',18:'M',19:'M',20:'T',21:'T',
      22:'N1',23:'NB',24:'L',25:'L',26:'L',27:'L',28:'M',
      29:'M',30:'T',31:'T'
    }
  },
  noviembre: {
    nombre: 'Noviembre', dias: 30, offset: 6, mesNum: 11,
    turnos: {
      1:'N1',2:'NB',3:'L',4:'L',5:'L',6:'L',7:'M',
      8:'M',9:'T',10:'T',11:'N1',12:'NB',13:'L',14:'L',
      15:'L',16:'L',17:'M',18:'M',19:'T',20:'T',21:'N1',
      22:'NB',23:'L',24:'L',25:'L',26:'L',27:'M',28:'M',
      29:'T',30:'T'
    }
  }
};

// Ya no necesitamos el objeto MAYO solo
const MAYO = {
  1:'M',2:'M',3:'T',4:'T',5:'N1',6:'NB',7:'NS',
  8:'L',9:'L',10:'L',11:'M',12:'M',13:'T',14:'T',
  15:'N1',16:'NB',17:'NS',18:'L',19:'L',20:'L',
  21:'M',22:'M',23:'T',24:'M',25:'N1',26:'NB',27:'NS',
  28:'L',29:'L',30:'L',31:'M'
};

// Colores: intenso sin completar → claro al completar
const COL = {
  M:  { full:'#2563EB', light:'#BFDBFE', tag:'M', label:'Mañana',       sub:'Sale 7:25h · Llega 15:20h' },
  T:  { full:'#16A34A', light:'#BBF7D0', tag:'T', label:'Tarde',        sub:'Sale 14:40h · Llega 22:20h' },
  N1: { full:'#EAB308', light:'#FEF08A', tag:'N', label:'Entrada Noche',sub:'Día libre · Entra 22:00h' },
  NB: { full:'#EAB308', light:'#FEF08A', tag:'N', label:'Entre Noches', sub:'Sale 8:20h · Bisagra · Entra 22:00h' },
  NS: { full:'#DC2626', light:'#FECACA', tag:'N', label:'Saliente Noche',  sub:'Sale 8:20h · El más duro' },
  L:  { full:'#DC2626', light:'#FECACA', tag:'L', label:'Día Libre',    sub:'Sin turno · A tope' },
};

const BLOCKS = {
  M:[
    {time:'06:00–06:45',icon:'📖',name:'Oposición — 45 min',detail:'Primer bloque antes del turno. Alarma 6:00h.',bar:'b-opo'},
    {time:'06:45–07:25',icon:'🍳',name:'Desayuno + preparación',detail:'Sale de casa 7:25h para llegar a las 8:00h.',bar:'b-desc'},
    {time:'08:00–15:00',icon:'🏥',name:'Turno enfermería',detail:'Llega 8:00h · Sale 15:00h · En casa ~15:20h.',bar:'b-trabajo'},
    {time:'15:20–16:30',icon:'🍽️',name:'Llegada · Comida · Descanso',detail:'70 min. Come bien y descansa antes del gym.',bar:'b-desc'},
    {time:'16:30–18:30',icon:'💪',name:'Gym — si toca hoy',detail:'20 min andando c/lado · ~1h20 sala · 2h total.',bar:'b-gym'},
    {time:'18:30–18:55',icon:'🥗',name:'Dieta MASSQNUTRICIÓN — 1 paciente',detail:'25 min. Adelanto del viernes.',bar:'b-dieta'},
    {time:'18:55–19:25',icon:'📖',name:'Oposición — 30 min',detail:'Completa 1h15 del día.',bar:'b-opo'},
    {time:'19:25 →',icon:'🌙',name:'Noche libre',detail:'Cena, vida. Duerme antes de las 23h.',bar:'b-desc'},
  ],
  T:[
    {time:'07:30–08:45',icon:'📖',name:'Oposición — 1h15 completa',detail:'Tu mejor momento. Cabeza fresca.',bar:'b-opo'},
    {time:'08:45–09:15',icon:'☕',name:'Desayuno',detail:'Pausa real entre opo y gym.',bar:'b-desc'},
    {time:'09:15–11:15',icon:'💪',name:'Gym — si toca hoy',detail:'2h total. Gym tranquilo a esta hora.',bar:'b-gym'},
    {time:'11:15–11:40',icon:'🥗',name:'Dieta MASSQNUTRICIÓN — 1 paciente',detail:'25 min. Siempre.',bar:'b-dieta'},
    {time:'11:40–13:30',icon:'🏢',name:'MASSQNUTRICIÓN — clínica',detail:'Consultas y gestión antes del turno.',bar:'b-clinica'},
    {time:'13:30–14:40',icon:'🍽️',name:'Comida + preparación turno',detail:'Sale de casa 14:40h para llegar a las 15:00h.',bar:'b-desc'},
    {time:'15:00–22:00',icon:'🏥',name:'Turno enfermería',detail:'Todo hecho. A trabajar.',bar:'b-trabajo'},
    {time:'22:20 →',icon:'🌃',name:'Vuelta · cena · dormir',detail:'Nada de opo. Has cumplido.',bar:'b-desc'},
  ],
  N1:[
    {time:'08:30–09:45',icon:'📖',name:'Oposición — 1h15',detail:'Sin prisa. Tienes el día entero.',bar:'b-opo'},
    {time:'09:45–10:15',icon:'☕',name:'Desayuno tranquilo',detail:'',bar:'b-desc'},
    {time:'10:15–12:15',icon:'💪',name:'Gym — si toca hoy',detail:'Hazlo por la mañana. Esta noche trabajas.',bar:'b-gym'},
    {time:'12:15–12:40',icon:'🥗',name:'Dieta MASSQNUTRICIÓN — 1 paciente',detail:'25 min.',bar:'b-dieta'},
    {time:'12:40–14:30',icon:'🏢',name:'MASSQNUTRICIÓN — clínica',detail:'Buen momento antes de la noche.',bar:'b-clinica'},
    {time:'14:30–17:30',icon:'😴',name:'Comida + siesta obligatoria',detail:'3h. Vas a trabajar toda la noche.',bar:'b-desc'},
    {time:'17:30–19:30',icon:'📚',name:'Máster / trámites',detail:'Si tienes energía. Si no, más descanso.',bar:'b-master'},
    {time:'19:30–21:40',icon:'🍽️',name:'Cena + preparación',detail:'Sale de casa 21:40h.',bar:'b-desc'},
    {time:'22:00–08:00',icon:'🏥',name:'Turno de noche (1ª)',detail:'Has hecho todo lo que tocaba.',bar:'b-trabajo'},
  ],
  NB:[
    {time:'08:20–15:00',icon:'😴',name:'Sueño — mínimo 6h',detail:'Llegas de noche. Sin alarma. Sin culpa.',bar:'b-desc'},
    {time:'15:00–15:30',icon:'🍽️',name:'Comida',detail:'Tu cuerpo está recuperando.',bar:'b-desc'},
    {time:'15:30–16:00',icon:'📖',name:'Oposición — 30 min',detail:'Solo 30 min. Repaso. Pero hoy también.',bar:'b-opo'},
    {time:'16:00–16:25',icon:'🥗',name:'Dieta — si puedes',detail:'25 min si el cuerpo responde. Si no, se salta.',bar:'b-dieta'},
    {time:'16:30–21:40',icon:'🛋️',name:'Descanso · cena · preparación',detail:'Sin gym hoy. Sale 21:40h para la 2ª noche.',bar:'b-desc'},
    {time:'22:00–08:00',icon:'🏥',name:'Turno de noche (2ª)',detail:'Segunda noche seguida.',bar:'b-trabajo'},
  ],
  NS:[
    {time:'08:20–16:00',icon:'😴',name:'Sueño — prioridad total',detail:'7h mínimo sin alarma. Dos noches encima.',bar:'b-danger'},
    {time:'16:00–16:30',icon:'🍽️',name:'Comida',detail:'Come algo nutritivo.',bar:'b-desc'},
    {time:'16:30–17:00',icon:'📖',name:'Oposición — 30 min',detail:'Solo esto hoy. La constancia se forja aquí.',bar:'b-opo'},
    {time:'17:00 →',icon:'🌙',name:'Recuperación total',detail:'Sin gym. Sin clínica. Sin culpa.',bar:'b-desc'},
  ],
  L:[
    {time:'07:30–08:15',icon:'📖',name:'Oposición — 45 min',detail:'Primer bloque. Cabeza fresca.',bar:'b-opo'},
    {time:'08:15–09:00',icon:'☕',name:'Desayuno',detail:'',bar:'b-desc'},
    {time:'09:00–11:00',icon:'💪',name:'Gym',detail:'2h total. Fuerza o Hyrox.',bar:'b-gym'},
    {time:'11:00–12:15',icon:'🥗',name:'Dietas — 2-3 pacientes',detail:'Día libre = adelantar dietas. 50-75 min.',bar:'b-dieta'},
    {time:'12:15–14:15',icon:'🏢',name:'MASSQNUTRICIÓN — clínica 2h',detail:'Bloque grande en día libre.',bar:'b-clinica'},
    {time:'14:15–15:30',icon:'🍽️',name:'Comida · descanso',detail:'Para. Mereces la pausa.',bar:'b-desc'},
    {time:'15:30–16:00',icon:'📖',name:'Oposición — 30 min',detail:'Completa 1h15 del día.',bar:'b-opo'},
    {time:'16:00–17:30',icon:'📚',name:'Máster dermoestética',detail:'El día libre es cuando toca el máster.',bar:'b-master'},
    {time:'17:30 →',icon:'🌙',name:'Vida',detail:'Has hecho todo. Descansa sin culpa.',bar:'b-desc'},
  ],
};

let curKey=null,curType=null,curMes='mayo';

const MESES_LIST = ['mayo','junio','julio','agosto','septiembre','octubre','noviembre'];
const MESES_TABS = ['tab-mayo','tab-junio','tab-julio','tab-agosto','tab-septiembre','tab-octubre','tab-noviembre'];

function switchMes(idx){
  curMes = MESES_LIST[idx];
  MESES_TABS.forEach((tabId, i) => {
    const el = document.getElementById(tabId);
    if(!el) return;
    el.style.cssText = i===idx
      ? 'text-align:center;padding:6px 2px;border-radius:8px;font-weight:800;font-size:11px;cursor:pointer;background:#2563EB;color:white;'
      : 'text-align:center;padding:6px 2px;border-radius:8px;font-weight:800;font-size:11px;cursor:pointer;background:#F0F0F0;color:#666;border:1.5px solid #DDD;';
  });
  document.getElementById('mes-titulo').textContent = MESES_DATA[curMes].nombre;
  buildCal();
}
const k=d=>'mayo-'+d;
const getC=k=>{try{return JSON.parse(localStorage.getItem('c-'+k)||'[]')}catch(e){return[]}};
const setC=(k,a)=>{try{localStorage.setItem('c-'+k,JSON.stringify(a))}catch(e){}};

// Interpola hex: 0% = color intenso, 100% = color claro
function lerp(a,b,t){
  const h=c=>[parseInt(c.slice(1,3),16),parseInt(c.slice(3,5),16),parseInt(c.slice(5,7),16)];
  const [ar,ag,ab]=h(a), [br,bg,bb]=h(b);
  return `rgb(${Math.round(ar+(br-ar)*t)},${Math.round(ag+(bg-ag)*t)},${Math.round(ab+(bb-ab)*t)})`;
}

function buildCal(){
  const grid=document.getElementById('cal-grid');
  grid.innerHTML='';
  const today=new Date();
  const mesData=MESES_DATA[curMes];
  const isEsteMes=today.getMonth()+1===mesData.mesNum;
  const td=today.getDate();
  const dias=['Dom','Lun','Mar','Mié','Jue','Vie','Sáb'];
  const meses=['Ene','Feb','Mar','Abr','May','Jun','Jul','Ago','Sep','Oct','Nov','Dic'];
  document.getElementById('chip').textContent=dias[today.getDay()]+' '+td+' '+meses[today.getMonth()];

  // Mes ya pasado completamente
  const mesPasado = today.getMonth()+1 > mesData.mesNum;
  // Mes futuro completamente
  const mesFuturo = today.getMonth()+1 < mesData.mesNum;

  for(let i=0;i<mesData.offset;i++){const e=document.createElement('div');e.className='cell empty';grid.appendChild(e);}

  for(let d=1;d<=mesData.dias;d++){
    const type=mesData.turnos[d]||'L';
    const col=COL[type];
    const dayKey=curMes+'-'+d;
    const chks=getC(dayKey);
    const total=(BLOCKS[type]||[]).length;
    const done=chks.filter(Boolean).length;
    const pct=total?Math.round(done/total*100):0;
    const isToday=isEsteMes&&d===td;
    const isPast=mesPasado||(isEsteMes&&d<td);
    const isFuture=mesFuturo||(isEsteMes&&d>td);

    const bg=total>0?lerp(col.full,col.light,pct/100):col.full;

    const cell=document.createElement('div');
    cell.className='cell'+(isToday?' today':'')+(isPast?' past':'');
    cell.style.background=bg;
    cell.style.borderColor=isToday?'#111':bg;

    let pctHtml='';
    if(total>0){
      if(pct===100) pctHtml='<div class="cp">✓ 100%</div>';
      else if(pct>0) pctHtml=`<div class="cp">${pct}%</div>`;
    }

    cell.innerHTML=`<div class="cn">${d}</div><div class="ct">${col.tag}</div>${pctHtml}`;
    if(!isPast) cell.onclick=()=>openDay(d,type,dayKey);
    grid.appendChild(cell);
  }
}

function openDay(day,type,dayKey){
  curKey=dayKey||k(day); curType=type;
  const col=COL[type];
  const wd=['Lun','Mar','Mié','Jue','Vie','Sáb','Dom'];
  const mesNombre=MESES_DATA[curMes].nombre; document.getElementById('d-title').textContent=wd[(MESES_DATA[curMes].offset+day-1)%7]+' '+day+' de '+mesNombre;
  document.getElementById('d-sub').textContent=col.sub;
  document.getElementById('d-tag').textContent=col.label;

  // Header con color del turno de fondo — texto NEGRO
  const head=document.getElementById('day-head');
  head.style.background=col.light;
  head.style.borderBottomColor=col.full+'66';

  renderChk();
  document.getElementById('screen-cal').classList.remove('active');
  document.getElementById('screen-day').classList.add('active');
  document.querySelector('.chk-scroll').scrollTop=0;
}

function goBack(){
  document.getElementById('screen-day').classList.remove('active');
  document.getElementById('screen-cal').classList.add('active');
  buildCal();
}



function tog(i){
  const blocks=BLOCKS[curType]||[];
  let chks=getC(curKey);
  while(chks.length<blocks.length) chks.push(false);
  chks[i]=!chks[i]; setC(curKey,chks); renderChk();
}



function resetDay(){
  // Resetea fijos y personalizados
  setC(curKey,[]);
  saveDayActs(curKey, null); // null = resetear a bloques por defecto
  renderChk();
}

// ─── SISTEMA UNIFICADO DE ACTIVIDADES ───
// Cada día tiene una lista "acts" guardada en localStorage.
// Al primer acceso se inicializa desde BLOCKS[type].
// Cada acto: {icon, name, detail, bar, timeStart, timeEnd, done, fixed}
// fixed=true → venía del turno original (pero se puede eliminar)

function timeToMin(t){
  if(!t||!t.includes(':')) return 9999;
  const [h,m]=t.split(':').map(Number);
  return h*60+m;
}
function minToTime(m){
  if(m>=9999||m<0) return '';
  const h=Math.floor(m/60)%24;
  const mm=m%60;
  return String(h).padStart(2,'0')+':'+String(mm).padStart(2,'0');
}
function parseTimeRange(str){
  // "HH:MM–HH:MM" o "HH:MM → " etc
  const clean=(str||'').replace('→','').trim();
  const parts=clean.split(/[–-]/);
  const start=(parts[0]||'').trim().substring(0,5);
  const end=(parts[1]||'').trim().substring(0,5);
  return {start, end};
}

function getDayActs(key, type){
  try{
    const raw=localStorage.getItem('acts-'+key);
    if(raw) return JSON.parse(raw);
  }catch(e){}
  // Inicializar desde BLOCKS
  const blocks=BLOCKS[type]||[];
  return blocks.map(b=>{
    const {start,end}=parseTimeRange(b.time);
    return {icon:b.icon, name:b.name, detail:b.detail||'', bar:b.bar,
            timeStart:start, timeEnd:end, done:false, fixed:true};
  });
}
function saveDayActs(key, acts){
  if(acts===null){
    localStorage.removeItem('acts-'+key);
  } else {
    try{localStorage.setItem('acts-'+key,JSON.stringify(acts))}catch(e){}
  }
}

function actsTimeStr(a){
  if(a.timeStart&&a.timeEnd) return a.timeStart+'–'+a.timeEnd;
  if(a.timeStart) return a.timeStart+' →';
  return 'Sin horario';
}

// Desplaza en cadena todas las actividades desde índice idx en adelante
function cascadeShift(acts, insertedIdx){
  // Solo desplaza si hay timeStart/timeEnd definidos
  for(let i=insertedIdx+1;i<acts.length;i++){
    const prev=acts[i-1];
    const cur=acts[i];
    if(!prev.timeEnd||!cur.timeStart) continue;
    const prevEnd=timeToMin(prev.timeEnd);
    const curStart=timeToMin(cur.timeStart);
    if(curStart<prevEnd){
      // Calcular duración actual
      const curEnd=timeToMin(cur.timeEnd);
      const dur=(cur.timeEnd&&curEnd>curStart)?(curEnd-curStart):0;
      acts[i].timeStart=minToTime(prevEnd);
      acts[i].timeEnd=dur>0?minToTime(prevEnd+dur):'';
    } else {
      break; // Ya no hay solapamiento, parar
    }
  }
}

const PRESETS = [
  {icon:'🛒', name:'Compras',          bar:'b-desc'},
  {icon:'📚', name:'Máster estética',  bar:'b-master'},
  {icon:'🤸', name:'Calistenia',       bar:'b-gym'},
  {icon:'💪', name:'Extra gimnasio',   bar:'b-gym'},
  {icon:'🧾', name:'Recados',          bar:'b-desc'},
  {icon:'✈️', name:'Viaje',            bar:'b-opo'},
  {icon:'🧳', name:'Organizar maleta', bar:'b-desc'},
  {icon:'🍳', name:'Cocina / tuppers', bar:'b-dieta'},
  {icon:'👑', name:'Actividad lujo',   bar:'b-clinica'},
];

const CATS = [
  {label:'⚡ URGENTE',  bar:'b-urgente', color:'#F59E0B', urgente:true},
  {label:'Oposición',  bar:'b-opo',     color:'#7C3AED'},
  {label:'Gym',        bar:'b-gym',     color:'#EA580C'},
  {label:'Trabajo',    bar:'b-trabajo', color:'#2563EB'},
  {label:'Dieta',      bar:'b-dieta',   color:'#0891B2'},
  {label:'Clínica',    bar:'b-clinica', color:'#059669'},
  {label:'Máster',     bar:'b-master',  color:'#DB2777'},
  {label:'General',    bar:'b-desc',    color:'#CBD5E1'},
];

let selPreset=null, selCat='b-desc';

function renderChk(){
  const acts=getDayActs(curKey,curType);
  // Ordenar cronológicamente
  acts.sort((a,b)=>{
    const ta=timeToMin(a.timeStart);
    const tb=timeToMin(b.timeStart);
    return ta-tb;
  });
  const list=document.getElementById('chk-list');
  list.innerHTML='';
  const total=acts.length;
  const done=acts.filter(a=>a.done).length;

  acts.forEach((a,i)=>{
    const el=document.createElement('div');
    el.className='chk-item'+(a.done?' done':'')+(a.bar==='b-urgente'?' urgente':'');
    el.innerHTML=`
      <div class="chk-bar ${a.bar}"></div>
      <div class="chk-icon">${a.icon}</div>
      <div class="chk-body">
        <div class="chk-time">${actsTimeStr(a)}</div>
        ${a.bar==='b-urgente'?'<div class="urgente-badge">⚡ Urgente</div>':''}<div class="chk-name">${a.name}</div>
        ${a.detail?`<div class="chk-detail">${a.detail}</div>`:''}
      </div>
      <div style="display:flex;align-items:center;gap:6px;flex-shrink:0;margin-right:2px;">
        <div class="del-btn" data-i="${i}" style="width:26px;height:26px;border-radius:50%;background:#FEE2E2;border:1.5px solid #FCA5A5;display:flex;align-items:center;justify-content:center;font-size:13px;cursor:pointer;color:#DC2626;flex-shrink:0;" onclick="deleteAct(event,${i})">✕</div>
        <div class="chk-circle">${a.done?'✓':''}</div>
      </div>
    `;
    el.onclick=(e)=>{
      if(e.target.closest('.del-btn')) return;
      const fresh=getDayActs(curKey,curType);
      // re-sort para mismo índice
      fresh.sort((a,b)=>timeToMin(a.timeStart)-timeToMin(b.timeStart));
      fresh[i].done=!fresh[i].done;
      saveDayActs(curKey,fresh);
      renderChk();
    };
    list.appendChild(el);
  });

  const pct=total?Math.round(done/total*100):0;
  const col=COL[curType];
  document.getElementById('prog-fill').style.width=pct+'%';
  document.getElementById('prog-fill').style.background=col.full;
  document.getElementById('prog-pct').textContent=pct+'%';
  document.getElementById('prog-pct').style.color=col.full;
  const cel=document.getElementById('cel');
  pct===100&&total>0?cel.classList.add('show'):cel.classList.remove('show');
}

function deleteAct(e,i){
  e.stopPropagation();
  const acts=getDayActs(curKey,curType);
  acts.sort((a,b)=>timeToMin(a.timeStart)-timeToMin(b.timeStart));
  const removed=acts[i];
  acts.splice(i,1);
  // Si la actividad eliminada tenía horario, subir las de abajo
  if(removed.timeStart && removed.timeEnd){
    const removedDur=timeToMin(removed.timeEnd)-timeToMin(removed.timeStart);
    for(let j=i;j<acts.length;j++){
      const a=acts[j];
      if(!a.timeStart) continue;
      // Solo subir si el hueco es real (la siguiente empezaba justo después o solapaba)
      const aStart=timeToMin(a.timeStart);
      const removedStart=timeToMin(removed.timeStart);
      if(aStart>=timeToMin(removed.timeStart)){
        const newStart=Math.max(removedStart, aStart-removedDur);
        const dur=(a.timeEnd&&timeToMin(a.timeEnd)>aStart)?timeToMin(a.timeEnd)-aStart:0;
        acts[j].timeStart=minToTime(newStart);
        acts[j].timeEnd=dur>0?minToTime(newStart+dur):'';
      } else {
        break;
      }
    }
  }
  saveDayActs(curKey,acts);
  renderChk();
}

function openAddPanel(){
  selPreset=null; selCat='b-desc';
  document.getElementById('add-name-input').value='';
  document.getElementById('add-time-start').value='';
  document.getElementById('add-time-end').value='';
  const pg=document.getElementById('pre-grid'); pg.innerHTML='';
  PRESETS.forEach((p,i)=>{
    const b=document.createElement('div');
    b.className='pre-btn';
    b.innerHTML=`<span class="pre-icon">${p.icon}</span><span class="pre-name">${p.name}</span>`;
    b.onclick=()=>{
      document.querySelectorAll('.pre-btn').forEach(x=>x.classList.remove('sel'));
      b.classList.add('sel');
      selPreset=i;
      document.getElementById('add-name-input').value=p.name;
      selCat=p.bar;
      renderCats();
    };
    pg.appendChild(b);
  });
  renderCats();
  document.getElementById('add-overlay').classList.add('open');
}

function renderCats(){
  const cr=document.getElementById('cat-row'); cr.innerHTML='';
  CATS.forEach(c=>{
    const ch=document.createElement('div');
    ch.className='cat-chip'+(selCat===c.bar?' sel':'');
    if(selCat===c.bar) ch.style.background=c.color;
    ch.textContent=c.label;
    ch.onclick=()=>{selCat=c.bar; renderCats();};
    cr.appendChild(ch);
  });
}

function closeAddPanel(e){
  if(e&&e.target!==document.getElementById('add-overlay')) return;
  document.getElementById('add-overlay').classList.remove('open');
}


function getCustom(k){try{return JSON.parse(localStorage.getItem('x-'+k)||'[]')}catch(e){return[]}}
function setCustom(k,a){try{localStorage.setItem('x-'+k,JSON.stringify(a))}catch(e){}}

function openAddPanel(){
  selPreset=null; selCat='b-desc';
  document.getElementById('add-name-input').value='';
  document.getElementById('add-time-start').value='';
  document.getElementById('add-time-end').value='';

  // Render presets
  const pg=document.getElementById('pre-grid'); pg.innerHTML='';
  PRESETS.forEach((p,i)=>{
    const b=document.createElement('div');
    b.className='pre-btn'; b.dataset.i=i;
    b.innerHTML=`<span class="pre-icon">${p.icon}</span><span class="pre-name">${p.name}</span>`;
    b.onclick=()=>{
      document.querySelectorAll('.pre-btn').forEach(x=>x.classList.remove('sel'));
      b.classList.add('sel');
      selPreset=i;
      document.getElementById('add-name-input').value=p.name;
      selCat=p.bar;
      renderCats();
    };
    pg.appendChild(b);
  });

  // Render cats
  renderCats();
  document.getElementById('add-overlay').classList.add('open');
}

function renderCats(){
  const cr=document.getElementById('cat-row'); cr.innerHTML='';
  CATS.forEach(c=>{
    const ch=document.createElement('div');
    ch.className='cat-chip'+(selCat===c.bar?' sel':'');
    if(selCat===c.bar) ch.style.background=c.color;
    ch.textContent=c.label;
    ch.onclick=()=>{selCat=c.bar; renderCats();};
    cr.appendChild(ch);
  });
}

function closeAddPanel(e){
  if(e&&e.target!==document.getElementById('add-overlay')) return;
  document.getElementById('add-overlay').classList.remove('open');
}

function confirmAddAct(){
  const name=document.getElementById('add-name-input').value.trim();
  if(!name){document.getElementById('add-name-input').focus();return;}
  const ts=document.getElementById('add-time-start').value;
  const te=document.getElementById('add-time-end').value;
  const p=selPreset!==null?PRESETS[selPreset]:null;
  const newAct={
    icon:p?p.icon:'📌',
    name,
    detail:'',
    bar:selCat,
    timeStart:ts||'',
    timeEnd:te||'',
    done:false,
    fixed:false
  };
  const acts=getDayActs(curKey,curType);
  acts.sort((a,b)=>timeToMin(a.timeStart)-timeToMin(b.timeStart));
  const newStart=timeToMin(ts||'99:99');
  let insertIdx=acts.length;
  for(let i=0;i<acts.length;i++){
    if(timeToMin(acts[i].timeStart)>newStart){insertIdx=i;break;}
  }
  acts.splice(insertIdx,0,newAct);
  if(ts&&te) cascadeShift(acts,insertIdx);
  saveDayActs(curKey,acts);
  document.getElementById('add-overlay').classList.remove('open');
  renderChk();
}

// Inicialización — ir al mes actual
function initApp(){
  const _hoy = new Date();
  const _m = _hoy.getMonth()+1;
  const _idx = [5,6,7,8,9,10,11].indexOf(_m);
  if(_idx >= 0){ switchMes(_idx); } else { buildCal(); }
}
window.addEventListener('DOMContentLoaded', initApp);
if(document.readyState !== 'loading'){ initApp(); }
</script>
</body>
</html>
