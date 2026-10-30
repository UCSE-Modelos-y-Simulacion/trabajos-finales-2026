# RESOLUCIÓN ESPERADA: CASO 3 — GUARDIA CENTRAL CLÍNICA SAN MARTÍN (GRUPO 3)

> **Aclaración institucional:** El presente caso constituye una situación hipotética elaborada exclusivamente con fines académicos. Los personajes, datos operativos, costos, decisiones y situaciones descriptas no representan información real de las organizaciones mencionadas.

## 1. Identificación del Sistema y sus 5 Elementos
* **Tipo de sistema:** Sistema estocástico de líneas de espera multicanal en paralelo $M/M/c$ con fila única gestionada por turnero electrónico (disciplina FIFO).
* **Entidades:** Pacientes que ingresan a la Guardia Central de Emergencias.
* **Atributos:** Hora de ingreso, nivel de gravedad clínica (triage), obra social/prepaga, tiempo requerido de atención médica.
* **Actividades:** Espera en la sala de guardia, evaluación clínica, estabilización y registro en historia clínica dentro del box médico.
* **Estados del sistema:** Estado de cada box médico (Libre / Ocupado), cantidad de pacientes aguardando en la sala de espera ($L_q$).
* **Eventos:** Arribo de un paciente a la guardia, llamado e ingreso del paciente al box médico (inicio de servicio), alta o derivación del paciente (fin de servicio).
* **Diferenciación conceptual entre Médicos, Boxes y Servidores:** A los efectos del modelo, cada box habilitado cuenta con exactamente un médico asignado durante toda la franja analizada. Por lo tanto, la cantidad de servidores $c$ en la formulación de líneas de espera coincide con la cantidad de boxes médicos operativos y con los médicos simultáneamente disponibles.

---

## 2. Resolución Analítica y Metodológica

### a) Diagnóstico de la Dotación Médica Actual ($M/M/c$)
* **Datos de entrada:**
  * Tasa media de arribos: $\lambda = 36\text{ pacientes/hora}$
  * Tiempo medio de atención en box médico: $t_s = 12\text{ min} \implies \mu = \frac{60}{12} = 5\text{ pacientes/hora}$ por médico.
* **Intensidad de tráfico y condición de estabilidad:**
  $$r = \frac{\lambda}{\mu} = \frac{36}{5} = 7,20\text{ Erlangs} \quad \implies \quad c_{\min} = \lceil 7,20 \rceil = 8\text{ boxes médicos para no saturar el sistema } (\rho < 1)$$
* **Evaluación de valores enteros factibles de $c$ y tabla de discrepancia:**
  Se evalúan los enteros estables ($c \ge 8$) y se calcula el error absoluto respecto de los valores observados en auditoría: $P_1^{\text{obs}} = 0,43$ y $P_2^{\text{obs}} = 0,28$:

  | Dotación ($c$) | $\rho$ | $P(W_q > 0)$ | $P(W_q > 3\text{ min})$ | $P(W_q \le 3\text{ min})$ | Error $|P(W_q>0) - 0,43|$ | Error $|P(W_q>3) - 0,28|$ | **Suma de Errores** |
  | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
  | **$c = 8$** | $90,00\%$ | $70,15\%$ | $57,44\%$ | $42,56\%$ | $|0,7015 - 0,43| = 0,2715$ | $|0,5744 - 0,28| = 0,2944$ | **$0,5659$** |
  | **$c = 9$** | **$80,00\%$** | **$43,22\%$** | **$27,56\%$** | **$72,44\%$** | **$|0,4322 - 0,43| = 0,0022$** | **$|0,2756 - 0,28| = 0,0044$** | **$0,0066$ (Mínimo)** |
  | **$c = 10$** | $72,00\%$ | $25,36\%$ | $12,59\%$ | $87,41\%$ | $|0,2536 - 0,43| = 0,1764$ | $|0,1259 - 0,28| = 0,1541$ | **$0,3305$** |

* **Justificación del criterio:**
  El criterio de mínima discrepancia conjunta (suma de errores absolutos $= 0,0066$) selecciona unívocamente a **$c = 9$**, reproduciendo de manera exacta los datos históricos.
