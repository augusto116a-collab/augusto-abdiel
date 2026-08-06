import docx
from docx import Document
from docx.shared import Inches, Pt, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.enum.table import WD_TABLE_ALIGNMENT
from docx.oxml import OxmlElement, parse_xml
from docx.oxml.ns import nsdecls, qn

doc = Document()

# Page Setup (A4, standard margins)
for section in doc.sections:
    section.top_margin = Inches(1)
    section.bottom_margin = Inches(1)
    section.left_margin = Inches(1)
    section.right_margin = Inches(1)

# Helper function for setting cell shading
def set_cell_background(cell, fill_hex):
    tcPr = cell._tc.get_or_add_tcPr()
    shd = parse_xml(f'<w:shd {nsdecls("w")} w:fill="{fill_hex}"/>')
    tcPr.append(shd)

# Helper function for cell margins
def set_cell_margins(cell, top=100, bottom=100, left=150, right=150):
    tcPr = cell._tc.get_or_add_tcPr()
    tcMar = OxmlElement('w:tcMar')
    for m, val in [('top', top), ('bottom', bottom), ('left', left), ('right', right)]:
        node = OxmlElement(f'w:{m}')
        node.set(qn('w:w'), str(val))
        node.set(qn('w:type'), 'dxa')
        tcMar.append(node)
    tcPr.append(tcMar)

# Helper function for paragraph formatting
def add_custom_heading(doc, text, level):
    h = doc.add_heading(text, level=level)
    h.paragraph_format.space_before = Pt(14)
    h.paragraph_format.space_after = Pt(6)
    h.paragraph_format.keep_with_next = True
    for run in h.runs:
        run.font.name = 'Calibri'
        if level == 1:
            run.font.size = Pt(18)
            run.font.bold = True
            run.font.color.rgb = RGBColor(31, 78, 121) # Deep Navy
        elif level == 2:
            run.font.size = Pt(14)
            run.font.bold = True
            run.font.color.rgb = RGBColor(46, 117, 182) # Slate Blue
    return h

# --- COVER / PRESENTACIÓN ---
p_title_space = doc.add_paragraph()
p_title_space.paragraph_format.space_before = Pt(36)

p_title = doc.add_paragraph()
p_title.alignment = WD_ALIGN_PARAGRAPH.CENTER
r_title = p_title.add_run("BITÁCORA DE APRENDIZAJE Y PRÁCTICAS\nMANTENIMIENTO PREVENTIVO Y DESARROLLO DE CHECKLIST DIGITAL")
r_title.font.name = 'Calibri'
r_title.font.size = Pt(24)
r_title.font.bold = True
r_title.font.color.rgb = RGBColor(31, 78, 121)

p_sub = doc.add_paragraph()
p_sub.alignment = WD_ALIGN_PARAGRAPH.CENTER
r_sub = p_sub.add_run("Informe semanal del Proceso de Aprendizaje y Desarrollo de Proyecto ABT/ABR")
r_sub.font.name = 'Calibri'
r_sub.font.size = Pt(14)
r_sub.font.italic = True
r_sub.font.color.rgb = RGBColor(89, 89, 89)

doc.add_paragraph().paragraph_format.space_before = Pt(120)

# Presentación / Datos table
table_pres = doc.add_table(rows=5, cols=2)
table_pres.alignment = WD_TABLE_ALIGNMENT.CENTER
data_pres = [
    ("Estudiante:", "Augusto Aguilar Macre"),
    ("Especialidad:", "Técnico en Vehículos Livianos"),
    ("Institución:", "Instituto Técnico Superior Especializado (ITSE)"),
    ("Asignatura:", "Mantenimiento Preventivo / Proyecto ABR"),
    ("Fecha:", "2026")
]
for i, (k, v) in enumerate(data_pres):
    row = table_pres.rows[i]
    row.cells[0].paragraphs[0].add_run(k).bold = True
    row.cells[0].paragraphs[0].runs[0].font.color.rgb = RGBColor(31, 78, 121)
    row.cells[0].paragraphs[0].runs[0].font.name = 'Calibri'
    row.cells[1].paragraphs[0].add_run(v).font.name = 'Calibri'
    set_cell_background(row.cells[0], "F2F2F2")
    set_cell_background(row.cells[1], "FFFFFF")
    set_cell_margins(row.cells[0], top=120, bottom=120, left=150, right=150)
    set_cell_margins(row.cells[1], top=120, bottom=120, left=150, right=150)

