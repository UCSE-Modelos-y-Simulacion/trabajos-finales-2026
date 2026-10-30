# RESOLUCIÓN ESPERADA: CASO 1 — OPERACIONES DE CHECK-IN DE UNA COMPAÑÍA AÉREA (GRUPO 1)

> **Aclaración institucional:** El presente caso constituye una situación hipotética elaborada exclusivamente con fines académicos. Los personajes, datos operativos, costos, decisiones y situaciones descriptas no representan información real de las organizaciones mencionadas.

## 1. Identificación del Sistema y sus 5 Elementos
* **Tipo de sistema:** Sistema estocástico de líneas de espera multicanal en paralelo $M/M/4$ (disciplina FIFO, capacidad ilimitada) en el escenario base; en las alternativas se divide en subsistemas independientes ($M/M/1 + M/M/3$ y $M/M/1 + M/M/2 + M/M/1$).
* **Entidades:** Pasajeros que arriban a la terminal aeroportuaria para efectuar el despacho y registro (*check-in*).
* **Atributos:** Hora de llegada, tipo de trámite (registro regular vs. caso especial con problemas de reserva o tarifa), tipo de equipaje (despacho a bodega vs. solo bolso de mano en cabina).
* **Actividades:** Espera en la fila común o sala de butacas, atención en el mostrador (verificación documental, pesaje y despacho de valijas, emisión de tarjeta de embarque).
* **Estados del sistema:** Estado de cada agente en su mostrador (Libre / Ocupado), cantidad de pasajeros en fila de espera ($L_q$), ocupación de butacas.
* **Eventos:** Arribo de un pasajero al sector de mostradores, inicio de atención por parte de un agente (comienzo de servicio), conclusión del despacho y partida del pasajero hacia pre-embarque (fin de servicio).
* **Convención terminológica:**
  * *Agente:* Recurso humano asignado a la atención.
  * *Mostrador:* Infraestructura física donde se procesa al pasajero.
  * *Servidor:* Concepto matemático en la formulación de líneas de espera.

---

## 2. Resolución Analítica y Resultados de Simulación

### 1ro. Diagnóstico de la Operación Actual Unificada ($M/M/4$)
* **Parámetros de entrada:**
  * Tasa de arribo: $\lambda = 132\text{ pas/h}$ ($2,2\text{ pas/min}$)
  * Tiempo medio de servicio: $t_s = 90\text{ seg} = 1,5\text{ min} \implies \mu = 40\text{ pas/h}$ por agente ($0,6667\text{ pas/min}$)
  * Cantidad de servidores: $c = 4\text{ mostradores atendidos por 4 agentes}$.
* **Intensidad de tráfico y factor de utilización:**
  $$r = \frac{\lambda}{\mu} = \frac{132}{40} = 3,30\text{ Erlangs} \quad \implies \quad \rho = \frac{r}{c} = \frac{3,30}{4} = \mathbf{0,8250 \quad (82,50\%)}$$
* **Probabilidad de espera teórica vs. simulación:**
  * Probabilidad de sistema vacío: $P_0 = 0,0227$ ($2,27\%$)
  * Probabilidad teórica de espera: $P(W_q > 0) = P(n \ge 4) = \mathbf{0,6422 \quad (64,22\%)}$
  * *Equivalente empírico en FlexSim:* Se mide como la **proporción estimada de pasajeros cuyo tiempo de espera en cola sea mayor que cero** respecto del total procesado.
* **Indicadores operativos esperados:**
  * Pasajeros promedio esperando en fila: **$L_q = 3,0273\text{ pasajeros}$**
  * Pasajeros promedio en el sector: **$L = L_q + r = 3,0273 + 3,30 = 6,3273\text{ pasajeros}$**
  * Tiempo promedio de espera en fila: **$W_q = \frac{L_q}{\lambda} = \frac{3,0273}{132} \times 60 = \mathbf{1,3761\text{ minutos}}$ ($82,56\text{ seg}$)**
  * Tiempo promedio total en el sector: **$W = W_q + t_s = 1,3761 + 1,50 = \mathbf{2,8761\text{ minutos}}$ ($172,56\text{ seg}$)**

---

### 2do. Propuesta de Separación en Dos Filas Especializadas

