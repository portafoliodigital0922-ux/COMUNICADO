# Flujo de la Aplicación (App Flow)

**Producto:** Comunicados Académicos
**Versión:** 1.1 · 17 de octubre de 2026

Los diagramas usan Mermaid, que GitHub muestra automáticamente.

---

## 1. Flujo principal del usuario

```mermaid
flowchart TD
    A([Abrir la aplicación]) --> B[Se restauran datos guardados<br/>o se cargan valores por defecto]
    B --> C[Paso 1: Datos del comunicado]
    C --> C1[Unidad educativa, curso, materia,<br/>docente, M.T.A. y tema]
    C1 --> D[Paso 2: Texto]
    D --> D1[Revisar introducción]
    D1 --> D2[Elegir frases rápidas<br/>o escribir una línea a cumplir]
    D2 --> E[Paso 3: Estudiantes]
    E --> E1[Pegar lista del curso]
    E1 --> E2[Clasificar con los botones 1, 2 y 3<br/>o eliminar de la lista]
    E2 --> F{¿Listo para descargar?}
    F -- Sí --> G[Pulsar Descargar PDF]
    F -- Falta algo --> C
    G --> H{¿Campos obligatorios completos?}
    H -- No --> I[Resaltar campo, mostrar aviso<br/>y llevar el foco]
    I --> C
    H -- Sí --> J[Generar PDF A4]
    J --> K[Confirmar guardado]
    K --> L([PDF guardado])
```

Los pasos 1, 2 y 3 pueden hacerse en cualquier orden; la vista previa se actualiza con cada cambio.

## 2. Estados de un estudiante

```mermaid
stateDiagram-v2
    [*] --> SinNovedad: Se carga la lista
    SinNovedad --> NoRealizo: Botón 1
    SinNovedad --> Incompleta: Botón 2
    SinNovedad --> NoAsistio: Botón 3
    SinNovedad --> Eliminado: Botón 4 Eliminar
    Eliminado --> [*]: Sale de la lista
    NoRealizo --> SinNovedad: Quitar
    Incompleta --> SinNovedad: Quitar
    NoAsistio --> SinNovedad: Quitar
    SinNovedad --> [*]: No aparece en el comunicado
    NoRealizo --> [*]: Aparece en la lista 1
    Incompleta --> [*]: Aparece en la lista 2
    NoAsistio --> [*]: Aparece en la lista 3
```

Cada cambio de estado actualiza, en este orden: las casillas de la pantalla, la introducción automática (si no fue editada), la vista previa, la revisión de redacción y el guardado local.

## 3. Carga de la lista de estudiantes

```mermaid
flowchart TD
    A[Pegar texto en el cuadro de lista<br/>o pulsar Cargar lista] --> B[Dividir por líneas, punto y coma y tabulaciones]
    B --> C[Quitar números iniciales y viñetas]
    C --> D[Normalizar espacios y descartar líneas vacías o numéricas]
    D --> E{¿Hay nombres?}
    E -- No --> F[Aviso: pega un estudiante por línea]
    E -- Sí --> G[Descartar duplicados<br/>sin distinguir mayúsculas]
    G --> H[Agregar a Sin novedad]
    H --> I[Aviso: N estudiantes cargados<br/>y repetidos omitidos]
```

## 4. Revisión de redacción

```mermaid
flowchart TD
    A[El usuario escribe en Tema,<br/>Introducción o Texto complementario] --> B[Esperar 300 ms sin escribir]
    B --> C{¿Campo vacío?}
    C -- Sí --> D[Ocultar panel]
    C -- No --> E[Aplicar reglas de revisión]
    E --> F{¿Hay observaciones?}
    F -- No --> G[Mostrar: Sin observaciones]
    F -- Sí --> H[Listar cada observación con fragmento marcado]
    H --> I{Acción del usuario}
    I -- Corregir una --> J[Reemplazar fragmento]
    I -- Corregir todo --> K[Reemplazar todas de atrás hacia adelante]
    I -- Seguir escribiendo --> A
    J --> A
    K --> A
```