doc.add_page_break()

# --- ÍNDICE ---
add_custom_heading(doc, "ÍNDICE GENERAL", level=1)

p_toc = doc.add_paragraph()
p_toc.paragraph_format.space_after = Pt(18)
toc_items = [
    ("1. Sobre Mí", "3"),
    ("2. Registro de Actividades y Aprendizaje Semanal", "4"),
    ("   - Semana 1: Herramientas de Mantenimiento Preventivo y Desarme de Motor", "4"),
    ("   - Semana 2: Introducción al Proyecto ABR y Reconocimiento de Taller", "4"),
    ("   - Semana 3: Mediciones Eléctricas con Voltímetro y Práctica Preventiva", "5"),
    ("   - Semana 4: Clasificación de Herramientas e Investigación de Fusibles", "5"),
    ("   - Semana 5: Sensores en Motores Diésel y Gasolina", "6"),
    ("   - Semana 6: Primera Prueba Piloto del Checklist Digital", "6"),
    ("   - Semana 7: Segunda Prueba, Reordenamiento y Flujo de Trabajo", "7"),
    ("   - Semana 8: Trabajo en Equipo y Adición del Apartado Fotográfico", "7"),
    ("   - Semana 9: Validación Final e Implementación del Checklist", "8"),
    ("   - Semana 10: Diagnóstico de Fusibles y Medición Directa", "8"),
    ("3. Conclusión General", "9")
]

tbl_toc = doc.add_table(rows=len(toc_items), cols=2)
tbl_toc.alignment = WD_TABLE_ALIGNMENT.CENTER
for idx, (title, page) in enumerate(toc_items):
    row = tbl_toc.rows[idx]
    p0 = row.cells[0].paragraphs[0]
    p1 = row.cells[1].paragraphs[0]
    
    r0 = p0.add_run(title)
    r0.font.name = 'Calibri'
    if not title.startswith("   -"):
        r0.font.bold = True
        r0.font.color.rgb = RGBColor(31, 78, 121)
    else:
        r0.font.color.rgb = RGBColor(59, 59, 59)
        
    r1 = p1.add_run(page)
    r1.font.name = 'Calibri'
    r1.font.bold = True
    p1.alignment = WD_ALIGN_PARAGRAPH.RIGHT
    
    set_cell_margins(row.cells[0], top=60, bottom=60, left=0, right=100)
    set_cell_margins(row.cells[1], top=60, bottom=60, left=100, right=0)

doc.add_page_break()

# --- SOBRE MÍ ---
add_custom_heading(doc, "1. SOBRE MÍ", level=1)

p_about = doc.add_paragraph()
p_about.paragraph_format.line_spacing = 1.15
p_about.paragraph_format.space_after = Pt(12)
r_about = p_about.add_run(
    "¡Hola! Mi nombre es Augusto Aguilar Macre. Tengo 19 años y actualmente soy estudiante de la carrera técnica "
    "en Vehículos Livianos en el Instituto Técnico Superior Especializado (ITSE). Apasionado por la mecánica automotriz, "
    "el diagnóstico vehicular y las nuevas tecnologías aplicadas al mantenimiento de autos, busco constantemente "
    "combinar la teoría práctica del taller con soluciones digitales e innovadoras.\n\n"
    "Mi objetivo a través de esta bitácora es consolidar todo el conocimiento práctico y teórico adquirido a lo largo "
    "de 10 semanas de formación intensiva. Desde la manipulación directa de herramientas y componentes del motor "
    "hasta la conceptualización, prueba y perfeccionamiento de un checklist digital para el Mantenimiento Basado en "
    "Retos (ABR), este documento refleja mi evolución, comprensión y capacidad diagnóstica como futuro profesional del área automotriz."
)
r_about.font.name = 'Calibri'
r_about.font.size = Pt(11)

# Highlight Callout Box
tbl_box = doc.add_table(rows=1, cols=1)
tbl_box.alignment = WD_TABLE_ALIGNMENT.CENTER
cell_box = tbl_box.rows[0].cells[0]
set_cell_background(cell_box, "F2F4F8")
set_cell_margins(cell_box, top=150, bottom=150, left=200, right=200)

