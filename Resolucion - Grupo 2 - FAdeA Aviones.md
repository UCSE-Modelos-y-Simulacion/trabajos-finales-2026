# RESOLUCIÓN ESPERADA: CASO 2 — FÁBRICA ARGENTINA DE AVIONES (FAdeA - GRUPO 2)

> **Aclaración institucional:** El presente caso constituye una situación hipotética elaborada exclusivamente con fines académicos. Los personajes, datos operativos, costos, decisiones y situaciones descriptas no representan información real de las organizaciones mencionadas.

## 1. Identificación del Sistema y sus 5 Elementos
* **Tipo de sistema:** Red abierta de dos estaciones de colas estocásticas en serie (tándem):
  * **Conservación de Flujo:** No existen rechazos ni retrabajos; toda pieza conformada en prensas pasa a la estación de inspección en régimen permanente.
  * **Etapa 1 (Prensado):** Sistema multicanal en paralelo $M/M/10$ (disciplina FIFO, capacidad ilimitada).
  * **Etapa 2 (Inspección):** Sistema monocanal $M/M/1$ en la operación normal y Propuesta 1 (según el Teorema de Burke, la salida de un sistema $M/M/c$ estable es un proceso de Poisson con la misma tasa media $\lambda = 7\text{ piezas/hora}$). En la Propuesta 2 se transforma en un modelo $M/G/1$.
* **Entidades:** Láminas de metal aeronáutico procesadas y conformadas en secciones de ala.
* **Atributos:** Hora de ingreso a planta, estación actual, tiempo de conformado requerido, resultado de la inspección.
* **Actividades:** Espera en cola de prensas, conformado en prensa hidráulica, espera en cola de inspección, revisión de calidad en el puesto de inspección.
* **Estados del sistema:** Cantidad de prensas activas ($0 \le n_1 \le 10$), longitud de cola en prensas ($L_{q1}$), estado del inspector (Libre/Ocupado) y longitud de cola en inspección ($L_{q2}$).
* **Eventos:** Arribo de lámina metálica al sector de prensado, fin de conformado y arribo a inspección, inicio de inspección, fin de inspección y despacho de la pieza.

---

## 2. Resolución Analítica y Económica por Escenario

### a) Operación Normal (Línea Base: $M/M/10 \to M/M/1$)
1. **Etapa 1 — Prensas Hidráulicas ($M/M/10, \lambda = 7/\text{h}, \mu_1 = 1/\text{h}, c = 10$):**
   * Intensidad de tráfico: $r_1 = \frac{7}{1} = 7,0\text{ Erlangs}$.
   * Factor de utilización: $\rho_1 = \frac{7}{10} = 0,7000$ ($70\%$).
   * Probabilidad de sistema vacío: $P_0 = 0,0009$ ($0,09\%$).
   * Probabilidad de esperar: $P(W_{q1} > 0) = 0,2217$ ($22,17\%$).
   * Longitud de cola: $L_{q1} = 0,5174\text{ láminas}$.
   * **WIP en Prensas ($L_1$):** $L_1 = L_{q1} + r_1 = 0,5174 + 7,0 = \mathbf{7,5174\text{ láminas}}$.
2. **Etapa 2 — Estación de Inspección ($M/M/1, \lambda = 7/\text{h}, \mu_2 = 8/\text{h}, c = 1$):**
   * Factor de utilización: $\rho_2 = \frac{7}{8} = 0,8750$ ($87,5\%$).
   * Longitud de cola: $L_{q2} = \frac{\rho_2^2}{1 - \rho_2} = \frac{0,875^2}{0,125} = 6,1250\text{ secciones}$.
   * **WIP en Inspección ($L_2$):** $L_2 = \frac{\rho_2}{1 - \rho_2} = \frac{0,875}{0,125} = \mathbf{7,0000\text{ secciones}}$.
3. **Observación puntual vs. Promedios estacionarios y Percentiles Empíricos:**
   * La observación de la recorrida (5 láminas en prensas y 13 secciones en inspección) corresponde a una realización estocástica puntual en un instante de tiempo.
   * **Prensas ($M/M/10$):** Con $L_1 = 7,5174$ y $c=10$, la probabilidad de encontrar 5 o más piezas en el sector de prensas ($P(N_1 \ge 5)$) es muy alta (superior al $85\%$), ubicando a la observación de 5 piezas muy cerca de la mediana de la distribución.
   * **Inspección ($M/M/1$):** Con $\rho_2 = 0,875$, la probabilidad de encontrar exactamente $n=13$ unidades en inspección es $P(n=13) = (1 - \rho)\rho^{13} = 0,125 \cdot (0,875)^{13} \approx 0,0221$ ($2,2\%$), y la probabilidad acumulada de tener 13 o más es $P(N_2 \ge 13) = \rho^{13} = (0,875)^{13} \approx 0,1768$ ($17,7\%$, correspondiente al percentil 82,3 de la distribución empírica).
   * En simulación, los alumnos deben registrar la distribución empírica de estados y ubicar estas realizaciones para corroborar que no implican desbalance estructural sino variabilidad propia de procesos estocásticos con alta utilización.