* **Conclusión parte a):** La clínica opera actualmente con **$c = 9$ boxes médicos y 9 profesionales**.
  * Utilización media: $\rho = 80,00\%$
  * Probabilidad de esperar en sala: $P(W_q > 0) = 43,22\%$
  * Longitud media de cola en sala: $L_q = 1,7289\text{ pacientes}$ ($L = 8,9289\text{ pacientes}$)
  * Tiempo medio de espera en sala: $W_q = 2,8815\text{ minutos}$ ($W = 14,8815\text{ minutos}$)
  * Porcentaje con espera menor o igual que $3\text{ minutos}$: $P(W_q \le 3\text{ min}) = 1 - 0,2756 = \mathbf{72,44\%}$

---

### b) Cumplimiento del SLA del Ministerio de Salud ($95\%$ espera menor a $3\text{ minutos}$)
Evaluando $P(W_q \le t) = 1 - P(W_q > 0) \cdot e^{-c \mu (1 - \rho) t}$ para $t = 3\text{ min} = 0,05\text{ horas}$:

| Boxes Médicos ($c$) | Utilización ($\rho$) | $P(W_q > 0)$ | $L_q\text{ (pac)}$ | $W_q\text{ (min)}$ | $P(W_q > 3\text{ min})$ | **$P(W_q \le 3\text{ min})$** | ¿Cumple SLA $\ge 95\%$? |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **$c = 9$ (Actual)** | $80,00\%$ | $43,22\%$ | $1,7289$ | $2,8815$ | $27,56\%$ | **$72,44\%$** | No |
| **$c = 10$** | $72,00\%$ | $25,36\%$ | $0,6521$ | $1,0869$ | $12,59\%$ | **$87,41\%$** | No |
| **$c = 11$ (Uno menos)**| $65,45\%$ | $14,13\%$ | $0,2678$ | $0,4463$ | $5,47\%$ | **$94,53\%$** | Límite ($94,53\%$) |
| **$c = 12$ (Óptimo)** | $60,00\%$ | $7,47\%$ | $0,1120$ | $0,1867$ | $2,25\%$ | **$97,75\%$** | **Sí ($\ge 95\%$)** |

* **Resultado esperado:**
  * Para garantizar estrictamente el umbral del $95\%$, se requieren **$c = 12$ boxes médicos simultáneos** ($97,75\%$).
  * **Evaluación con un box médico menos ($c = 11$):** Se alcanza un **$94,53\%$**, quedando a tan solo $0,47$ puntos porcentuales de la meta. Requiere 3 médicos adicionales en vez de 4.

---

### c) Análisis de Sensibilidad: Tele-Triage vs. Historia Clínica con IA

#### Alternativa 1: Portal de Tele-Triage Previo ($-25\%$ de arribos presenciales $\implies \lambda' = 27\text{ pac/h}, \mu = 5\text{ pac/h}$)
* Nueva intensidad de tráfico: $r' = \frac{27}{5} = 5,40\text{ Erlangs}$ ($c_{\min} = 6$).
* Con $c = 8$ boxes médicos: $\rho = 67,50\%$, $P(\text{espera}) = 23,27\%$, $W_q = 1,0741\text{ min}$, $P(W_q \le 3\text{ min}) = 87,85\%$.
* Con **$c = 9$ boxes médicos (la dotación actual)**:
  * Utilización: $\rho = 60,00\%$
  * Probabilidad de esperar: $P(W_q > 0) = 11,86\%$
  * Longitud de cola: $L_q = 0,1779\text{ pacientes}$
  * Tiempo medio de espera: $W_q = 0,3954\text{ min}$ ($23,7\text{ segundos}$)
  * **Cumplimiento SLA:** **$P(W_q \le 3\text{ min}) = 95,18\% \ge 95\%$**.