p_box = cell_box.paragraphs[0]
r_box_title = p_box.add_run("Perfil Académico & Enfoque:\n")
r_box_title.bold = True
r_box_title.font.name = 'Calibri'
r_box_title.font.color.rgb = RGBColor(31, 78, 121)

r_box_text = p_box.add_run(
    "• Estudiante de Mecánica y Vehículos Livianos (ITSE)\n"
    "• Enfoque principal: Diagnóstico eléctrico, sistemas de inyección/sensores y digitalización del mantenimiento preventivo.\n"
    "• Compromiso: Calidad, orden, seguridad en el taller y trabajo colaborativo eficiente."
)
r_box_text.font.name = 'Calibri'
r_box_text.font.size = Pt(10.5)

doc.add_paragraph().paragraph_format.space_after = Pt(12)

# --- REGISTRO DE ACTIVIDADES Y APRENDIZAJE SEMANAL ---
add_custom_heading(doc, "2. REGISTRO DE ACTIVIDADES Y APRENDIZAJE SEMANAL", level=1)

weeks_data = [
    {
        "week": "SEMANA 1",
        "title": "Herramientas de Mantenimiento Preventivo y Desarme de Motor",
        "desc": "Vimos las distintas herramientas que se utilizan en un mantenimiento preventivo. Probamos las herramientas desarmando un motor y hablamos de cuál sería el proyecto de ABR.",
        "learned": "En esta primera semana aprendí a identificar y clasificar las herramientas manuales y específicas necesarias para ejecutar un mantenimiento preventivo seguro. Al usarlas directamente en el desarme del motor, entendí la importancia fundamental del torque adecuado, la selección correcta del dado/llave para no aislar la tornillería y el orden sistemático al desensamblar. Además, comprendí el propósito del proyecto ABR (Aprendizaje Basado en Retos), enfocando nuestro trabajo práctico hacia la solución de un problema real del taller."
    },
    {
        "week": "SEMANA 2",
        "title": "Introducción al Proyecto ABR y Reconocimiento de Taller",
        "desc": "Hablamos de la ABR donde desarrollaremos un checklist digital. Nos presentaron una hoja de un checklist para guiarnos de cómo debe ser. Bajamos al taller a ver los equipos y familiarizarnos con ellos.",
        "learned": "Aprendí a estructurar la información clave que debe contener una inspección técnica automotriz. Analizando el formato físico en papel, identifiqué qué puntos críticos no pueden faltar al evaluar un vehículo. Al recorrer el taller y reconocer los equipos (elevadores, alineadoras, bancos de prueba), entendí cómo trasladar esa inspección manual a una herramienta digital que optimice el tiempo, reduzca errores de registro y facilite el trabajo del técnico."
    },
    {
        "week": "SEMANA 3",
        "title": "Mediciones Eléctricas con Voltímetro y Práctica Preventiva",
        "desc": "Vimos las mediciones con un voltímetro. Pusimos a prueba cómo se hace un mantenimiento preventivo.",
        "learned": "En esta semana comprendí el uso práctico e imprescindible del voltímetro en el diagnóstico automotriz. Aprendí a medir la tensión de la batería en reposo, durante el arranque y con el alternador en funcionamiento para verificar el sistema de carga. También apliqué una rutina real de mantenimiento preventivo, comprendiendo que la electricidad y la mecánica van de la mano para asegurar el correcto funcionamiento del vehículo."
    },
    {
        "week": "SEMANA 4",
        "title": "Clasificación de Herramientas e Investigación de Fusibles",
        "desc": "Se vieron todos los tipos de herramientas y sus tamaños. Se investigó los tipos de fusibles.",
        "learned": "Profundicé en la metrología y dimensiones de las herramientas (diferencia entre métrico y estándar/pulgadas) y el uso de acoples según el esfuerzo requerido. Respecto a los fusibles, aprendí su función crucial de protección térmica en los circuitos eléctricos, identificando sus tipos (Mini, Standard, Maxi, fusibles de cartucho/JCase) y el código de colores estandarizado según su amperaje (por ejemplo, 10A rojo, 15A azul, 20A amarillo)."
    },
    {
        "week": "SEMANA 5",
        "title": "Sensores en Motores Diésel y Gasolina",
        "desc": "Investigamos los tipos de sensores de un motor diésel y gasolina.",
        "learned": "Entendí cómo la unidad de control del motor (ECU) monitorea los parámetros del vehículo a través de sensores. Aprendí a diferenciar los sensores de un motor de gasolina (CKP, CMP, MAF, TPS, sonda Lambda) de los sensores específicos en motores diésel (como el sensor de presión de riel común - FRP, sensor de presión de sobrealimentación MAP/Turbo, y sensores de temperatura de gases de escape EGT). Esto me dio una visión clara de cómo la electrónica gestiona la mezcla y la combustión."
    },
    {
        "week": "SEMANA 6",
        "title": "Primera Prueba Piloto del Checklist Digital",
        "desc": "Pusimos en prueba el checklist elaborado y vimos qué faltaba en el checklist.",
        "learned": "Al aplicar la primera versión de nuestro checklist digital directamente en un vehículo en el taller, aprendí que la teoría no siempre encaja a la primera con la práctica. Descubrí inconsistencias, campos ambiguos y pasos que habíamos omitido (como la verificación previa de niveles o estado de luces). Entendí que la retroalimentación en campo es esencial para crear una herramienta realmente útil para un mecánico."
    },
    {
        "week": "SEMANA 7",
        "title": "Segunda Prueba, Reordenamiento y Flujo de Trabajo",
        "desc": "Pusimos en prueba de nuevo el checklist viendo qué faltaba y el orden que debería ir el checklist.",
        "learned": "Aprendí la importancia de la ergonomía y la secuencia lógica en la inspección vehicular. En esta segunda prueba, me di cuenta de que el orden del checklist debe seguir el flujo físico que realiza el técnico alrededor del auto (por ejemplo: inspección exterior -> compartimiento del motor -> elevación/subchasis -> interior) para evitar desplazamientos innecesarios y optimizar el tiempo de revisión."
    },
    {
        "week": "SEMANA 8",
        "title": "Trabajo en Equipo y Adición del Apartado Fotográfico",
        "desc": "Nos reunimos en grupo para ver qué faltaría en el checklist y cómo sería el orden, y discutimos que deberíamos agregar un apartado de imágenes.",
        "learned": "Fortalecí mis habilidades de trabajo en equipo y debate técnico. Llegamos a la conclusión unánime de que una inspección moderna requiere respaldo visual. Aprendí que incorporar un apartado de capturas de imágenes/fotos directas en el checklist digital aporta transparencia hacia el cliente, sirviendo como evidencia técnica irrefutable del estado de las piezas dañadas o desgastadas antes de realizar el mantenimiento."
    },
    {
        "week": "SEMANA 9",
        "title": "Validación Final e Implementación del Checklist",
        "desc": "Pusimos en prueba el checklist ya terminado.",
        "learned": "En la evaluación final con la versión completa del checklist digital, experimenté la satisfacción de aplicar un instrumento intuitivo, rápido y completo. Aprendí a ejecutar un proceso estandarizado de inspección, comprobando que la planificación, la corrección iterativa y la digitalización aumentan notablemente la profesionalidad y precisión del diagnóstico preventivo."
    },
    {
        "week": "SEMANA 10",
        "title": "Diagnóstico de Fusibles y Medición Directa",
        "desc": "Medimos todos los fusibles.",
        "learned": "Aplicamos técnicas de diagnóstico eléctrico directo en las cajas de fusibles del vehículo (tanto la del compartimiento del motor como la del habitáculo). Aprendí a medir la continuidad y la caída de voltaje en las puntas de prueba de cada fusible con el multímetro sin necesidad de extraerlos uno a uno, lo que permite identificar rápidamente fusibles quemados o abiertos sin dañar los terminales de la fusilera."
    }
]

