# Diseño de Interfaz y Experiencia de Usuario (UI/UX)

**Producto:** Comunicados Académicos
**Versión:** 1.1 · 17 de octubre de 2026

---

## 1. Principios de diseño

1. **Un solo camino, de arriba hacia abajo.** Tres pasos numerados que reflejan el orden real de trabajo: datos, texto y estudiantes.
2. **Lo automático ya está hecho.** Fecha, hora, introducción y datos del docente llegan completos; el usuario solo corrige.
3. **Ver el resultado mientras se escribe.** La hoja del comunicado se actualiza en vivo junto al formulario.
4. **Un toque por estudiante.** Clasificar no requiere escribir, arrastrar ni abrir menús.
5. **Errores que se corrigen con un botón.** Cada observación de redacción muestra el fragmento marcado y su corrección.
6. **Sobrio y legible.** Es una herramienta de trabajo institucional: poco adorno, buen contraste, tipografía clara.

## 2. Estructura de pantalla

### Escritorio (960 px o más)

```
┌──────────────────────────────────────────────────────────────┐
│ Comunicados académicos                       [Nuevo comunicado] │
├───────────────────────────────┬──────────────────────────────┤
│ 1 Datos del comunicado        │ Vista previa   [Descargar PDF]│
│   Unidad educativa            │ ┌──────────────────────────┐ │
│   Fecha y hora  (automática)  │ │ UNIDAD EDUCATIVA "…"     │ │
│   Grado/Curso | Materia       │ │ NOTIFICACIÓN …           │ │
│   Docente     | M.T.A.        │ │ • Fecha: …               │ │
│   Tema + revisión             │ │ • Hora: …                │ │
│ 2 Texto del comunicado        │ │ Texto…                   │ │
│   Introducción + revisión     │ │ REPORTE DE CUMPLIMIENTO  │ │
│   Texto complementario        │ │ …                        │ │
│ 3 Estudiantes                 │ └──────────────────────────┘ │
│   Pegar lista · clasificar    │   (columna fija al desplazar)│
│ Opciones · [Descargar PDF]    │                              │
└───────────────────────────────┴──────────────────────────────┘
```

### Teléfono (menos de 960 px)

- Una sola columna: formulario primero y vista previa debajo.
- Barra inferior fija con el botón **Descargar PDF**, siempre visible.
- Márgenes laterales de 16 px o más; respeta las zonas seguras del teléfono.

## 3. Sistema visual

### Color

| Token | Claro | Oscuro | Uso |
|---|---|---|---|
| `--bg` | `#EDF0F3` | `#0E1418` | Fondo de página |
| `--surface` | `#FFFFFF` | `#162026` | Paneles y campos |
| `--surface-2` | `#F4F6F8` | `#1C2830` | Fondos secundarios |
| `--ink` | `#14202B` | `#E7EDF1` | Texto principal |
| `--ink-2` | `#4B5A67` | `#9DAEBB` | Texto secundario |
| `--line` | `#D3DAE0` | `#2B3943` | Bordes |
| `--accent` | `#0F5B66` | `#5CC3CF` | Acción principal, paso activo, foco |
| `--c1` (no realizó) | `#B3261E` | `#FF8A80` | Casilla 1 |
| `--c2` (incompleta) | `#8E5400` | `#F2B35C` | Casilla 2 y observaciones de redacción |
| `--c3` (no asistió) | `#365A8C` | `#8FB1E6` | Casilla 3 |
| `--ok` | `#1E7A46` | `#6FD39A` | Texto sin observaciones |

La hoja del comunicado siempre es blanca con texto oscuro, igual que el papel, en ambos temas.

Las tres casillas usan colores distintos **y** números (1, 2, 3), de modo que no dependen solo del color.

### Tipografía

| Rol | Fuente | Uso |
|---|---|---|
| Interfaz | Public Sans | Campos, botones, etiquetas |
| Títulos | Bricolage Grotesque | Título de la página y de los pasos |
| Documento | Arimo | Hoja de vista previa (equivalente a Arial/Helvetica, como el PDF) |

Tamaño base 15 px, etiquetas de 13 px en negrita, líneas de texto de hasta unos 65 caracteres.

### Espaciado y forma

- Paneles con borde de 1 px y radio de 12 px; campos con radio de 8 px y altura mínima de 42 px.
- Espacios entre grupos de 14 a 24 px, resueltos con rejillas (`grid` y `flex`).
- Sombras solo en la hoja de vista previa.

## 4. Componentes