#### A. Fila 1: Pasajeros con Problemas Especiales ($M/M/1, c = 1$)
* **Parámetros:** $\lambda_1 = 12\text{ pas/h}$, $t_{s1} = 3\text{ min} \implies \mu_1 = 20\text{ pas/h}$, $c_1 = 1\text{ agente}$.
* **Resultados:**
  * $\rho_1 = \frac{12}{20} = 0,6000$ ($60\%$), $P_0 = 0,4000$, $P(W_{q1} > 0) = 0,6000$.
  * **$L_{q1} = \frac{\rho_1^2}{1 - \rho_1} = \frac{0,36}{0,40} = \mathbf{0,9000\text{ pasajeros}}$**
  * **$L_1 = 1,5000\text{ pasajeros}$**
  * **$W_{q1} = \frac{0,90}{12} \times 60 = \mathbf{4,5000\text{ minutos}}$**
  * **$W_1 = 4,50 + 3,00 = \mathbf{7,5000\text{ minutos}}$**

#### B. Fila 2: Pasajeros de Check-in Regular ($M/M/3, c = 3$)
* **Parámetros:** $\lambda_2 = 120\text{ pas/h}$, $t_{s2} = 80\text{ seg} = 1,3333\text{ min} \implies \mu_2 = 45\text{ pas/h}$, $c_2 = 3\text{ agentes}$.
* **Resultados:**
  * $r_2 = \frac{120}{45} = 2,6667\text{ Erlangs} \implies \rho_2 = \frac{2,6667}{3} = \mathbf{0,8889 \quad (88,89\%)}$.
  * $P_0 = 0,0280$, $P(W_{q2} > 0) = 0,7975$ ($79,75\%$).
  * **$L_{q2} = \mathbf{6,3801\text{ pasajeros}}$**
  * **$L_2 = 6,3801 + 2,6667 = \mathbf{9,0467\text{ pasajeros}}$**
  * **$W_{q2} = \frac{6,3801}{120} \times 60 = \mathbf{3,1900\text{ minutos}}$**
  * **$W_2 = 3,1900 + 1,3333 = \mathbf{4,5234\text{ minutos}}$**

#### C. Comparación Ponderada Global y Análisis Causal
* **Ponderación global por proporción de arribos:**
  * Proporción de casos especiales: $p_1 = \frac{12}{132} \approx 9,09\%$
  * Proporción de check-in regular: $p_2 = \frac{120}{132} \approx 90,91\%$
  * **Tiempo medio de espera global ponderado:**
    $$W_{q,\text{global}} = \frac{12}{132} W_{q1} + \frac{120}{132} W_{q2} = \frac{12}{132}(4,50) + \frac{120}{132}(3,19) = 0,4091 + 2,9000 = \mathbf{3,3091\text{ minutos}}$$
    *(En el sistema unificado $M/M/4$ era de $1,3761\text{ min}$: ¡la espera media global sube un $+140\%$!).*
  * **Longitud total en cola:** $L_{q,\text{total}} = L_{q1} + L_{q2} = 0,9000 + 6,3801 = \mathbf{7,2801\text{ pasajeros}}$ (frente a $3,0273$ de la fila única).
* **Distinción causal mediante Escenarios de Control:**
  Para aislar el efecto de la pérdida de flexibilidad (*resource pooling*) del efecto del cambio en las tasas de servicio ($\mu$), se analiza:
  * **Escenario de Control A (Fraccionamiento puro con servicio homogéneo):** Si se dividen los servidores ($1\text{ agente}$ y $3\text{ agentes}$) manteniendo para ambos el tiempo de servicio base de $90\text{ seg}$ ($\mu = 40\text{ pas/h}$), el grupo de $3\text{ agentes}$ recibiría $\lambda = 120\text{ pas/h}$, alcanzando $\rho = 120 / (3 \times 40) = 1,00$ (sistema inestable y saturado). Esto demuestra que la separación física es inviable sin la mayor rapidez en el check-in regular.
  * **Escenario de Control B (Fila única con tiempos heterogéneos $M/G/4$):** Si se mantiene una sola fila común pero cada pasajero tarda según su tipo ($12/132$ tardan $3\text{ min}$ y $120/132$ tardan $80\text{ seg}$), el tiempo medio es $\bar{t}_s = \frac{12}{132}(180) + \frac{120}{132}(80) = 89,09\text{ seg}$ ($1,4848\text{ min}$), reduciendo la espera media a $W_q \approx 1,35\text{ min}$.
  * **Conclusión causal:** La separación de filas degrada el rendimiento global debido al fraccionamiento de servidores, a pesar de que el servicio regular es más rápido individualmente.