for w in weeks_data:
    add_custom_heading(doc, f"{w['week']}: {w['title']}", level=2)
    
    # Overview Box
    tbl_w = doc.add_table(rows=2, cols=1)
    tbl_w.alignment = WD_TABLE_ALIGNMENT.CENTER
    
    # Cell 1: Resume / Actividad
    c1 = tbl_w.rows[0].cells[0]
    set_cell_background(c1, "F9FAFC")
    set_cell_margins(c1, top=100, bottom=100, left=150, right=150)
    p_act = c1.paragraphs[0]
    r_act_title = p_act.add_run("Resumen de Actividades:\n")
    r_act_title.bold = True
    r_act_title.font.name = 'Calibri'
    r_act_title.font.color.rgb = RGBColor(31, 78, 121)
    r_act_text = p_act.add_run(w['desc'])
    r_act_text.font.name = 'Calibri'
    r_act_text.font.size = Pt(10.5)
    
    # Cell 2: Lo que aprendí (Expresión personal)
    c2 = tbl_w.rows[1].cells[0]
    set_cell_background(c2, "FFFFFF")
    set_cell_margins(c2, top=100, bottom=100, left=150, right=150)
    p_lrn = c2.paragraphs[0]
    r_lrn_title = p_lrn.add_run("Lo que aprendí y entendí personal y técnicamente:\n")
    r_lrn_title.bold = True
    r_lrn_title.font.name = 'Calibri'
    r_lrn_title.font.italic = True
    r_lrn_title.font.color.rgb = RGBColor(46, 117, 182)
    r_lrn_text = p_lrn.add_run(w['learned'])
    r_lrn_text.font.name = 'Calibri'
    r_lrn_text.font.size = Pt(10.5)
    
    doc.add_paragraph().paragraph_format.space_after = Pt(8)

