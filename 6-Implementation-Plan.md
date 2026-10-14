# 6. Implementation Plan — Plan de Implementación

**Producto:** Coloreador DUA 2.2

---

## 1. Resumen

Plan por fases para construir, validar y publicar la herramienta. Las fases 1 a 6 corresponden a la versión 2.2 ya construida; las fases 7 y 8 cubren la publicación en GitHub y la evolución futura.

| Fase | Nombre | Estado |
|------|--------|--------|
| 1 | Base y editor | ✅ Completada |
| 2 | Detección de códigos y pintado | ✅ Completada |
| 3 | Clasificación e inferencia | ✅ Completada |
| 4 | Observaciones y transparencia | ✅ Completada |
| 5 | Pegado y copiado con formato | ✅ Completada |
| 6 | Accesibilidad, tema y pruebas | ✅ Completada |
| 7 | Publicación en GitHub | ⬜ Pendiente |
| 8 | Evolución futura | ⬜ Planificada |

## 2. Fases y tareas

### Fase 1 — Base y editor
- [x] Estructura HTML en una columna (herramientas, hoja, observaciones)
- [x] Editor `contenteditable` con aspecto de hoja Word
- [x] Variables CSS, tipografía Atkinson Hyperlegible y leyenda de colores
- [x] Botones de edición: cortar, copiar, pegar, deshacer, rehacer, seleccionar todo, limpiar

**Entregable:** interfaz funcional sin lógica DUA.

### Fase 2 — Detección de códigos y pintado
- [x] Catálogos `COLORS`, `MAXPT`, `DESC`, `groupOf()`
- [x] Expresiones `pureRe` y `codeRe` para códigos entre paréntesis
- [x] Función `run()` con cálculo de rangos y `mkSpan()`
- [x] Regla de color dominante y negrita en líneas mixtas
- [x] Validación de códigos inexistentes
- [x] `unpaint()` y botón «Quitar colores»

**Entregable:** coloreado fiel de la codificación del docente.

### Fase 3 — Clasificación e inferencia
- [x] `STRUCT` para títulos, tiempos y destrezas
- [x] `classify()` con criterios de estructura vs. actividad
- [x] `RULES` ponderadas (~35 reglas por pauta)
- [x] `VERBCLS` como respaldo por verbo
- [x] `infer()` con preferencia por principios distintos
- [x] Etiqueta `[DUA sugerido: …]` y casilla para desactivar

**Entregable:** sugerencias razonadas para líneas sin código.

### Fase 4 — Observaciones y transparencia
- [x] Estadísticas (docente, mixtas, interpretadas, sin pintar)
- [x] Tabla de texto interpretado con código y razón
- [x] Tabla de texto no pintado con motivo
- [x] Avisos de códigos inexistentes
- [x] Botón «Copiar tabla para Word»

**Entregable:** panel de observaciones completo.

### Fase 5 — Pegado y copiado con formato
- [x] `sanitize()` con lista blanca y limpieza de estilos `mso-*`
- [x] `insertPasted()` con respaldo a texto plano
- [x] Copiar/cortar con HTML limpio y marcadores `StartFragment`/`EndFragment`
- [x] `copyRich()` con `ClipboardItem` y respaldo por `execCommand`
- [x] «Copiar todo para Word»
- [x] Integración con la pila de deshacer

**Entregable:** flujo Word → herramienta → Word sin pérdida de formato.

### Fase 6 — Accesibilidad, tema y pruebas
- [x] Roles y etiquetas ARIA, `aria-live` en el toast
- [x] Foco visible y objetivos táctiles de 40 px
- [x] Modo claro/oscuro automático
- [x] Diseño responsive (≤ 700 px)
- [x] `prefers-reduced-motion`
- [x] Texto de ejemplo precargado
- [x] Pruebas manuales (ver sección 4)

**Entregable:** versión 2.2 estable.

