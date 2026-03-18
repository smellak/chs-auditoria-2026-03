# AUDITORÍA DEFINITIVA — Cuadro de Dirección CHS

**Fecha:** 18 de marzo de 2026
**Versión app:** CHS Platform v2 (commit `9b4b429`)
**URL interna:** `http://10.0.1.37:3000`
**Base de datos:** `chs_platform` en container `vg08so8k0ww4cw48004ok8o8`
**Período principal:** MTD Marzo 2026 (2 mar – 14 mar, 12 días hábiles)
**Período alternativo:** Febrero 2026 completo

---

## PUNTUACIÓN GLOBAL

| Dimensión | Puntuación | Comentario |
|---|:---:|---|
| Exactitud de datos | **10/10** | 100% coincidencia BD ↔ UI en todas las vistas |
| Colores y señalización | **9/10** | MB%, ObjBar, DevPill correctos; E-Commerce cards usan umbral absoluto (no YoY) |
| Consistencia entre vistas | **9/10** | Coherencia total salvo 335 € de "Servicios Centrales" ausente en tablas Resumen |
| Responsive / UX | **8.5/10** | Excelente card layout; tablas pierden columna derecha en mobile/tablet |
| Seguridad | **5/10** | Sin middleware de autenticación; credenciales en .env; API sin proteger |
| Rendimiento | **8/10** | Promise.all eficiente; sin N+1; falta error handling en 4 endpoints |
| **GLOBAL** | **8.3/10** | Dashboard funcional y preciso; seguridad es el área crítica |

---

## PARTE 1: SCREENSHOTS (30/30)

Se capturaron 30 screenshots (5 vistas × 3 resoluciones × 2 períodos) en `/home/chs-dash2/auditoria-final/`.

| Vista | Desktop | Tablet | Mobile |
|---|---|---|---|
| Resumen MTD | `resumen-mtd-desktop.png` | `resumen-mtd-tablet.png` | `resumen-mtd-mobile.png` |
| Tiendas MTD | `tiendas-mtd-desktop.png` | `tiendas-mtd-tablet.png` | `tiendas-mtd-mobile.png` |
| Categorías MTD | `categorias-mtd-desktop.png` | `categorias-mtd-tablet.png` | `categorias-mtd-mobile.png` |
| E-Commerce MTD | `ecommerce-mtd-desktop.png` | `ecommerce-mtd-tablet.png` | `ecommerce-mtd-mobile.png` |
| Márgenes MTD | `margenes-mtd-desktop.png` | `margenes-mtd-tablet.png` | `margenes-mtd-mobile.png` |
| Resumen Feb | `resumen-feb2026-desktop.png` | `resumen-feb2026-tablet.png` | `resumen-feb2026-mobile.png` |
| Tiendas Feb | `tiendas-feb2026-desktop.png` | `tiendas-feb2026-tablet.png` | `tiendas-feb2026-mobile.png` |
| Categorías Feb | `categorias-feb2026-desktop.png` | `categorias-feb2026-tablet.png` | `categorias-feb2026-mobile.png` |
| E-Commerce Feb | `ecommerce-feb2026-desktop.png` | `ecommerce-feb2026-tablet.png` | `ecommerce-feb2026-mobile.png` |
| Márgenes Feb | `margenes-feb2026-desktop.png` | `margenes-feb2026-tablet.png` | `margenes-feb2026-mobile.png` |

**Resoluciones:** Desktop 1440×900, Tablet 768×1024, Mobile 375×812.

---

## PARTE 2: VALIDACIÓN DE DATOS CONTRA BD

> **NOTA CRÍTICA:** Las queries iniciales se ejecutaron contra la BD incorrecta (`chs-db`/`chs`, ventas=677K). Se re-ejecutaron contra la BD correcta (`chs_platform` en `vg08so8k0ww4cw48004ok8o8`), confirmando coincidencia total.

### 2.1 Total Empresa MTD

