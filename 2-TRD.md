# 2. TRD — Documento de Requisitos Técnicos

**Producto:** Coloreador DUA 2.2
**Tipo de aplicación:** SPA estática de un solo archivo (client-side only)

---

## 1. Resumen técnico

Aplicación 100 % cliente: HTML + CSS + JavaScript sin frameworks, sin proceso de compilación y sin servidor. Se despliega como archivo estático (GitHub Pages) o se abre localmente.

## 2. Stack tecnológico

| Capa | Tecnología |
|------|-----------|
| Marcado | HTML5 semántico (`main`, `header`, `section`) con roles ARIA |
| Estilos | CSS3 con variables (`:root`), `clamp()`, flexbox, `prefers-color-scheme` |
| Lógica | JavaScript ES2020+ (vanilla) |
| Fuente | Atkinson Hyperlegible (Google Fonts) con respaldo del sistema |
| APIs del navegador | `contenteditable`, `Selection`/`Range`, `document.execCommand`, `Clipboard API`, `ClipboardItem`, `DOMParser` |
| Dependencias de build | Ninguna |

## 3. Arquitectura

```
┌────────────────────────────────────────────────────┐
│                  index.html (único)                │
│                                                    │
│  ┌──────────┐   ┌──────────────┐   ┌────────────┐  │
│  │   UI     │──▶│  Motor DUA   │──▶│ Observa-   │  │
│  │ (botones,│   │ classify()   │   │ ciones     │  │
│  │ editor)  │   │ infer()      │   │ render()   │  │
│  └────┬─────┘   │ run()        │   └────────────┘  │
│       │         └──────┬───────┘                   │
│       ▼                ▼                           │
│  ┌──────────┐   ┌──────────────┐                   │
│  │ Portapa- │   │ DOM (editor  │                   │
│  │ peles    │   │ contenteditable)│                │
│  └──────────┘   └──────────────┘                   │
└────────────────────────────────────────────────────┘
          Sin red · Sin almacenamiento · Sin backend
```

## 4. Módulos y funciones

| Módulo | Funciones principales | Responsabilidad |
|--------|----------------------|-----------------|
| **Constantes DUA** | `COLORS`, `MAXPT`, `NAMES`, `DESC`, `groupOf()` | Colores, máximos por pauta, descripciones y grupo de cada código |
| **Detección de códigos** | `pureRe`, `codeRe`, `CODE` | Reconocer paréntesis que contienen solo códigos DUA |
| **Reglas de sugerencia** | `RULES`, `VERBCLS`, `infer()` | Puntuar palabras clave y, si no hay coincidencias, interpretar por verbo |
| **Clasificación** | `STRUCT`, `classify()`, `isActivity()` | Distinguir estructura (neutral) de actividad (analizable) |
| **Estructura DOM** | `normalizeRoot()`, `collect()` | Envolver texto suelto en `<p>` y mapear texto → nodos |
| **Pintado** | `run()`, `mkSpan()`, `unpaint()` | Calcular rangos y envolver con `<span data-dua>` |
| **Observaciones** | `render()`, `chips()`, `copyObs()` | Estadísticas y tablas de observaciones |
| **Edición** | `setContent()`, `working()`, `colorize()`, `uncolor()` | Aplicar cambios como edición deshacible |
| **Pegado** | `sanitize()`, `cleanNode()`, `cleanStyle()`, `insertPasted()` | Limpiar HTML pegado conservando formato útil |
| **Copiado** | `onCopy()`, `selectionClone()`, `copyRich()`, `plainOf()` | Entregar HTML limpio + texto plano al portapapeles |
| **UI** | Manejadores de botones, `toast()` | Interacción y mensajes |

## 5. Flujo de procesamiento (colorear)

1. `colorize()` clona el contenido del editor (`working()`), quita pintura previa (`unpaint()`) y normaliza bloques (`normalizeRoot()`).
2. `run(root)` recorre los bloques hoja (`p, li, h1–h6, td, th, div, blockquote`).
3. Para cada bloque, `collect()` construye el texto plano y la lista de nodos de texto con sus desplazamientos.
4. Para cada línea:
   - Si tiene paréntesis con solo códigos → **código del docente**.
   - Si no → `classify()`:
     - `neutral` → se registra en «no pintado».
     - `analizar` → si «Sugerir» está activo, `infer()`; si devuelve resultado, se pinta como sugerido.
5. Se acumulan **rangos** `{s, e, color, bold}` por bloque.
6. Los rangos se aplican sobre los nodos de texto con `mkSpan()` y se añade la etiqueta `[DUA sugerido: …]` donde corresponda.
7. `setContent()` reinserta el HTML con `execCommand("insertHTML")` para conservar el historial de deshacer.
8. `render()` actualiza estadísticas y tablas.

