# Coloreador DUA 2.2

Herramienta web de un solo archivo para **pintar planificaciones microcurriculares según los códigos del Diseño Universal para el Aprendizaje (DUA 2.2)** y copiarlas a Word conservando el formato original.

No requiere instalación, servidor ni conexión a bases de datos: todo se ejecuta en el navegador y el texto de la planificación **nunca sale del equipo**.

## Características

- **Pegado con formato:** conserva tablas, negritas, listas y estilos de Word, descartando el código basura (`mso-*`, imágenes, scripts).
- **Colorea por pauta DUA:** cada actividad toma el color de su código.
- **Respeta la codificación del docente:** los códigos que ya están escritos (por ejemplo `(3.1 - 1.1)`) nunca se modifican.
- **Sugiere códigos si faltan:** analiza cada línea (medio, producto, verbo, forma de trabajo) y propone el código como `[DUA sugerido: …]`, para revisión docente. Se puede desactivar.
- **Líneas mixtas:** cuando una línea combina principios, el texto toma el color del primer código (dominante) y cada código conserva el suyo en negrita.
- **Observaciones transparentes:** muestra qué se interpretó y por qué, y qué no se pintó y la razón (títulos, destrezas CN/LL/M, tiempos, etc.).
- **Validación de códigos:** avisa si un código no existe en el DUA 2.2 (por ejemplo `4.5`).
- **Copiar para Word:** copia todo el documento, o la tabla de observaciones, con colores y formato.
- **Edición completa:** cortar, copiar, pegar, deshacer, rehacer, seleccionar todo y limpiar.
- **Accesible:** tipografía Atkinson Hyperlegible, foco visible, modo claro/oscuro automático y diseño adaptable a móvil.

## Leyenda de colores

| Color | Pautas | Principio |
|-------|--------|-----------|
| Morado `#725196` | 1 – 3 | Representación |
| Azul `#1A7CBB` | 4 – 6 | Acción y expresión |
| Verde `#128A43` | 7 – 9 | Compromiso |

## Uso

1. Abre `index.html` en el navegador (Chrome, Edge o Firefox).
2. Pulsa **Limpiar** y pega tu planificación con `Ctrl+V` (`⌘+V` en Mac).
3. Decide si dejas activada la casilla **Sugerir código si falta**.
4. Pulsa **Colorear**. Puedes deshacer con **Deshacer** o **Quitar colores**.
5. Revisa la sección **Observaciones**.
6. Pulsa **Copiar todo para Word** y pega en tu documento.

> Si el botón **Pegar** no funciona en tu navegador (por permisos del portapapeles), usa `Ctrl+V` dentro del cuadro de texto.

## Pautas del DUA 2.2 reconocidas

| Código | Pauta | Código | Pauta |
|--------|-------|--------|-------|
| 1.1 | Personalizar la visualización | 6.1 | Establecer metas |
| 1.2 | Alternativas para información auditiva | 6.2 | Planificar estrategias |
| 1.3 | Alternativas para información visual | 6.3 | Gestionar información y recursos |
| 2.1 | Aclarar vocabulario y símbolos | 6.4 | Monitorear el progreso |
| 2.2 | Aclarar sintaxis y estructura | 7.1 | Elección y autonomía |
| 2.3 | Decodificar texto y notación | 7.2 | Relevancia y contexto real |
| 2.4 | Comprensión entre idiomas | 7.3 | Reducir amenazas y distracciones |
| 2.5 | Ilustrar con múltiples medios | 8.1 | Hacer visibles las metas |
| 3.1 | Activar conocimiento previo | 8.2 | Variar demandas y recursos |
| 3.2 | Resaltar patrones e ideas clave | 8.3 | Colaboración y comunidad |
| 3.3 | Guiar el procesamiento de la información | 8.4 | Retroalimentación orientada al dominio |
| 3.4 | Transferencia y generalización | 9.1 | Expectativas que motivan |
| 4.1 | Variar métodos de respuesta | 9.2 | Estrategias de afrontamiento |
| 4.2 | Herramientas de asistencia | 9.3 | Autoevaluación y reflexión |
| 5.1 | Múltiples medios de comunicación | | |
| 5.2 | Herramientas de construcción | | |
| 5.3 | Apoyos graduados | | |

## Cómo interpreta el texto

Para cada línea, la herramienta aplica este orden:

1. **Si trae código del docente** entre paréntesis (por ejemplo `(5.1 - 9.1)`), lo respeta y pinta según el primer código.
2. **Si es estructura** (título de unidad, tiempo estimado, destreza con criterio de desempeño, encabezados o fases cortas), no la pinta y explica el motivo.
3. **Si es una actividad sin código**, busca medios, productos y formas de trabajo (rúbrica, mapa conceptual, parejas, laboratorio, etc.) y propone hasta dos códigos de principios distintos. Si no hay coincidencias, interpreta por el verbo inicial.
4. **Si no se puede interpretar**, la deja sin pintar e indica por qué.

Las sugerencias son una **propuesta para revisión docente**, no una codificación definitiva.

## Publicar en GitHub Pages

1. Renombra el archivo `Coloreador DUA 2.2.html` a **`index.html`** (evita espacios en el nombre).
2. Sube `index.html`, este `README.md` y la carpeta `docs/` a un repositorio nuevo.
3. En el repositorio, ve a **Settings → Pages**.
4. En *Build and deployment*, elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`.
5. Guarda. En unos minutos la herramienta estará disponible en `https://<usuario>.github.io/<repositorio>/`.

## Estructura del repositorio

```
.
├── index.html   # La aplicación completa (HTML + CSS + JavaScript)
├── README.md    # Esta documentación
└── docs/
    ├── 1-PRD.md
    ├── 2-TRD.md
    ├── 3-UI-UX-Design.md
    ├── 4-App-Flow.md
    ├── 5-Backend-Schema.md
    └── 6-Implementation-Plan.md
```

## Tecnología

- HTML, CSS y JavaScript sin dependencias ni proceso de compilación.
- Única conexión externa: fuente **Atkinson Hyperlegible** desde Google Fonts (si no carga, usa la fuente del sistema).
- API del navegador: `contenteditable`, `Selection`, `Clipboard` y `DOMParser`.

## Privacidad

Todo el procesamiento ocurre localmente en el navegador. No se envían, almacenan ni registran datos de las planificaciones.

## Limitaciones conocidas

- La detección de códigos sugeridos se basa en palabras clave y verbos; puede no acertar en redacciones poco habituales.
- El botón **Pegar** depende de los permisos del portapapeles del navegador.
- El resultado al pegar en Word puede variar ligeramente según la versión del programa.

## Contribuir

Las sugerencias y mejoras son bienvenidas: abre un *issue* o envía un *pull request*. Las reglas de interpretación están en el arreglo `RULES` y en `VERBCLS` dentro de `index.html`.

## Licencia

Define aquí la licencia del proyecto (por ejemplo MIT o CC BY 4.0) y agrega el archivo `LICENSE` correspondiente.

## Autor

MSc. Carlos Reinoso — Unidad Educativa "Otto Sharnow", Cangahua, Cayambe, Ecuador.