4. **Tratamiento Teórico Independiente vs. Red de Simulación en Serie:**
   * Analíticamente, el Teorema de Burke garantiza que la salida de un sistema $M/M/10$ en régimen estacionario es un proceso de Poisson puro, lo que permite desacoplar ambas estaciones y resolver la segunda como un $M/M/1$ clásico con llegadas Poisson independientes.
   * En la simulación en FlexSim, el flujo entre estaciones es explícito: las piezas terminadas en prensas ingresan a la cola de inspección según los instantes reales de salida. En corridas finitas o ante variaciones en el tiempo de servicio (como la Gamma de la Propuesta 2), las ligeras discrepancias entre la teoría y la simulación provienen de la correlación temporal de las salidas de una estación con múltiples procesadores frente al supuesto analítico de independencia estocástica total.
4. **WIP Total y Costo Horario Total (Base):**
   * **WIP Total ($L_1 + L_2$):** $7,5174 + 7,0000 = \mathbf{14,5174\text{ unidades}}$.
   * Costo de WIP ($\text{USD } 8,00/\text{u}\cdot\text{h}$): $14,5174 \times 8 = \text{USD } 116,14/\text{h}$ ($\text{USD } 60,14$ en prensas + $\text{USD } 56,00$ en inspección).
   * Costo de las 10 prensas ($10 \times \text{USD } 7,00$): $\text{USD } 70,00/\text{h}$.
   * Salario de la inspectora: $\text{USD } 17,00/\text{h}$.
   * **Costo Total Esperado por Hora: $\mathbf{\text{USD } 203,14/\text{hora}}$**.

---

### b) Evaluación de la Propuesta 1 (Ajuste de Potencia de las Prensas)
* **Contraste crítico de la hipótesis gerencial:**
  Reducir la potencia de las prensas para "mantener mejor el paso" con inspección es una falacia en estado estacionario. Por el principio de conservación de flujo, mientras el sector de prensas sea estable ($\rho_1 < 1$), su tasa media de salida es idéntica a su tasa media de entrada ($\lambda = 7\text{ unidades/hora}$). En consecuencia, la tasa de arribos a inspección no disminuye y el inventario esperado en inspección permanece inalterable en **$L_2 = 7,0000\text{ secciones}$**.
* **Sub-escenario 1A — Reducir potencia ($t_{s1} = 1,2\text{ h} \implies \mu_1 = 0,8333/\text{h}$, costo $\text{USD } 6,50/\text{h}$ por prensa):**
  * $r_1 = 7 \times 1,2 = 8,40\text{ Erlangs}$, $\rho_1 = \frac{8,40}{10} = 0,8400$ ($84\%$).
  * $L_{q1} = 2,6474 \implies \mathbf{L_1 = 11,0474\text{ láminas}}$ (el inventario en prensas aumenta $+3,53\text{ láminas}$).
  * Costo WIP Total: $(11,0474 + 7,0000) \times 8 = \text{USD } 144,38/\text{h}$.
  * Costo Prensas: $10 \times \text{USD } 6,50 = \text{USD } 65,00/\text{h}$ | Inspectora: $\text{USD } 17,00/\text{h}$.
  * **Costo Total (Bajar poder): $\mathbf{\text{USD } 226,38/\text{hora}}$** (¡Empeora en **+$\text{USD } 23,24/\text{hora}$** frente a la línea base!).