doc.add_page_break()

# --- CONCLUSIÓN ---
add_custom_heading(doc, "3. CONCLUSIÓN GENERAL", level=1)

p_conc = doc.add_paragraph()
p_conc.paragraph_format.line_spacing = 1.15
p_conc.paragraph_format.space_after = Pt(12)
r_conc = p_conc.add_run(
    "Al finalizar estas 10 semanas de trabajo práctico y conceptual, puedo concluir que mi formación técnica ha dado "
    "un salto cualitativo trascendental. La combinación de prácticas mecánicas directas (como el desarme de motor, "
    "la identificación de herramientas y el diagnóstico eléctrico de sensores y fusibles) junto con el desarrollo "
    "del Proyecto ABR, me ha brindado una visión integral de lo que exige la industria automotriz moderna.\n\n"
    "El proceso de creación del checklist digital demostró que el verdadero valor de la tecnología radica en solucionar "
    "necesidades reales del taller. A través del ensayo, el error y el reordenamiento continuo, logramos transformar una simple "
    "hoja de inspección de papel en una herramienta digital optimizada, estructurada lógicamente y enriquecida con evidencia fotográfica.\n\n"
    "En lo personal, esta experiencia fortaleció mi capacidad analítica, mi disciplina para el diagnóstico eléctrico y "
    "mantenimiento preventivo, y mi habilidad para trabajar en equipo compartiendo ideas y soluciones. Me siento totalmente "
    "preparado y motivado para seguir aplicando estos conocimientos en el campo profesional automotriz."
)
r_conc.font.name = 'Calibri'
r_conc.font.size = Pt(11)

# Summary Box at the end
tbl_end = doc.add_table(rows=1, cols=1)
tbl_end.alignment = WD_TABLE_ALIGNMENT.CENTER
c_end = tbl_end.rows[0].cells[0]
set_cell_background(c_end, "EBF1F5")
set_cell_margins(c_end, top=120, bottom=120, left=180, right=180)

p_end = c_end.paragraphs[0]
r_end_title = p_end.add_run("Puntos Clave del Aprendizaje:\n")
r_end_title.bold = True
r_end_title.font.name = 'Calibri'
r_end_title.font.color.rgb = RGBColor(31, 78, 121)

r_end_text = p_end.add_run(
    "1. Dominio de herramientas manuales y equipos de medición eléctrica (voltímetro/multímetro).\n"
    "2. Comprensión del funcionamiento de sensores (gasolina/diésel) y protección de circuitos (fusibles).\n"
    "3. Desarrollo e implementación exitosa de un Checklist Digital estandarizado con registro fotográfico.\n"
    "4. Trabajo colaborativo eficaz orientado a la resolución de problemas (ABR)."
)
r_end_text.font.name = 'Calibri'
r_end_text.font.size = Pt(10.5)

output_path = "Bitacora_Mantenimiento_Preventivo_Augusto.docx"
doc.save(output_path)
print(f"Document saved to {output_path}")
