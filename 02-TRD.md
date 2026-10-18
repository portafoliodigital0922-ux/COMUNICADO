# TRD · Documento de Requisitos Técnicos

**Producto:** Comunicados Académicos
**Versión:** 1.1 · 17 de octubre de 2026

---

## 1. Visión técnica

Aplicación web de una sola página, sin servidor propio. Todo el código (HTML, CSS y JavaScript) está en un único archivo. El estado vive en el navegador y el PDF se genera en el cliente.

```
┌──────────────────────── Navegador ────────────────────────┐
│  Interfaz (HTML + CSS)                                    │
│     │                                                     │
│  Lógica (JavaScript sin framework)                        │
│     ├─ Estado (objeto `state`)  ⇄  localStorage           │
│     ├─ Revisor de redacción (reglas locales)              │
│     ├─ Vista previa (DOM)                                 │
│     └─ Generador de PDF (jsPDF)                           │
└───────────────────────────────────────────────────────────┘
        │ opcional, solo dentro de Claude
        ▼
   Claude (revisar y redactar texto)
```

## 2. Stack y dependencias

| Componente | Tecnología | Notas |
|---|---|---|
| Lenguajes | HTML5, CSS3, JavaScript (ES2018+) | Sin framework ni paso de compilación |
| PDF | jsPDF 2.5.1 (UMD) desde cdnjs | `https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js` |
| Tipografías | Google Fonts: Public Sans, Bricolage Grotesque, Arimo | Con alternativas del sistema |
| Persistencia | `localStorage` | Clave `comunicados-academicos-v1` |
| Fecha y hora | `Intl.DateTimeFormat` con zona `America/Guayaquil` | Sin librerías |
| IA opcional | Capacidad `sample` de Claude | Solo existe dentro de Claude |

## 3. Estructura del código

El archivo se organiza en secciones:

| Sección | Responsabilidad |
|---|---|
| Catálogos | `GRADOS`, `MATERIAS`, `MTA`, `DIAS`, `CATS` |
| Estado | `DEF`, `state`, `loadState()`, `save()` |
| Fecha y hora | `nowEC()`, `fechaLarga()`, `syncClock()` |
| Textos base | `introBase()`, `FR` (frases rápidas), `unir()` |
| Revisor | `ACCENT`, `MISSPELL`, `FRASES`, `checkText()`, `revisar()` |
| Claude | `claudeRun()` |
| Estudiantes | `parseList()`, `cargar()`, `setCat()`, `renderStudents()` |
| Vista previa | `renderPreview()` |
| PDF | `makePdf()`, `validar()`, `descargar()` |
| Arranque | `init()` y enlaces de eventos |

## 4. Modelo de estado

```json
{
  "inst": "OTTO SHARNOW",
  "auto": true,
  "fecha": "2026-10-15",
  "hora": "15:28",
  "grado": "8.° de EGB",
  "materia": "Ciencias Naturales",
  "materiaOtra": "",
  "docente": "MSc. Carlos Reinoso",
  "mta": "Fase 3: Socialización",
  "tema": "Lección: Los niveles de organización de la materia viva.",
  "intro": "…",
  "introEdited": false,
  "comp": "…",
  "dia": "miércoles",
  "horaAt": "13:30",
  "instr": "",
  "alumnos": [{ "id": 1, "name": "APELLIDO APELLIDO NOMBRE NOMBRE", "cat": 0 }],
  "mayus": true,
  "firma": false,
  "dirNombre": "",
  "dirCargo": "Director/a",
  "tabla": true,
  "nextId": 2
}
```

`cat` toma los valores: `0` sin novedad, `1` no realizó, `2` entregó incompleta, `3` no asistió.

Al cargar, el estado guardado se mezcla con los valores por defecto; se descartan estudiantes con datos inválidos y se recalcula `nextId`.

## 5. Algoritmos clave

### 5.1 Revisor de redacción (`checkText`)

