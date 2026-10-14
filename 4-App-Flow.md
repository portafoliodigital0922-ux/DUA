# 4. App Flow — Flujo de la Aplicación

**Producto:** Coloreador DUA 2.2

> Los diagramas usan **Mermaid**, que GitHub renderiza automáticamente en archivos `.md`.

---

## 1. Flujo principal del usuario

```mermaid
flowchart TD
    A([Abrir la herramienta]) --> B[Se carga texto de ejemplo y se colorea]
    B --> C{¿Qué quiere hacer?}
    C -->|Usar su planificación| D[Pulsar Limpiar]
    D --> E[Pegar con Ctrl+V o botón Pegar]
    E --> F[Se sanitiza y se inserta con formato]
    F --> G{¿Sugerir código si falta?}
    G -->|Sí| H[Dejar casilla marcada]
    G -->|No| I[Desmarcar casilla]
    H --> J[Pulsar Colorear]
    I --> J
    J --> K[Revisar texto pintado y Observaciones]
    K --> L{¿Conforme?}
    L -->|No| M[Editar texto o códigos]
    M --> J
    L -->|Deshacer| N[Deshacer / Quitar colores]
    N --> J
    L -->|Sí| O[Copiar todo para Word]
    O --> P([Pegar en Word])
```

## 2. Flujo de «Colorear»

```mermaid
flowchart TD
    A([Clic en Colorear]) --> B{¿Hay texto?}
    B -->|No| B1[Toast: Pega primero el texto] --> Z([Fin])
    B -->|Sí| C[Clonar contenido del editor]
    C --> D[Quitar pintura previa]
    D --> E[Normalizar bloques]
    E --> F[Recorrer cada bloque y cada línea]
    F --> G{¿Línea con paréntesis de códigos?}
    G -->|Sí| H[Código del docente]
    H --> H1{¿Códigos de varios principios?}
    H1 -->|Sí| H2[Color del primer código + cada código en negrita con su color]
    H1 -->|No| H3[Toda la línea del color del principio]
    G -->|No| I[classify]
    I --> J{¿Resultado?}
    J -->|Neutral| K[Registrar en 'No pintado' con motivo]
    J -->|Analizar| L{¿Sugerir activado?}
    L -->|No| K
    L -->|Sí| M[infer]
    M --> N{¿Se encontró código?}
    N -->|No| K
    N -->|Sí| O[Pintar + etiqueta DUA sugerido + registrar razón]
    H2 --> P[Siguiente línea]
    H3 --> P
    K --> P
    O --> P
    P --> Q[Insertar HTML resultante como edición]
    Q --> R[Actualizar estadísticas y tablas]
    R --> S[Toast: Texto coloreado]
    S --> Z
```

## 3. Árbol de decisión de clasificación

```mermaid
flowchart TD
    A[Línea sin código] --> B{¿Es 'Unidad N', 'Tiempo estimado' o destreza CN/LL/M?}
    B -->|Sí| N1[Neutral: estructura]
    B -->|No| C{¿Es encabezado H1–H6?}
    C -->|Sí| N2[Neutral: encabezado]
    C -->|No| D{¿Sin verbo inicial, ≤ 14 palabras y sin signo final?}
    D -->|Sí| N3[Neutral: fase o título]
    D -->|No| E{¿Menos de 4 palabras?}
    E -->|Sí| N4[Neutral: muy breve]
    E -->|No| F[Analizable]
    F --> G{¿Coincide con alguna regla de palabras clave?}
    G -->|Sí| H[Elegir hasta 2 códigos, preferir principios distintos]
    G -->|No| I{¿Verbo inicial reconocido?}
    I -->|Sí| J[Código por verbo]
    I -->|No| K[Sin pintar: no interpretable]
```

## 4. Flujo de pegado

```mermaid
flowchart TD
    A([Ctrl+V o botón Pegar]) --> B{¿Origen?}
    B -->|Ctrl+V| C[Evento paste: leer text/html y text/plain]
    B -->|Botón| D[navigator.clipboard.read]
    D -->|Permiso denegado| D1[Toast: usa Ctrl+V] --> Z([Fin])
    D -->|OK| C
    C --> E{¿Hay HTML?}
    E -->|Sí| F[sanitize: lista blanca, limpiar estilos mso]
    E -->|No| G[textToHtml: cada línea en un párrafo]
    F --> H[Insertar con insertHTML]
    G --> H
    H --> I[Ocultar banner de ejemplo]
    I --> Z
```

## 5. Flujo de copiado a Word

```mermaid
flowchart TD
    A([Copiar todo para Word]) --> B{¿Hay texto?}
    B -->|No| B1[Toast: No hay texto] --> Z([Fin])
    B -->|Sí| C[Seleccionar todo el editor]
    C --> D[execCommand copy]
    D --> E{¿Éxito?}
    E -->|Sí| F[Evento copy: HTML limpio + texto plano]
    E -->|No| G[Clonar, limpiar marcas y usar copyRich]
    F --> H[Toast: Copiado]
    G --> H
    H --> I([Pegar en Word con Ctrl+V])
```

## 6. Estados de la pantalla

| Estado | Editor | Observaciones | Botones activos |
|--------|--------|---------------|-----------------|
| Inicial | Texto de ejemplo coloreado | Estadísticas del ejemplo | Todos |
| Vacío (tras Limpiar) | Placeholder | Estadísticas en 0 | Todos |
| Con texto sin colorear | Texto pegado | Sin cambios | Todos |
| Coloreado | Texto pintado + etiquetas | Estadísticas y tablas | Todos |
| Colores quitados | Texto sin pintura | Estadísticas en 0 | Todos |

## 7. Mapa de acciones

| Acción | Atajo | Resultado |
|--------|-------|-----------|
| Cortar | `Ctrl+X` | Copia limpia y elimina selección |
| Copiar | `Ctrl+C` | HTML limpio + texto plano |
| Pegar | `Ctrl+V` | Pegado con formato sanitizado |
| Deshacer | `Ctrl+Z` | Revierte la última edición (incluye colorear) |
| Rehacer | `Ctrl+Y` | Reaplica la edición |
| Seleccionar todo | `Ctrl+A` | Selecciona el contenido del editor |
| Limpiar | — | Vacía editor y observaciones |
| Colorear | — | Pinta y genera observaciones |
| Quitar colores | — | Elimina pintura y sugerencias |
| Copiar todo para Word | — | Copia el documento completo |
| Copiar tabla para Word | — | Copia la tabla de observaciones |