---

### 3ro. Subdivisión del Check-in Regular (Bodega vs. Carry-on)
* **Demanda y tiempos de atención de las subcolas:**
  * Subcola A (Bodega): $\lambda_A = 54\text{ pas/h}$, $t_{sA} = 96\text{ seg} = 1,6\text{ min} \implies \mu_A = 37,5\text{ pas/h}$.
    Intensidad de tráfico: $r_A = \frac{54}{37,5} = \mathbf{1,44\text{ Erlangs}}$.
  * Subcola B (Cabina): $\lambda_B = 66\text{ pas/h}$, $t_{sB} = 48\text{ seg} = 0,8\text{ min} \implies \mu_B = 75\text{ pas/h}$.
    Intensidad de tráfico: $r_B = \frac{66}{75} = \mathbf{0,88\text{ Erlangs}}$.
* **Única asignación entera factible para estabilidad:**
  * Para que la Subcola A no colapse, se requiere obligatoriamente $c_A > r_A = 1,44 \implies c_A \ge 2$.
  * Dado que la dotación total es de $3\text{ agentes}$, la única partición entera matemáticamente estable es:
    $$\mathbf{c_A = 2\text{ agentes para Bodega}} \quad \text{y} \quad \mathbf{c_B = 1\text{ agente para Cabina}}$$
  * *(Si se asignara $c_A = 1$ y $c_B = 2$, la Subcola A tendría $\rho_A = 1,44 > 1$ y su fila crecería indefinidamente).*
* **Indicadores operativos de las subcolas:**
  * **Subcola A ($M/M/2, c_A = 2$):** $\rho_A = \frac{1,44}{2} = 0,7200$, $P_0 = 0,1628$, $P(W_{qA} > 0) = 0,6028$.
    $L_{qA} = \mathbf{1,5500\text{ pas}}$, $W_{qA} = \mathbf{1,7223\text{ minutos}}$.
  * **Subcola B ($M/M/1, c_B = 1$):** $\rho_B = 0,8800$ ($88\%$), $P_0 = 0,1200$, $P(W_{qB} > 0) = 0,8800$.
    $L_{qB} = \frac{0,88^2}{1 - 0,88} = \mathbf{6,4533\text{ pas}}$, $W_{qB} = \frac{6,4533}{66} \times 60 = \mathbf{5,8667\text{ minutos}}$.
* **Distinción pedagógica entre estabilidad matemática y calidad de servicio:**
  Aunque la asignación $2 + 1$ cumple estrictamente la condición de estabilidad $\rho < 1$, la Subcola B opera con $\rho = 88\%$ en un único servidor, lo que genera esperas inaceptables de casi $6\text{ minutos}$. La congestión total de las 3 colas combinadas suma $L_q = 0,90 + 1,55 + 6,45 = \mathbf{8,90\text{ pasajeros en espera}}$.

---

### 4to. Diseño de Capacidad de Sala de Espera (Dimensionamiento de Butacas)
* **Selección del escenario óptimo:**
  El dimensionamiento debe realizarse sobre el **Escenario 1 (Fila Única $M/M/4$)**, por ser el más eficiente, robusto y con menor tiempo de espera promedio ($W_q = 1,38\text{ min}$).
* **Dimensionamiento en Sala Común vs. Sectores Separados:**
  * **Opción A (Sala de Espera Común - Recomendada):** Si los pasajeros comparten una única sala de butacas con turnero unificado, el dimensionamiento se realiza con la distribución estacionaria de la cola total.
  * **Opción B (Sectores Separados por Tipo de Trámite):** Si se asignan sectores de butacas independientes:
    * *Sector Casos Especiales ($M/M/1, \rho_1 = 0,60$):*
      $$P(L_{q1} > B_1) = \rho_1^{B_1+1} \le 0,05 \implies B_1 \ge \frac{\ln(0,05)}{\ln(0,60)} - 1 = \frac{-2,9957}{-0,5108} - 1 \approx 4,86 \implies \mathbf{B_1 = 5\text{ butacas}} \quad (B_1^{99\%} = 8\text{ butacas})$$
    * *Sector Check-in Regular ($M/M/3, \rho_2 = 0,8889, P(W_q > 0) = 0,7975$):*
      $$0,7975 \cdot (0,8889)^{B_2} \le 0,05 \implies B_2 \ge \frac{\ln(0,05 / 0,7975)}{\ln(0,8889)} = \frac{-2,7695}{-0,1178} \approx 23,51 \implies \mathbf{B_2 = 24\text{ butacas}} \quad (B_2^{99\%} = 37\text{ butacas})$$
    * *Total acumulado en sectores separados:* **$29\text{ butacas}$** al $95\%$ y **$45\text{ butacas}$** al $99\%$. Al compartimentar la sala, la capacidad requerida aumenta de $14$ a $29\text{ butacas}$ (+107%) debido a que no se aprovecha la variabilidad compensada de la sala común.
