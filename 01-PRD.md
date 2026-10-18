# PRD · Documento de Requisitos del Producto

**Producto:** Comunicados Académicos
**Autor:** MSc. Carlos Reinoso
**Versión:** 1.1 · 17 de octubre de 2026
**Estado:** Versión inicial funcional (aplicación web de una sola página)

---

## 1. Resumen

Comunicados Académicos es una aplicación web que permite a un docente generar en minutos un comunicado formal dirigido al representante legal, con el estado de cumplimiento de una actividad por estudiante, y descargarlo como PDF listo para enviar.

Hoy el docente redacta este documento a mano en un procesador de texto: repite encabezados, corrige redacción, copia nombres y arma tres listas. La aplicación automatiza el encabezado, la fecha y la hora, ayuda con la redacción y convierte la clasificación de estudiantes en tres botones.

## 2. Problema

| Dolor actual | Consecuencia |
|---|---|
| Se vuelve a escribir el mismo encabezado y los mismos datos en cada comunicado | Pérdida de tiempo y errores de copia |
| Los textos se redactan con prisa | Faltas de tildes, puntuación y redacción en un documento oficial |
| Los nombres se copian y pegan entre listas a mano | Estudiantes omitidos o repetidos |
| El formato cambia de un comunicado a otro | Imagen institucional inconsistente |

## 3. Objetivos

1. Reducir el tiempo de elaboración de un comunicado a pocos minutos.
2. Garantizar un formato institucional uniforme.
3. Detectar errores de ortografía, puntuación y redacción antes de enviar.
4. Permitir clasificar estudiantes con un solo toque por persona.
5. Entregar un PDF final sin pasos adicionales.

**No es objetivo de la versión 1.0:** enviar el comunicado por correo o mensajería, guardar historial en un servidor, ni gestionar cuentas de usuario.

## 4. Usuarios

| Usuario | Descripción | Necesidad principal |
|---|---|---|
| Docente de EGB o BGU (principal) | Informa el desempeño a representantes legales de uno o varios cursos | Rapidez y facilidad de uso |
| Docente tutor | Consolida novedades de su curso | Reutilizar datos de curso y docente |
| Autoridad institucional (futuro) | Revisa y aprueba comunicados | Trazabilidad |

**Contexto:** Unidad Educativa "Otto Sharnow", Ecuador. Currículo Nacional (EGB y BGU). Idioma de la interfaz: español.

## 5. Alcance de la versión 1.0

### Incluido

- Formulario de datos del comunicado con listas desplegables.
- Fecha y hora automáticas con la zona horaria de Ecuador (America/Guayaquil).
- Revisión de ortografía, puntuación y redacción en los campos de texto.
- Texto complementario con frases rápidas y una línea indicada por el docente.
- Carga de lista de estudiantes, clasificación en tres casillas y botón para eliminar estudiantes de la lista.
- Vista previa en vivo del documento.
- Descarga en PDF tamaño A4.
- Tabla de seguimiento: a la derecha de cada estudiante, espacios para llenar a mano en la atención a padres (asistencia del representante legal con fecha y hora, acuerdos o compromisos, y firma del representante legal).
- Recuerdo local de datos del docente y de la unidad educativa.

### Fuera de alcance

- Cuentas de usuario y sincronización entre dispositivos.
- Envío directo por correo, WhatsApp u otro canal.
- Historial de comunicados y reportes estadísticos.
- Registro digital del resultado de la atención a padres (se completa a mano en el papel).
- Importación de listas desde archivos (Excel, CSV).

## 6. Requisitos funcionales