Función pura que recibe un texto y devuelve una lista de observaciones `{s, e, type, msg, rep}` (inicio, fin, tipo, mensaje y reemplazo opcional).

Reglas implementadas:

- Espacios dobles y espacio antes de signos de puntuación.
- Falta de espacio después de coma, punto y coma, dos puntos, punto, «?» y «!».
- Texto o frase que inicia en minúscula (con excepción de abreviaturas comunes como «Msc.», «etc.», «Sr.»).
- Palabras repetidas consecutivas.
- Signos de apertura «¿» y «¡» faltantes o sin cerrar, paréntesis y comillas desbalanceados.
- Tildes faltantes: diccionario de palabras frecuentes y regla general para terminaciones -ción y -sión.
- Errores comunes: «aveces», «enserio», «porfavor», entre otros.
- Frases: «de acuerdo a», «en base a», «darse cuenta que», «debe de + infinitivo», «habían muchos», «pienso de que».
- Punto final faltante en párrafos.
- Oraciones de más de 55 palabras.

Las observaciones que se solapan se descartan, y cada una con reemplazo puede aplicarse individualmente o todas a la vez (de atrás hacia adelante, para no desplazar posiciones).

**Limitación:** es un revisor por reglas, no un corrector gramatical completo. No valida concordancia ni uso correcto de palabras homófonas.

### 5.2 Lectura de la lista de estudiantes (`parseList`)

1. Divide por saltos de línea, punto y coma y tabulaciones.
2. Quita números iniciales («1.», «2)», «3 -») y viñetas.
3. Normaliza espacios y descarta líneas vacías o solo numéricas.
4. En `cargar()` se descartan duplicados sin distinguir mayúsculas.
5. `eliminar(id)` quita al estudiante de `state.alumnos`, actualiza casillas, introducción y vista previa, y mueve el foco al siguiente estudiante.

### 5.3 Introducción automática (`introBase`)

Arma una oración según las casillas con estudiantes y usa la conjunción correcta («y» o «e» antes de palabras que inician con «i»). Mientras `introEdited` sea falso, se recalcula en cada cambio.

### 5.4 Fecha y hora

`Intl.DateTimeFormat('en-CA', { timeZone: 'America/Guayaquil', hourCycle: 'h23' })` entrega los componentes para los campos. La fecha larga se obtiene con `es-EC` a mediodía UTC para evitar desfases por zona horaria. Un temporizador de 15 segundos refresca los valores mientras el modo automático esté activo.

### 5.5 Generación del PDF (`makePdf`)

- jsPDF, unidades en milímetros, A4, márgenes de 20 mm, fuente Helvetica.
- Texto justificado calculado línea por línea (se reparte el espacio entre palabras; la última línea de cada párrafo queda alineada a la izquierda).
- Saltos de página automáticos con control de títulos huérfanos.
- Viñetas dibujadas como círculos para no depender de la codificación de la fuente.
- Se limpian caracteres fuera de Latin-1 (comillas tipográficas, rayas largas, etc.) antes de escribir.
- Tabla de seguimiento: cada categoría con estudiantes se dibuja como tabla de cuatro columnas (48, 36, 46 y 40 mm): estudiante, asistencia del representante legal, acuerdos y firma del representante legal. Las filas miden 15 mm o más, crecen si el nombre ocupa varias líneas, no se parten entre páginas y el encabezado se repite en la página siguiente.
- Firmas: el docente y la autoridad (cargo elegido de una lista de siete) en dos columnas, cada una con su línea, nombre (si se escribió, con ajuste de línea) y cargo alineado en la misma altura.
- Numeración «Página X de N» solo si hay más de una página.
- Nombre de archivo: `Comunicado_<grado>_<materia>_<fecha>.pdf`, sin tildes ni espacios.

## 6. Seguridad y privacidad