* **Fundamentación matemática mediante percentiles de la distribución estacionaria (Sala Común):**
  En un sistema $M/M/c$ con $\rho < 1$, la probabilidad de que haya más de $B$ pasajeros esperando en la cola (es decir, $n > c + B$ pasajeros en el sistema) es:
  $$P(L_q > B) = P(W_q > 0) \cdot \rho^B$$
  Para asegurar que a lo sumo una fracción $\alpha$ de los pasajeros encuentre la sala llena:
  $$P(W_q > 0) \cdot \rho^B \le \alpha \implies B \ge \frac{\ln(\alpha / P(W_q > 0))}{\ln(\rho)}$$
  Con $P(W_q > 0) = 0,6422$ y $\rho = 0,8250$:
  * **Percentil 95 ($\alpha = 0,05$):**
    $$0,6422 \cdot (0,8250)^B \le 0,05 \implies B \ge \frac{\ln(0,05 / 0,6422)}{\ln(0,8250)} = \frac{-2,553}{ -0,1924} \approx 13,27 \implies \mathbf{B = 14\text{ butacas}}$$
    *(Si se formula como $P(L_q \ge B+1) \le 0,05$, da $13\text{ a } 14\text{ butacas}$).*
  * **Percentil 99 ($\alpha = 0,01$):**
    $$0,6422 \cdot (0,8250)^B \le 0,01 \implies B \ge \frac{\ln(0,01 / 0,6422)}{\ln(0,8250)} = \frac{-4,162}{-0,1924} \approx 21,63 \implies \mathbf{B = 22\text{ butacas}}$$
* **Distinción entre percentiles y niveles de confianza:**
  Un percentil del 95 % o 99 % representa la cobertura de la distribución acumulada de la variable aleatoria $L_q$, mientras que un nivel de confianza del 95 % refiere al intervalo estadístico donde se encuentra la media muestral estimada.
* **Rol del *Maximum Content* en FlexSim:**
  En una corrida de 120 horas (~15.840 pasajeros), el estadístico `Maximum Content` de la cola correspondiente en FlexSim registrará picos muestrales extremos (habitualmente entre 25 y 35 pasajeros). Dicho valor debe informarse como **dato complementario de cota extrema** y no como único criterio de dimensionamiento, debido a su sensibilidad a la duración de la corrida y a la semilla estocástica.

---

## 3. Pautas para la Simulación en FlexSim y Procesamiento en Excel
* **Condiciones experimentales:** Mantener constantes las condiciones experimentales entre escenarios, modificando únicamente la estructura de colas y los parámetros asignados a cada caso.
* **Warm-up y horizonte de simulación:**
  * Realizar una corrida de simulación de **120 horas efectivas de recolección de datos** en FlexSim para cada escenario.
  * Determinar y justificar el período de calentamiento (*warm-up*) previo para garantizar el estado estacionario, excluyéndolo de la recolección de métricas.
  * Si un grupo decidiera no utilizar calentamiento, deberá demostrar que iniciar desde tiempo cero con la cola vacía no introduce un sesgo significativo frente a las 120 horas de horizonte simulado.
* **Procesamiento de datos en Excel:**
  * Para cada indicador operativo del sistema ($\rho, W_q, W, L_q, L$), registrar los valores simulados en FlexSim y contrastarlos cuantitativamente con los valores teóricos calculados analíticamente en la planilla Excel.
  * Verificar si la utilización individual de los servidores en FlexSim es equivalente bajo la hipótesis de servidores homogéneos (verificando reglas de ruteo como *Round Robin if Available* o *First Available* balanceado).
