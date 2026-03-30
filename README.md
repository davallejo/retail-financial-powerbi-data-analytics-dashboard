# 🏪 Dashboard de Seguimiento Financiero — OurMarket Store EC
**Power BI** 📈 + **Power Query** 🔄 + **DAX** 🧮 + **Azure Maps** 🗺️

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power%20Query-217346?style=for-the-badge&logo=microsoft&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Azure Maps](https://img.shields.io/badge/Azure%20Maps-0089D6?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)

Solución integral de Business Intelligence para el seguimiento y análisis financiero de una red de tiendas minoristas en Ecuador. Transforma datos transaccionales brutos en información estratégica para la toma de decisiones, permitiendo evaluar el rendimiento por unidad de negocio, optimizar comisiones y mitigar riesgos operativos.

**Datos transaccionales → ETL → Modelo estrella → DAX → Dashboard geoespacial interactivo**

Elaborado por: **Diego Vallejo**

---

## 🎯 Propósito y Problemática a Solucionar

La organización enfrentaba el reto de gestionar un volumen considerable de datos (**más de 4.700 transacciones anuales**) dispersos en múltiples fuentes. El proyecto resuelve cuatro problemas estructurales:

- 🗺️ **Falta de visibilidad regional:** Centralizar el desempeño de ventas por tienda y ubicación geográfica en el territorio ecuatoriano.
- ⚠️ **Concentración de riesgo:** Identificar si la facturación depende excesivamente de pocas tiendas o líneas de productos.
- 💰 **Ineficiencia en incentivos:** Automatizar el cálculo de comisiones por ventas para asegurar precisión financiera sin intervención manual.
- 🔍 **Detección de anomalías:** Identificar tiendas con bajo rendimiento para implementar planes de acción preventivos antes de que impacten la rentabilidad global.

> **Resultado:** Un ecosistema de datos que no solo automatiza el reporte financiero, sino que dota a la empresa de capacidad de análisis preventivo. Al identificar que pocas tiendas concentran la mayor parte de la facturación, el negocio puede diversificar sus estrategias y mitigar riesgos de forma proactiva.

---

## 🏗️ Arquitectura del Sistema

```
┌──────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  📋 FUENTES DE DATOS      🔄 POWER QUERY         📐 MODELADO        │
│  ────────────────────     ───────────────          ──────────        │
│  • Tabla: Tiendas     →   ETL: normalización  →   Esquema           │
│  • Tabla: Ventas          de tipos, limpieza       estrella          │
│  • Tabla: Productos       y coordenadas            (3 tablas)        │
│                                                                      │
│         ↓                      ↓                       ↓            │
│                                                                      │
│  🧮 MEDIDAS DAX           📊 DASHBOARD          🔍 SEGMENTACIÓN     │
│  ──────────────           ─────────────          ───────────────    │
│  Facturación Total,       6 visualizaciones      Por producto        │
│  Comisiones (5%),         + mapa de burbujas     con drill-down      │
│  % Participación          geoespacial            en tiempo real      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 🔍 KPIs Principales del Dashboard

```
┌──────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  K1 💰 FACTURACIÓN TOTAL    Ingreso bruto consolidado de todas      │
│                              las tiendas en el período analizado     │
│                                                                      │
│  K2 📦 TOTAL DE PEDIDOS     Volumen total de transacciones          │
│                              procesadas en el año                    │
│                                                                      │
│  K3 🤝 COMISIÓN TOTAL       5% sobre la facturación total,         │
│                              calculado automáticamente con DAX       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## ✨ Características Principales

- **Modelo Estrella (Star Schema):** Tres tablas vinculadas mediante llaves únicas (ID de producto y Código de tienda), permitiendo filtrado dinámico y preciso entre todas las dimensiones.
- **Mapa de Burbujas con Azure Maps:** Geolocalización exacta de cada tienda usando coordenadas decimales. El tamaño de la burbuja representa la facturación, permitiendo identificar la concentración regional del negocio de un vistazo.
- **Tabla con Formato Condicional:** Barras de datos integradas en la tabla de rendimiento por tienda que actúan como un embudo visual, destacando líderes de facturación frente a unidades que requieren intervención.
- **Cálculo Automático de Comisiones:** Medida DAX que calcula el 5% de comisión sobre la facturación total sin intervención manual, eliminando errores de cálculo.
- **Filtro por Producto (Drill-Down):** Slicer visual con imágenes de productos que permite analizar el comportamiento de cada categoría en tiempo real.
- **Análisis Temporal Mensual:** Gráfico de barras con la evolución de facturación mes a mes durante todo el año 2023.

---

## 🛠️ Implementación Técnica

### 🔄 Fase 1 — ETL con Power Query

```
Tres fuentes de datos (Tiendas, Ventas, Productos)
       ↓
  Power Query Editor
       ↓
  ┌───────────────────────────────────────────────────────────────┐
  │  Paso 1: Extracción de las 3 tablas fuente                    │
  │  Paso 2: Normalización de tipos — decimal fijo para montos    │
  │  Paso 3: Coordenadas decimales exactas para geolocalización   │
  │  Paso 4: Limpieza mediante reemplazo de valores erróneos      │
  │  Paso 5: Estandarización de códigos de tienda y producto      │
  │  Paso 6: Carga al modelo en esquema estrella                  │
  └───────────────────────────────────────────────────────────────┘
       ↓
  Modelo estrella listo para medidas DAX
```

### 📐 Fase 2 — Modelado de Datos (Esquema Estrella)

```
          ┌────────────────────┐
          │   Tabla: Tiendas   │
          │  ────────────────  │
          │  Código Tienda (PK)│
          │  Nombre Tienda     │
          │  Ciudad / Estado   │
          │  Latitud           │
          │  Longitud          │
          └────────┬───────────┘
                   │ 1
                   │
                   │ N
     ┌─────────────┴──────────────┐      ┌──────────────────────┐
     │     Tabla: Ventas (hechos) │      │   Tabla: Productos   │
     │  ─────────────────────     │      │  ──────────────────  │
     │  ID Venta (PK)             │ N  1 │  ID Producto (PK)    │
     │  Código Tienda (FK)        ├──────┤  Nombre Producto     │
     │  ID Producto (FK)          │      │  Categoría           │
     │  Fecha                     │      │  Imagen              │
     │  Facturación               │      └──────────────────────┘
     │  Cantidad                  │
     └────────────────────────────┘
```
<img width="930" height="434" alt="image" src="https://github.com/user-attachments/assets/4ad2d522-c235-416a-bac6-c7b673a2c57c" />

### 🧮 Fase 3 — Medidas DAX

```dax
-- Ingreso bruto consolidado
Facturación Total = SUM(Ventas[Facturación])

-- Comisión automática del 5% sobre facturación
Comisión = [Facturación Total] * 0.05

-- Participación porcentual de cada tienda en el total
% Participación =
    DIVIDE(
        [Facturación Total],
        CALCULATE([Facturación Total], ALL(Tiendas))
    )
```

---

## 📊 Visualizaciones Implementadas

| # | Tipo de Visual | Nombre en Dashboard | Descripción |
|---|---------------|---------------------|-------------|
| 1 | **Card (KPI)** | Facturación | Ingreso bruto total consolidado |
| 2 | **Card (KPI)** | Total de Pedidos | Volumen de transacciones del período |
| 3 | **Card (KPI)** | Comisión | 5% calculado automáticamente con DAX |
| 4 | **Mapa de Burbujas** | Dónde estamos | Geolocalización de tiendas en Ecuador con Azure Maps |
| 5 | **Gráfico de Barras** | Facturación por período | Evolución mensual de ventas con etiquetas de valor |
| 6 | **Tabla con formato condicional** | Ranking de tiendas | Facturación y % total por tienda con barras de datos |
| 7 | **Slicer visual** | Filtro por produto | Selector con imágenes de los productos de la marca |

---

## 🗄️ Modelo de Datos

### Tabla de Hechos: `RegistroVentas`

```
┌──────────────────────────────────────────────────────────────────┐
│                            Ventas                                │
├───────────────────────┬───────────┬──────────────────────────────┤
│ Campo                 │ Tipo      │ Descripción                  │
├───────────────────────┼───────────┼──────────────────────────────┤
│ ID Venta              │ Entero    │ Identificador único          │
│ Código Tienda         │ Texto     │ Llave foránea → Tiendas      │
│ ID Producto           │ Texto     │ Llave foránea → Productos    │
│ Fecha                 │ Fecha     │ Fecha de la transacción      │
│ Facturación           │ Decimal   │ Monto de la venta            │
│ Cantidad              │ Entero    │ Unidades vendidas            │
└───────────────────────┴───────────┴──────────────────────────────┘
```

### Tabla Dimensional: `Tiendas`

```
┌──────────────────────────────────────────────────────────────────┐
│                           Tiendas                                │
├───────────────────────┬───────────┬──────────────────────────────┤
│ Campo                 │ Tipo      │ Descripción                  │
├───────────────────────┼───────────┼──────────────────────────────┤
│ Código Tienda         │ Texto     │ Llave primaria (PK)          │
│ Nombre Tienda         │ Texto     │ Nombre de la sucursal        │
│ Ciudad                │ Texto     │ Ciudad de la tienda          │
│ Latitud               │ Decimal   │ Coordenada geodésica         │
│ Longitud              │ Decimal   │ Coordenada geodésica         │
└───────────────────────┴───────────┴──────────────────────────────┘
```

### Tabla Dimensional: `Productos`

```
┌──────────────────────────────────────────────────────────────────┐
│                          Productos                               │
├───────────────────────┬───────────┬──────────────────────────────┤
│ Campo                 │ Tipo      │ Descripción                  │
├───────────────────────┼───────────┼──────────────────────────────┤
│ ID Producto           │ Texto     │ Llave primaria (PK)          │
│ Nombre Producto       │ Texto     │ Descripción del artículo     │
│ Categoría             │ Texto     │ Línea del producto           │
│ Imagen                │ URL       │ Imagen para el slicer visual │
└───────────────────────┴───────────┴──────────────────────────────┘
```

### Catálogo de Productos

```
┌──────────────────────┬──────────────────────┐
│   Producto           │   Producto           │
├──────────────────────┼──────────────────────┤
│  Alicate de Presión  │  Juego de            │
│                      │  Destornilladores    │
│  Carretilla          │  Martillo            │
│  Reforzada           │                      │
│  Casco de Seguridad  │  Moto Sierra         │
│  Cinta Métrica       │  Sierra Circular     │
│                      │  Eléctrica           │
│  Escalera de         │  Taladro             │
│  Aluminio            │                      │
└──────────────────────┴──────────────────────┘
```

---

## 📊 Análisis de Datos

### 🔎 1. Visión General de KPIs

| Métrica | Valor |
|---------|-------|
| 💰 Facturación total | $42.185.172 |
| 📦 Total de pedidos | 4.800 |
| 🤝 Comisión total (5%) | $2.110.000 (aprox.) |

Con una facturación que supera los **$42 millones** y casi 5.000 pedidos procesados en el año, OurMarket Store EC mantiene un ticket promedio de **~$8.789 por pedido**, lo que es consistente con una tienda de herramientas y equipos de trabajo de valor medio-alto. La comisión automática de $2.11 millones representa un costo operativo significativo que justifica plenamente su automatización mediante DAX para evitar errores de cálculo manual.

---

### 🏪 2. Ranking de Tiendas por Facturación

| Posición | Tienda | Facturación | % del Total |
|----------|--------|-------------|-------------|
| 🥇 1° | Tienda Cuenca | $4.543.001 | 10,77% |
| 🥈 2° | Tienda Eloy Alfaro | $4.517.614 | 10,71% |
| 🥉 3° | Tienda Latacunga | $4.485.565 | 10,63% |
| 4° | Tienda La Garzota | $4.409.680 | 10,45% |
| 5° | Tienda Mall del Sol | $4.262.080 | 10,10% |
| 6° | Tienda Ibarra | $4.094.492 | 9,71% |
| 7° | Tienda Santo Domingo | $4.002.417 | 9,49% |
| — | **Total** | **$42.185.172** | **100%** |

#### 🧠 Interpretación

La distribución de facturación entre tiendas es **sorprendentemente equilibrada**, con la primera (Cuenca, 10.77%) y la última visible (Santo Domingo, 9.49%) separadas apenas por 1.28 puntos porcentuales. Este es un indicador muy positivo de gestión: la red no depende críticamente de una sola sucursal para sostener su facturación global.

**Cuenca lidera el ranking** con $4.54 millones, seguida muy de cerca por Eloy Alfaro y Latacunga. Las tres primeras tiendas concentran el 32.11% de la facturación total —una concentración razonable y saludable para una red de 7+ sucursales.

La distribución geográfica refuerza este equilibrio: hay presencia tanto en la Sierra (Cuenca, Latacunga, Ibarra, Santo Domingo) como en la Costa (Eloy Alfaro, La Garzota, Mall del Sol), lo que reduce la exposición a riesgos regionales.

> 💡 **Insight clave:** A diferencia de redes donde 2–3 tiendas concentran el 60–70% de la facturación, OurMarket Store EC presenta una distribución casi uniforme. Esto refleja una red madura y bien dimensionada, aunque también sugiere que **ninguna tienda está explotando su potencial máximo** de manera destacada — una oportunidad de diferenciación hacia arriba.

---

### 📅 3. Facturación por Período (Evolución Mensual 2023)

| Mes | Facturación |
|-----|-------------|
| Enero | $3.215 mil |
| Febrero | $3.415 mil |
| Marzo | $3.189 mil |
| Abril | $3.867 mil |
| Mayo | $3.731 mil |
| Junio | $3.421 mil |
| Julio | $3.498 mil |
| Agosto | $3.602 mil |
| Septiembre | $3.780 mil |
| Octubre | $3.269 mil |
| Noviembre | $4.030 mil |
| Diciembre | $3.167 mil |

#### 🧠 Interpretación

La evolución mensual muestra una facturación **relativamente estable** a lo largo del año, oscilando entre $3.167 millones (diciembre) y $4.030 millones (noviembre), una variación del 27% entre el mes más bajo y el más alto —rango moderado para el sector retail.

Se identifican tres momentos destacados:

**Pico principal — Noviembre ($4.030 mil):** El mes de mayor facturación del año, consistente con el efecto de promociones de fin de año y el inicio de la temporada de compras navideñas en Ecuador.

**Segundo pico — Abril ($3.867 mil):** Un incremento notable que podría asociarse a la temporada escolar del segundo período en Ecuador (inicio del régimen Sierra en mayo) y el aumento en la compra de herramientas y equipos previo a la temporada lluviosa en la Costa.

**Valle de cierre — Diciembre ($3.167 mil):** Contraintuitivamente, diciembre es el mes más bajo del año. Esto sugiere que el segmento de clientes de OurMarket Store EC (profesionales y constructores) reduce sus compras en el período festivo, a diferencia del retail de consumo masivo.

> 💡 **Insight estratégico:** La caída de diciembre representa una oportunidad no explotada. Campañas de fin de año orientadas a regalos de herramientas o kits de construcción podrían revertir este patrón y convertir el mes más débil en uno de los más fuertes.

---

### 🗺️ 4. Distribución Geográfica (Mapa de Burbujas)

El mapa de Azure Maps muestra la presencia de OurMarket Store EC distribuida en el territorio ecuatoriano con tiendas identificadas en las siguientes provincias:

- **Sierra Norte:** Imbabura (Ibarra), Pichincha (área Quito)
- **Sierra Centro:** Cotopaxi (Latacunga), Tungurahua (Ambato)
- **Sierra Sur:** Azuay (Cuenca)
- **Costa:** Guayas (La Garzota, Mall del Sol — Guayaquil), Manabí
- **Región Intermedia:** Santo Domingo de los Tsáchilas

#### 🧠 Interpretación

La red cubre los tres principales ejes de consumo del país: el corredor Sierra (Quito–Ambato–Cuenca), el centro comercial de la Costa (Guayaquil) y los mercados intermedios (Santo Domingo). Esta distribución reduce la exposición a shocks regionales —una caída de demanda en la Sierra no paraliza la operación total si la Costa mantiene su ritmo.

Las burbujas del mapa de tamaño homogéneo confirman el hallazgo del ranking: no existe una tienda visualmente dominante, lo que valida la distribución equilibrada de facturación encontrada en la tabla.

> 💡 **Oportunidad geográfica:** Regiones como El Oro (Machala), Loja y el norte de Pichincha —visibles en el mapa pero sin burbuja de tienda— representan mercados no atendidos que podrían ser candidatos naturales para la próxima expansión de la red.

---

### 🛒 5. Análisis por Línea de Producto

El slicer visual de productos permite el análisis individualizado de cada artículo. El catálogo comprende 10 productos de la línea de herramientas y construcción:

**Productos de alto valor unitario (potencial de facturación elevado):**
- Moto Sierra, Sierra Circular Eléctrica, Taladro

**Productos de volumen (alta rotación y menor margen):**
- Alicate de Presión, Martillo, Cinta Métrica, Juego de Destornilladores

**Productos de ticket medio:**
- Carretilla Reforzada, Casco de Seguridad, Escalera de Aluminio

#### 🧠 Interpretación

La mezcla de productos cubre tanto a profesionales de la construcción (Moto Sierra, Sierra Circular, Carretilla) como a usuarios técnicos y DIY (Alicate, Martillo, Destornilladores, Taladro). Esta diversificación dentro de una misma categoría (herramientas) reduce la dependencia de un solo segmento de cliente.

El filtro interactivo por producto permite a la gerencia responder preguntas críticas: ¿Qué tiendas venden más Taladros? ¿En qué meses se dispara la demanda de Cascos de Seguridad? ¿La Moto Sierra tiene mejor penetración en la Sierra o la Costa?

> 💡 **Insight de mix de productos:** Con solo 10 SKUs activos, la empresa mantiene un catálogo focalizado. Antes de expandir el portafolio, el dashboard permite identificar cuáles productos tienen mayor tracción por región para tomar decisiones de abastecimiento basadas en datos.

---

### 🔥 Conclusión General

```
┌──────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  🟢 FORTALEZAS IDENTIFICADAS       🟡 OPORTUNIDADES DETECTADAS      │
│  ────────────────────────────      ───────────────────────────       │
│  • Facturación equilibrada entre   • Diciembre es el mes más        │
│    todas las tiendas — sin           bajo: oportunidad de           │
│    dependencia crítica              campañas de fin de año          │
│  • Presencia geográfica            • Regiones sin cobertura         │
│    diversificada en Sierra           (Machala, Loja) como           │
│    y Costa                          candidatos de expansión         │
│  • $42 mill. con solo 7            • Ninguna tienda destaca         │
│    sucursales activas                marcadamente: oportunidad      │
│  • Comisiones automatizadas          de potenciar a las líderes     │
│    con DAX — cero errores          • Ticket promedio de ~$8.789     │
│    de cálculo manual                 sugiere margen para            │
│                                      productos premium              │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 💡 Recomendaciones Estratégicas

### 1. 📍 Plan de Expansión Geográfica Basado en Datos
El mapa de Azure Maps revela zonas de Ecuador sin presencia de la marca: El Oro (Machala), Loja y el norte de Esmeraldas. Con una facturación promedio de $6 millones por tienda al año, cada nueva sucursal bien ubicada puede aportar un incremento sustancial. Los datos del dashboard deben ser la base del estudio de viabilidad.

### 2. 📅 Estrategia de Activación para Diciembre
Diciembre es el mes con menor facturación ($3.167 mil), a pesar de ser el mes de mayor gasto en retail general. Diseñar una campaña específica de herramientas como regalo navideño o kits de inicio de proyecto para el año nuevo puede revertir este patrón y agregar entre $800 mil y $1 millón adicionales en el mes.

### 3. 🏆 Programa de Incentivos Diferenciado por Tienda
Con la distribución actual tan uniforme, no existe una tienda clara que inspire a las demás. Implementar un ranking visible mensualmente —posibilitado por el dashboard— y ligarlo a bonos adicionales puede crear competencia positiva y elevar el desempeño del grupo.

### 4. 🛒 Análisis de Rentabilidad por Producto
El siguiente nivel analítico es cruzar facturación con margen por producto. Una Moto Sierra puede vender menos unidades que un Martillo, pero aportar 10 veces más al margen bruto. Incorporar costos de producto al modelo permitiría priorizar el mix de ventas por tienda.

### 5. 🤝 Revisión Trimestral de la Estructura de Comisiones
El cálculo automático del 5% es el punto de partida. Con el dashboard operativo, la dirección puede evaluar si estructuras escalonadas (mayor comisión por superar metas) generan mayor motivación y retorno que el porcentaje fijo actual.

---

## 📂 Estructura del Proyecto

```
dashboard-ventas-sucursales/
│
├── 📊 Dashboard_Ventas_de_Sucursales.pbix   ← Archivo principal Power BI
│                                              (modelo + ETL + DAX + visuales)
│
├── 📁 data/Ventas_Ecuador_2024.xlsx
│   ├── Tiendas           ← Tabla dimensional de tiendas
│   ├── RegistroVentas    ← Tabla de hechos transaccional
│   └── Productos         ← Tabla dimensional de productos
│
└── 📄 README.md               ← Este archivo
```

---

## 🚀 Cómo usar el Dashboard

**Paso 1 — Prerrequisitos**

Descargar e instalar **Power BI Desktop** (gratuito):
👉 https://www.microsoft.com/es-es/power-platform/products/power-bi/desktop

**Paso 2 — Abrir el proyecto**

```
1. Descargar Dashboard_Ventas_de_Sucursales.pbix desde este repositorio
2. Abrir el archivo con Power BI Desktop
3. Si solicita credenciales, seleccionar "Anónimo" para fuentes locales
```

**Paso 3 — Actualizar la fuente de datos (opcional)**

```
Inicio → Transformar datos → Configuración del origen de datos
→ Apuntar a los archivos Excel en tu ruta local
→ Cerrar y aplicar
```

**Paso 4 — Interactuar con el Dashboard**

```
✅ Usar el slicer de productos (con imágenes) para filtrar
   la vista completa por línea de producto
✅ Clic en cualquier tienda de la tabla para ver solo su
   facturación mensual en el gráfico de barras
✅ Usar el mapa de burbujas para seleccionar una región
   geográfica y filtrar las métricas de esa zona
✅ Comparar el % Total entre tiendas para identificar
   líderes y rezagadas de forma inmediata
✅ Publicar en Power BI Service para compartir reportes
   mensuales automáticos con la dirección
```
<img width="884" height="495" alt="image" src="https://github.com/user-attachments/assets/9bdb127e-eb3a-4a88-8ec3-4092d49d6572" />

---

## 🛠️ Stack Tecnológico

| Herramienta | Rol en el proyecto |
|-------------|-------------------|
| 📈 **Power BI Desktop** | Modelado de datos y creación de visualizaciones interactivas |
| 🔄 **Power Query (M)** | ETL: limpieza, normalización y carga de las 3 tablas fuente |
| 🧮 **DAX** | Medidas de facturación, comisiones y participación porcentual |
| 🗺️ **Azure Maps** | Mapa de burbujas geoespacial con coordenadas de tiendas |
| 📋 **Excel** | Fuente de datos origen (Tiendas, Ventas, Productos) |
| 🪟 **Windows** | Sistema operativo compatible (10 / 11) |

---

## 📈 Posibles Extensiones

- 📊 **Análisis de Rentabilidad:** Incorporar costos de producto para calcular margen bruto por tienda y por SKU, pasando de análisis de ingresos a análisis de rentabilidad real.
- 🤖 **Forecasting de Ventas:** Integrar modelos de predicción con Python o la función de pronóstico nativa de Power BI para proyectar la facturación de los próximos 3–6 meses.
- 🎯 **Dashboard de Metas vs. Real:** Agregar una tabla de objetivos mensuales por tienda y construir visuales de semáforo (RAG) que indiquen el avance hacia la meta en tiempo real.
- 🌐 **Publicación en Power BI Service:** Configurar actualizaciones automáticas diarias y suscripciones de correo con el resumen ejecutivo para la dirección.
- 🔐 **Row-Level Security (RLS):** Seguridad por tienda para que cada gerente de sucursal acceda únicamente a sus propios datos sin ver el desempeño de las demás.
- 📱 **Vista Mobile:** Optimizar el layout para la app móvil de Power BI, permitiendo consultas de KPIs desde cualquier dispositivo durante visitas a sucursales.
- 💳 **Análisis de Ticket Promedio:** Incorporar la dimensión de número de ítems por pedido para identificar oportunidades de upselling y cross-selling por tienda.

---

## 🧰 Solución de Problemas Comunes

**❌ Error: No se puede abrir el archivo `.pbix`**
```
→ Asegúrate de tener Power BI Desktop instalado
→ Descarga la versión más reciente desde Microsoft Store o el sitio oficial
```

**❌ El mapa de Azure Maps no carga**
```
→ Azure Maps requiere conexión a internet activa para renderizar
→ Verificar que las columnas Latitud y Longitud estén categorizadas
  correctamente en las propiedades de columna del modelo
→ Columna → Herramientas de columna → Categoría de datos → Latitud / Longitud
```

**❌ Error: No se encuentran los archivos de datos**
```
→ Inicio → Transformar datos → Configuración del origen de datos
→ Cambiar origen → Navegar hasta la ubicación de los Excel en tu equipo
→ Verificar que los 3 archivos estén accesibles desde Power BI Desktop
```

**❌ Las imágenes del slicer de productos no aparecen**
```
→ Verificar que la columna de URLs de imagen esté categorizada como
  "URL de imagen" en las propiedades de columna
→ Columna → Herramientas de columna → Categoría de datos → URL de imagen
→ Confirmar que las URLs sean accesibles públicamente
```

**❌ La medida de Comisión devuelve un valor incorrecto**
```
→ Verificar que la columna Facturación esté en tipo "Número decimal fijo"
  y no como texto en Power Query
→ Revisar que la fórmula DAX referencie exactamente el nombre de la medida
  base: [Facturación Total] * 0.05
```

---

> Construido con 📈 Power BI + 🔄 Power Query + 🧮 DAX + 🗺️ Azure Maps
>
> Para uso educativo y demostración de soluciones de Business Intelligence aplicadas
> al seguimiento financiero de redes de retail minoristas
>
> Elaborado por: **Diego Vallejo**