#### Alternativa 2: Historia Clínica Asistida por IA ($t_s = 10\text{ min} \implies \mu' = 6\text{ pac/h}, \lambda = 36\text{ pac/h}$)
* Nueva intensidad de tráfico: $r'' = \frac{36}{6} = 6,00\text{ Erlangs}$ ($c_{\min} = 7$).
* Con $c = 9$ boxes médicos: $\rho = 66,67\%$, $P(\text{espera}) = 19,60\%$, $W_q = 0,6533\text{ min}$, $P(W_q \le 3\text{ min}) = 92,03\%$ (insuficiente).
* Con **$c = 10$ boxes médicos**:
  * Utilización: $\rho = 60,00\%$
  * Probabilidad de esperar: $P(W_q > 0) = 10,13\%$
  * Longitud de cola: $L_q = 0,1519\text{ pacientes}$
  * Tiempo medio de espera: $W_q = 0,2532\text{ min}$ ($15,2\text{ segundos}$)
  * **Cumplimiento SLA:** **$P(W_q \le 3\text{ min}) = 96,95\% \ge 95\%$**.
  * **Factibilidad física:** Es viable ediliciamente porque la clínica cuenta con 10 boxes físicos construidos (9 operativos + 1 inactivo), pero exige equipar y habilitar ese 10º box e incorporar 1 médico de guardia adicional ($\text{USD } 4.500/\text{mes}$).

#### Comparación Técnica y Recomendación
1. **Recomendación Técnica:**
   Se recomienda técnicamente la **Alternativa 1 (Tele-Triage)** porque cumple el SLA ($95,18\% \ge 95\%$) manteniendo los **9 boxes médicos operativos actuales** y sin necesidad de habilitar nueva capacidad física en el edificio.
2. **Información económica adicional necesaria para una evaluación costo-beneficio definitiva:**
   * **Inversión inicial (CAPEX):** Costo de desarrollo o adquisición de la plataforma de Tele-Triage vs. licenciamiento de la suite de IA y dictado por voz.
   * **Costo operativo corriente (OPEX):** Honorarios de los tele-médicos que atienden las consultas virtuales del 25% derivado vs. costo salarial del 10º médico presencial ($\text{USD } 4.500/\text{mes}$).
   * **Tasa real de adopción del paciente:** Si menos del 25% de los pacientes acepta la atención remota, la demanda presencial superará $\lambda' = 27$ y el SLA no se cumplirá con 9 boxes.
   * **Costo de equipamiento y mantenimiento** del 10º box físico inactivo para la alternativa de IA.

---

## 3. Pautas para la Simulación en FlexSim y Procesamiento en Excel
* **Configuraciones experimentales obligatorias (4 modelos):**
  Se deberán modelar y evaluar en FlexSim 4 configuraciones: 1) Situación actual ($c = 9$), 2) Cumplimiento estricto del SLA ($c = 12$), 3) Alternativa 1: Tele-Triage ($c = 9, \lambda' = 27$) y 4) Alternativa 2: Historia Clínica con IA ($c = 10, \mu' = 6$).
* **Condiciones experimentales:** Mantener constantes los parámetros experimentales entre escenarios (disciplina FIFO, capacidad infinita, distribución de tiempos de arribo y atención), variando únicamente la dotación $c$, la tasa $\lambda$ o la tasa $\mu$ según el escenario.
* **Warm-up y horizonte de simulación:**
  * Realizar una corrida de simulación de **120 horas efectivas de recolección de datos** en FlexSim para cada configuración.
  * Cada corrida deberá comprender un período de calentamiento, determinado y justificado mediante un procedimiento gráfico o estadístico, seguido de 120 horas efectivas de recolección de resultados. El período de calentamiento no deberá incluirse dentro de esas 120 horas.
  * Si se decide no utilizar calentamiento, deberá demostrarse que las condiciones iniciales representan adecuadamente el estado estacionario.
* **Procesamiento de datos en Excel:**
  * Para cada escenario, registrar en Excel los indicadores operativos obtenidos en FlexSim ($W_q, W, L_q, L, \rho$) y contrastarlos con los cálculos teóricos del modelo $M/M/c$.
  * Verificar si la utilización individual de los boxes médicos es homogénea en FlexSim (controlando que la lógica de ruteo sea equitativa, por ejemplo *Round Robin if Available* o *First Available* balanceado).