## 5. Texto con ayuda de Claude (opcional)

Disponible solo cuando la página se abre dentro de Claude.

```mermaid
sequenceDiagram
    actor U as Docente
    participant P as Página
    participant C as Claude
    U->>P: Escribe (opcional) la línea que debe cumplir el texto
    U->>P: Pulsa Redactar con Claude / Revisar con Claude
    P->>P: Deshabilita el botón y muestra Revisando…
    P->>C: Texto, curso, materia, tema e indicación
    C-->>P: Versión sugerida
    P-->>U: Tarjeta con la sugerencia
    alt Acepta
        U->>P: Usar esta versión
        P->>P: Reemplaza el texto y vuelve a revisar
    else Rechaza
        U->>P: Descartar
    end
```

Si la consulta falla o no hay permiso, se muestra un aviso y el resto de la aplicación sigue funcionando.

## 6. Generación y descarga del PDF

```mermaid
sequenceDiagram
    actor U as Docente
    participant P as Página
    participant J as jsPDF
    participant S as Guardado del navegador
    U->>P: Descargar PDF
    P->>P: Validar unidad, curso, materia, docente y tema
    alt Falta un campo
        P-->>U: Resalta el campo y muestra el motivo
    else Todo completo
        P->>J: Construir documento A4
        J-->>P: Archivo PDF
        P->>S: Ofrecer archivo Comunicado_curso_materia_fecha.pdf
        S-->>U: Confirmación de guardado
        alt Acepta
            P-->>U: PDF guardado
        else Cancela
            P-->>U: Descarga cancelada
        end
    end
```

## 7. Reinicio

```mermaid
flowchart LR
    A[Nuevo comunicado] --> B[Cambia a ¿Borrar tema y estudiantes?<br/>Pulsa otra vez]
    B -->|En 4 s| C[Se borran tema, fase, estudiantes e instrucción<br/>Se restablece el texto complementario]
    B -->|Sin segunda pulsación| D[Vuelve al texto original]
    C --> E[Se conservan unidad educativa, docente,<br/>curso y materia]
```

## 8. Persistencia

| Evento | Qué se guarda |
|---|---|
| Cualquier cambio en el formulario o en las casillas | El estado completo, 350 ms después del último cambio |
| Abrir la página | Se restaura el estado; si no existe, se usan los valores por defecto |
| Modo automático activo | Fecha y hora se vuelven a calcular al abrir y cada 15 segundos |

## 9. Mapa de pantallas

La aplicación tiene una única pantalla con tres secciones y una vista previa:

| Sección | Contenido | Acciones |
|---|---|---|
| Encabezado | Título y «Nuevo comunicado» | Reiniciar |
| 1 · Datos | Unidad, fecha y hora, curso, materia, docente, M.T.A., tema | Elegir, escribir, corregir |
| 2 · Texto | Introducción, frases rápidas, atención a padres, texto complementario, línea a cumplir | Editar, insertar, restablecer, consultar a Claude |
| 3 · Estudiantes | Lista, «Sin novedad» y tres casillas | Cargar, clasificar, quitar, eliminar, vaciar |
| Opciones | Mayúsculas y firmas del docente y de la autoridad (cargo y nombre), y espacios para llenar a mano | Activar o desactivar, escribir el nombre, descargar |
| Vista previa | Hoja del comunicado | Solo lectura; descargar PDF |

## 10. Uso del documento impreso en atención a padres

```mermaid
flowchart LR
    A[El docente descarga el PDF<br/>con espacios para llenar a mano] --> B[Imprime y entrega o envía<br/>el comunicado]
    B --> C[El representante asiste<br/>a atención a padres]
    C --> D[Anota en la fila de su representado:<br/>fecha y hora, acuerdos y firma]
    D --> E[El docente archiva el documento firmado]
```

Esta parte ocurre fuera de la aplicación: el registro queda en el papel.