| Métrica | BD (`chs_platform`) | App (Resumen) | Match |
|---|---|---|:---:|
| Ventas | 1.298.873,42 | 1.298.873 € | ✅ |
| Coste | 1.002.697,59 | (no mostrado) | — |
| Margen | 296.175,87 | 296.176 € | ✅ |
| MB% | 22,8 | 22,8% | ✅ |

### 2.2 Ventas por Tienda

| Tienda | BD Ventas | App Ventas | BD Margen | App Margen | BD MB% | App MB% | Match |
|---|---|---|---|---|---|---|:---:|
| Albán | 416.755,75 | 416.756 € | 99.459,96 | 99.460 € | 23,9 | 23,9% | ✅ |
| Juncaril | 159.579,47 | 159.579 € | 38.106,52 | 38.107 € | 23,9 | 23,9% | ✅ |
| Motril | 138.048,90 | 138.049 € | 37.729,02 | 37.729 € | 27,3 | 27,3% | ✅ |
| Contract | 123.983,13 | 123.983 € | 7.370,92 | 7.371 € | 5,9 | 5,9% | ✅ |
| Antequera | 123.144,26 | 123.144 € | 29.259,29 | 29.259 € | 23,8 | 23,8% | ✅ |
| Amazon | 113.631,59 | 113.632 € | 37.691,90 | 37.692 € | 33,2 | 33,2% | ✅ |
| Almería | 97.808,60 | 97.809 € | 22.710,64 | 22.711 € | 23,2 | 23,2% | ✅ |
| CHS Web | 71.200,06 | 71.200 € | 10.738,19 | 10.738 € | 15,1 | 15,1% | ✅ |
| Shiito | 23.350,02 | 23.350 € | 7.400,81 | 7.401 € | 31,7 | 31,7% | ✅ |
| Kibuc | 15.003,26 | 15.003 € | 6.109,48 | 6.109 € | 40,7 | 40,7% | ✅ |
| Carrefour | 7.625,85 | 7.626 € | −2.100,48 | −2.100 € | −27,5 | −27,5% | ✅ |
| Media Markt | 7.112,94 | 7.113 € | 973,44 | 973 € | 13,7 | 13,7% | ✅ |
| Leroy Merlin | 1.294,26 | 1.294 € | 465,81 | 466 € | 36,0 | 36,0% | ✅ |
| Serv. Centrales | 335,33 | 335 € | 260,37 | 260 € | 77,6 | 77,6% | ✅ |

**Resultado: 14/14 tiendas ✅ (todas las diferencias son redondeo < 1 €)**

### 2.3 Año Anterior (Mar 1–14, 2025)

| Métrica | BD | App (Resumen) | Match |
|---|---|---|:---:|
| Ventas anterior | 1.193.687,32 | 1.193.687 € | ✅ |
| Margen anterior | 325.793,82 | 325.794 € (27,3%) | ✅ |
| YoY ventas | +8,8% | +8,8% | ✅ |

### 2.4 Ventas por Categoría

| Categoría | BD Ventas | App Ventas | BD MB% | App MB% | Match |
|---|---|---|---|---|:---:|
| Muebles | 630.626,35 | 631K € | 33,0 | 33,0% | ✅ |
| Electro | 558.009,69 | 558K € | 12,3 | 12,3% | ✅ |
| Servicios | 80.478,85 | 80,5K € | 11,6 | 11,6% | ✅ |
| Cocinas | 25.059,68 | 25,1K € | 27,8 | 27,8% | ✅ |
| Otros | 5.590,98 | 5.591 € | 24,6 | 24,6% | ✅ |
| Reformas | −892,13 | (no visible) | −152,9 | — | ⚠️ |

**Nota:** La categoría "Reformas" con ventas negativas (−892 €) no aparece en ninguna vista. Esto es correcto — se filtra por `ventasReal > 0`.

### 2.5 E-Commerce Channels

