# 3. UI/UX Design — Diseño de Interfaz y Experiencia

**Producto:** Coloreador DUA 2.2

---

## 1. Principios de diseño

1. **Una sola columna de trabajo:** herramientas arriba, hoja al centro, observaciones debajo.
2. **La hoja parece Word:** fondo blanco, texto oscuro, tipografía tipo Calibri, para que el docente reconozca su documento.
3. **Transparencia:** todo lo que la herramienta interpreta o decide no pintar se explica.
4. **Accesible primero:** tipografía hiperlegible, foco visible, alto contraste.
5. **Cero fricción:** sin registro, sin menús ocultos; un clic para colorear.

## 2. Estructura de la pantalla

```
┌──────────────────────────────────────────────┐
│ Coloreador DUA 2.2                           │  ← Encabezado
│ Descripción breve                            │
├──────────────────────────────────────────────┤
│ [Edición]  | [Pintado]     | [Salida]        │  ← Barra de herramientas
│ Cortar…    | Colorear      | Copiar todo     │
│            | Quitar colores| para Word       │
│            | ☑ Sugerir     |                 │
│ ● Pautas 1–3  ● Pautas 4–6  ● Pautas 7–9     │  ← Leyenda
├──────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────┐ │
│ │      Hoja tipo Word (editable)           │ │  ← Editor
│ └──────────────────────────────────────────┘ │
├──────────────────────────────────────────────┤
│ Observaciones                                │
│ [4 estadísticas]                             │  ← Resultados
│ Tabla: texto interpretado                    │
│ Tabla: texto no pintado                      │
└──────────────────────────────────────────────┘
```

## 3. Sistema visual

### 3.1 Colores semánticos (principios DUA)

| Token | Hex | Principio | Pautas |
|-------|-----|-----------|--------|
| `--purple` | `#725196` | Representación | 1–3 |
| `--blue` | `#1A7CBB` | Acción y expresión | 4–6 |
| `--green` | `#128A43` | Compromiso | 7–9 |

### 3.2 Colores de interfaz

| Token | Claro | Oscuro | Uso |
|-------|-------|--------|-----|
| `--bg` | `#eef1f4` | `#12161b` | Fondo de página |
| `--surface` | `#ffffff` | `#1a2027` | Paneles |
| `--ink` | `#1c2530` | `#e6eaee` | Texto |
| `--muted` | `#55606c` | `#9aa5b1` | Texto secundario |
| `--line` | `#d3dae2` | `#2d353e` | Bordes |
| `--btn` | `#e4e9ee` | `#29313a` | Botón secundario |
| `--primary` | `#1c2530` | `#e6eaee` | Botón principal |
| `--focus` | `#1A7CBB` | `#6cb8ea` | Anillo de foco |

> La **hoja** del editor se mantiene siempre blanca (`--paper`) en ambos temas, igual que Word.

### 3.3 Tipografía

| Elemento | Fuente | Tamaño |
|----------|--------|--------|
| Interfaz | Atkinson Hyperlegible (respaldo: system-ui) | 16 px base |
| Título H1 | Atkinson Hyperlegible | `clamp(1.5rem, 3.4vw, 2rem)` |
| Hoja del editor | Calibri / Carlito | 16 px, interlineado 1.55 |
| Botones | Atkinson, negrita | 0.9 rem |

### 3.4 Espaciado y forma

- Paneles con radio de 12 px, borde de 1 px.
- Botones con radio de 8 px y altura mínima de 40 px (objetivo táctil).
- Márgenes laterales de 16 px; ancho máximo 1040 px.
- Separación entre secciones de 18 px.

## 4. Componentes

| Componente | Descripción | Estados |
|-----------|-------------|---------|
| Botón secundario | Fondo `--btn`, texto oscuro | normal, hover (brillo 95 %), foco |
| Botón primario | Fondo `--primary` (Colorear, Copiar para Word) | normal, hover, foco |
| Casilla | «Sugerir código si falta», marcada por defecto | marcada, desmarcada, foco |
| Chip de leyenda | Punto de color + texto | estático |
| Editor | `contenteditable`, rol `textbox` | vacío (placeholder), con texto, foco |
| Estadística | Número destacado + etiqueta | estático |
| Tabla de observaciones | Encabezados en mayúscula, filas con borde inferior | desplazamiento horizontal en móvil |
| Código (`.code`) | Insignia blanca con borde y color del principio | estático |
| Aviso (`.warn`) | Cuadro con borde para códigos inexistentes | visible/oculto |
| Toast | Mensaje flotante inferior central, 2.6 s | visible/oculto (`aria-live`) |

## 5. Mensajes (microcopy)

| Situación | Mensaje |
|-----------|---------|
| Editor vacío | «Pega aquí tu planificación (Ctrl+V). Se conserva el formato de Word.» |
| Sin texto al colorear | «Pega primero el texto de la planificación.» |
| Coloreado exitoso | «Texto coloreado.» |
| Quitar colores | «Colores y sugerencias quitados.» |
| Copia exitosa | «Copiado con formato y colores. Pégalo en Word con Ctrl+V.» |
| Pegar bloqueado | «Tu navegador no permite pegar con el botón. Usa Ctrl+V (⌘+V en Mac) dentro del cuadro.» |
| Sin selección | «Selecciona primero el texto que quieres copiar/cortar.» |
| Sin sugerencias | «Ninguna línea necesitó interpretación: todas traen código del docente.» |

## 6. Representación visual del resultado

| Caso | Cómo se ve |
|------|-----------|
| Código del docente (un principio) | Todo el texto de la línea en el color del principio |
| Código del docente (principios mezclados) | Texto en color del primer código; cada código en negrita con su propio color |
| Código sugerido | Texto pintado + etiqueta `[DUA sugerido: x.x - y.y]` con códigos en negrita |
| Estructura (título, tiempo, destreza) | Sin pintar; aparece en «Texto que no se pintó y por qué» |

## 7. Accesibilidad

- Fuente **Atkinson Hyperlegible**, diseñada para mejorar la legibilidad.
- Anillo de foco de 3 px con desplazamiento de 2 px en todos los controles.
- Roles y etiquetas ARIA: `role="group"`, `aria-label`, `role="textbox"`, `aria-multiline`.
- Toast con `role="status"` y `aria-live="polite"`.
- Información de color **complementada** con texto (códigos visibles y en negrita en líneas mixtas).
- Respeta `prefers-reduced-motion` (sin transición del toast).
- Navegable completamente por teclado.

## 8. Diseño responsive

| Ancho | Comportamiento |
|-------|---------------|
| ≥ 701 px | Grupos de la barra separados por línea vertical |
| ≤ 700 px | Grupos apilados sin separador; botones envuelven en varias filas |
| Móvil | Tablas con desplazamiento horizontal (`min-width: 560px`); hoja con relleno reducido (`clamp`) |

## 9. Tema claro / oscuro

- Automático con `prefers-color-scheme`.
- Forzable con el atributo `data-theme="light|dark"` en `<html>`.
- La hoja de papel permanece blanca para fidelidad con Word.

## 10. Recorrido de la persona usuaria

1. **Llega:** ve un texto de ejemplo y la leyenda de colores.
2. **Limpia y pega** su planificación.
3. **Pulsa Colorear:** ve el texto pintado y las estadísticas.
4. **Revisa observaciones:** entiende qué se sugirió y qué no se pintó.
5. **Copia para Word:** recibe confirmación y pega en su documento.
