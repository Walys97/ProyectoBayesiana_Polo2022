# Proyecto final – Análisis Bayesiano de Datos

**Modelamiento espacial bayesiano de las desigualdades en la letalidad por COVID-19.
Replicación de Polo et al. (2022)**

Walys Vera Herrera, Pregrado en Estadística, Universidad de Antioquia.
Prof. Johanna Marcela Orozco Castañeda.

## Contenido

| Archivo | Descripción |
|---|---|
| `Proyecto_Final_Bayesiana_Final.pdf` | Informe final. |
| `Presentacion_Proyecto_Bayesiana.pdf` | Diapositivas de la exposición. |
| `Proyecto_Bayesiana_Polo2022.ipynb` | Cuaderno con todo el análisis, ejecutado y con sus salidas. |
| `datos_polo2022.csv` | Datos de los 33 departamentos, transcritos de las Tablas 1 y 2 de Polo et al. (2022). |
| `Gráficos/` | Figuras generadas por el cuaderno (mapas, posteriores y riesgo por departamento). |

## Variables de `datos_polo2022.csv`

- `codigo`, `departamento`: código DANE y nombre.
- `area_km2`, `poblacion`, `casos`: información descriptiva de la Tabla 1.
- `Y`: muertes por COVID-19 (16 mar – 4 oct 2020).
- `E`: muertes esperadas con la letalidad nacional.
- `CFR`: letalidad bruta. `SCFR`: letalidad estandarizada (Y/E).
- `HMPI`, `EMPI`, `WMPI`, `DMPI`, `YMPI`: dimensiones del IPM (salud, educación, trabajo, vivienda, niñez y juventud).

## Cómo reproducir los resultados

1. Python 3 con `numpy`, `pandas`, `scipy`, `matplotlib` y `statsmodels`.
   `geopandas` es opcional: solo se usa para los mapas y para verificar la matriz de vecindad.
   ```
   pip install numpy pandas scipy matplotlib statsmodels geopandas
   ```
2. Abrir `Proyecto_Bayesiana_Polo2022.ipynb` (Jupyter, VS Code o Google Colab) y ejecutar las celdas en orden.
3. El mapa de departamentos se descarga de internet dentro del cuaderno. Si no hay conexión, el resto del análisis funciona igual; solo se omiten los mapas.

## Orden del análisis en el cuaderno

1. Datos y verificación de totales (28 882 muertes).
2. Matriz de vecindad `W` por contigüidad, verificada con el mapa (simétrica; San Andrés sin vecinos).
3. GLM Poisson frecuentista y sobredispersión.
4. Índice de Moran de la letalidad y de los residuos (10 000 permutaciones).
5. Poisson bayesiano sin efectos espaciales.
6. Modelo CAR de Leroux con Metropolis dentro de Gibbs, programado desde cero:
   3 cadenas × 300 000 iteraciones, burn-in 50 000, thinning 250.
7. Convergencia (trazas, R-hat, tamaño efectivo, Geweke).
8. Riesgos relativos, comparación de modelos (DIC) y sensibilidad a la priori de τ².
9. Riesgo por departamento, figuras y mapas.

## Notas

- Las cadenas usan semillas fijas, así que los resultados son reproducibles.
- La parte del MCMC tarda varios minutos; el cuaderno se entrega ya ejecutado para consultar los resultados sin volver a correrlo.
- La priori de β es N(0, 10⁵ I), valor por defecto de CARBayes, porque el artículo no reporta sus hiperparámetros.