* **Sub-escenario 1B — Incrementar potencia al máximo ($t_{s1} = 0,8\text{ h} \implies \mu_1 = 1,25/\text{h}$, costo $\text{USD } 7,50/\text{h}$ por prensa):**
  * $r_1 = 7 \times 0,8 = 5,60\text{ Erlangs}$, $\rho_1 = \frac{5,60}{10} = 0,5600$ ($56\%$).
  * $L_{q1} = 0,0884 \implies \mathbf{L_1 = 5,6884\text{ láminas}}$ (se reduce el inventario en prensas en $1,83\text{ unidades}$).
  * Costo WIP Total: $(5,6884 + 7,0000) \times 8 = \text{USD } 101,51/\text{h}$.
  * Costo Prensas: $10 \times \text{USD } 7,50 = \text{USD } 75,00/\text{h}$ | Inspectora: $\text{USD } 17,00/\text{h}$.
  * **Costo Total (Subir poder): $\mathbf{\text{USD } 193,51/\text{hora}}$** (Ahorro neto de **$\text{USD } 9,63/\text{hora}$** respecto de la línea base, pues el ahorro de WIP de $\$14,63/\text{h}$ supera el sobrecosto de prensas de $\$5,00/\text{h}$).

---

### c) Evaluación de la Propuesta 2 (Recurso Alternativo con Tiempo $M/G/1$)
* **Parámetros del servicio de inspección:**
  * $\lambda = 7/\text{h}$
  * Tiempo medio: $E[S] = 7,2\text{ min} = 0,12\text{ horas}$ ($\mu_2 = 8,3333/\text{h}$)
  * Desvío estándar: $\sigma = 5,0\text{ min} = \frac{5}{60}\text{ h} = 0,08333\text{ horas}$
* **Fórmula de Pollaczek-Khinchine ($M/G/1$):**
  $$\rho_2 = \lambda \cdot E[S] = 7 \times 0,12 = 0,8400 \quad (84\%)$$
  $$L_{q2} = \frac{\lambda^2 \sigma^2 + \rho_2^2}{2(1 - \rho_2)} = \frac{49 \times (0,08333)^2 + 0,84^2}{2(1 - 0,84)} = \frac{0,34028 + 0,7056}{0,32} = \mathbf{3,2684\text{ secciones}}$$
  $$L_2 = L_{q2} + \rho_2 = 3,2684 + 0,8400 = \mathbf{4,1084\text{ secciones}}$$
* **Parametrización de la distribución Gamma en FlexSim:**
  * En minutos: media $\mu_s = 7,2$, desvío $\sigma = 5,0$.
  * Parámetro de forma: $k = \alpha = \frac{\mu_s^2}{\sigma^2} = \frac{7,2^2}{5,0^2} = \frac{51,84}{25} = \mathbf{2,0736}$.
  * Parámetro de escala: $\theta = \beta = \frac{\sigma^2}{\mu_s} = \frac{25}{7,2} \approx \mathbf{3,4722\text{ minutos}}$.
  * En FlexSim: configurar el Processor de inspección con `distribution: Gamma`, `location: 0`, `shape: 2.0736`, `scale: 3.4722`.
* **Costo Total por Hora (Propuesta 2 con Prensas Normales):**
  * WIP Total: $L_1 + L_2 = 7,5174 + 4,1084 = \mathbf{11,6258\text{ unidades}}$.
  * Costo WIP: $11,6258 \times 8 = \text{USD } 93,01/\text{h}$.
  * Costo Prensas: $\text{USD } 70,00/\text{h}$ | Costo Recurso Alternativo: $\text{USD } 19,00/\text{h}$.
  * **Costo Total (Propuesta 2): $\mathbf{\text{USD } 182,01/\text{hora}}$** (Ahorro neto de **$\text{USD } 21,13/\text{hora}$** frente a la línea base).

---

### d) Recomendación Integral de Ingeniería (Selección del Escenario Óptimo)
* **Criterio de optimalidad:** Selección del escenario que minimice el Costo Total Esperado por Hora, condicionado a la estabilidad matemática de ambas estaciones ($\rho_1 < 1, \rho_2 < 1$).
* **Incorporación de dos inspectores en paralelo ($M/M/2, c_2 = 2$ a $\text{USD } 17/\text{h}$ cada uno = $\text{USD } 34/\text{h}$):**
  * Intensidad de tráfico: $r_2 = \frac{7}{8} = 0,875\text{ Erlangs}$.
  * Factor de utilización: $\rho_2 = \frac{0,875}{2} = 0,4375$ ($43,75\%$).
  * $P_0 = 0,3913$, $P(W_{q2} > 0) = 0,1678$, $L_{q2} = 0,2071\text{ secciones}$.
  * **WIP en Inspección:** $L_2 = 0,2071 + 0,875 = \mathbf{1,0821\text{ secciones}}$ (¡reducción de $7,00$ a $1,08$!).
* **Tabla Comparativa Consolidada de Alternativas:**