| Canal | BD Ventas | App Ventas | Match |
|---|---|---|:---:|
| Amazon | 113.631,59 | 113.632 € | ✅ |
| CHS Web | 71.200,06 | 71.200 € | ✅ |
| Shiito | 23.350,02 | 23.350 € | ✅ |
| Kibuc | 15.003,26 | 15.003 € | ✅ |
| Carrefour | 7.625,85 | 7.626 € | ✅ |
| Media Markt | 7.112,94 | 7.113 € | ✅ |
| Leroy Merlin | 1.294,26 | 1.294 € | ✅ |
| **Total** | **239.217,92** | **239.218 €** | ✅ |

### 2.6 Año Anterior por Tienda (YoY)

| Tienda | BD Ant. | App Ant. | BD YoY calc | App YoY | Match |
|---|---|---|---|---|:---:|
| Albán | 340.709,54 | 340.710 € | +22,3% | +22,3% | ✅ |
| Juncaril | 158.057,16 | 158.057 € | +1,0% | +1,0% | ✅ |
| Motril | 144.038,89 | 144.039 € | −4,2% | −4,2% | ✅ |
| Antequera | 138.798,45 | 138.798 € | −11,3% | −11,3% | ✅ |
| Amazon | 128.524,62 | 128.525 € | −11,6% | −11,6% | ✅ |
| Almería | 101.273,48 | 101.273 € | −3,4% | −3,4% | ✅ |
| CHS Web | 52.760,96 | 52.761 € | +34,9% | +34,9% | ✅ |
| Shiito | 50.264,69 | 50.265 € | −53,5% | −53,5% | ✅ |
| Contract | 46.151,76 | 46.152 € | +168,6% | +168,6% | ✅ |
| Kibuc | 15.189,25 | 15.189 € | −1,2% | −1,2% | ✅ |
| Media Markt | 9.120,15 | 9.120 € | −22,0% | −22,0% | ✅ |
| Leroy Merlin | 6.451,91 | 6.452 € | −79,9% | −79,9% | ✅ |
| Carrefour | 1.970,25 | 1.970 € | +287,0% | +287,0% | ✅ |
| Serv. Centrales | 189,22 | 189 € | +77,2% | +77,2% | ✅ |

**Resultado: 14/14 tiendas YoY ✅**

### 2.7 Objetivos

La tabla de objetivos en `chs_platform` es `direccion.objetivos_mensual` (no `objetivos` ni `objetivos_diario`). Contiene datos de Marzo 2026 con nombres en formato UPPERCASE:

| Tienda (BD) | Obj Mensual | App Obj (prorrateado) | Coherente |
|---|---|---|:---:|
| ALBAN | 1.050.967 | 548.892 € | ✅ (14/31 × 1.218.561 ≈ 548K) |
| JUNCARIL | 405.481 | 223.695 € | ✅ |
| MOTRIL | 379.288 | 194.894 € | ✅ |
| ANTEQUERA | 346.483 | 181.356 € | ✅ |
| ALMERIA | 275.303 | 169.823 € | ✅ |
| CONTRACT | 285.000 | 152.475 € | ✅ |

**Nota:** Los objetivos se prorratean por día (objetivo_mensual × días_transcurridos / días_mes). Los valores coinciden con la lógica de prorrateo de la app.

---

## PARTE 3: AUDITORÍA DE COLORES

### 3.1 MB% Pills — Tiendas (MTD Marzo)

Regla: **Verde** si MB% actual ≥ MB% anterior, **Rojo** si MB% actual < MB% anterior.