| Aspecto | Medida |
|---|---|
| Inyección de código (XSS) | Todo texto del usuario se inserta con `textContent` o nodos de texto, nunca con `innerHTML`. |
| Datos de estudiantes | No se envían a ningún servidor propio. Se guardan en `localStorage` del navegador. |
| Datos enviados a Claude | Solo el texto del campo, el curso, la materia y el tema, y únicamente al pulsar un botón de Claude. No se envían nombres de estudiantes. |
| Recursos externos | Solo cdnjs (jsPDF) y Google Fonts. |
| Equipos compartidos | El estado persiste en el navegador; el botón «Vaciar estudiantes» y «Nuevo comunicado» lo limpian. |

**Recomendación:** al estar en un repositorio público, no incluir nombres reales de estudiantes en pruebas ni en ejemplos. La lista de ejemplo de la aplicación usa nombres inventados.

## 7. Compatibilidad

- Se usan expresiones regulares con aserciones de retroceso (`(?<!…)`) y propiedades Unicode (`\p{L}`): Chrome, Edge y Firefox actuales y Safari 16.4 o superior.
- `Intl.DateTimeFormat` con `hourCycle` y `formatToParts`.
- Diseño verificado a ancho de teléfono (400 px) y escritorio, con tema claro y oscuro.

## 8. Brechas conocidas antes de publicar en GitHub

> **Estado.** La aplicación se construyó dentro de Claude. Para publicarla como página independiente (por ejemplo, en GitHub Pages) ya se resolvió la descarga del PDF (brecha 1). Las otras dos brechas son opcionales.

| # | Brecha | Efecto fuera de Claude | Cambio requerido |
|---|---|---|---|
| 1 ✅ | La descarga usaba la capacidad `downloads` de Claude (`window.claude.use('downloads')`). | Sin Claude, el botón no guardaba el archivo. | **Resuelta en `index.html`:** usa la capacidad de Claude si existe y, si no, la descarga estándar con `URL.createObjectURL(blob)` y un enlace `<a download>`. |
| 2 | Los botones «Revisar/Redactar con Claude» usan la capacidad `sample`. | Permanecen ocultos (funciona bien), pero se pierde esa función. | Opcional: conectar con una API de IA mediante un servidor intermedio que guarde la clave. |
| 3 | jsPDF y las fuentes se cargan desde CDN. | Requiere internet. | Opcional: copiar `jspdf.umd.min.js` al repositorio y cargarlo localmente. |

Estructura sugerida del repositorio:

```
comunicados-academicos/
├── index.html          ← la aplicación
├── README.md
├── LICENSE
└── docs/
    ├── 01-PRD.md
    ├── 02-TRD.md
    ├── 03-UI-UX-Design.md
    ├── 04-App-Flow.md
    └── 05-Backend-Schema.md
```

Despliegue: GitHub Pages desde la rama `main`, carpeta raíz.

## 9. Pruebas recomendadas

| Tipo | Qué probar |
|---|---|
| Unitarias | `checkText` (casos con y sin errores), `parseList` (listas con números, viñetas, tabulaciones, duplicados), `introBase`, `unir` |
| Manuales | Flujo completo con 0, 1 y 40 estudiantes; casilla vacía; texto muy largo en Tema; cambio de tema claro y oscuro |
| PDF | Documento de una y de varias páginas; nombres largos; caracteres con tilde y «ñ»; apertura en Adobe Reader y visores del navegador |
| Responsive | 400 px, 768 px, 1280 px |
| Accesibilidad | Navegación solo con teclado; lector de pantalla en botones de clasificación |

## 10. Mejoras técnicas propuestas

1. ~~Descarga independiente del entorno (brecha 1).~~ Hecho.
2. Importar listas desde CSV y Excel.
3. Separar el código en archivos (`index.html`, `styles.css`, `app.js`) cuando crezca.
4. Pruebas automatizadas con un ejecutor ligero (por ejemplo, Vitest).
5. Historial local de comunicados con `IndexedDB`.
6. Respaldo en servidor y cuentas de usuario (ver `05-Backend-Schema.md`).
