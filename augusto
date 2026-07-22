<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hoja de Inspección - Centro de Diagnóstico Automotriz</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background-color: #f4f7f6; padding: 0; margin: 0; color: #333; }
        
        /* Pantalla de Inicio / Splash Screen */
        #pantalla-inicio {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.8)), 
                        url('https://images.unsplash.com/photo-1614162692292-7ac56d7f7f1e?auto=format&fit=crop&q=80&w=1920') no-repeat center center/cover;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            color: white;
            text-align: center;
            transition: opacity 0.5s ease, visibility 0.5s ease;
        }
        #pantalla-inicio h1 {
            font-size: 3.5rem;
            margin: 0;
            text-transform: uppercase;
            letter-spacing: 4px;
            text-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }
        #pantalla-inicio p {
            font-size: 1.2rem;
            margin: 15px 0 30px 0;
            color: #ddd;
            text-shadow: 0 2px 5px rgba(0,0,0,0.5);
        }
        .btn-ingresar {
            background-color: #2980b9;
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.1rem;
            font-weight: bold;
            border-radius: 30px;
            cursor: pointer;
            text-transform: uppercase;
            box-shadow: 0 4px 15px rgba(41, 128, 185, 0.4);
            transition: all 0.3s ease;
        }
        .btn-ingresar:hover {
            background-color: #3498db;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(52, 152, 219, 0.6);
        }

        /* Contenedor Principal de la Hoja de Trabajo */
        .container { max-width: 950px; margin: 40px auto; background: white; padding: 25px; border-radius: 8px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); position: relative; }
        
        /* Panel de Control */
        .control-panel { display: flex; justify-content: space-between; align-items: center; background: #e8f4fd; padding: 12px 15px; border-radius: 6px; margin-bottom: 20px; border: 1px solid #b3d7f7; }
        .status-save { font-size: 0.9em; color: #2c3e50; font-weight: bold; display: flex; align-items: center; gap: 8px; }
        .btn-group { display: flex; gap: 10px; }
        .btn-action { color: white; border: none; padding: 8px 16px; border-radius: 4px; cursor: pointer; font-weight: bold; font-size: 0.85em; transition: background 0.2s; }
        .btn-save { background: #2980b9; }
        .btn-save:hover { background: #1f618d; }
        .btn-clear { background: #e74c3c; }
        .btn-clear:hover { background: #c0392b; }

        /* Apartado de Presentación */
        .header-presentacion { border: 2px solid #2c3e50; border-radius: 6px; padding: 20px; margin-bottom: 25px; background-color: #fafafa; }
        .header-top { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #2c3e50; padding-bottom: 15px; margin-bottom: 15px; }
        
        /* Logo del Taller */
        .logo-container { position: relative; width: 130px; height: 75px; }
        .logo-area { width: 100%; height: 100%; border: 2px dashed #95a5a6; display: flex; align-items: center; justify-content: center; font-size: 0.75em; color: #7f8c8d; text-align: center; border-radius: 4px; cursor: pointer; overflow: hidden; background: #fff; transition: all 0.2s; }
        .logo-area:hover { border-color: #2980b9; background-color: #f7f9fa; color: #2980b9; }
        .logo-area img { width: 100%; height: 100%; object-fit: contain; }
        #logo_file_input { display: none; }

        .title-area { text-align: right; }
        .title-area h1 { margin: 0; color: #2c3e50; font-size: 1.6em; text-transform: uppercase; }
        .title-area p { margin: 5px 0 0 0; color: #7f8c8d; font-size: 0.9em; }
        
        .info-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; }
        .info-box { border: 1px solid #dcdde1; border-radius: 4px; padding: 10px; background: #fff; }
        .info-box h3 { margin: 0 0 8px 0; font-size: 0.9em; color: #2980b9; text-transform: uppercase; border-bottom: 1px solid #f1f2f6; padding-bottom: 4px; }
        .info-row { display: flex; margin-bottom: 6px; font-size: 0.9em; }
        .info-row span { font-weight: bold; color: #555; min-width: 90px; }
        .info-val { flex-grow: 1; border-bottom: 1px dotted #ccc; padding-left: 5px; min-height: 18px; }

        /* Estilos de la Tabla Principal */
        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        th, td { border: 1px solid #dcdde1; padding: 10px; text-align: left; font-size: 0.95em; }
        th { background-color: #2c3e50; color: white; text-align: center; text-transform: uppercase; font-size: 0.85em; }
        .cat { background-color: #f1f2f6; font-weight: bold; color: #2980b9; border-left: 5px solid #2980b9; }
        .check-col { text-align: center; width: 75px; }
        
        [contenteditable="true"]:hover { background-color: #fffde7; outline: 1px dashed #f1c40f; }
        [contenteditable="true"]:focus { outline: 2px solid #2980b9; background-color: #fff; }
        input[type="checkbox"] { transform: scale(1.4); cursor: pointer; }
        
        /* Bloques de Observaciones, Materiales y Fotos */
        .rec-box { margin-top: 25px; border: 1px solid #dcdde1; padding: 15px; border-radius: 5px; background-color: #fafafa; }
        .rec-title { font-weight: bold; color: #2c3e50; display: block; margin-bottom: 8px; }
        .text-editor { min-height: 100px; border: 1px dashed #ccc; padding: 10px; background-color: #fff; border-radius: 4px; }
        
        /* Sección de Fotos */
        .fotos-container { margin-top: 15px; }
        .fotos-uploader { display: flex; align-items: center; justify-content: center; border: 2px dashed #95a5a6; padding: 20px; border-radius: 5px; background-color: #fff; cursor: pointer; color: #7f8c8d; transition: all 0.2s; }
        .fotos-uploader:hover { border-color: #2980b9; color: #2980b9; background-color: #f7f9fa; }
        #fotos_file_input { display: none; }
        .fotos-galeria { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 12px; margin-top: 15px; }
        .foto-preview-wrapper { position: relative; border: 1px solid #dcdde1; border-radius: 6px; padding: 4px; background: white; box-shadow: 0 2px 5px rgba(0,0,0,0.05); }
        .foto-preview-wrapper img { width: 100%; height: 110px; object-fit: cover; border-radius: 4px; }
        .btn-remove-foto { position: absolute; top: -5px; right: -5px; background: #e74c3c; color: white; border: none; border-radius: 50%; width: 22px; height: 22px; font-size: 11px; cursor: pointer; font-weight: bold; display: flex; align-items: center; justify-content: center; box-shadow: 0 2px 4px rgba(0,0,0,0.2); }
        
        /* Apartado de Historial / Guardados */
        .history-section { margin-top: 40px; border-top: 3px double #2c3e50; padding-top: 20px; }
        .history-section h2 { color: #2c3e50; font-size: 1.4em; margin-bottom: 10px; display: flex; align-items: center; gap: 8px; }
        .history-table { width: 100%; border-collapse: collapse; margin-top: 10px; background: #fff; }
        .history-table th { background-color: #7f8c8d; color: white; }
        .history-table td { padding: 8px 12px; }
        .btn-table { padding: 4px 8px; border: none; border-radius: 3px; cursor: pointer; font-weight: bold; font-size: 0.8em; }
        .btn-load { background: #3498db; color: white; margin-right: 5px; }
        .btn-load:hover { background: #2980b9; }
        .btn-delete { background: #e74c3c; color: white; }
        .btn-delete:hover { background: #c0392b; }

        .print-btn { display: block; width: 100%; padding: 15px; background: #2ed573; color: white; border: none; border-radius: 5px; font-size: 16px; font-weight: bold; cursor: pointer; margin-top: 20px; }
        .print-btn:hover { background: #26af5f; }
        
        @media print { 
            .print-btn, .control-panel, .history-section, .fotos-uploader, .btn-remove-foto { display: none !important; } 
            body { padding: 0; background: white; } 
            .container { box-shadow: none; width: 100%; max-width: 100%; margin: 0; padding: 0; } 
            [contenteditable="true"]:hover { outline: none; } 
            .logo-area { border: none !important; }
            .fotos-galeria { grid-template-columns: repeat(3, 1fr); } 
        }
    </style>
</head>
<body>

<!-- Pantalla de Inicio / Splash Screen con Audi RS7 de fondo -->
<div id="pantalla-inicio">
    <h1>Centro de Diagnóstico</h1>
    <p>Tecnología y precisión para tu vehículo de alto rendimiento</p>
    <button class="btn-ingresar" onclick="ingresarAlSistema()">Ingresar al Sistema</button>
</div>

<div class="container">

    <div class="control-panel">
        <div class="status-save">
            <span style="font-size: 1.2em;">🔧</span> Panel de Trabajo
        </div>
        <div class="btn-group">
            <button class="btn-action btn-save" onclick="guardarProgreso()">💾 Guardar Reporte</button>
            <button class="btn-action btn-clear" onclick="limpiarFormulario()">🔄 Nuevo Limpio</button>
        </div>
    </div>
    
    <div class="header-presentacion">
        <div class="header-top">
            <div class="logo-container">
                <div class="logo-area" onclick="document.getElementById('logo_file_input').click()" id="logo_container_display">
                    <span id="logo_placeholder_text">[ HAZ CLIC PARA SUBIR LOGO ]</span>
                </div>
                <input type="file" id="logo_file_input" accept="image/*" onchange="cargarImagenLogo(event)">
            </div>

            <div class="title-area">
                <h1 contenteditable="true" id="save_main_title">Centro de Diagnóstico Automotriz</h1>
            </div>
        </div>
        
        <div class="info-grid">
            <div class="info-box">
                <h3>Datos Generales</h3>
                <div class="info-row"><span>Cliente:</span><div class="info-val" contenteditable="true" id="save_cli_name"></div></div>
                <div class="info-row"><span>Fecha:</span><div class="info-val" contenteditable="true" id="save_cli_date"></div></div>
                <div class="info-row"><span>Orden N°:</span><div class="info-val" contenteditable="true" id="save_cli_order"></div></div>
                <div class="info-row"><span>Técnico:</span><div class="info-val" contenteditable="true" id="save_cli_tech"></div></div>
            </div>
            
            <div class="info-box">
                <h3>Especificaciones del Vehículo</h3>
                <div class="info-row"><span>Vehículo:</span><div class="info-val" contenteditable="true" id="save_veh_brand"></div></div>
                <div class="info-row"><span>Año / Placa:</span><div class="info-val" contenteditable="true" id="save_veh_plate"></div></div>
                <div class="info-row"><span>Kilometraje:</span><div class="info-val" contenteditable="true" id="save_veh_km"></div></div>
                <div class="info-row"><span>Motor/Vin:</span><div class="info-val" contenteditable="true" id="save_veh_vin"></div></div>
            </div>
        </div>
    </div>
    
    <table>
        <thead>
            <tr>
                <th>Elemento de Inspección</th>
                <th class="check-col" style="background:#2ed573">Bueno</th>
                <th class="check-col" style="background:#ffa502">Regular</th>
                <th class="check-col" style="background:#ff4757">Malo</th>
                <th class="check-col" style="background:#747d8c">N/A</th>
            </tr>
        </thead>
        <tbody id="tabla-inspeccion">
            <!-- 1° SISTEMA DEL MOTOR -->
            <tr class="cat"><td colspan="5" contenteditable="true" id="cat_1">Sistema del Motor</td></tr>
            <tr><td contenteditable="true" id="item_1">Fugas de aceite</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_2">Fugas de refrigerante motor</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_3">Condición del líquido refrigerante</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_4">Correas de accesorios (Alternador, AC)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_5">Filtros de aire (Motor)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_6">Sistemas de luces testigos (Check Engine / Presión)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_7">Escobillas wiper (Limpiaparabrisas)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_8">Soportes de motor</td><td></td><td></td><td></td><td></td></tr>

            <!-- 2° SISTEMA ELÉCTRICO Y ENCENDIDO -->
            <tr class="cat"><td colspan="5" contenteditable="true" id="cat_2">Sistema Eléctrico y Encendido</td></tr>
            <tr><td contenteditable="true" id="item_14">Batería (Bornes, voltaje y estado de carga)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_9">Luces de posición (Pilotos)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_10">Luces de frenos</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_11">Luces altas</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_12">Luces bajas</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_13">Intermitentes direccionales y de emergencia</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_15">Sistema de carga (Alternador)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_16">Motor de arranque</td><td></td><td></td><td></td><td></td></tr>

            <!-- 3° SISTEMA DE SUSPENSIÓN Y DIRECCIÓN -->
            <tr class="cat"><td colspan="5" contenteditable="true" id="cat_3">Sistema de Suspensión y Dirección</td></tr>
            <tr><td contenteditable="true" id="item_17">Reapriete de suspensión y dirección</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_18">Amortiguadores delanteros</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_19">Amortiguadores traseros</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_20">Terminales y rótulas de dirección</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_21">Links cauchos de barra estabilizadora</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_22">Líquido de power steering (Dirección asistida)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_23">Brazos de suspensión esféricas (Muñones)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_24">Brazos suspensión traseros / Bujes</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_25">Botas de cremallera de dirección</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_26">Llantas (Presión, desgaste y profundidad)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_27">Rodamientos de rueda (Bocines)</td><td></td><td></td><td></td><td></td></tr>

            <!-- 4° SISTEMA DE FRENOS -->
            <tr class="cat"><td colspan="5" contenteditable="true" id="cat_4">Sistema de Frenos</td></tr>
            <tr><td contenteditable="true" id="item_28">Tacos (Pastillas) de frenos delanteros</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_29">Tacos (Pastillas) de frenos traseros</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_30">Latiguillos de freno (Mangueras)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_31">Tuberías metálicas de freno</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_32">Líquido de frenos (Nivel y humedad)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_33">Bandas de frenos (Zapatas) y tambores</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_34">Discos de freno (Grosor y superficie)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_35">Freno de mano (Estacionamiento)</td><td></td><td></td><td></td><td></td></tr>

            <!-- 5° SISTEMA DE TRANSMISIÓN Y EMBRAGUE -->
            <tr class="cat"><td colspan="5" contenteditable="true" id="cat_5">Sistema de Transmisión y Embrague</td></tr>
            <tr><td contenteditable="true" id="item_36">Fugas en cárter o sellos de transmisión</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_37">Botas de ejes/flechas (Guardapolvos)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_38">Ejes de mando o cardan / Cruces</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_39">Nivel/Condición del aceite de transmisión</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_40">Operación del embrague (Clutch)</td><td></td><td></td><td></td><td></td></tr>
            <tr><td contenteditable="true" id="item_41">Soportes de transmisión</td><td></td><td></td><td></td><td></td></tr>
        </tbody>
    </table>

    <div class="rec-box">
        <span class="rec-title">📋 Recomendaciones y Observaciones Técnicas:</span>
        <div contenteditable="true" id="save_recommendations" class="text-editor">Escriba aquí los detalles del diagnóstico...</div>
    </div>

    <!-- Sección Materiales Utilizados -->
    <div class="rec-box">
        <span class="rec-title">🛠️ Materiales, Repuestos y Consumibles Utilizados:</span>
        <div contenteditable="true" id="save_materials" class="text-editor">Escriba aquí los repuestos e insumos requeridos o instalados...</div>
    </div>

    <!-- Evidencia Fotográfica / Múltiples Imágenes -->
    <div class="rec-box">
        <span class="rec-title">📸 Registro de Evidencia Fotográfica:</span>
        <div class="fotos-container">
            <div class="fotos-uploader" onclick="document.getElementById('fotos_file_input').click()">
                <span>[ CLIC PARA CARGAR FOTOGRAFÍAS DEL DIAGNÓSTICO ]</span>
            </div>
            <input type="file" id="fotos_file_input" accept="image/*" multiple onchange="cargarMultiplesFotos(event)">
            <div class="fotos-galeria" id="galeria_evidencias"></div>
        </div>
    </div>

    <button class="print-btn" onclick="window.print()">Imprimir o Guardar Reporte en PDF</button>

    <div class="history-section">
        <h2>📁 Historial de Reportes Guardados</h2>
        <p style="font-size: 0.85em; color: #7f8c8d; margin: 0 0 10px 0;">Aquí abajo aparecerán todos los vehículos que vayas guardando para poder recuperarlos después.</p>
        <table class="history-table">
            <thead>
                <tr>
                    <th>Fecha</th>
                    <th>Orden N°</th>
                    <th>Cliente</th>
                    <th>Vehículo / Placa</th>
                    <th>Acciones</th>
                </tr>
            </thead>
            <tbody id="lista-historial">
                <tr><td colspan="5" style="text-align:center; color:#95a5a6;">No hay reportes guardados en el historial todavía.</td></tr>
            </tbody>
        </table>
    </div>

</div>

<script>
    let imagenLogoBase64 = "";
    let listaFotosBase64 = []; 

    function ingresarAlSistema() {
        const pantalla = document.getElementById('pantalla-inicio');
        pantalla.style.opacity = '0';
        setTimeout(() => {
            pantalla.style.visibility = 'hidden';
        }, 500);
    }

    function cargarImagenLogo(event) {
        const archivo = event.target.files[0];
        if (archivo) {
            const lector = new FileReader();
            lector.onload = function(e) {
                imagenLogoBase64 = e.target.result;
                mostrarLogoEnPantalla(imagenLogoBase64);
            }
            lector.readAsDataURL(archivo);
        }
    }

    function mostrarLogoEnPantalla(srcBase64) {
        const contenedor = document.getElementById('logo_container_display');
        if (srcBase64) {
            contenedor.innerHTML = `<img src="${srcBase64}" alt="Logo Taller">`;
        } else {
            contenedor.innerHTML = `<span id="logo_placeholder_text">[ HAZ CLIC PARA SUBIR LOGO ]</span>`;
        }
    }

    function cargarMultiplesFotos(event) {
        const archivos = event.target.files;
        if (!archivos) return;

        Array.from(archivos).forEach(archivo => {
            const lector = new FileReader();
            lector.onload = function(e) {
                const base64Str = e.target.result;
                listaFotosBase64.push(base64Str);
                renderizarGaleriaEvidencias();
            }
            lector.readAsDataURL(archivo);
        });
        event.target.value = "";
    }

    function renderizarGaleriaEvidencias() {
        const galeria = document.getElementById('galeria_evidencias');
        galeria.innerHTML = "";
        
        listaFotosBase64.forEach((foto, index) => {
            const wrapper = document.createElement('div');
            wrapper.className = "foto-preview-wrapper";
            
            wrapper.innerHTML = `
                <img src="${foto}" alt="Evidencia ${index + 1}">
                <button class="btn-remove-foto" onclick="eliminarFotoEvidencia(${index})">×</button>
            `;
            galeria.appendChild(wrapper);
        });
    }

    function eliminarFotoEvidencia(index) {
        listaFotosBase64.splice(index, 1);
        renderizarGaleriaEvidencias();
    }

    // Inyectar dinámicamente checkboxes correspondientes a la nueva estructura de la tabla
    document.querySelectorAll('#tabla-inspeccion tr:not(.cat)').forEach((row, rowIndex) => {
        const cells = row.querySelectorAll('td');
        for (let i = 1; i <= 4; i++) {
            const check = document.createElement('input');
            check.type = 'checkbox';
            check.id = `check_${rowIndex}_${i}`;
            check.onclick = function() {
                row.querySelectorAll('input').forEach(cb => { if(cb !== this) cb.checked = false; });
            };
            cells[i].classList.add('check-col');
            cells[i].appendChild(check);
        }
    });

    function guardarProgreso() {
        const ordenNum = document.getElementById('save_cli_order').innerText.trim() || "S-N";
        const cliente = document.getElementById('save_cli_name').innerText.trim() || "Desconocido";
        const vehiculo = document.getElementById('save_veh_brand').innerText.trim() || "General";
        const placa = document.getElementById('save_veh_plate').innerText.trim() || "";
        const fecha = document.getElementById('save_cli_date').innerText.trim() || new Date().toLocaleDateString();

        const datosFormulario = {};
        
        document.querySelectorAll('[contenteditable="true"]').forEach(el => {
            if (el.id) datosFormulario[el.id] = el.innerText;
        });

        document.querySelectorAll('tbody#tabla-inspeccion input[type="checkbox"]').forEach(cb => {
            datosFormulario[cb.id] = cb.checked;
        });

        let historial = JSON.parse(localStorage.getItem('historialTalleres')) || [];
        historial = historial.filter(item => item.idOrden !== ordenNum);

        historial.push({
            idOrden: ordenNum,
            fecha: fecha,
            cliente: cliente,
            vehiculo: vehiculo + " " + placa,
            logoImg: imagenLogoBase64,
            evidenciaFotos: listaFotosBase64, 
            cuerpo: datosFormulario
        });

        try {
            localStorage.setItem('historialTalleres', JSON.stringify(historial));
            alert("💾 ¡Reporte e imágenes guardados exitosamente en el historial!");
            actualizarTablaHistorial();
        } catch (e) {
            alert("⚠️ Error: El almacenamiento del navegador está lleno. Intenta reducir la cantidad o peso de las fotos de evidencia.");
        }
    }

    function actualizarTablaHistorial() {
        const historial = JSON.parse(localStorage.getItem('historialTalleres')) || [];
        const tabla = document.getElementById('lista-historial');
        
        if (historial.length === 0) {
            tabla.innerHTML = `<tr><td colspan="5" style="text-align:center; color:#95a5a6;">No hay reportes guardados en el historial todavía.</td></tr>`;
            return;
        }

        tabla.innerHTML = "";
        historial.forEach((reporte) => {
            const fila = document.createElement('tr');
            fila.innerHTML = `
                <td>${reporte.fecha}</td>
                <td><strong>${reporte.idOrden}</strong></td>
                <td>${reporte.cliente}</td>
                <td>${reporte.vehiculo}</td>
                <td>
                    <button class="btn-table btn-load" onclick="cargarReporte('${reporte.idOrden}')">📂 Abrir</button>
                    <button class="btn-table btn-delete" onclick="eliminarReporte('${reporte.idOrden}')">❌ Borrar</button>
                </td>
            `;
            tabla.appendChild(fila);
        });
    }

    function cargarReporte(idOrden) {
        const historial = JSON.parse(localStorage.getItem('historialTalleres')) || [];
        const reporte = historial.find(item => item.idOrden === idOrden);

        if (!reporte) return;

        imagenLogoBase64 = reporte.logoImg || "";
        mostrarLogoEnPantalla(imagenLogoBase64);

        listaFotosBase64 = reporte.evidenciaFotos || [];
        renderizarGaleriaEvidencias();

        const datos = reporte.cuerpo;

        document.querySelectorAll('tbody#tabla-inspeccion input[type="checkbox"]').forEach(cb => cb.checked = false);

        Object.keys(datos).forEach(id => {
            const el = document.getElementById(id);
            if (el) {
                if (el.type === 'checkbox') {
                    el.checked = datos[id];
                } else {
                    el.innerText = datos[id];
                }
            }
        });
        
        alert(`📂 Reporte y registros de la Orden N° ${idOrden} cargados.`);
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    function eliminarReporte(idOrden) {
        if (confirm(`¿Seguro que quieres eliminar definitivamente la Orden N° ${idOrden} del historial?`)) {
            let historial = JSON.parse(localStorage.getItem('historialTalleres')) || [];
            historial = historial.filter(item => item.idOrden !== idOrden);
            localStorage.setItem('historialTalleres', JSON.stringify(historial));
            actualizarTablaHistorial();
        }
    }

    function limpiarFormulario() {
        if(confirm("¿Quieres limpiar la pantalla actual para recibir un vehículo nuevo? (No afectará tu historial guardado abajo)")) {
            document.getElementById('save_cli_name').innerText = "Cliente Nuevo";
            document.getElementById('save_cli_order').innerText = Math.floor(1000 + Math.random() * 9000);
            document.getElementById('save_cli_date').innerText = new Date().toLocaleDateString();
            document.getElementById('save_veh_brand').innerText = "Marca / Modelo";
            document.getElementById('save_veh_plate').innerText = "Placa";
            document.getElementById('save_veh_km').innerText = "0 Km";
            document.getElementById('save_veh_vin').innerText = "-";
            document.getElementById('save_recommendations').innerText = "Escriba aquí los detalles del diagnóstico...";
            document.getElementById('save_materials').innerText = "Escriba aquí los repuestos e insumos requeridos o instalados...";
            
            listaFotosBase64 = [];
            renderizarGaleriaEvidencias();
            
            document.querySelectorAll('tbody#tabla-inspeccion input[type="checkbox"]').forEach(cb => cb.checked = false);
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }
    }

    window.onload = actualizarTablaHistorial;
</script>

</body>
</html>