| Componente | Comportamiento |
|---|---|
| **Campo de texto / área de texto** | Etiqueta arriba, foco con contorno de 2 px en el color de acento. |
| **Lista desplegable** | Grupos con título (por ejemplo «EGB · Superior», «BGU · Bachillerato»). |
| **Interruptor «Automática»** | Activado: fecha y hora deshabilitadas y sincronizadas. Desactivado: editables. |
| **Panel de observaciones** | Aparece bajo el campo. Con errores: cantidad, botón «Corregir todo» y una fila por error con etiqueta (Ortografía, Puntuación, Mayúscula, Redacción), mensaje, fragmento marcado y botón «Corregir». Sin errores: franja verde «Sin observaciones…». |
| **Tarjeta de sugerencia de Claude** | Borde de acento; botones «Usar esta versión» y «Descartar». Solo se muestra si la función está disponible. |
| **Frases rápidas (chips)** | Botones tipo píldora que insertan o quitan una frase; el estado activo se ve con color de acento. |
| **Fila de estudiante** | Nombre y cuatro botones: «1 No realizó», «2 Incompleta», «3 No asistió», cada uno con su color, y «× Eliminar», en gris, que se vuelve rojo al pasar el cursor. |
| **Interruptor «Espacios para llenar a mano»** | Casilla activada por defecto en Opciones. Cambia el reporte de lista de nombres a tabla con columnas en blanco. |
| **Bloque de la autoridad que firma** | Aparece al activar «Incluir firmas…». Lista desplegable «Cargo de quien firma» (Rector/a, Vicerrector/a, Director/a, Subdirector/a, Inspector/a General, Subinspector/a, Coordinador/a) y casillero «Nombre de la autoridad». Se imprime junto a la firma del docente. |
| **Casilla de clasificación** | Borde izquierdo de color, título, contador y lista de nombres con botón «Quitar». |
| **Barra inferior móvil** | Botón «Descargar PDF» de ancho completo. |
| **Aviso (toast)** | Mensaje breve centrado, desaparece solo, anunciado a lectores de pantalla. |

## 5. Estados de la interfaz

| Situación | Qué ve el usuario |
|---|---|
| Primera vez | Datos de unidad educativa y docente precargados; introducción y texto complementario ya escritos; lista de estudiantes vacía con el botón «Probar con lista de ejemplo». |
| Lista cargada | Bloque «Sin novedad» con todos los nombres y tres casillas vacías que dicen «Ninguno registrado.» |
| Estudiante eliminado | Desaparece de la lista, aparece el aviso «NOMBRE eliminado de la lista.» y el foco pasa al siguiente estudiante. |
| Todos clasificados | «Todos los estudiantes están clasificados.» |
| Campo obligatorio vacío | Al descargar, el campo se resalta en rojo, recibe el foco y aparece un aviso con el motivo. |
| Acción destructiva | «Vaciar estudiantes» y «Nuevo comunicado» cambian su texto a «¿Seguro? Pulsa otra vez» durante 4 segundos. No se usan ventanas emergentes. |
| Revisión con Claude en curso | El botón muestra «Revisando…» y queda deshabilitado. |
| Función no disponible | Los botones de Claude no se muestran. |
| Error al generar o guardar | Aviso con el motivo y la acción sugerida. |

## 6. Textos de la interfaz

Reglas: español de Ecuador, verbos que dicen exactamente qué ocurre, errores con la solución.

| Elemento | Texto |
|---|---|
| Acción principal | Descargar PDF |
| Reinicio | Nuevo comunicado |
| Carga | Cargar lista · Vaciar estudiantes · Probar con lista de ejemplo |
| Casillas | No realizó · Incompleta · No asistió |
| Falta tema | «Escribe el tema del comunicado.» |
| Sin errores | «Sin observaciones de ortografía, puntuación ni redacción» |
| Vacío de estudiantes | «Cada estudiante aparecerá con tres botones para enviarlo a su casilla. Los que queden sin clasificar no aparecen en el comunicado.» |

## 7. Accesibilidad

- Todos los campos tienen etiqueta asociada; los botones de clasificación tienen nombre accesible con el nombre del estudiante.
- Foco visible en cada elemento interactivo; tras clasificar, el foco pasa al siguiente estudiante.
- Los avisos usan `role="status"` y `aria-live="polite"`.
- Contraste de texto verificado en tema claro y oscuro; el estado no se comunica solo con color.
- Objetivos táctiles de 34 px o más en botones de clasificación y 40 a 48 px en acciones principales.
- Respeta `prefers-reduced-motion` y `prefers-color-scheme`.

## 8. Puntos de quiebre

| Ancho | Cambios |
|---|---|
| 960 px o más | Dos columnas; vista previa fija; sin barra inferior. |
| Menos de 960 px | Una columna; barra inferior con PDF; botón de PDF de la vista previa oculto. |
| 560 px o menos | Campos en una columna; los cuatro botones de cada estudiante en dos filas de dos bajo el nombre; hoja con menos relleno. |

## 9. Documento PDF

Fuente Helvetica, A4, márgenes de 20 mm, texto justificado de 10,5 pt.
Jerarquía: título 14 pt en negrita; subtítulos 11,5 pt en negrita; datos con viñetas; encabezado «REPORTE DE CUMPLIMIENTO» con línea; tres listas con título en negrita.
La vista previa en pantalla replica esta jerarquía para que lo que se ve sea lo que se descarga.
Cuando las firmas están activas, el docente (izquierda) y la autoridad (derecha) llevan cada uno su línea, el nombre en negrita y el cargo debajo.
En el reporte de cumplimiento, cada categoría con estudiantes es una tabla: Estudiante (48 mm), Asistencia del representante legal (fecha y hora) (36 mm), Acuerdos / Compromisos (46 mm) y Firma del representante legal (40 mm). Las filas miden al menos 15 mm para escribir a mano y el encabezado tiene fondo gris claro.
