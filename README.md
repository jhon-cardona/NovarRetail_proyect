# Explorando factores de comportamiento en NovaRetail+

Análisis exploratorio y correlacional de 15,000 clientes de NovaRetail+, una plataforma de comercio electrónico en Latinoamérica, para identificar qué factores del comportamiento del cliente se asocian con el ingreso anual que genera para la empresa.

> **Importante:** este proyecto es correlacional. Todos los resultados describen asociaciones entre variables y **no prueban causalidad**.

## Pregunta de negocio

¿Qué factores del comportamiento del cliente están más fuertemente asociados con el ingreso anual generado?

## Datos

Archivo: `novaretail_comportamiento_clientes_2024.csv` (15,000 registros, 12 columnas, sin valores nulos).

| Columna | Descripción |
|---|---|
| `id_cliente` | Identificador único del cliente |
| `edad` | Edad del cliente |
| `nivel_ingreso` | Ingreso anual estimado del cliente |
| `visitas_mes` | Visitas a la app o sitio durante el mes |
| `compras_mes` | Compras realizadas en el mes |
| `gasto_publicidad_dirigida` | Gasto en anuncios asignado al usuario |
| `satisfaccion` | Calificación de satisfacción (1 a 5) |
| `miembro_premium` | Suscripción premium (1) o no (0) |
| `abandono` | Abandonó la plataforma (1) o no (0) |
| `tipo_dispositivo` | Móvil, escritorio o tablet |
| `region` | Norte, sur, oeste o este |
| `ingreso_anual` | Ingreso anual generado por el cliente para la empresa (variable principal) |

## Metodología

1. **Carga y exploración:** revisión de tipos de datos, nulos y rangos. Se convirtió `edad` de `float64` a `int64`, tras verificar que no tenía decimales.
2. **Diagnóstico por tipo de variable:** estadísticas descriptivas, binarias y categóricas.
3. **Visualización:** heatmap de correlaciones, scatterplot general (pairplot) y scatterplots de pares clave.
4. **Coeficientes según el tipo de variable:**
   - **Pearson** (relaciones lineales) y **Spearman** (relaciones monótonas) para variables numéricas.
   - **Punto-biserial** para variable binaria vs numérica.
   - **V de Cramér** para variables categóricas, con una variable auxiliar `genera_ingreso` (1 si `ingreso_anual` > 0) creada solo para esa prueba.
5. **Interpretación de negocio:** cada hallazgo incluye evidencia visual, evidencia numérica, interpretación no causal, lo que no podemos afirmar y su implicación.

## Hallazgos principales

1. **Compras e ingreso casi se mueven juntos.** `compras_mes` vs `ingreso_anual`: Pearson = 0.967 y Spearman = 0.967. Los 4,567 clientes (30.45%) con 0 compras son exactamente los mismos con ingreso 0, por lo que esta asociación debe leerse con cautela y no como un hallazgo independiente.
2. **Visitas y gasto en publicidad dirigida muestran asociaciones menores.** `visitas_mes` vs `ingreso_anual`: 0.337 (Pearson) y 0.321 (Spearman). `gasto_publicidad_dirigida` vs `ingreso_anual`: 0.197 y 0.185. Visitas y gasto también se relacionan entre sí (0.58).
3. **Premium se asocia con mayor ingreso y menor abandono, con magnitud modesta.** Ingreso medio de 44.58 en premium frente a 35.30 en no premium (r punto-biserial = 0.093). Abandono de 4.4% en premium frente a 16.8% en no premium (V de Cramér = 0.120).
4. **Sin asociación relevante con el ingreso:** `edad` (0.017), `nivel_ingreso` (0.017), `satisfaccion` (0.056), `tipo_dispositivo`, `region` y `abandono`.

## Limitaciones

- Correlación no implica causalidad.
- Los datos son de un solo corte, sin información temporal. `compras_mes` y `visitas_mes` son mensuales, mientras que `ingreso_anual` es anual.
- Los coeficientes evalúan relaciones de a pares (lineales o monótonas) y no combinaciones ni interacciones entre variables.
- El gasto en publicidad es una asignación por usuario cuyo criterio se desconoce, por lo que no puede leerse como un experimento.
- Con 15,000 registros, asociaciones muy pequeñas resultan estadísticamente significativas; se priorizó la magnitud de los coeficientes.

## Próximos pasos

- Repetir las correlaciones solo con clientes que compran (`compras_mes` > 0).
- Análisis multivariado (por ejemplo, regresión múltiple) y revisión de la relación conjunta entre visitas y gasto.
- Pruebas controladas (A/B) para evaluar el gasto en publicidad dirigida y los incentivos a la membresía premium.
- Incorporar datos por periodo o cohorte.

## Cómo ejecutarlo

1. Clona el repositorio e instala las dependencias:

   ```bash
   pip install pandas numpy matplotlib seaborn scipy jupyter
   ```

2. Coloca el archivo `novaretail_comportamiento_clientes_2024.csv` en el repositorio y ajusta la ruta en la celda de carga del notebook, que por defecto apunta a `/datasets/novaretail_comportamiento_clientes_2024.csv`:

   ```python
   df = pd.read_csv("novaretail_comportamiento_clientes_2024.csv")
   ```

3. Abre `NovaRetail_Proyecto_Completo.ipynb` y ejecuta todas las celdas de arriba abajo.

## Estructura del repositorio

```
├── NovaRetail_Proyecto_Completo.ipynb   # Análisis completo
└── README.md
```

## Tecnologías

Python, pandas, NumPy, Matplotlib, Seaborn y SciPy.
└── README.md
Tecnologías

Python, pandas, NumPy, Matplotlib, Seaborn y SciPy.
