# Medida equivalente de martingala y valoración de opciones europeas sobre el EUR/USD

**Autor:** Luis Martínez Gonzales
**Asesor:** Freddy H. Marín Sánchez
**Curso:** Procesos Estocásticos II (CM0438), Universidad EAFIT, noviembre de 2026

Este proyecto modela la tasa de cambio dólares por euro (serie DEXUSEU de FRED) como un movimiento browniano geométrico (MBG):

$$dS_t = \mu S_t\,dt + \sigma S_t\,dB_t$$

El trabajo verifica las condiciones del modelo, estima sus parámetros, evalúa pronósticos sobre la muestra y valora 20 opciones call europeas. Las opciones se valoran bajo la medida equivalente de martingala con cuatro métodos: Black-Scholes, árbol binomial, Monte Carlo y diferencias finitas explícitas.

## Contenido

| Archivo | Descripción |
|---|---|
| `ProyectoFinalMBG_Entrega.mlx` | Live Script con todo el código, el texto explicativo y los resultados. |
| `ProyectoFinalMBG_Entrega.m` | El mismo Live Script en formato de texto plano de MATLAB (R2025a o posterior). Se puede leer en GitHub sin MATLAB. |
| `DEXUSEU.xlsx` | Datos de FRED: tasa de cambio diaria USD por EUR, consultada el 11 de noviembre de 2026. |
| `genhurst.m` | Exponente de Hurst generalizado (T. Aste). Función provista en el curso. |
| `fractalvol.m` | Dimensión fractal por conteo de cajas. Función provista en el curso. |
| `docs/Articulo_MartinezGonzales.pdf` | Artículo final en formato IEEE. |
| `docs/ProyectoFinalMBG_Entrega.pdf` | Live Script exportado a PDF, con el código y todos los resultados. |

## Estructura del Live Script

1. **Configuración.** Reúne todos los parámetros: ventana de 251 precios, Δt = 1/252, 10 000 trayectorias, semilla 2026, r = 0.05, vencimientos T = 1, 1/2, 1/3, 1/4 y percentiles 10, 30, 50, 70 y 90 para los precios de ejercicio.
2. **Parte A. Datos y condiciones del MBG.**
   - Carga y control de calidad de los datos, más estadística descriptiva.
   - Autocorrelación parcial, exponente de Hurst y dimensión fractal.
   - Pruebas de normalidad (Jarque-Bera, Kolmogorov-Smirnov, Anderson-Darling, Lilliefors), estacionariedad (ADF), varianza constante (ARCH, Ljung-Box sobre E²) e independencia (Ljung-Box), y volatilidad acumulada.
3. **Parte B. Estimación y pronósticos.** Estimación de μ y σ con errores estándar, y pronósticos inducido y simulado con 10 000 trayectorias cada uno. Las métricas son MAPE, MAE, RMSE, cobertura de la banda del 95 % y R². Al final se comparan los momentos simulados con los teóricos.
4. **Parte C. Valoración de opciones call europeas.**
   - Simulación fuera de muestra desde S₀ = 1.1154, de la que salen los precios de ejercicio K.
   - Valoración con Black-Scholes, árbol binomial (n = 1000), Monte Carlo (solución exacta, 1000 × 10 000 trayectorias) y diferencias finitas explícitas.
   - Cada método se valida primero con el ejemplo del curso y luego se estudia su convergencia.
5. **Comparación con Black-Scholes y conclusiones.** Tablas de error relativo de cada método frente a la fórmula cerrada.

## Cómo ejecutarlo

1. Clonar o descargar el repositorio y abrir la carpeta en MATLAB.
2. Abrir `ProyectoFinalMBG_Entrega.mlx`. El `.m` también sirve en R2025a o posterior.
3. Ejecutar todo con **Run All**. El script lee `DEXUSEU.xlsx` desde la carpeta actual y usa `genhurst.m` y `fractalvol.m`, que deben estar en la misma carpeta.

**Requisitos:**
- MATLAB, desarrollado y probado en R2025b.
- Statistics and Machine Learning Toolbox (`jbtest`, `kstest`, `adtest`, `lillietest`, `histfit`, `qqplot`, `normcdf`, …).
- Econometrics Toolbox (`adftest`, `archtest`, `lbqtest`, `parcorr`).
- Financial Toolbox es **opcional**. Solo se usa `blsprice` para contrastar la fórmula de Black-Scholes, y si no está instalado el script deja ese valor en NaN y sigue.

Las simulaciones usan semillas fijas, así que los precios, errores y métricas se reproducen exactamente. Los tiempos de cómputo, en cambio, cambian en cada corrida.

## Resultados principales

| Resultado | Valor |
|---|---|
| Parámetros estimados (anuales) | μ̂ = −0.0327, σ̂ = 0.0549 (IC 95 % de σ: [0.0501, 0.0597]) |
| MAPE medio, pronóstico inducido | 0.38 % (cobertura de la banda: 92.8 %) |
| MAPE medio, pronóstico simulado | 3.39 % (cobertura de la banda: 100 %) |
| Error relativo medio frente a BS, binomial | 0.0058 % (máximo 0.049 %) |
| Error relativo medio frente a BS, Monte Carlo | 0.0168 % (máximo 0.059 %; BS dentro del IC 95 % en las 20 opciones) |
| Error relativo medio frente a BS, diferencias finitas | 0.0039 % (máximo 0.020 %) |

El análisis completo y la discusión están en el [artículo](docs/Articulo_MartinezGonzales.pdf).

## Fuentes

- Datos: Board of Governors of the Federal Reserve System (US), *U.S. Dollars to Euro Spot Exchange Rate* [DEXUSEU], obtenidos de FRED, Federal Reserve Bank of St. Louis: <https://fred.stlouisfed.org/series/DEXUSEU>.
- Métodos: notas de clase y códigos del curso Procesos Estocásticos II (F. H. Marín, Universidad EAFIT).
- `genhurst.m`: T. Aste (2013). Método de T. Di Matteo et al., *Physica A* 324 (2003) 183–188.
- `fractalvol.m`: función provista en el material del curso. La autoría y las condiciones de uso son las indicadas en el encabezado del archivo.