| ID | Requisito | Prioridad |
|---|---|---|
| RF-01 | El usuario puede editar el nombre de la unidad educativa, que se muestra en el encabezado en mayúsculas. | Alta |
| RF-02 | La fecha y la hora se completan automáticamente con la hora de Ecuador y se actualizan mientras el modo automático esté activo. El usuario puede desactivarlo y editarlas. | Alta |
| RF-03 | El campo Grado/Curso es una lista desplegable con 1.° a 10.° de EGB y 1.° a 3.° de BGU, agrupada por subnivel. | Alta |
| RF-04 | El campo Materia es una lista desplegable agrupada (EGB y BGU, EGB, Preparatoria, BGU) con la opción «Otra materia…» que habilita un campo de texto. | Alta |
| RF-05 | El campo Docente es de texto libre y se recuerda para la próxima sesión. | Alta |
| RF-06 | El campo M.T.A. ofrece Fase 1: Autoaprendizaje, Fase 2: Coaprendizaje y Fase 3: Socialización. Es opcional. | Alta |
| RF-07 | El campo Tema es de texto libre y obligatorio para generar el PDF. | Alta |
| RF-08 | Los campos Tema, Introducción y Texto complementario muestran observaciones de tildes, ortografía, puntuación, mayúsculas, palabras repetidas y redacción, con botón «Corregir» y «Corregir todo». | Alta |
| RF-09 | La introducción se genera automáticamente y se adapta a las casillas que tengan estudiantes (incumplimientos, entregas parciales, inasistencias). Si el usuario la edita, deja de actualizarse y puede restablecerse. | Media |
| RF-10 | El texto complementario ofrece frases rápidas: invitar a atención a padres (día y hora configurables), conversar posibles soluciones y pedir justificación de inasistencias. | Media |
| RF-11 | El usuario puede escribir una línea que el texto complementario debe cumplir y pedir a Claude que lo redacte, cuando esa función esté disponible. | Baja |
| RF-12 | El usuario pega una lista de estudiantes. El sistema elimina números y viñetas, descarta duplicados y muestra el total cargado. | Alta |
| RF-13 | Cada estudiante sin clasificar muestra cuatro botones: «No realizó», «Incompleta», «No asistió» y «Eliminar». Los tres primeros pasan el nombre a la casilla correspondiente; «Eliminar» lo quita de la lista. | Alta |
| RF-14 | Cada nombre en una casilla tiene un botón «Quitar» que lo devuelve a «Sin novedad». Quien queda sin clasificar no aparece en el comunicado. | Alta |
| RF-15 | La vista previa se actualiza en vivo con cada cambio. | Alta |
| RF-16 | El botón «Descargar PDF» valida los campos obligatorios, genera un PDF A4 y pide confirmación para guardarlo. | Alta |
| RF-17 | El usuario puede mostrar los nombres en mayúsculas o tal como los escribió, y agregar las líneas de firma del docente y de una autoridad institucional, una junto a la otra. | Media |
| RF-18 | El estado del formulario se conserva entre sesiones en el mismo navegador. | Media |
| RF-19 | «Nuevo comunicado» limpia tema, fase, estudiantes e instrucción, y conserva unidad educativa, docente, curso y materia. Requiere una segunda pulsación para confirmar. | Media |
| RF-20 | Una casilla sin estudiantes aparece en el comunicado con el texto «Ninguno registrado.» | Alta |
| RF-21 | «Eliminar» quita al estudiante de la lista y de todas las casillas, muestra un aviso con su nombre y mueve el foco al siguiente estudiante. Para recuperarlo se vuelve a pegar su nombre. | Alta |
| RF-22 | Al activar las firmas aparece una lista para elegir el cargo de quien firma (Rector/a, Vicerrector/a, Director/a, Subdirector/a, Inspector/a General, Subinspector/a, Coordinador/a) y un casillero para su nombre. Se imprime bajo su línea de firma, junto a la del docente. Si el nombre queda vacío, la línea muestra solo el cargo para firmar a mano. | Alta |
| RF-23 | Cada categoría con estudiantes se presenta como una tabla con las columnas: Estudiante, Asistencia del representante legal (fecha y hora), Acuerdos / Compromisos y Firma del representante legal. Las tres últimas quedan en blanco para llenarlas a mano en el documento impreso. La opción puede desactivarse y entonces se muestra la lista simple de nombres. | Alta |

## 7. Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | **Facilidad de uso:** un docente nuevo completa un comunicado sin instrucciones. |
| RNF-02 | **Responsive:** funciona en teléfono (400 px) y escritorio; el botón de PDF siempre es accesible. |
| RNF-03 | **Accesibilidad:** foco visible, etiquetas en todos los campos, contraste suficiente en tema claro y oscuro, navegación por teclado. |
| RNF-04 | **Privacidad:** los nombres de estudiantes permanecen en el navegador del usuario. Solo se envía texto a Claude cuando el usuario pulsa un botón de Claude. |
| RNF-05 | **Rendimiento:** la vista previa responde de inmediato con listas de hasta 60 estudiantes. |
| RNF-06 | **Compatibilidad:** navegadores actuales (Chrome, Edge, Firefox, Safari 16.4 o superior). |
| RNF-07 | **Idioma:** toda la interfaz y el documento en español de Ecuador. |

## 8. Historias de usuario y criterios de aceptación

**HU-1. Como docente, quiero que la fecha y la hora se llenen solas para no escribirlas.**
- Dado que el modo automático está activo, cuando abro la aplicación, entonces veo la fecha y hora actuales de Ecuador.
- Cuando desactivo el modo automático, entonces puedo editar ambos campos.