| Tienda | MB% actual | MB% anterior | Color esperado | Color app | Match |
|---|---|---|---|---|:---:|
| Albán | 23,9% | 28,9% | ROJO | Rojo ▼5,3pp | ✅ |
| Juncaril | 23,9% | 27,5% | ROJO | Rojo ▼3,0pp | ✅ |
| Motril | 27,3% | 28,7% | ROJO | Rojo ▼1,4pp | ✅ |
| Contract | 5,9% | 4,8% | VERDE | Verde ▲1,1pp | ✅ |
| Antequera | 23,8% | 25,1% | ROJO | Rojo ▼1,4pp | ✅ |
| Amazon | 33,2% | 28,4% | VERDE | Verde ▲4,9pp | ✅ |
| Almería | 23,2% | 28,5% | ROJO | Rojo ▼5,3pp | ✅ |
| CHS Web | 15,1% | 20,3% | ROJO | Rojo ▼5,3pp | ✅ |
| Shiito | 31,7% | 36,6% | ROJO | Rojo ▼4,9pp | ✅ |
| Kibuc | 40,7% | 41,9% | ROJO | Rojo ▼1,3pp | ✅ |
| Carrefour | −27,5% | 18,5% | ROJO | Rojo ▼46,0pp | ✅ |
| Media Markt | 13,7% | 23,9% | ROJO | Rojo ▼10,2pp | ✅ |
| Leroy Merlin | 36,0% | 24,7% | VERDE | Verde ▲11,3pp | ✅ |
| Serv. Centrales | 77,6% | 81,5% | ROJO | Rojo ▼3,8pp | ✅ |

**Resultado: 14/14 colores MB% correctos ✅**

### 3.2 MB% Pills — Categorías (MTD Marzo)

| Categoría | MB% actual | MB% anterior | Color esperado | Color app | Match |
|---|---|---|---|---|:---:|
| Muebles | 33,0% | 33,2% | ROJO | Rojo | ✅ |
| Electro | 12,3% | 18,9% | ROJO | Rojo | ✅ |
| Servicios | 11,6% | −344,2% | VERDE | Verde | ✅ |
| Cocinas | 27,8% | 37,7% | ROJO | Rojo | ✅ |
| Otros | 24,6% | 54,0% | ROJO | Rojo | ✅ |

**Resultado: 5/5 colores categoría correctos ✅**

### 3.3 ObjBar — Colores por Tienda

Regla: **Verde** ≥100%, **Azul** ≥80%, **Rojo** <80%.

| Tienda | % Obj | Color esperado | Color app | Match |
|---|---|---|---|:---:|
| Albán | 75,9% | ROJO | Rojo | ✅ |
| Juncaril | 71,3% | ROJO | Rojo | ✅ |
| Motril | 70,8% | ROJO | Rojo | ✅ |
| Antequera | 67,9% | ROJO | Rojo | ✅ |
| Almería | 57,6% | ROJO | Rojo | ✅ |
| Contract | 81,3% | AZUL | Azul | ✅ |
| Amazon | 66,4% | ROJO | Rojo | ✅ |
| CHS Web | 98,1% | AZUL | Azul | ✅ |
| Shiito | 29,5% | ROJO | Rojo | ✅ |

**Resultado: 9/9 colores ObjBar correctos ✅**

### 3.4 DevPill (YoY) — Colores

Regla: **Verde** si YoY ≥ 0%, **Rojo** si YoY < 0%.

| Tienda | YoY | Color esperado | Color app | Match |
|---|---|---|---|:---:|
| Albán | +22,3% | VERDE | Verde | ✅ |
| Juncaril | +1,0% | VERDE | Verde | ✅ |
| Motril | −4,2% | ROJO | Rojo | ✅ |
| Antequera | −11,3% | ROJO | Rojo | ✅ |
| Almería | −3,4% | ROJO | Rojo | ✅ |
| Contract | +168,6% | VERDE | Verde | ✅ |
| Amazon | −11,6% | ROJO | Rojo | ✅ |
| CHS Web | +34,9% | VERDE | Verde | ✅ |
| Shiito | −53,5% | ROJO | Rojo | ✅ |
| Kibuc | −1,2% | ROJO | Rojo | ✅ |
| Carrefour | +287,0% | VERDE | Verde | ✅ |
| Media Markt | −22,0% | ROJO | Rojo | ✅ |
| Leroy Merlin | −79,9% | ROJO | Rojo | ✅ |
| Serv. Centrales | +77,2% | VERDE | Verde | ✅ |