## 6. Algoritmo de clasificación e inferencia

### 6.1 `classify(line, isHeading)`
Orden de decisión:
1. Coincide con `STRUCT` (Unidad N, Tiempo estimado, destreza `CN.4.3.18`) → neutral.
2. Es encabezado `H1–H6` → neutral.
3. Sin verbo inicial reconocible, ≤ 14 palabras y sin signo final → neutral (título o fase).
4. Menos de 4 palabras → neutral.
5. En otro caso → analizable.

### 6.2 `infer(text)`
1. Evalúa cada regla de `RULES` (`[código, regex, peso]`) y acumula coincidencias.
2. Ordena por peso descendente y luego por orden de definición.
3. El primer código es el de mayor peso; el segundo se elige entre coincidencias con peso ≥ `primero − 1.5`, **prefiriendo otro grupo de color**.
4. Sin coincidencias → intenta `VERBCLS` por verbo inicial.
5. Sin verbo reconocible → `null` (queda sin pintar).

## 7. Pegado y sanitización

- Se intercepta el evento `paste` y se previene el comportamiento por defecto.
- `sanitize()` usa `DOMParser` y reconstruye el HTML con una **lista blanca de etiquetas** (`ALLOWED`) y una **lista negra** (`DROP`: script, style, img, svg, iframe…).
- `cleanStyle()` conserva solo propiedades de `KEEP` (fuente, tamaño, color, bordes, alineación…) y descarta valores `mso-*`, `windowtext`, `auto`, `inherit`.
- Atributos permitidos: `style`, `colspan`, `rowspan` y `href` solo si es `http(s)`.
- El texto se escapa con `esc()` antes de insertarse.

## 8. Portapapeles

| Acción | Mecanismo principal | Respaldo |
|--------|--------------------|----------|
| Pegar (Ctrl+V) | Evento `paste` + `insertPasted()` | Texto plano → `<p>` |
| Pegar (botón) | `navigator.clipboard.read()` | Mensaje para usar Ctrl+V |
| Copiar / cortar | Evento `copy`/`cut` con HTML + texto | — |
| Copiar todo para Word | `execCommand("copy")` tras seleccionar todo | `copyRich()` con `ClipboardItem` |
| Copiar tabla | `copyRich()` | Elemento oculto + `execCommand("copy")` |

El HTML copiado se envuelve con `<!--StartFragment-->` / `<!--EndFragment-->` para mejorar la compatibilidad con Word y se eliminan los atributos `data-dua` internos.

## 9. Seguridad

| Aspecto | Medida |
|---------|--------|
| XSS por contenido pegado | Lista blanca de etiquetas, escape de texto, `href` solo `http(s)` |
| Scripts embebidos | `SCRIPT`, `IFRAME`, `OBJECT`, `EMBED` descartados |
| Privacidad | Sin `fetch`, sin `XMLHttpRequest`, sin analítica, sin almacenamiento |
| Dependencias externas | Solo hoja de estilos de Google Fonts |

## 10. Rendimiento

- Complejidad lineal respecto al número de líneas × número de reglas (~35 expresiones regulares).
- Las operaciones sobre el DOM se hacen sobre un clon y se inyectan una sola vez.
- `normalize()` fusiona nodos de texto tras quitar la pintura.

## 11. Compatibilidad

| Navegador | Estado |
|-----------|--------|
| Chrome / Edge (reciente) | Soportado |
| Firefox (reciente) | Soportado (el botón Pegar puede requerir Ctrl+V) |
| Safari | Parcial (permisos de portapapeles más estrictos) |

## 12. Limitaciones técnicas

- `document.execCommand` está en desuso pero sigue siendo la única vía fiable para ediciones deshacibles en `contenteditable`.
- La interpretación por palabras clave no entiende contexto semántico profundo.
- El formato exacto al pegar en Word depende de la versión del programa.
- Una sola vista previa a la vez (sin pestañas ni múltiples documentos).

## 13. Estrategia de pruebas

| Tipo | Qué se verifica |
|------|----------------|
| Manual funcional | Pegado desde Word, colorear, quitar, deshacer, copiar |
| Casos de código | Un código, dos del mismo principio, dos de distinto principio, códigos inexistentes |
| Casos de clasificación | Títulos, tiempos, destrezas, actividades cortas y largas |
| Casos de seguridad | Pegar HTML con `<script>`, `onerror`, `javascript:` |
| Accesibilidad | Navegación por teclado, lector de pantalla, contraste |
| Responsive | 360 px, 768 px, 1280 px |
