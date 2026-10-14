# 1. PRD — Documento de Requisitos del Producto

**Producto:** Coloreador DUA 2.2
**Versión:** 2.2
**Autor:** MSc. Carlos Reinoso
**Estado:** Publicado (versión estable)

---

## 1. Resumen

Coloreador DUA 2.2 es una herramienta web de un solo archivo que permite a docentes pegar una planificación microcurricular, **pintar cada actividad según su código del Diseño Universal para el Aprendizaje (DUA 2.2)** y copiar el resultado a Word conservando el formato original.

## 2. Problema

Los docentes ecuatorianos de EGB y BGU deben codificar las actividades de sus planificaciones con las pautas DUA. Hacerlo a mano en Word implica:

- Pintar línea por línea con tres colores distintos según el principio (Representación, Acción y expresión, Compromiso).
- Revisar que los códigos existan y correspondan a la pauta correcta.
- Decidir qué código asignar a actividades que no lo traen.
- Perder tiempo en una tarea repetitiva y propensa a errores.

## 3. Usuarios objetivo

| Usuario | Necesidad principal |
|---------|--------------------|
| Docente de EGB / BGU | Colorear su planificación rápido sin perder el formato de Word |
| Coordinador / directivo académico | Revisar de forma visual la distribución de principios DUA |
| Docente que recién conoce el DUA | Recibir sugerencias de código con su justificación |

## 4. Objetivos

1. Colorear una planificación completa en **menos de un minuto** con un solo clic.
2. **Respetar siempre** la codificación hecha por el docente.
3. Proponer códigos para líneas sin código, **explicando la razón**.
4. Conservar tablas, negritas, listas y estilos al copiar a Word.
5. Funcionar sin instalación, sin cuentas y **sin enviar datos a ningún servidor**.

## 5. Fuera de alcance (no objetivos)

- No es un editor de texto completo ni reemplaza a Word.
- No genera planificaciones (eso lo hace el skill de planificación microcurricular).
- No guarda documentos ni historial.
- No exporta archivos `.docx` directamente (se copia y pega).
- No usa inteligencia artificial remota: la interpretación es por reglas locales.

## 6. Historias de usuario

| ID | Como… | Quiero… | Para… |
|----|-------|---------|-------|
| HU-1 | docente | pegar mi planificación desde Word | no reescribirla |
| HU-2 | docente | que las líneas con código se pinten del color de su principio | verificar visualmente mi codificación |
| HU-3 | docente | que las líneas con dos códigos conserven el color de cada uno | ver la combinación de principios |
| HU-4 | docente | recibir códigos sugeridos para líneas sin código | completar mi planificación |
| HU-5 | docente | saber por qué se sugirió un código | decidir si lo acepto |
| HU-6 | docente | saber qué líneas no se pintaron y por qué | no dudar de si fue un error |
| HU-7 | docente | desactivar las sugerencias | colorear solo lo que yo codifiqué |
| HU-8 | docente | deshacer el coloreado | corregir sin perder el texto |
| HU-9 | docente | copiar todo a Word con colores | entregar el documento final |
| HU-10 | docente | recibir aviso si escribí un código inexistente | corregir errores de digitación |

## 7. Requisitos funcionales