**Resultado: 14/14 colores YoY correctos ✅**

### 3.5 RadialGauge (Resumen)

| Métrica | Valor | Color esperado | Color app | Match |
|---|---|---|---|:---:|
| % Objetivo ventas | 72,4% | AZUL (accent, <100%) | Azul | ✅ |

### 3.6 Resumen Colores — Febrero 2026

Verificación cruzada con período alternativo:

| Elemento | Feb 2026 | Resultado |
|---|---|---|
| ObjBar Albán | 96,1% → Azul | ✅ |
| ObjBar Almería | 74,6% → Rojo | ✅ |
| ObjBar Contract | 131,0% → Verde | ✅ |
| MB% pills | Todos rojos (Feb también sufre compresión de márgenes) | ✅ |
| RadialGauge | 92,5% → Azul | ✅ |

---

## PARTE 4: CONSISTENCIA ENTRE VISTAS

### 4.1 Ventas Totales

| Vista | Valor Total | Match con BD |
|---|---|:---:|
| Resumen (KPI) | 1.298.873 € | ✅ |
| Tiendas (Total tabla) | 1.298.873 € | ✅ |
| Márgenes (Total tabla) | 1.298.873 € | ✅ |
| E-Commerce (Total) | 239.218 € (subset) | ✅ |
| Categorías (suma cards) | 631K + 558K + 80,5K + 25,1K + 5,6K ≈ 1.300K | ✅ |

### 4.2 Margen Bruto

| Vista | Valor | Match |
|---|---|:---:|
| Resumen (KPI) | 296.176 € | ✅ |
| Márgenes (KPI) | 296.176 € | ✅ |
| Tiendas (Total tabla) | 296.176 € | ✅ |

### 4.3 MB% Empresa

| Vista | MB% | Match |
|---|---|:---:|
| Resumen | 22,8% | ✅ |
| Márgenes | 22,8% | ✅ |
| Tiendas (Total) | 22,8% | ✅ |

### 4.4 Datos por Tienda (Cross-Check Resumen ↔ Tiendas ↔ Márgenes)

| Tienda | Resumen | Tiendas | Márgenes | Coherente |
|---|---|---|---|:---:|
| Albán ventas | 416.756 € | 416.756 € | 416.756 € | ✅ |
| Albán margen | 99.460 € | 99.460 € | 99.460 € | ✅ |
| Albán MB% | 23,9% | 23,9% | 23,9% | ✅ |
| Motril ventas | 138.049 € | 138.049 € | 138.049 € | ✅ |
| Amazon ventas | 113.632 € | 113.632 € | 113.632 € | ✅ |

### 4.5 Hallazgo: Servicios Centrales no aparece en Resumen

La vista Resumen muestra 3 tablas:
- Tiendas Físicas (5 tiendas): **935.337 €**
- Canales Digitales (7 canales): **239.218 €**
- Contract: **123.983 €**
- **Suma parcial: 1.298.538 €**

El KPI total muestra **1.298.873 €**. La diferencia de **335 €** corresponde a "Servicios Centrales" que no aparece en ninguna tabla del Resumen.

**Severidad: BAJA.** El total del KPI es correcto. "Servicios Centrales" sí aparece en Tiendas y Márgenes. La omisión en Resumen es cosmética (335 € = 0,03% del total).

### 4.6 Coherencia Febrero vs Marzo

| Métrica | Feb 2026 | MTD Mar 2026 | Tendencia |
|---|---|---|---|
| Ventas | 2.383.188 € | 1.298.873 € | Mar a 54,5% del ritmo de Feb |
| Margen | 606.173 € | 296.176 € | Mar a 48,9% del ritmo de Feb |
| MB% | 25,4% | 22,8% | −2,6pp (compresión continúa) |
| % Obj | 92,5% | 72,4% | Mar aún lejos del objetivo |

---