| Escenario Evaluado | WIP Prensas ($L_1$) | WIP Insp. ($L_2$) | WIP Total | Costo WIP | Costo Prensas | Costo Insp. | **Costo Total ($\text{USD/h}$)** | Ahorro vs. Base |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **a) Línea Base (Normal)** | $7,5174$ | $7,0000$ | $14,5174$ | $\$116,14$ | $\$70,00$ | $\$17,00$ | **$\$203,14$** | — |
| **b1) Prop. 1: Prensas $1,2\text{ h}$** | $11,0474$ | $7,0000$ | $18,0474$ | $\$144,38$ | $\$65,00$ | $\$17,00$ | **$\$226,38$** | $-\$23,24$ (Pérdida) |
| **b2) Contraste: Prensas $0,8\text{ h}$** | $5,6884$ | $7,0000$ | $12,6884$ | $\$101,51$ | $\$75,00$ | $\$17,00$ | **$\$193,51$** | $+\$9,63$ |
| **c) Prop. 2: Recurso $M/G/1$** | $7,5174$ | $4,1084$ | $11,6258$ | $\$93,01$ | $\$70,00$ | $\$19,00$ | **$\$182,01$** | $+\$21,13$ |
| **d1) 2 Inspectores ($M/M/2$)** | $7,5174$ | $1,0821$ | $8,5995$ | $\$68,80$ | $\$70,00$ | $\$34,00$ | **$\$172,80$** | $+\$30,34$ |
| **d2) Óptimo Global: $0,8\text{ h}$ + 2 Insp.** | **$5,6884$** | **$1,0821$** | **$6,7705$** | **$\$54,16$** | **$\$75,00$** | **$\$34,00$** | **$\$163,16$** | **$+\$39,98$** |

* **Análisis de conflicto de objetivos (Costo vs. WIP):**
  No existe conflicto entre minimizar el costo total y minimizar el WIP. La alternativa **d2 (Prensas a máxima potencia $0,8\text{ h}$ combinadas con 2 inspectores en paralelo)** logra simultáneamente el **menor costo horario total ($\text{USD } 163,16/\text{h}$)** y el **menor nivel de inventario en proceso ($6,77\text{ unidades}$)**, reduciendo el WIP global en un **$53,4\%$** y generando un ahorro neto anual de casi **$\text{USD } 350.000$** (operando en régimen 24/7).

---

## 3. Pautas para la Simulación en FlexSim y Procesamiento en Excel
* **Condiciones experimentales:** Mantener constantes los parámetros generales (tasa de arribos $\lambda = 7$, distribución de llegadas de Poisson, costos unitarios de WIP y recursos), modificando únicamente la potencia de prensas y la configuración de inspección de cada escenario. Para favorecer la comparación entre alternativas, se recomienda utilizar semillas comunes entre escenarios equivalentes, manteniendo una semilla diferente para cada réplica.
* **Warm-up y réplicas independientes:**
  * Realizar al menos **10 réplicas independientes de 120 horas efectivas de recolección cada una**, adicionales al período de calentamiento determinado, utilizando semillas aleatorias independientes (*Random Streams*) en cada corrida.
  * Determinar y justificar el período de calentamiento (*warm-up*) mediante inspección gráfica (método de Welch, análisis de medias móviles o series temporales acumuladas) o criterio analítico, excluyéndolo de la recolección de estadísticas de inventario en proceso (WIP), tiempos de ciclo y cálculo de costos.
  * Si se decidiera no utilizar calentamiento, deberá demostrarse cuantitativamente que las condiciones iniciales representan adecuadamente el estado estacionario.
* **Cálculo del Costo Total de WIP:**
  * El Costo Total Esperado por Hora simulado deberá calcularse utilizando únicamente las observaciones recolectadas después del período de calentamiento.
* **Procesamiento estadístico en Excel:**
  * Para cada indicador de desempeño ($\rho_1$, $\rho_2$, $L_1$, $L_2$, tiempos de espera $W_q$, tiempos en sistema $W$ y costo total), calcular la media muestral $\bar{X}$, el desvío estándar $s$ y el intervalo de confianza del 95%:
    $$\text{IC}_{95\%} = \left[\bar{X} - t_{9;\,0,025}\frac{s}{\sqrt{10}}, \; \bar{X} + t_{9;\,0,025}\frac{s}{\sqrt{10}}\right]$$
  * **Verificación de utilizaciones:** Contrastar la utilización media teórica de las 10 prensas ($\rho_1 = 70\%$) con la utilización empírica registrada en cada una de las 10 prensas en FlexSim, verificando si la asignación de piezas produce cargas de trabajo balanceadas entre servidores homogéneos.
