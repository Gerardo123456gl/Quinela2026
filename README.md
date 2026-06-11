<!DOCTYPE html>
<html lang="en" class=""><head><script>window.livecodes = window.livecodes || {};</script><title>DeepAI Code Snippet</title><meta name="title" content="DeepAI Code Snippet"><meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0"><style id="__livecodes_styles__"></style><script src="https://cdn.jsdelivr.net/npm/es-module-shims@1.10.0/dist/es-module-shims.js" async=""></script><script type="importmap">{
  "imports": {}
}</script></head><body>


<meta charset="UTF-8">
<title>Quiniela FIFA 2026</title>
<!-- Incluimos la librería jsPDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
  body { font-family: Arial, sans-serif; background-color: #f0f2f5; margin: 20px; }
  h1 { text-align: center; color: #333; }
  .section { background-color: #fff; padding: 20px; border-radius: 8px; box-shadow: 0 0 10px rgba(0,0,0,0.1); margin-bottom: 20px; }
  .section h2 { margin-top: 0; color: #555; }
  .flex-container { display: flex; flex-wrap: wrap; gap: 20px; }
  .column { flex: 1; min-width: 300px; }
  input[type="text"], input[type="number"], select { width: 100%; padding: 8px; margin-top: 5px; margin-bottom: 10px; border-radius: 4px; border: 1px solid #ccc; }
  button { padding: 8px 12px; margin-right: 10px; border: none; border-radius: 4px; background-color: #007bff; color: #fff; cursor: pointer; }
  button:hover { background-color: #0056b3; }
  table { width: 100%; border-collapse: collapse; margin-top: 10px; }
  th, td { border: 1px solid #ccc; padding: 8px; text-align: center; }
  th { background-color: #eee; }
  .buttons-group { display: flex; gap: 10px; margin-top: 10px; flex-wrap: wrap; }
</style>



<h1>Quiniela FIFA 2026</h1>

<div class="section" id="inicio">
  <h2>Ingresa tu nombre para comenzar</h2>
  <input type="text" id="nombreJugadorPrincipal" placeholder="Tu nombre">
  <button onclick="guardarNombreJugador()">Comenzar</button>
</div>

<div class="section" id="principal" style="display:none;">
  <div class="flex-container">
    <!-- Sección de fechas y Partidos -->
    <div class="column">
      <h2>Fechas y Partidos</h2>
      <select id="fecha" onchange="mostrarPartidos()"></select>
      <button onclick="mostrarPartidos()">Mostrar partidos</button>
      <div id="partidos" style="margin-top:10px; max-height:300px; overflow:auto;"></div>
    </div>
    <!-- Sección de resultados -->
    <div class="column">
      <h2>Registrar Resultado</h2>
      <label>Partido:</label>
      <select id="partidoParaResultado"></select>
      <label>Resultado (marcador):</label>
      <input type="text" id="resultadoPartido" placeholder="Ejemplo: 2-1">
      <button onclick="guardarResultadoPartido()">Guardar resultado</button>
      
      <!-- Mostrar resultados guardados -->
      <h3>Resultados Guardados</h3>
      <table id="tablaResultados">
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Partido</th>
            <th>Resultado</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
      <div class="buttons-group">
        <button onclick="descargarResultados()">Descargar Resultados</button>
        <button onclick="imprimirResultadosPDF()">Imprimir en PDF</button>
      </div>
    </div>
  </div>
</div>

<script>
// Variables y datos
let nombreJugadorPrincipal = '';
const resultadosGuardadosArray = []; // array para guardar resultados en la tabla

// Función para guardar el nombre y mostrar la sección principal
function guardarNombreJugador() {
  const inputNombre = document.getElementById('nombreJugadorPrincipal');
  const nombre = inputNombre.value.trim();
  if (nombre === '') {
    alert('Por favor ingresa tu nombre.');
    return;
  }
  nombreJugadorPrincipal = nombre;
  document.getElementById('inicio').style.display = 'none';
  document.getElementById('principal').style.display = 'block';
}

// Datos de partidos, incluyendo todos desde el 11 al 29 de junio
const cronogramaFifa = {
  "2026-06-11": [
    { hora: "15:00", equipos: "México vs Sudáfrica", grupo: "A", estadio: "Ciudad de México" },
    { hora: "22:00", equipos: "Corea del Sur vs República Checa", grupo: "A", estadio: "Guadalajara" }
  ],
  "2026-06-12": [
    { hora: "15:00", equipos: "Canadá vs Bosnia y Herzegovina", grupo: "B", estadio: "Toronto" },
    { hora: "21:00", equipos: "Estados Unidos vs Paraguay", grupo: "D", estadio: "Los Ángeles" }
  ],
  "2026-06-13": [
    { hora: "15:00", equipos: "Catar vs Suiza", grupo: "B", estadio: "Bahía de San Francisco" },
    { hora: "18:00", equipos: "Brasil vs Marruecos", grupo: "C", estadio: "Nueva York/Nueva Jersey" },
    { hora: "21:00", equipos: "Haití vs Escocia", grupo: "C", estadio: "Boston" },
    { hora: "00:00", equipos: "Australia vs Turquía", grupo: "D", estadio: "Vancouver" }
  ],
  "2026-06-14": [
    { hora: "13:00", equipos: "Alemania vs Curazao", grupo: "E", estadio: "Houston" },
    { hora: "16:00", equipos: "Países Bajos vs Japón", grupo: "F", estadio: "Dallas" },
    { hora: "19:00", equipos: "Costa de Marfil vs Ecuador", grupo: "E", estadio: "Filadelfia" },
    { hora: "22:00", equipos: "Suecia vs Túnez", grupo: "F", estadio: "Monterrey" }
  ],
  "2026-06-15": [
    { hora: "12:00", equipos: "España vs Cabo Verde", grupo: "H", estadio: "Atlanta" },
    { hora: "15:00", equipos: "Bélgica vs Egipto", grupo: "G", estadio: "Seattle" },
    { hora: "18:00", equipos: "Arabia Saudita vs Uruguay", grupo: "H", estadio: "Miami" },
    { hora: "21:00", equipos: "Irán vs Nueva Zelanda", grupo: "G", estadio: "Los Ángeles" }
  ],
  "2026-06-16": [
    { hora: "15:00", equipos: "Francia vs Senegal", grupo: "I", estadio: "Nueva York/Nueva Jersey" },
    { hora: "18:00", equipos: "Irak vs Noruega", grupo: "I", estadio: "Boston" },
    { hora: "21:00", equipos: "Argentina vs Argelia", grupo: "J", estadio: "Kansas City" },
    { hora: "00:00", equipos: "Austria vs Jordania", grupo: "J", estadio: "Bahía de San Francisco" }
  ],
  "2026-06-17": [
    { hora: "13:00", equipos: "Portugal vs RD Congo", grupo: "K", estadio: "Houston" },
    { hora: "16:00", equipos: "Inglaterra vs Croacia", grupo: "L", estadio: "Dallas" },
    { hora: "19:00", equipos: "Ghana vs Panamá", grupo: "L", estadio: "Toronto" },
    { hora: "22:00", equipos: "Uzbekistán vs Colombia", grupo: "K", estadio: "Ciudad de México" }
  ],
  "2026-06-18": [
    { hora: "12:00", equipos: "República Checa vs Sudáfrica", grupo: "A", estadio: "Atlanta" },
    { hora: "15:00", equipos: "Suiza vs Bosnia y Herzegovina", grupo: "B", estadio: "Los Ángeles" },
    { hora: "18:00", equipos: "Canadá vs Catar", grupo: "B", estadio: "Vancouver" },
    { hora: "21:00", equipos: "México vs Corea del Sur", grupo: "A", estadio: "Guadalajara" }
  ],
  // Agregamos partidos del 24 al 29 de junio
  "2026-06-24": [
    { hora: "15:00", equipos: "Suiza vs Canadá", grupo: "B", estadio: "Vancouver" },
    { hora: "15:00", equipos: "Bosnia y Herzegovina vs Catar", grupo: "B", estadio: "Vancouver" },
    { hora: "18:00", equipos: "Escocia vs Brasil", grupo: "C", estadio: "Estadio de San Francisco" },
    { hora: "18:00", equipos: "Marruecos vs Haití", grupo: "C", estadio: "Estadio de San Francisco" },
    { hora: "21:00", equipos: "República Checa vs México", grupo: "A", estadio: "Estadio Azteca" },
    { hora: "21:00", equipos: "Sudáfrica vs Corea del Sur", grupo: "A", estadio: "Estadio Azteca" }
  ],
  "2026-06-25": [
    { hora: "16:00", equipos: "Curazao vs Costa de Marfil", grupo: "E", estadio: "Estadio de Houston" },
    { hora: "16:00", equipos: "Ecuador vs Alemania", grupo: "E", estadio: "Estadio de Houston" },
    { hora: "19:00", equipos: "Japón vs Suecia", grupo: "F", estadio: "Estadio de Dallas" },
    { hora: "19:00", equipos: "Túnez vs Países Bajos", grupo: "F", estadio: "Estadio de Dallas" },
    { hora: "22:00", equipos: "Turquía vs Estados Unidos", grupo: "D", estadio: "Estadio de Los Angeles" },
    { hora: "22:00", equipos: "Paraguay vs Australia", grupo: "D", estadio: "Estadio de Los Angeles" }
  ],
  "2026-06-26": [
    { hora: "15:00", equipos: "Noruega vs Francia", grupo: "I", estadio: "Estadio de Atlanta" },
    { hora: "15:00", equipos: "Senegal vs Irak", grupo: "I", estadio: "Estadio de Atlanta" },
    { hora: "20:00", equipos: "Cabo Verde vs Arabia Saudita", grupo: "H", estadio: "Estadio de Madrid" },
    { hora: "20:00", equipos: "Uruguay vs España", grupo: "H", estadio: "Estadio de Madrid" },
    { hora: "23:00", equipos: "Egipto vs Irán", grupo: "G", estadio: "Estadio de Barcelona" },
    { hora: "23:00", equipos: "Nueva Zelanda vs Bélgica", grupo: "G", estadio: "Estadio de Barcelona" }
  ],
  "2026-06-27": [
    { hora: "17:00", equipos: "Panamá vs Inglaterra", grupo: "L", estadio: "Estadio de Londres" },
    { hora: "17:00", equipos: "Croacia vs Ghana", grupo: "L", estadio: "Estadio de Londres" },
    { hora: "19:30", equipos: "Colombia vs Portugal", grupo: "K", estadio: "Estadio de Lisboa" },
    { hora: "19:30", equipos: "RD Congo vs Uzbekistán", grupo: "K", estadio: "Estadio de Lisboa" },
    { hora: "22:00", equipos: "Argelia vs Austria", grupo: "J", estadio: "Estadio de Viena" },
    { hora: "22:00", equipos: "Jordania vs Argentina", grupo: "J", estadio: "Estadio de Buenos Aires" }
  ]
};

// Función para cargar las fechas en el selector
function cargarFechas() {
  const selector = document.getElementById('fecha');
  selector.innerHTML = '';
  const fechas = [
    { fecha: '2026-06-11', label: 'Jueves 11 de junio' },
    { fecha: '2026-06-12', label: 'Viernes 12 de junio' },
    { fecha: '2026-06-13', label: 'Sábado 13 de junio' },
    { fecha: '2026-06-14', label: 'Domingo 14 de junio' },
    { fecha: '2026-06-15', label: 'Lunes 15 de junio' },
    { fecha: '2026-06-16', label: 'Martes 16 de junio' },
    { fecha: '2026-06-17', label: 'Miércoles 17 de junio' },
    { fecha: '2026-06-18', label: 'Jueves 18 de junio' },
    { fecha: '2026-06-24', label: 'Miércoles 24 de junio' },
    { fecha: '2026-06-25', label: 'Jueves 25 de junio' },
    { fecha: '2026-06-26', label: 'Viernes 26 de junio' },
    { fecha: '2026-06-27', label: 'Sábado 27 de junio' }
  ];
  fechas.forEach(f => {
    const option = document.createElement('option');
    option.value = f.fecha;
    option.textContent = f.label;
    selector.appendChild(option);
  });
  // Seleccionar la primera por defecto
  selector.value = fechas[0].fecha;
}

// Cuando cargue la página, cargar las fechas
window.onload = function() {
  cargarFechas();
  // Ocultar sección principal hasta ingresar nombre
  document.getElementById('principal').style.display = 'none';
}

// Función para mostrar partidos
function mostrarPartidos() {
  const fechaSeleccionada = document.getElementById('fecha').value;
  const partidosDiv = document.getElementById('partidos');
  const partidosSelect = document.getElementById('partidoParaResultado');

  // Limpiar
  partidosDiv.innerHTML = '';
  partidosSelect.innerHTML = '';

  const partidosHoy = cronogramaFifa[fechaSeleccionada];

  if (!partidosHoy || partidosHoy.length === 0) {
    partidosDiv.innerHTML = '<p>No hay partidos programados para esta fecha.</p>';
    return;
  }

  let html = '<table><tr><th>Hora</th><th>Equipos</th><th>Grupo</th><th>Estadio</th></tr>';
  partidosHoy.forEach(p => {
    html += `<tr>
      <td>${p.hora}</td>
      <td>${p.equipos}</td>
      <td>${p.grupo}</td>
      <td>${p.estadio}</td>
    </tr>`;
    // Añadir opción para registrar resultado
    const option = document.createElement('option');
    option.value = p.equipos;
    option.textContent = p.equipos;
    document.getElementById('partidoParaResultado').appendChild(option);
  });
  html += '</table>';
  document.getElementById('partidos').innerHTML = html;
}

// Función para guardar resultados
function guardarResultadoPartido() {
  const partidoSeleccionado = document.getElementById('partidoParaResultado').value;
  const resultado = document.getElementById('resultadoPartido').value.trim();

  if (!resultado || !resultado.includes('-')) {
    alert('Por favor ingresa un marcador válido, ejemplo: 2-1.');
    return;
  }

  resultadosGuardadosArray.push({ nombre: nombreJugadorPrincipal, partido: partidoSeleccionado, resultado });
  actualizarTablaResultados();

  document.getElementById('resultadoPartido').value = '';
  alert('Resultado guardado.');
}

// Función para actualizar la tabla de resultados
function actualizarTablaResultados() {
  const tbody = document.querySelector('#tablaResultados tbody');
  tbody.innerHTML = '';
  resultadosGuardadosArray.forEach(r => {
    const fila = document.createElement('tr');
    fila.innerHTML = `<td>${r.nombre}</td><td>${r.partido}</td><td>${r.resultado}</td>`;
    tbody.appendChild(fila);
  });
}

// Función para descargar resultados en CSV
function descargarResultados() {
  let csv = 'Nombre,Partido,Resultado\n';
  resultadosGuardadosArray.forEach(r => {
    csv += `"${r.nombre}","${r.partido}","${r.resultado}"\n`;
  });
  const blob = new Blob([csv], { type: 'text/csv' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'resultados_guardados.csv';
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
}

// Función mejorada para imprimir resultados en PDF con márgenes ajustados
function imprimirResultadosPDF() {
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF();

  const margenIzquierdo = 20;
  const margenSuperior = 20;
  const anchoPagina = 210; // tamaño A4 en mm
  const anchoColumna1 = 60;
  const anchoColumna2 = 70;
  const anchoColumna3 = 40;
  const altoLinea = 8;

  // Encabezado
  doc.setFontSize(16);
  doc.text('Resultados Guardados', margenIzquierdo, margenSuperior);
  // Línea debajo del encabezado
  doc.setLineWidth(0.5);
  doc.line(margenIzquierdo, margenSuperior + 2, anchoPagina - margenIzquierdo, margenSuperior + 2);

  let yActual = margenSuperior + 10;

  // Encabezados de columnas
  doc.setFontSize(12);
  doc.setFont('helvetica', 'bold');
  doc.text('Nombre', margenIzquierdo, yActual);
  doc.text('Partido', margenIzquierdo + anchoColumna1, yActual);
  doc.text('Resultado', margenIzquierdo + anchoColumna1 + anchoColumna2, yActual);

  yActual += altoLinea;

  // Línea debajo de encabezados
  doc.setLineWidth(0.2);
  doc.line(margenIzquierdo, yActual - 3, anchoPagina - margenIzquierdo, yActual - 3);

  // Poner los resultados
  doc.setFontSize(11);
  resultadosGuardadosArray.forEach((r) => {
    if (yActual > 280) { // si pasa el margen inferior, agregar página
      doc.addPage();
      yActual = margenSuperior + 10;
    }
    // Agregar texto
    doc.setFont('helvetica', 'normal');
    doc.text(r.nombre, margenIzquierdo, yActual);
    doc.text(r.partido, margenIzquierdo + anchoColumna1, yActual);
    doc.text(r.resultado, margenIzquierdo + anchoColumna1 + anchoColumna2, yActual);
    yActual += altoLinea;
  });

  // Mostrar en ventana
  window.open(doc.output('bloburl'));
}
</script>


<script></script></body></html>
