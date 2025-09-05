# Sprint 8 
## 4. AED (Empresas & Barrios) 
## 5. Prueba de Hipótesis (Sábados Lluviosos)

Este repositorio contiene dos cuadernos de trabajo (Jupyter Notebooks) con un flujo **limpio, modular y reproducible** para:
1) **Análisis Exploratorio de Datos (AED)** de viajes por **empresas** y **barrios**.
2) **Prueba de hipótesis** sobre la **duración de viajes** en **sábados** bajo condiciones **lluvioso vs sin lluvia**.

> Enfoque: validaciones explícitas, tipado consistente, reglas realistas de limpieza, visualizaciones robustas y reporte ejecutivo con tamaños de efecto, IC bootstrap e implicaciones prácticas.

---

## Estructura de archivos

```
.
├── notebooks/
│   ├── Sprint_8_Paso_4_AED_Empresas_y_Barrios.ipynb
│   └── Sprint_8_Paso_5_Hipotesis_Sabado_Lluvia.ipynb
├── data/                      # CSVs de entrada
├── README.md
├── requirements.txt
└── .gitignore
```

> **Nota:** Los archivos de datos no se versionan. Ubícalos en `data/` (o ajusta las rutas en los notebooks).
> En los cuadernos originales se usan rutas relativas como `../data/project_sql_result_*.csv`. Si mueves los notebooks a `notebooks/`, actualiza las rutas a `../data/` o modifica la constante de ruta según tu layout.

---

## Notebooks

### 1) `Sprint_8_Paso_4_AED_Empresas_y_Barrios.ipynb`
**Objetivo:** caracterizar la **distribución** y **concentración** de viajes en dos niveles:
- **Empresas** (`company_name`, `trips_amount`) — periodo 15–16 nov 2017
- **Barrios** (`dropoff_location_name`, `average_trips`) — promedio nov 2017

**Pipeline (resumen):**
- **Configuración base**: estilo gráfico global y utilidades (`fmt_miles_inter`, `str_miles_inter`, `fmt_pct`).
- **Carga y revisión**: lectura con `try/except`, `head()`, dimensiones.
- **Validación/Tipado/Limpieza**: strings estandarizados, conversión numérica con `errors='coerce'`, nulos, valores ≤0, duplicados por clave, redondeo para presentación.
- **EDA**: descriptivos, **sesgo/curtosis** (interpretados), **CV**. Histogramas.
- **Top 10**: ranking, participaciones, concentración acumulada (**Top10/Top5/Top1**). Barras horizontales anotadas.
- **Concentración (Pareto + métricas)**: **k(80%)**, **CR4/CR8**, **HHI** + gráfico de Pareto.
- **Conclusiones ejecutivas**: tabla comparativa y recomendaciones.

**Supuestos clave:**
- Columnas mínimas:  
  - Empresas: `company_name`, `trips_amount`  
  - Barrios: `dropoff_location_name`, `average_trips`
- Valores no negativos; conteos o promedios > 0 para métricas de concentración.

---

### 2) `Sprint_8_Paso_5_Hipotesis_Sabado_Lluvia.ipynb`
**Objetivo:** contrastar la **duración** de viajes (Loop --> O'Hare) en **sábados**, entre **lluvioso** y **sin lluvia**.

**Pipeline (resumen):**
- **Parámetros globales:** `ALFA`, `GRAF`, `EDA`, `NBOOT`, `RM_OUT`, `MIN_S`, `MAX_S`, `RNG_SEED`.
- **Renombrado/Normalización**: mapeos robustos de columnas y categorías de `clima` (`lluvioso` / `sin lluvia`).
- **Limpieza y validación**: nulos, duraciones inválidas (≤0), filtros realistas `[MIN_S, MAX_S]`, etiqueta de día.
- **AED orientado a decisiones**: métricas, conteos por hora/fecha/clima, distribución; **gráficas** (tendencias, barras, violín + box + jitter).
- **Utilidades estadísticas**: `pnorm` (Shapiro / D’Agostino), **g de Hedges**, **IC bootstrap** (medias/medianas).
- **Árbol de decisión de prueba**:  
  - Si normalidad en ambos --> **t de Student** (Welch si varianzas desiguales).  
  - Si no → **Mann–Whitney U**.  
  Reporte de tamaño de efecto (`g` o `r`) e IC (minutos).
- **Conclusiones ejecutivas**: diferencia puntual e IC (min), interpretación de magnitud y recomendaciones operativas.

**Archivos esperados (ejemplos):**
- `project_sql_result_07.csv` (contiene al menos `start_ts` / `inicio_ts`, `duracion_s`, `clima`)

---

## Requisitos

- **Python** ≥ 3.10
- Ver dependencias en `requirements.txt`

Instalación rápida (entorno virtual con `venv`):

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -U pip
pip install -r requirements.txt
```

Con **Conda**:

```bash
conda create -n sprint8 python=3.10 -y
conda activate sprint8
pip install -r requirements.txt
```

---

## Ejecución

1. Coloca los CSVs dentro de `data/` (o ajusta las rutas en los notebooks).  
2. Activa el entorno e instala dependencias.  
3. Arranca Jupyter:

```bash
jupyter lab
# o
jupyter notebook
```

4. Abre los cuadernos en `notebooks/` y ejecuta las celdas en orden.

> **Tip de rutas:** si el notebook vive en `notebooks/`, usa rutas relativas `../data/archivo.csv`. Si lo dejas en la raíz, usa `data/archivo.csv`.

---

## Reproducibilidad

- **Semillas**: `RNG_SEED` para jitter/bootstrapping.  
- **Bootstrap**: `NBOOT` (por defecto 9,999), IC percentílico 95% (2.5–97.5).  
- **Outliers**: `RM_OUT=True` activa IQR por grupo; los gráficos pueden usarse con/sin outliers.  
- **Rangos realistas**: `MIN_S` / `MAX_S` acotan duraciones plausibles.

---

## Salidas

- Visualizaciones: histogramas, barras con anotaciones, Pareto, violín + box + jitter.  
- Consola/Markdown: descriptivos, métricas de concentración, resultados de pruebas, **conclusiones** listas para informe.

---

## Solución de problemas (FAQ)

- **FileNotFoundError**: verifica rutas relativas (`../data/` vs `data/`).  
- **`.dt` accessor error**: asegúrate de parsear fechas con `parse_dates=[...]` y de que la columna sea `datetime64[ns]`.  
- **Gráficos no se ven**: usa un backend soportado (Jupyter clásico o JupyterLab + ipykernel actualizado).  
- **Valores raros/NaN**: revisa filtros `≤0`, nulos y mapeos de `clima`.  
- **Ambiente**: si usas conda, confirma que `scipy`, `numpy` y `matplotlib` sean compatibles con la versión de Python.

---

## Licencia

Sin licencia

---

## Autoría

Análisis y cuadernos preparados con mejores prácticas de **analítica aplicada** y **visualización reproducible**.