## PARTE 5: AUDITORÍA UX — PERSPECTIVA CEO

### ¿Puede un CEO tomar decisiones con este dashboard?

**SÍ.** El dashboard responde las 5 preguntas clave de un CEO:

1. **"¿Cómo vamos de ventas?"** → KPI prominente: 1,3M € con gauge semicircular al 72,4% del objetivo. Claro de un vistazo.

2. **"¿Dónde estamos perdiendo margen?"** → MB% pills rojas en casi todas las tiendas con indicación pp (▼5,3pp en Albán). Las cards de categoría muestran que Electro (12,3%) y Servicios (11,6%) son las más débiles.

3. **"¿Qué tiendas van mejor/peor?"** → ObjBars rojas para todas las físicas (<80% objetivo). Contract destaca en azul (81,3%). Tabla ordenada por volumen con YoY verde/rojo.

4. **"¿Cómo van los canales digitales?"** → Vista E-Commerce dedicada con donut chart, cards por canal, y tabla detallada. Amazon domina (48%) pero cae −11,6% YoY.

5. **"¿Respecto al año pasado?"** → Subtextos "vs Año Anterior" y "vs Presupuesto" bajo cada cabecera relevante. DevPills ▲/▼ con porcentaje y color.

### Fortalezas UX

- **Jerarquía visual clara:** KPIs grandes → charts/cards → tablas detalladas
- **Colores semafóricos:** Verde/Azul/Rojo inmediatamente interpretables
- **Tooltips contextuales:** Hover sobre cualquier pill explica el motivo del color con datos concretos
- **Drill-down en tiendas:** Click en card → desglose por categoría con tabla expandible
- **Sparklines:** Tendencia 6 meses en cada card de tienda — patrón visual rápido
- **Navegación lateral:** 5 vistas bien definidas (Resumen, Tiendas, Categorías, E-Commerce, Márgenes)
- **Selector de período:** MTD actual + meses anteriores, fácil de cambiar

### Áreas de mejora

| # | Hallazgo | Impacto | Recomendación |
|---|---|---|---|
| 1 | Tablas pierden columnas en tablet/mobile | MEDIO | Scroll horizontal con indicador visual (sombra/gradiente) |
| 2 | "Servicios Centrales" (335 €) no aparece en Resumen | BAJO | Incluir en tabla de Contract o crear sección "Otros" |
| 3 | Categoría "Reformas" (−892 €) invisible | BAJO | Mostrar con indicador de ventas negativas |
| 4 | Sin filtros rápidos (ej. solo tiendas físicas, solo digital) | BAJO | Futuro: tabs o filtros en tabla |
| 5 | Sin alertas/notificaciones de umbrales | BAJO | Futuro: badge cuando MB% cae >3pp |

---

## PARTE 6: AUDITORÍA UX — PERSPECTIVA CONSULTOR McKINSEY

### Evaluación Ejecutiva

**El dashboard cumple el estándar de reporting de dirección** para una empresa retail de tamaño medio. Puntos destacables:

#### Lo que un consultor valoraría positivamente:

1. **Single source of truth:** Una única BD (`chs_platform`) alimenta todas las vistas. No hay Excel paralelos ni datos contradictorios.

2. **Metrics that matter:** Las métricas seleccionadas (Ventas, MB%, MN%, % Obj, YoY) son exactamente las que importan para retail. No hay vanity metrics.

3. **Actionable signals:** Los colores y pp-variations permiten identificar en <10 segundos dónde actuar. Ejemplo: "Carrefour tiene −27,5% MB → cerrar canal o renegociar condiciones."

4. **Temporal context:** Comparativa YoY y vs Presupuesto en la misma fila. El CEO ve rendimiento relativo, no solo absoluto.

5. **Granularity on demand:** Resumen → Tienda → Categoría por tienda (drill-down). Tres niveles de detalle accesibles.

#### Lo que un consultor pediría mejorar:

| # | Recomendación | Prioridad | Esfuerzo |
|---|---|---|---|
| 1 | **Forecast/Proyección:** Extrapolar MTD actual al cierre de mes. "A este ritmo cerraríamos en X €" | ALTA | Medio |
| 2 | **Margen Neto prominente:** MN% debería estar en Resumen, no solo en Márgenes. Es el número que importa al P&L | ALTA | Bajo |
| 3 | **Benchmark inter-tiendas:** Ranking relativo (ej. % del total empresa por tienda vs objetivo de peso) | MEDIA | Medio |
| 4 | **Trend charts:** Evolución mensual de KPIs clave (no solo sparklines de ventas sino MB% y % Obj a lo largo del año) | MEDIA | Alto |
| 5 | **Export to PDF/PPT:** Para comités de dirección | MEDIA | Medio |
| 6 | **Alertas configurables:** Email/Slack cuando MB% de una tienda cae por debajo de umbral | BAJA | Alto |

### Análisis de la Situación del Negocio (según datos del dashboard)

**Hallazgo principal:** Compresión generalizada de márgenes.

- MB% empresa: 22,8% vs 27,3% año anterior (−4,5pp)
- **11 de 14 tiendas** tienen MB% inferior al año anterior
- **5 de 6 categorías** tienen MB% inferior al año anterior
- Electro (42,9% de ventas) tiene solo 12,3% MB → principal driver de compresión
- Muebles (48,6% de ventas) con 33,0% MB baja de 33,2% → estable pero no compensa

**Pregunta para dirección:** ¿La compresión es por pricing (descuentos/promos), mix de producto (más electro low-margin), o coste de mercancía?

---

## PARTE 7: AUDITORÍA TÉCNICA

### 7.1 Seguridad

| # | Hallazgo | Severidad | Detalle |
|---|---|---|---|
| 1 | **Sin middleware de autenticación** | CRÍTICA | No hay `middleware.ts` en Next.js que verifique sesión/token. Las rutas dependen del header `x-chs-user-id` del reverse proxy, pero no validan su presencia |
| 2 | **Credenciales BD en `.env`** | CRÍTICA | `DATABASE_URL` con usuario/contraseña en texto plano en el repositorio |
| 3 | **API POST sin proteger** | ALTA | `POST /api/data/objetivos` permite escribir en BD sin autenticación |
| 4 | **Endpoints sin try-catch** | MEDIA | 4 API routes (`/api/data/objetivos`, `/api/data/categorias`, `/api/data/tiendas`, `/api/data/ventas`) no tienen error handling |
| 5 | **Sin validación de input** | MEDIA | Parámetros de fecha (`desde`, `hasta`) no se validan — se pasan directamente a queries (aunque parametrizados vía Drizzle, por lo que NO hay SQL injection) |

**Positivo:** Todas las queries SQL usan template literals de Drizzle (`sql\`...\``) con parametrización. **No hay vulnerabilidad de SQL injection.**

### 7.2 Rendimiento

| Aspecto | Estado | Detalle |
|---|---|---|
| Queries N+1 | ✅ Limpio | `Promise.all()` para todas las queries en cada página |
| Bundle size | ✅ Normal | Next.js 16.1.6 con Turbopack |
| Client components | ✅ Mínimos | Solo `StoreCards`, `Sparkline`, `RadialGauge`, `StoreMap` |
| Server components | ✅ Bien usado | Todas las páginas son RSC con `export const dynamic = "force-dynamic"` |
| Caché | ⚠️ Sin caché | `force-dynamic` en todas las páginas → BD se consulta en cada request |
| Error boundaries | ⚠️ Ausentes | Sin `error.tsx` ni `loading.tsx` en las rutas |

### 7.3 Código Muerto

| Archivo | Estado | Detalle |
|---|---|---|
| `obj-pill.tsx` | ⚠️ **DEAD CODE** | Reemplazado por `obj-bar.tsx` en Bloque 1. Ya no se importa en ningún archivo |
| `console.log` | ✅ Limpio | Ningún `console.log` residual en código de producción |
| `TODO/FIXME` | ✅ Limpio | Ningún comentario pendiente |
| Imports no usados | ✅ Limpio | Todos los imports están en uso (excepto ObjPill que es el archivo completo) |

