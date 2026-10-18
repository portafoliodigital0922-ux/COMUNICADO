# Comunicados Académicos

Aplicación web para que un docente genere en minutos un **comunicado formal dirigido al representante legal** con el estado de cumplimiento de una actividad por estudiante, y lo descargue como **PDF** listo para enviar.

Pensada para la educación general básica (EGB) y el bachillerato general unificado (BGU) del Ecuador.

**Página publicada:** `https://TU-USUARIO.github.io/comunicados-academicos/` (cambia `TU-USUARIO` por tu usuario de GitHub).

## Qué hace

- **Datos con listas desplegables:** grado/curso (1.° a 10.° de EGB y 1.° a 3.° de BGU), materia, fase del método M.T.A. (Autoaprendizaje, Coaprendizaje, Socialización).
- **Fecha y hora automáticas** con la hora de Ecuador, con opción de editarlas.
- **Revisión de redacción:** señala faltas de tildes, puntuación, mayúsculas, palabras repetidas y problemas de redacción, y las corrige con un botón.
- **Texto complementario** con frases rápidas (por ejemplo, invitar a la atención a padres) y una línea que el texto debe cumplir.
- **Clasificación de estudiantes:** pegas la lista y cada nombre pasa con un botón a una de tres casillas: no realizó la actividad, entregó incompleta, no asistió. Un cuarto botón, **Eliminar**, quita al estudiante de la lista.
- **Vista previa en vivo** del documento y **descarga en PDF** tamaño A4.
- **Tabla para atención a padres:** junto a cada estudiante del reporte, deja espacios para completar a mano: asistencia del representante legal (fecha y hora), acuerdos o compromisos y firma del representante legal. Se puede desactivar.
- **Firmas:** opcionalmente imprime, lado a lado, la firma del docente y la de una autoridad (rector/a, vicerrector/a, director/a, subdirector/a, inspector/a general, subinspector/a o coordinador/a), con su nombre.
- Funciona en teléfono y en computador, con tema claro y oscuro.

## Cómo usarla

1. Abre `index.html` en el navegador (o la página publicada).
2. Completa los datos del comunicado y revisa el texto.
3. Pega la lista del curso y clasifica a los estudiantes.
4. Pulsa **Descargar PDF**.

No necesita instalación ni servidor.

## Publicarla con GitHub Pages

1. Sube estos archivos a un repositorio.
2. En **Settings → Pages**, elige la rama `main` y la carpeta `/ (root)`.
3. Espera un minuto: GitHub te mostrará la dirección pública.

## Privacidad

- No hay servidor: los datos se guardan solo en el navegador del usuario (`localStorage`).
- Los nombres de estudiantes no se envían a ningún sitio.
- **No subas a este repositorio listas ni comunicados con nombres reales de estudiantes.** El archivo `.gitignore` bloquea PDF, Excel y CSV por precaución.
- Los botones «Revisar con Claude» y «Redactar con Claude» solo aparecen cuando la página se abre dentro de Claude; fuera de allí quedan ocultos y el resto funciona igual.

## Tecnologías

HTML, CSS y JavaScript sin frameworks. PDF con [jsPDF](https://github.com/parallax/jsPDF) 2.5.1 (cargado desde cdnjs). Tipografías de Google Fonts. Requiere conexión a internet para esos dos recursos.

## Documentación

| Documento | Contenido |
|---|---|
| [PRD](docs/01-PRD.md) | Requisitos del producto |
| [TRD](docs/02-TRD.md) | Requisitos técnicos |
| [UI/UX](docs/03-UI-UX-Design.md) | Diseño de interfaz y experiencia |
| [App Flow](docs/04-App-Flow.md) | Flujos de la aplicación |
| [Backend Schema](docs/05-Backend-Schema.md) | Esquema de datos actual y propuesta 2.0 |

## Autor

MSc. Carlos Reinoso

## Licencia

[MIT](LICENSE)