### Fase 7 — Publicación en GitHub *(siguiente)*
- [ ] Crear el repositorio (por ejemplo `coloreador-dua`)
- [ ] Renombrar `Coloreador DUA 2.2.html` a `index.html`
- [ ] Subir `index.html`, `README.md` y la carpeta `docs/` con los 6 documentos
- [ ] Agregar archivo `LICENSE`
- [ ] Activar **GitHub Pages** (Settings → Pages → rama `main`, carpeta `/ (root)`)
- [ ] Verificar el enlace público en Chrome, Edge y Firefox
- [ ] Agregar el enlace público al README
- [ ] Crear la primera *release* `v2.2.0`

**Entregable:** herramienta publicada y documentada.

### Fase 8 — Evolución futura
- [ ] Exportación directa a `.docx`
- [ ] Reglas de sugerencia editables por el docente
- [ ] Gráfico de distribución por principio
- [ ] Pruebas automatizadas de `classify()` e `infer()`
- [ ] Guía para contribuir (`CONTRIBUTING.md`)

## 3. Estructura final del repositorio

```
coloreador-dua/
├── index.html                 # Aplicación completa
├── README.md                  # Presentación y uso
├── LICENSE                    # Licencia del proyecto
└── docs/
    ├── 1-PRD.md
    ├── 2-TRD.md
    ├── 3-UI-UX-Design.md
    ├── 4-App-Flow.md
    ├── 5-Backend-Schema.md
    └── 6-Implementation-Plan.md
```

## 4. Plan de pruebas (lista de verificación)

| # | Prueba | Resultado esperado |
|---|--------|--------------------|
| 1 | Abrir la página | Se muestra el ejemplo coloreado |
| 2 | Pegar desde Word con tablas y negritas | Se conserva el formato |
| 3 | Línea con `(3.1)` | Texto morado |
| 4 | Línea con `(5.1 - 9.1)` | Texto azul; `5.1` azul y `9.1` verde en negrita |
| 5 | Línea con `(4.5)` | Aviso de código inexistente |
| 6 | Línea «Unidad 1» y «Tiempo estimado» | Sin pintar, con motivo |
| 7 | Línea de destreza `CN.4.3.18.` | Sin pintar, con motivo |
| 8 | Actividad sin código con «mapa conceptual» | Sugerencia 3.3 con razón |
| 9 | Casilla «Sugerir» desmarcada | Actividades sin código quedan sin pintar |
| 10 | Colorear → Deshacer | Vuelve al estado anterior |
| 11 | Quitar colores | Texto sin pintura ni etiquetas |
| 12 | Copiar todo → pegar en Word | Colores y formato conservados |
| 13 | Copiar tabla de observaciones | Tabla con bordes en Word |
| 14 | Pegar HTML con `<script>` | Script descartado |
| 15 | Navegar solo con teclado | Todos los controles alcanzables con foco visible |
| 16 | Ancho de 360 px | Sin desplazamiento horizontal de página |
| 17 | Modo oscuro del sistema | Interfaz oscura; hoja blanca |

## 5. Criterios de aceptación de la publicación

- [ ] El enlace de GitHub Pages abre la herramienta sin errores en consola.
- [ ] Las 17 pruebas anteriores pasan en Chrome o Edge.
- [ ] Los 6 documentos y el README se visualizan correctamente en GitHub (incluidos los diagramas Mermaid).
- [ ] El repositorio tiene licencia definida.

## 6. Riesgos del plan

| Riesgo | Mitigación |
|--------|-----------|
| Nombre de archivo con espacios rompe el enlace | Usar `index.html` |
| Permisos del portapapeles en Pages (requiere HTTPS) | GitHub Pages sirve por HTTPS por defecto |
| Cambios futuros rompen la lógica | Agregar pruebas automatizadas (Fase 8) |

## 7. Responsables y tiempos estimados

| Fase | Responsable | Estimación |
|------|-------------|-----------|
| 7 — Publicación | MSc. Carlos Reinoso | 30–60 minutos |
| 8 — Evolución | MSc. Carlos Reinoso / colaboradores | Según prioridad |