**HU-2. Como docente, quiero ver mis errores de redacción antes de enviar.**
- Cuando escribo «Lección:Los niveles», entonces la aplicación indica que falta un espacio después de los dos puntos y ofrece corregirlo con un botón.
- Cuando el texto no tiene observaciones, entonces veo un mensaje de confirmación.

**HU-3. Como docente, quiero pegar la lista del curso y clasificarla con botones.**
- Cuando pego una lista numerada, entonces se cargan los nombres sin números.
- Cuando pulso «No realizó» en un estudiante, entonces el nombre aparece en la casilla 1 y sale de «Sin novedad».
- Cuando pulso «Eliminar», entonces el estudiante desaparece de la lista y no aparece en el comunicado.

**HU-4. Como docente, quiero descargar un PDF con el formato institucional.**
- Cuando faltan grado, materia, docente o tema, entonces la aplicación lo indica y lleva el foco al campo.
- Cuando todo está completo, entonces el PDF tiene encabezado, datos, textos, reporte de cumplimiento y, si se pidió, firma.

**HU-5. Como docente, quiero que el texto cumpla una línea concreta.**
- Cuando escribo «invitar a reunirse para conversar posibles soluciones» y pulso «Redactar con Claude», entonces recibo una versión sugerida que puedo aceptar o descartar.

**HU-6. Como docente, quiero que el comunicado lleve también la firma de una autoridad de la institución.**
- Cuando activo las firmas, entonces puedo elegir el cargo en una lista (Rector/a, Vicerrector/a, Director/a, Subdirector/a, Inspector/a General, Subinspector/a, Coordinador/a) y escribir el nombre.
- Cuando descargo el PDF, entonces la firma del docente y la de la autoridad salen lado a lado, cada una con su línea, nombre y cargo.

**HU-7. Como docente, quiero que el representante que asiste a atención a padres pueda dejar constancia en el mismo papel.**
- Cuando descargo el PDF, entonces a la derecha de cada nombre hay espacios en blanco para anotar la fecha y hora de asistencia, los acuerdos o compromisos y la firma del representante legal.
- Cuando desactivo «Espacios para llenar a mano», entonces el reporte muestra solo la lista de nombres.

## 9. Estructura del comunicado generado

1. Encabezado: UNIDAD EDUCATIVA "NOMBRE" · NOTIFICACIÓN AL REPRESENTANTE LEGAL · COMUNICADO DE ACTIVIDADES ACADÉMICAS.
2. Datos: Fecha, Hora, Grado/Curso, Materia, Docente, Tema y M.T.A. (si se indicó).
3. Párrafo de introducción.
4. Párrafo complementario.
5. REPORTE DE CUMPLIMIENTO con tres listas (cada estudiante en una tabla con espacios para completar a mano: asistencia del representante legal con fecha y hora, acuerdos o compromisos, y firma del representante legal):
   - Estudiantes que no realizaron la actividad.
   - Estudiantes que entregaron la actividad de forma incompleta.
   - Estudiantes que no asistieron a clase.
6. Líneas de firma del docente y de una autoridad (con su cargo), lado a lado (opcional), y numeración de páginas si el documento tiene más de una.

## 10. Métricas de éxito

| Métrica | Meta |
|---|---|
| Tiempo para generar un comunicado de un curso de 35 estudiantes | Menos de 5 minutos |
| Comunicados con observaciones de redacción sin corregir al descargar | Menos del 10 % |
| Docentes que completan el flujo sin ayuda | 90 % |

## 11. Riesgos y supuestos

| Riesgo | Mitigación |
|---|---|
| Datos de menores de edad en el navegador | No hay servidor propio; los datos quedan en el equipo. Documentar la política de uso. |
| El revisor local no detecta todos los errores | Mostrarlo como ayuda; ofrecer revisión con Claude cuando esté disponible. |
| Cambios del currículo (materias, fases) | Catálogos aislados en una sola sección del código para actualizarlos con facilidad. |
| Dependencia de un CDN para generar PDF | Evaluar incluir la librería dentro del repositorio. |

## 12. Hoja de ruta sugerida

| Versión | Contenido |
|---|---|
| 1.1 (actual) | Funcionalidad descrita en este documento, con descarga de PDF fuera de Claude, botón «Eliminar», firma de la autoridad con cargo elegible y tabla de seguimiento para atención a padres |
| 1.2 | Importación de listas desde CSV y Excel |
| 1.3 | Historial local de comunicados; plantillas de texto propias |
| 2.0 | Cuentas, base de datos y envío por correo (ver documento de esquema de backend) |