| ID | Requisito | Prioridad |
|----|-----------|-----------|
| RF-01 | Editor con `contenteditable` que acepte pegado con formato | Alta |
| RF-02 | Sanitizar el HTML pegado (eliminar `mso-*`, scripts, imágenes, estilos innecesarios) | Alta |
| RF-03 | Detectar códigos DUA entre paréntesis con formato `N.N` (separados por `-`, `–`, `,`, `;`, `/` o `y`) | Alta |
| RF-04 | Pintar según grupo: Pautas 1–3 morado, 4–6 azul, 7–9 verde | Alta |
| RF-05 | En líneas con códigos de distintos principios, usar el color del primer código y negrita en cada código con su propio color | Alta |
| RF-06 | Nunca modificar los códigos escritos por el docente | Alta |
| RF-07 | Clasificar líneas sin código como *neutral* (estructura) o *analizable* (actividad) | Alta |
| RF-08 | Sugerir hasta dos códigos (de principios distintos si es posible) con la etiqueta `[DUA sugerido: …]` | Alta |
| RF-09 | Interpretar por verbo inicial cuando no hay medio ni producto explícito | Media |
| RF-10 | Casilla «Sugerir código si falta» activable/desactivable | Media |
| RF-11 | Mostrar estadísticas: con código del docente, combinan principios, interpretadas, sin pintar | Media |
| RF-12 | Tabla de observaciones: texto interpretado + código + razón | Alta |
| RF-13 | Tabla de texto no pintado + motivo | Alta |
| RF-14 | Advertir códigos inexistentes en el DUA 2.2 (por ejemplo `4.5`) | Media |
| RF-15 | Botones: cortar, copiar, pegar, deshacer, rehacer, seleccionar todo, limpiar | Media |
| RF-16 | «Quitar colores» que retire pintura y sugerencias | Media |
| RF-17 | «Copiar todo para Word» con formato y colores | Alta |
| RF-18 | «Copiar tabla para Word» de las observaciones | Media |
| RF-19 | Al copiar/cortar, eliminar marcas internas (`data-dua`) y entregar HTML limpio | Media |
| RF-20 | Cargar un texto de ejemplo al abrir | Baja |

## 8. Requisitos no funcionales

| ID | Requisito |
|----|-----------|
| RNF-01 | **Privacidad:** ningún dato sale del navegador |
| RNF-02 | **Portabilidad:** un único archivo HTML, sin dependencias de compilación |
| RNF-03 | **Accesibilidad:** foco visible, roles ARIA, contraste adecuado, tipografía hiperlegible |
| RNF-04 | **Responsive:** utilizable desde 360 px de ancho |
| RNF-05 | **Tema:** modo claro y oscuro automáticos según el sistema |
| RNF-06 | **Compatibilidad:** Chrome, Edge y Firefox actuales |
| RNF-07 | **Rendimiento:** coloreado percibido como instantáneo en planificaciones de hasta ~50 páginas |
| RNF-08 | **Reversibilidad:** el coloreado se integra a la pila de deshacer del navegador |

## 9. Reglas de negocio

1. Los códigos válidos son `1.1`–`9.3` según el máximo de cada pauta: `{1:3, 2:5, 3:4, 4:2, 5:3, 6:4, 7:3, 8:4, 9:3}`.
2. El **primer código** de una línea determina el color dominante.
3. Los títulos, tiempos y destrezas con criterio de desempeño (CN, LL, M…) **no son actividades** y no se pintan.
4. Las sugerencias son **propuestas para revisión docente**, nunca codificación definitiva.

## 10. Métricas de éxito

| Métrica | Meta |
|---------|------|
| Tiempo para colorear una planificación | < 1 minuto |
| Líneas con código del docente alteradas | 0 |
| Pérdida de formato al copiar a Word | Ninguna visible (tablas, negritas, listas) |
| Docentes que completan el flujo sin ayuda | ≥ 90 % |

## 11. Riesgos y mitigación

| Riesgo | Impacto | Mitigación |
|--------|---------|-----------|
| Las reglas por palabras clave sugieren un código poco adecuado | Medio | Mostrar siempre la razón; marcar como «sugerido»; permitir desactivar |
| El portapapeles varía entre navegadores | Medio | Respaldo con `execCommand` y mensajes de ayuda |
| Word altera el pegado según versión | Bajo | HTML limpio con marcadores `StartFragment`/`EndFragment` |
| Fuente externa no carga | Bajo | Pila de fuentes del sistema como respaldo |

## 12. Hoja de ruta (futuro)

- Exportación directa a `.docx`.
- Reglas editables por el docente.
- Resumen estadístico por principio (gráfico).
- Soporte para otras versiones de las pautas DUA.