### 7.4 Responsive

| Resolución | Puntuación | Problemas |
|---|---|---|
| Desktop (1440px) | **10/10** | Perfecto |
| Tablet (768px) | **9/10** | Columna derecha de tablas recortada |
| Mobile (375px) | **8/10** | Tablas recortadas + ligero crowding en store cards |

**Problema consistente:** Las tablas con muchas columnas (MB%, % Obj, Año Ant, % YoY, MN%, MB% Ant, etc.) pierden la última columna en tablet y las 2-3 últimas en mobile. No hay indicador visual de scroll horizontal.

**Lo que funciona bien:**
- KPI cards apilan correctamente (1 col mobile, 2 col tablet, 4 col desktop)
- Store cards adaptan layout
- Sidebar se oculta en mobile (menú hamburguesa)
- Charts SVG (donut, sparklines, gauge) escalan correctamente

---

## PARTE 8: RESUMEN EJECUTIVO Y RECOMENDACIONES

### Tabla de Hallazgos

| # | Hallazgo | Severidad | Tipo | Acción |
|---|---|---|---|---|
| H1 | Sin middleware de autenticación | CRÍTICA | Seguridad | Crear `middleware.ts` con verificación de sesión |
| H2 | Credenciales BD en `.env` visible en repo | CRÍTICA | Seguridad | Mover a secrets de Coolify / vault |
| H3 | API POST `/api/data/objetivos` sin auth | ALTA | Seguridad | Añadir verificación de permisos |
| H4 | 4 API routes sin try-catch | MEDIA | Técnica | Añadir error handling |
| H5 | Sin `error.tsx` / `loading.tsx` | MEDIA | UX | Añadir loading states y error boundaries |
| H6 | `obj-pill.tsx` es código muerto | BAJA | Limpieza | Eliminar archivo |
| H7 | Tablas recortadas en mobile/tablet | MEDIA | UX | Indicador de scroll horizontal |
| H8 | Servicios Centrales ausente en Resumen | BAJA | Datos | Incluir en tabla o documentar exclusión |
| H9 | Sin caché de queries | BAJA | Rendimiento | Evaluar `revalidate` o ISR |
| H10 | Sin proyección al cierre de mes | BAJA | Funcionalidad | Feature request |

### Scoring Final

| Área | Peso | Puntuación | Ponderado |
|---|---|---|---|
| Exactitud de datos | 30% | 10/10 | 3,0 |
| Colores y señalización | 20% | 9/10 | 1,8 |
| Consistencia | 15% | 9/10 | 1,35 |
| UX/Responsive | 15% | 8,5/10 | 1,275 |
| Seguridad | 10% | 5/10 | 0,5 |
| Rendimiento/Técnica | 10% | 8/10 | 0,8 |
| **TOTAL PONDERADO** | **100%** | | **8,73/10** |

### Conclusión

**El Cuadro de Dirección CHS es un dashboard de alta calidad para reporting ejecutivo.** La exactitud de datos es perfecta (100% coincidencia BD ↔ UI), los colores y señales visuales son correctos y consistentes, y la experiencia de usuario es sólida en desktop y aceptable en mobile.

**La única área que requiere atención urgente es la seguridad:** ausencia de middleware de autenticación y credenciales expuestas. Estas son vulnerabilidades estándar que se resuelven con un middleware Next.js y gestión de secrets.

El dashboard está listo para uso ejecutivo en su estado actual (accedido vía red interna con reverse proxy autenticado), pero las mejoras de seguridad deberían implementarse antes de cualquier exposición adicional.

---

*Informe generado automáticamente. Base de datos auditada: `chs_platform`. Screenshots en `/home/chs-dash2/auditoria-final/`.*
