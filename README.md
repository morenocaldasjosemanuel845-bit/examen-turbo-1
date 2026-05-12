from pathlib import Path
html = r'''<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Simulador de Examen PR-18 COES</title>
<style>
:root{
  --azul:#062b55;
  --azul2:#0b5fbd;
  --azul3:#e8f3ff;
  --amarillo:#ffd23f;
  --amarillo2:#fff4c2;
  --verde:#17803a;
  --rojo:#c62828;
  --gris:#eef3f8;
  --texto:#102033;
  --muted:#65758b;
  --card:#ffffff;
  --shadow:0 16px 38px rgba(6,43,85,.13);
}
*{box-sizing:border-box}
html{scroll-behavior:smooth}
body{
  margin:0;
  font-family:Arial, Helvetica, sans-serif;
  background:
    radial-gradient(circle at top left, rgba(255,210,63,.23), transparent 28%),
    linear-gradient(135deg,#f7fbff 0%,#edf4fb 54%,#f9fbff 100%);
  color:var(--texto);
}
.app-header{
  background:linear-gradient(120deg,var(--azul),#084d91 62%,#0b75c9);
  color:#fff;
  padding:26px 18px 30px;
  position:relative;
  overflow:hidden;
}
.app-header:before{
  content:"";
  position:absolute;
  right:-90px;
  top:-110px;
  width:330px;
  height:330px;
  background:rgba(255,210,63,.16);
  border-radius:50%;
}
.app-header:after{
  content:"";
  position:absolute;
  right:80px;
  bottom:-80px;
  width:190px;
  height:190px;
  border:18px solid rgba(255,255,255,.08);
  border-radius:50%;
}
.header-inner{
  max-width:1180px;
  margin:auto;
  position:relative;
  z-index:1;
  display:grid;
  grid-template-columns:1fr auto;
  gap:20px;
  align-items:center;
}
.brand h1{
  margin:0;
  font-size:clamp(1.55rem,3.2vw,3.2rem);
  line-height:1.05;
  letter-spacing:-.04em;
}
.brand p{
  margin:12px 0 0;
  color:#d9efff;
  font-size:clamp(.95rem,1.3vw,1.15rem);
}
.badge{
  display:inline-flex;
  align-items:center;
  gap:8px;
  background:rgba(255,210,63,.16);
  border:1px solid rgba(255,210,63,.45);
  color:#fff;
  padding:9px 13px;
  border-radius:999px;
  font-weight:700;
  margin-bottom:12px;
}
.stats-top{
  display:flex;
  gap:12px;
  flex-wrap:wrap;
  justify-content:flex-end;
}
.stat{
  min-width:120px;
  background:rgba(255,255,255,.12);
  border:1px solid rgba(255,255,255,.22);
  border-radius:18px;
  padding:14px;
  text-align:center;
  backdrop-filter:blur(8px);
}
.stat strong{
  display:block;
  font-size:1.5rem;
  color:var(--amarillo);
}
.stat span{font-size:.82rem;color:#e6f3ff}

main{
  max-width:1180px;
  margin:-24px auto 40px;
  padding:0 16px;
  position:relative;
  z-index:2;
}
.panel{
  background:var(--card);
  border-radius:24px;
  box-shadow:var(--shadow);
  border:1px solid rgba(6,43,85,.08);
}
.intro{
  padding:22px;
  display:grid;
  grid-template-columns:1.1fr .9fr;
  gap:18px;
  align-items:center;
}
.intro h2{margin:0 0 8px;color:var(--azul);font-size:1.35rem}
.intro p{margin:0;color:#34455a}
.modos{
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:10px;
}
.mode-card{
  border:2px solid #dce8f5;
  border-radius:18px;
  padding:14px;
  cursor:pointer;
  transition:.2s;
  background:#fff;
}
.mode-card:hover{transform:translateY(-2px);border-color:#7db9ef}
.mode-card.active{
  border-color:var(--azul2);
  background:linear-gradient(180deg,#f5fbff,#e8f3ff);
  box-shadow:0 10px 24px rgba(11,95,189,.13);
}
.mode-card b{display:block;color:var(--azul);margin-bottom:4px}
.mode-card small{color:var(--muted)}

.toolbar{
  margin-top:16px;
  padding:14px;
  display:grid;
  grid-template-columns:1fr auto auto auto;
  gap:12px;
  align-items:center;
  position:sticky;
  top:0;
  z-index:20;
}
.progress-wrap{
  background:#dce8f5;
  height:14px;
  border-radius:999px;
  overflow:hidden;
}
.progress-bar{
  width:0%;
  height:100%;
  background:linear-gradient(90deg,var(--azul2),var(--amarillo));
  transition:.25s ease;
}
.timer{
  min-width:110px;
  text-align:center;
  color:var(--azul);
  font-weight:800;
  background:var(--amarillo2);
  border:1px solid #f1cf46;
  padding:10px 12px;
  border-radius:14px;
}
.counter{
  min-width:105px;
  text-align:center;
  color:#fff;
  font-weight:800;
  background:var(--azul);
  padding:10px 12px;
  border-radius:14px;
}
button{
  border:0;
  border-radius:14px;
  padding:12px 16px;
  font-size:1rem;
  font-weight:800;
  cursor:pointer;
  transition:.18s;
}
button:hover{transform:translateY(-1px)}
button:disabled{
  opacity:.5;
  cursor:not-allowed;
  transform:none;
}
.btn-primary{background:linear-gradient(135deg,var(--azul2),#003f87);color:#fff}
.btn-yellow{background:var(--amarillo);color:var(--azul)}
.btn-gray{background:#e5edf5;color:var(--azul)}
.btn-danger{background:#ffe6e6;color:var(--rojo)}
.btn-success{background:#e0f7e8;color:var(--verde)}
.quiz-grid{
  display:grid;
  grid-template-columns:280px 1fr;
  gap:16px;
  margin-top:16px;
}
.navigator{
  padding:16px;
  align-self:start;
  position:sticky;
  top:86px;
}
.navigator h3{
  margin:0 0 12px;
  color:var(--azul);
}
.nav-grid{
  display:grid;
  grid-template-columns:repeat(5,1fr);
  gap:8px;
}
.nav-dot{
  width:42px;
  height:42px;
  border-radius:13px;
  border:1px solid #c8d8ea;
  background:#fff;
  color:var(--azul);
  font-weight:800;
  cursor:pointer;
}
.nav-dot.current{background:var(--azul);color:#fff;border-color:var(--azul)}
.nav-dot.answered{background:#d9efff;border-color:#74b8ef}
.nav-dot.correct{background:#daf5e3;border-color:#5bbd77;color:var(--verde)}
.nav-dot.wrong{background:#ffe2e2;border-color:#ef8181;color:var(--rojo)}
.legend{
  margin-top:14px;
  display:grid;
  gap:7px;
  color:var(--muted);
  font-size:.86rem;
}
.legend span{display:flex;gap:8px;align-items:center}
.box{width:15px;height:15px;border-radius:5px;display:inline-block;background:#fff;border:1px solid #c8d8ea}
.box.a{background:#d9efff}.box.c{background:#daf5e3}.box.w{background:#ffe2e2}

.question-card{
  padding:24px;
  min-height:520px;
}
.question-top{
  display:flex;
  justify-content:space-between;
  gap:12px;
  align-items:flex-start;
  margin-bottom:16px;
}
.question-tag{
  background:var(--azul3);
  color:var(--azul);
  border:1px solid #c9def4;
  border-radius:999px;
  padding:8px 12px;
  font-weight:800;
}
.difficulty{
  background:#fff7d5;
  color:#7c5800;
  border:1px solid #efd16b;
  border-radius:999px;
  padding:8px 12px;
  font-weight:800;
}
.question-card h2{
  margin:6px 0 18px;
  color:var(--azul);
  line-height:1.25;
  font-size:clamp(1.18rem,2vw,1.55rem);
}
.options{
  display:grid;
  gap:12px;
}
.option{
  display:grid;
  grid-template-columns:42px 1fr;
  gap:12px;
  align-items:center;
  border:2px solid #d8e4f0;
  background:#fff;
  border-radius:18px;
  padding:14px;
  cursor:pointer;
  transition:.18s;
}
.option:hover{
  border-color:#85bff2;
  background:#f5fbff;
}
.option input{display:none}
.letter{
  width:42px;
  height:42px;
  border-radius:14px;
  display:flex;
  align-items:center;
  justify-content:center;
  background:#eef5fc;
  color:var(--azul);
  font-weight:900;
}
.option.selected{
  border-color:var(--azul2);
  background:#eaf5ff;
}
.option.selected .letter{background:var(--azul2);color:#fff}
.option.correct{
  border-color:#30a653;
  background:#e5faec;
}
.option.correct .letter{background:#30a653;color:#fff}
.option.wrong{
  border-color:#e45757;
  background:#ffe9e9;
}
.option.wrong .letter{background:#e45757;color:#fff}
.feedback{
  display:none;
  margin-top:16px;
  padding:15px;
  border-radius:18px;
  border-left:6px solid;
}
.feedback.visible{display:block;animation:pop .2s ease}
.feedback.ok{background:#e5faec;border-left-color:#30a653;color:#13592b}
.feedback.bad{background:#ffe9e9;border-left-color:#e45757;color:#8d1d1d}
.feedback.warn{background:#fff5cc;border-left-color:#f2bd00;color:#705000}
@keyframes pop{from{transform:scale(.98);opacity:.4}to{transform:scale(1);opacity:1}}

.controls{
  display:flex;
  justify-content:space-between;
  gap:10px;
  margin-top:18px;
  flex-wrap:wrap;
}
.right-controls{display:flex;gap:10px;flex-wrap:wrap}

.result-screen{
  display:none;
  padding:24px;
  margin-top:16px;
}
.result-hero{
  text-align:center;
  padding:28px 18px;
  border-radius:24px;
  background:linear-gradient(135deg,#062b55,#0b5fbd);
  color:#fff;
}
.result-hero h2{margin:0 0 8px;font-size:2rem}
.result-hero .score{
  font-size:3.4rem;
  font-weight:900;
  color:var(--amarillo);
}
.result-cards{
  margin-top:16px;
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:12px;
}
.result-mini{
  background:#fff;
  border:1px solid #dce8f5;
  border-radius:18px;
  padding:16px;
  text-align:center;
}
.result-mini b{display:block;font-size:1.6rem;color:var(--azul)}
.review-list{
  margin-top:18px;
  display:grid;
  gap:10px;
}
.review-item{
  padding:14px;
  border-radius:16px;
  background:#f8fbff;
  border:1px solid #dce8f5;
}
.review-item.good{border-left:6px solid var(--verde)}
.review-item.bad{border-left:6px solid var(--rojo)}
.review-item.miss{border-left:6px solid #e0a800}
.review-item strong{color:var(--azul)}

.exam-lock .feedback{display:none!important}
.exam-lock .option.correct,.exam-lock .option.wrong{border-color:#d8e4f0;background:#fff}
.exam-lock .option.correct .letter,.exam-lock .option.wrong .letter{background:#eef5fc;color:var(--azul)}

.toast{
  position:fixed;
  left:50%;
  bottom:20px;
  transform:translateX(-50%) translateY(20px);
  background:var(--azul);
  color:#fff;
  padding:13px 18px;
  border-radius:999px;
  box-shadow:0 18px 30px rgba(0,0,0,.18);
  opacity:0;
  pointer-events:none;
  transition:.2s;
  z-index:99;
}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0)}

@media(max-width:900px){
  .header-inner{grid-template-columns:1fr}
  .stats-top{justify-content:flex-start}
  .intro{grid-template-columns:1fr}
  .modos{grid-template-columns:1fr}
  .toolbar{grid-template-columns:1fr 1fr;position:relative}
  .quiz-grid{grid-template-columns:1fr}
  .navigator{position:relative;top:auto}
  .nav-grid{grid-template-columns:repeat(10,1fr)}
  .nav-dot{width:100%;height:38px}
  .result-cards{grid-template-columns:repeat(2,1fr)}
}
@media(max-width:560px){
  main{padding:0 10px}
  .intro,.question-card,.navigator,.result-screen{padding:16px}
  .toolbar{grid-template-columns:1fr}
  .controls{display:grid}
  .right-controls{display:grid}
  button{width:100%}
  .nav-grid{grid-template-columns:repeat(5,1fr)}
  .result-cards{grid-template-columns:1fr}
  .option{grid-template-columns:36px 1fr;padding:12px}
  .letter{width:36px;height:36px}
}
</style>
</head>
<body>

<header class="app-header">
  <div class="header-inner">
    <div class="brand">
      <div class="badge">⚡ Simulador interactivo PR-18 COES</div>
      <h1>Examen dinámico sobre Potencia Efectiva de Centrales Hidroeléctricas</h1>
      <p>Practica, rinde en modo examen, revisa tus errores y mide tu avance en tiempo real.</p>
    </div>
    <div class="stats-top">
      <div class="stat"><strong>40</strong><span>preguntas</span></div>
      <div class="stat"><strong>70%</strong><span>nota mínima</span></div>
      <div class="stat"><strong>20 min</strong><span>modo examen</span></div>
    </div>
  </div>
</header>

<main>
  <section class="panel intro" id="inicio">
    <div>
      <h2>Instrucciones</h2>
      <p>Selecciona un modo, responde una pregunta a la vez y usa el panel lateral para navegar. En modo práctica recibirás retroalimentación inmediata; en modo examen las respuestas se bloquean hasta el final.</p>
    </div>
    <div class="modos">
      <div class="mode-card active" data-mode="practica" onclick="setMode('practica')">
        <b>🧠 Práctica</b>
        <small>Feedback inmediato y explicación por pregunta.</small>
      </div>
      <div class="mode-card" data-mode="examen" onclick="setMode('examen')">
        <b>⏱️ Examen</b>
        <small>20 minutos, sin mostrar respuestas hasta finalizar.</small>
      </div>
      <div class="mode-card" data-mode="repaso" onclick="setMode('repaso')">
        <b>🎯 Repaso rápido</b>
        <small>10 preguntas aleatorias para entrenar.</small>
      </div>
    </div>
  </section>

  <section class="panel toolbar" id="toolbar">
    <div>
      <div class="progress-wrap"><div class="progress-bar" id="progressBar"></div></div>
    </div>
    <div class="timer" id="timer">Sin tiempo</div>
    <div class="counter" id="counter">0 / 40</div>
    <button class="btn-yellow" type="button" onclick="nuevoIntento()">Nuevo intento</button>
  </section>

  <section class="quiz-grid" id="quizGrid">
    <aside class="panel navigator">
      <h3>Mapa del examen</h3>
      <div class="nav-grid" id="navGrid"></div>
      <div class="legend">
        <span><i class="box"></i> Pendiente</span>
        <span><i class="box a"></i> Respondida</span>
        <span><i class="box c"></i> Correcta</span>
        <span><i class="box w"></i> Incorrecta</span>
      </div>
    </aside>

    <section class="panel question-card" id="questionCard"></section>
  </section>

  <section class="panel result-screen" id="resultScreen"></section>
</main>

<div class="toast" id="toast"></div>

<script>
const bancoPreguntas = [
{q:"¿Cuál es el objetivo principal del PR-18?",o:{A:"Regular el mantenimiento anual de centrales hidroeléctricas.",B:"Determinar la Potencia Efectiva y el Caudal Turbinado de centrales hidroeléctricas.",C:"Calcular únicamente la potencia firme de centrales térmicas.",D:"Supervisar la operación comercial diaria del SEIN."},r:"B",e:"El PR-18 establece el procedimiento para determinar la Potencia Efectiva y el Caudal Turbinado de centrales hidroeléctricas."},
{q:"¿Qué representa la Potencia Efectiva de una Central Hidroeléctrica?",o:{A:"La potencia promedio mensual entregada al SEIN.",B:"La potencia máxima declarada por el fabricante.",C:"La máxima capacidad medida en bornes durante cinco horas continuas bajo condiciones específicas.",D:"La potencia garantizada después de descontar servicios auxiliares."},r:"C",e:"La Potencia Efectiva se mide en bornes de generación durante cinco horas continuas bajo condiciones de potencia efectiva."},
{q:"¿Qué es un EPE?",o:{A:"Un ensayo para determinar la Potencia Efectiva.",B:"Un estudio mensual de caudales naturales.",C:"Una evaluación económica del despacho hidroeléctrico.",D:"Un procedimiento exclusivo para mantenimiento mayor."},r:"A",e:"EPE significa Ensayo de Potencia Efectiva."},
{q:"¿Cuál es la duración mínima del EPE?",o:{A:"Tres horas continuas.",B:"Cinco horas continuas.",C:"Diez horas continuas.",D:"Veinticuatro horas continuas."},r:"B",e:"El ensayo debe durar como mínimo cinco horas continuas."},
{q:"Durante el EPE, ¿cada cuánto se registran las mediciones?",o:{A:"Cada 5 minutos.",B:"Cada 10 minutos.",C:"Cada 15 minutos.",D:"Cada 30 minutos."},r:"C",e:"El PR-18 exige registros cada quince minutos."},
{q:"¿Cuántas mediciones totales deben registrarse en el EPE?",o:{A:"10 mediciones.",B:"15 mediciones.",C:"20 mediciones.",D:"25 mediciones."},r:"C",e:"Cinco horas con mediciones cada quince minutos equivalen a veinte mediciones."},
{q:"¿Cuál es el mínimo de mediciones válidas para que el EPE pueda aceptarse?",o:{A:"Cinco mediciones válidas.",B:"Diez mediciones válidas.",C:"Quince mediciones válidas.",D:"Todas las mediciones registradas."},r:"B",e:"El EPE requiere al menos diez mediciones válidas."},
{q:"¿Con cuánta anticipación debe solicitarse la programación del EPE?",o:{A:"Cinco días calendario antes del ensayo.",B:"Diez días hábiles antes de la fecha tentativa.",C:"Quince días calendario después del ensayo.",D:"Veinte días hábiles antes del informe final."},r:"B",e:"La solicitud debe presentarse como mínimo diez días hábiles antes de la fecha tentativa."},
{q:"Si el COES observa la solicitud inicial del EPE, ¿qué plazo tiene el generador para subsanar?",o:{A:"Tres días hábiles.",B:"Cinco días hábiles.",C:"Diez días hábiles.",D:"Quince días hábiles."},r:"A",e:"El generador tiene tres días hábiles para subsanar observaciones de programación."},
{q:"¿Qué ocurre si el generador no subsana las observaciones de la solicitud?",o:{A:"El COES aprueba el EPE de manera provisional.",B:"El COES reduce automáticamente la potencia efectiva.",C:"El COES cancela la programación del EPE incluida en el PSO.",D:"El generador queda exonerado del ensayo."},r:"C",e:"Si no se absuelven observaciones, el COES cancela la programación del EPE."},
{q:"¿En qué plazo debe entregarse el Informe del EPE?",o:{A:"Hasta 10 días hábiles después del EPE.",B:"Hasta 15 días hábiles después del EPE.",C:"Hasta 20 días hábiles después del EPE.",D:"Hasta 30 días calendario después del EPE."},r:"C",e:"El informe debe entregarse hasta veinte días hábiles después del ensayo."},
{q:"¿Qué plazo tiene el COES para observar el Informe del EPE?",o:{A:"Hasta 5 días hábiles.",B:"Hasta 10 días hábiles.",C:"Hasta 15 días hábiles.",D:"Hasta 20 días hábiles."},r:"C",e:"El COES tiene hasta quince días hábiles para formular observaciones al informe."},
{q:"Si el Informe del EPE tiene observaciones, ¿cuánto tiempo tiene el integrante para subsanarlas?",o:{A:"Tres días hábiles.",B:"Cinco días hábiles.",C:"Diez días hábiles.",D:"Veinte días hábiles."},r:"C",e:"El integrante dispone de diez días hábiles para subsanar observaciones al informe."},
{q:"Si el informe no se entrega dentro del plazo, ¿cómo se considera?",o:{A:"Como aprobado tácitamente.",B:"Como observado parcialmente.",C:"Como no recibido.",D:"Como válido solo para caudal."},r:"C",e:"Si no se entrega dentro del plazo, el informe se considera como no recibido."},
{q:"¿Qué ocurre si una central no ejecuta y aprueba el EPE cuando corresponde?",o:{A:"Se mantiene indefinidamente su Potencia Efectiva anterior.",B:"Se considera como Potencia Garantizada el 85% del valor vigente.",C:"Se incrementa su Potencia Garantizada en 5%.",D:"Se suspende automáticamente su operación comercial."},r:"B",e:"El PR-18 prevé que se considere el 85% de la Potencia Garantizada vigente hasta aprobar el EPE."},
{q:"¿Qué fluctuación máxima de potencia se permite durante el EPE?",o:{A:"±0.5% respecto al promedio.",B:"±1.0% respecto al promedio.",C:"±1.5% respecto al promedio.",D:"±5.0% respecto al promedio."},r:"C",e:"La fluctuación de potencia no debe exceder ±1,5% respecto al promedio."},
{q:"¿Qué fluctuación máxima de caudal se permite durante el ensayo?",o:{A:"±0.5%.",B:"±1.0%.",C:"±1.5%.",D:"±2.0%."},r:"C",e:"La fluctuación de caudal no debe exceder ±1,5% respecto al promedio."},
{q:"¿Qué fluctuación máxima se permite para la velocidad de rotación?",o:{A:"±0.5%.",B:"±1.0%.",C:"±1.5%.",D:"±2.0%."},r:"A",e:"La velocidad de rotación no debe fluctuar más de ±0,5% respecto al promedio."},
{q:"Si ocurre una perturbación externa en el SEIN que afecta el ensayo, ¿qué sucede con esas mediciones?",o:{A:"Se mantienen si la potencia promedio no cambia.",B:"Se eliminan y deben repetirse.",C:"Se corrigen aplicando un factor de ajuste.",D:"Se consideran válidas solo si el generador lo solicita."},r:"B",e:"Las mediciones afectadas por perturbaciones externas se eliminan y deben repetirse."},
{q:"¿Quién participa como veedor en el EPE?",o:{A:"El fabricante de la turbina.",B:"El representante designado por el COES.",C:"El operador del embalse.",D:"El consultor externo contratado por OSINERGMIN."},r:"B",e:"El COES participa mediante un representante designado en calidad de veedor."},
{q:"¿Quién opera la central durante el EPE?",o:{A:"El veedor del COES.",B:"El Jefe de Ensayo.",C:"Un representante acreditado del Generador Integrante.",D:"El auditor de OSINERGMIN."},r:"C",e:"La operación corresponde al representante acreditado del Generador Integrante."},
{q:"¿Quién es responsable técnico del EPE?",o:{A:"El COES directamente.",B:"El Jefe de Ensayo y su Equipo Técnico.",C:"El operador del sistema interconectado.",D:"El área comercial del generador."},r:"B",e:"El Jefe de Ensayo y su Equipo Técnico son responsables técnicos del EPE."},
{q:"¿Qué precisión mínima debe tener el medidor de potencia?",o:{A:"Clase 0.2.",B:"Clase 0.5.",C:"Clase 1.0.",D:"Clase 2.0."},r:"A",e:"El medidor de energía/potencia debe ser Clase 0.2."},
{q:"¿Qué precisión mínima debe tener el medidor de flujo?",o:{A:"±0.2%.",B:"±0.5%.",C:"±1.0%.",D:"±2.5%."},r:"C",e:"El medidor de flujo debe tener precisión mínima de ±1,0%."},
{q:"¿Qué norma internacional sirve como referencia para el EPE?",o:{A:"ISO 9001.",B:"IEC 60041.",C:"IEEE 519.",D:"IEC 61850."},r:"B",e:"La referencia técnica principal indicada es la IEC 60041."},
{q:"¿Cuál de los siguientes métodos corresponde a una medición directa de caudal?",o:{A:"Promedio mensual de energía activa.",B:"Correntómetro.",C:"Estimación comercial por potencia firme.",D:"Cálculo solo con factor de potencia."},r:"B",e:"El correntómetro es uno de los métodos directos de medición de caudal."},
{q:"Según el Anexo 9, ¿qué método tiene primera preferencia para medir caudal?",o:{A:"Método volumétrico.",B:"Caudalímetro intrusivo en tubería forzada.",C:"Correntómetro en canal de descarga.",D:"Cálculo indirecto por eficiencia."},r:"C",e:"La primera preferencia es el correntómetro en canal de descarga de agua turbinada."},
{q:"¿Qué ocurre si el grupo o central sale de servicio por tercera vez durante el EPE por causas atribuibles al generador?",o:{A:"El EPE continúa con los datos disponibles.",B:"El EPE queda suspendido.",C:"El COES aprueba el ensayo con observaciones.",D:"Se reemplaza por una prueba documental."},r:"B",e:"Si sale de servicio por tercera vez por causas atribuibles al generador, el EPE queda suspendido."},
{q:"¿Quiénes suscriben el Acta de EPE?",o:{A:"Solo el representante del generador.",B:"El Jefe de Ensayo, el representante del generador y el veedor del COES.",C:"OSINERGMIN y el fabricante.",D:"El operador del SEIN y el área financiera."},r:"B",e:"El acta la suscriben el Jefe de Ensayo, el representante del Generador Integrante y el veedor del COES."},
{q:"¿Qué ocurre si el COES detecta que la Potencia Efectiva varió fuera del rango ±5%?",o:{A:"Se elimina la central del SEIN.",B:"Se coordina la realización del EPE correspondiente.",C:"Se mantiene el valor vigente sin cambios.",D:"Se aplica automáticamente una penalidad fija."},r:"B",e:"Cuando se detecta variación fuera de ±5%, el COES coordina la realización del EPE."},
{q:"¿Qué periodo usa el COES para evaluar variaciones de Potencia Efectiva?",o:{A:"Enero a diciembre del mismo año.",B:"Marzo a febrero del siguiente año.",C:"Septiembre del año anterior a agosto del año de evaluación.",D:"Solo el mes anterior al ensayo."},r:"C",e:"El periodo de análisis va desde setiembre del año anterior hasta agosto del año de evaluación."},
{q:"¿Quién asume el costo del EPE?",o:{A:"Siempre el COES.",B:"El Generador Integrante correspondiente.",C:"OSINERGMIN.",D:"El fabricante de la turbina."},r:"B",e:"El Generador Integrante asume el costo de ejecución del EPE, salvo reglas específicas para solicitudes de terceros."},
{q:"¿Cuál de los siguientes documentos puede ser requerido antes del EPE?",o:{A:"Diagramas, curvas de rendimiento y certificados de calibración.",B:"Solo facturas de energía vendida.",C:"Únicamente contratos laborales del personal.",D:"Reporte de mantenimiento de oficinas administrativas."},r:"A",e:"El PR-18 requiere información técnica, diagramas, curvas de rendimiento y certificados de calibración, entre otros."},
{q:"¿Qué ocurre si las temperaturas de cojinetes o devanados exceden los valores permitidos?",o:{A:"El ensayo sigue sin restricciones.",B:"Se considera que no se cumplen adecuadamente las condiciones del EPE.",C:"Se incrementa la potencia declarada.",D:"Se reemplaza la medición por el valor nominal."},r:"B",e:"Las temperaturas no deben exceder los valores del fabricante o del protocolo de recepción."},
{q:"¿Cómo se obtiene la Potencia Efectiva del grupo ensayado?",o:{A:"Con el valor máximo instantáneo registrado.",B:"Con el promedio de las mediciones válidas de potencia.",C:"Con la potencia de placa del generador.",D:"Con el caudal mínimo anual."},r:"B",e:"La Potencia Efectiva del grupo se obtiene promediando las mediciones válidas de potencia."},
{q:"¿Cómo se calcula la Potencia Efectiva total de la central?",o:{A:"Promediando la potencia de todos los años.",B:"Sumando la Potencia Efectiva de los grupos ensayados simultáneamente.",C:"Restando los servicios auxiliares del caudal.",D:"Tomando solo el grupo de mayor potencia."},r:"B",e:"La PE total de la central es la suma de las PE de los grupos ensayados simultáneamente."},
{q:"¿Qué caracteriza al método indirecto para determinar caudal?",o:{A:"Usa únicamente la lectura visual del nivel del embalse.",B:"Relaciona potencia, eficiencias y mediciones hidráulicas para estimar el caudal.",C:"Se aplica solo a centrales térmicas.",D:"No requiere datos de potencia ni presión."},r:"B",e:"El método indirecto usa potencia, eficiencias del grupo y mediciones hidráulicas."},
{q:"Para calcular el salto neto, ¿qué variables son relevantes?",o:{A:"Solo la potencia nominal del generador.",B:"Presión, velocidad, densidad, gravedad y diferencia de niveles.",C:"Únicamente el factor de potencia.",D:"Solo la temperatura ambiente."},r:"B",e:"El salto neto considera presiones, velocidades, densidad del agua, gravedad y diferencia de niveles."},
{q:"Si el generador decide repetir el EPE después de presentar el informe, ¿qué ocurre con los resultados presentados?",o:{A:"Se aprueban automáticamente.",B:"No serán aprobados.",C:"Se consideran válidos por seis meses.",D:"Se usan solo para calcular caudal."},r:"B",e:"Si el generador considera necesario repetir el EPE, el informe y sus resultados no serán aprobados."},
{q:"¿Cuál es una condición propia de Potencia Efectiva?",o:{A:"Operar con flujo inestable para evaluar condiciones extremas.",B:"Operar sin sobrecarga, a velocidad nominal y con flujo estable.",C:"Operar con sobrecarga eléctrica para hallar la potencia máxima absoluta.",D:"Operar con cualquier frecuencia siempre que haya caudal suficiente."},r:"B",e:"Las condiciones de potencia efectiva exigen flujo estable, sin sobrecarga y velocidad nominal."}
];

let modo = "practica";
let preguntas = [];
let respuestas = {};
let actual = 0;
let revisadas = {};
let examenFinalizado = false;
let timerInterval = null;
let tiempoRestante = 0;

const $ = id => document.getElementById(id);

function shuffle(arr){
  const a = [...arr];
  for(let i=a.length-1;i>0;i--){
    const j = Math.floor(Math.random()*(i+1));
    [a[i],a[j]]=[a[j],a[i]];
  }
  return a;
}

function setMode(nuevoModo){
  modo = nuevoModo;
  document.querySelectorAll(".mode-card").forEach(c=>c.classList.toggle("active", c.dataset.mode === modo));
  nuevoIntento();
}

function prepararPreguntas(){
  let base = bancoPreguntas.map((p,i)=>({...p, id:i+1}));
  if(modo === "repaso") base = shuffle(base).slice(0,10);
  if(modo === "examen" || modo === "repaso") base = shuffle(base);

  return base.map(p=>{
    const letras = Object.keys(p.o);
    const opcionesTexto = letras.map(l=>({old:l, text:p.o[l]}));
    const opcionesMezcladas = (modo === "practica") ? opcionesTexto : shuffle(opcionesTexto);
    const nuevas = {};
    let nuevaCorrecta = "";
    ["A","B","C","D"].forEach((letraNueva, idx)=>{
      nuevas[letraNueva] = opcionesMezcladas[idx].text;
      if(opcionesMezcladas[idx].old === p.r) nuevaCorrecta = letraNueva;
    });
    return {...p, o:nuevas, r:nuevaCorrecta};
  });
}

function nuevoIntento(){
  preguntas = prepararPreguntas();
  respuestas = {};
  actual = 0;
  revisadas = {};
  examenFinalizado = false;
  $("quizGrid").style.display = "grid";
  $("resultScreen").style.display = "none";
  $("questionCard").classList.toggle("exam-lock", modo === "examen");
  iniciarTimer();
  renderNav();
  renderPregunta();
  updateStatus();
  toast(modo === "examen" ? "Modo examen activado: respuestas al final." : modo === "repaso" ? "Repaso rápido: 10 preguntas aleatorias." : "Modo práctica activado.");
}

function iniciarTimer(){
  clearInterval(timerInterval);
  if(modo === "examen"){
    tiempoRestante = 20 * 60;
    actualizarTimer();
    timerInterval = setInterval(()=>{
      tiempoRestante--;
      actualizarTimer();
      if(tiempoRestante <= 0){
        clearInterval(timerInterval);
        finalizarExamen(true);
      }
    },1000);
  }else{
    $("timer").textContent = "Sin tiempo";
  }
}

function actualizarTimer(){
  const m = Math.floor(tiempoRestante/60).toString().padStart(2,"0");
  const s = (tiempoRestante%60).toString().padStart(2,"0");
  $("timer").textContent = `${m}:${s}`;
  $("timer").style.background = tiempoRestante <= 60 ? "#ffe3e3" : "var(--amarillo2)";
  $("timer").style.color = tiempoRestante <= 60 ? "var(--rojo)" : "var(--azul)";
}

function renderNav(){
  const nav = $("navGrid");
  nav.innerHTML = "";
  preguntas.forEach((p,i)=>{
    const b = document.createElement("button");
    b.type = "button";
    b.textContent = i+1;
    b.className = "nav-dot";
    if(i === actual) b.classList.add("current");
    if(respuestas[i]) b.classList.add("answered");
    if(revisadas[i]){
      b.classList.remove("answered");
      b.classList.add(revisadas[i] === "correct" ? "correct" : "wrong");
    }
    b.onclick = ()=>goTo(i);
    nav.appendChild(b);
  });
}

function renderPregunta(){
  const p = preguntas[actual];
  const seleccion = respuestas[actual] || "";
  const isReviewed = !!revisadas[actual];
  const disabled = examenFinalizado || (modo === "practica" && isReviewed);

  let html = `
    <div class="question-top">
      <span class="question-tag">Pregunta ${actual+1} de ${preguntas.length}</span>
      <span class="difficulty">${modo === "examen" ? "Modo examen" : modo === "repaso" ? "Repaso rápido" : "Práctica"}</span>
    </div>
    <h2>${p.q}</h2>
    <div class="options">
  `;

  for(const letra of ["A","B","C","D"]){
    let cls = "option";
    if(seleccion === letra) cls += " selected";
    if(isReviewed || examenFinalizado){
      if(letra === p.r) cls += " correct";
      else if(seleccion === letra && letra !== p.r) cls += " wrong";
    }
    html += `
      <label class="${cls}" onclick="seleccionar('${letra}')">
        <input type="radio" ${seleccion===letra?"checked":""} ${disabled?"disabled":""}>
        <span class="letter">${letra}</span>
        <span>${p.o[letra]}</span>
      </label>
    `;
  }

  let fbClass = "";
  let fbText = "";
  if((isReviewed || examenFinalizado) && seleccion){
    if(seleccion === p.r){
      fbClass = "visible ok";
      fbText = `<strong>Correcto.</strong> ${p.e}`;
    }else{
      fbClass = "visible bad";
      fbText = `<strong>Incorrecto.</strong> La alternativa correcta es <strong>${p.r}</strong>. ${p.e}`;
    }
  }else if((isReviewed || examenFinalizado) && !seleccion){
    fbClass = "visible warn";
    fbText = `<strong>Sin responder.</strong> La alternativa correcta es <strong>${p.r}</strong>. ${p.e}`;
  }

  html += `
    </div>
    <div class="feedback ${fbClass}">${fbText}</div>
    <div class="controls">
      <button type="button" class="btn-gray" onclick="anterior()" ${actual===0?"disabled":""}>← Anterior</button>
      <div class="right-controls">
        ${modo !== "examen" && !isReviewed ? `<button type="button" class="btn-success" onclick="revisarActual()">Revisar esta pregunta</button>` : ""}
        ${actual < preguntas.length-1 ? `<button type="button" class="btn-primary" onclick="siguiente()">Siguiente →</button>` : ""}
        <button type="button" class="btn-yellow" onclick="finalizarExamen(false)">${modo === "examen" ? "Finalizar examen" : "Ver resultado final"}</button>
      </div>
    </div>
  `;
  $("questionCard").innerHTML = html;
  renderNav();
  updateStatus();
}

function seleccionar(letra){
  if(examenFinalizado) return;
  if(modo === "practica" && revisadas[actual]) return;
  respuestas[actual] = letra;
  if(modo === "practica"){
    // mantiene seleccionada, pero no revela hasta que el usuario revise
  }
  renderPregunta();
}

function revisarActual(){
  if(!respuestas[actual]){
    revisadas[actual] = "wrong";
    renderPregunta();
    toast("Pregunta sin responder.");
    return;
  }
  revisadas[actual] = respuestas[actual] === preguntas[actual].r ? "correct" : "wrong";
  renderPregunta();
}

function goTo(i){
  actual = i;
  renderPregunta();
}

function anterior(){
  if(actual > 0){ actual--; renderPregunta(); }
}

function siguiente(){
  if(actual < preguntas.length-1){ actual++; renderPregunta(); }
}

function updateStatus(){
  const respondidas = Object.keys(respuestas).length;
  const pct = (respondidas / preguntas.length) * 100;
  $("progressBar").style.width = pct + "%";
  $("counter").textContent = `${respondidas} / ${preguntas.length}`;
}

function finalizarExamen(tiempoAgotado=false){
  clearInterval(timerInterval);
  examenFinalizado = true;

  let correctas = 0, incorrectas = 0, sinResponder = 0;
  preguntas.forEach((p,i)=>{
    if(!respuestas[i]) sinResponder++;
    else if(respuestas[i] === p.r) correctas++;
    else incorrectas++;
  });

  const total = preguntas.length;
  const porcentaje = Math.round((correctas/total)*1000)/10;
  const aprobado = porcentaje >= 70;

  $("quizGrid").style.display = "none";
  $("resultScreen").style.display = "block";

  let mensaje = "";
  if(porcentaje >= 90) mensaje = "Excelente dominio del PR-18.";
  else if(porcentaje >= 70) mensaje = "Aprobaste. Refuerza los detalles finos.";
  else if(porcentaje >= 50) mensaje = "Resultado regular. Repasa plazos, mediciones y causales.";
  else mensaje = "Necesitas repasar más antes del examen.";

  const review = preguntas.map((p,i)=>{
    const sel = respuestas[i];
    const estado = !sel ? "miss" : sel === p.r ? "good" : "bad";
    const etiqueta = !sel ? "Sin responder" : sel === p.r ? "Correcta" : "Incorrecta";
    return `
      <div class="review-item ${estado}">
        <strong>${i+1}. ${p.q}</strong><br>
        Tu respuesta: <b>${sel || "—"}</b> | Correcta: <b>${p.r}</b><br>
        <small>${etiqueta}. ${p.e}</small>
      </div>
    `;
  }).join("");

  $("resultScreen").innerHTML = `
    <div class="result-hero">
      <h2>${tiempoAgotado ? "Tiempo agotado" : "Resultado final"}</h2>
      <div class="score">${porcentaje}%</div>
      <p>${aprobado ? "✅ Aprobado" : "❌ No aprobado"} — ${mensaje}</p>
    </div>
    <div class="result-cards">
      <div class="result-mini"><b>${correctas}</b><span>Correctas</span></div>
      <div class="result-mini"><b>${incorrectas}</b><span>Incorrectas</span></div>
      <div class="result-mini"><b>${sinResponder}</b><span>Sin responder</span></div>
      <div class="result-mini"><b>${total}</b><span>Total</span></div>
    </div>
    <div class="controls">
      <button type="button" class="btn-primary" onclick="volverRevision()">Revisar pregunta por pregunta</button>
      <button type="button" class="btn-yellow" onclick="nuevoIntento()">Nuevo intento</button>
      <button type="button" class="btn-gray" onclick="window.print()">Imprimir / guardar PDF</button>
    </div>
    <div class="review-list">${review}</div>
  `;
  window.scrollTo({top:0,behavior:"smooth"});
}

function volverRevision(){
  examenFinalizado = true;
  $("quizGrid").style.display = "grid";
  $("resultScreen").style.display = "none";
  actual = 0;
  renderPregunta();
}

function toast(msg){
  const t = $("toast");
  t.textContent = msg;
  t.classList.add("show");
  setTimeout(()=>t.classList.remove("show"),2200);
}

nuevoIntento();
</script>
</body>
</html>'''
path = Path('/mnt/data/simulador_examen_PR18_COES_interactivo.html')
path.write_text(html, encoding='utf-8')
path

