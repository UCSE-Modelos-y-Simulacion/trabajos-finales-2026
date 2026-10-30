# RESOLUCIÓN ESPERADA: CASO 4 — TERMINAL PORTUARIA DEL PLATA (GRUPO 4)

> **Aclaración institucional:** El presente caso constituye una situación hipotética elaborada exclusivamente con fines académicos. Los personajes, datos operativos, costos, decisiones y situaciones descriptas no representan información real de las organizaciones mencionadas.

## 1. Identificación del Sistema y sus 5 Elementos
* **Tipo de sistema:** Sistema estocástico de líneas de espera multicanal en paralelo $M/M/c$ (disciplina FIFO, carril único de llamado a playa de transferencia de contenedores).
* **Entidades:** Camiones portacontenedores que ingresan a la terminal portuaria.
* **Atributos:** Hora de arribo a la terminal, tipo de operación (importación / exportación), ubicación del contenedor en el bloque de estiba, tiempo requerido de maniobra.
* **Actividades:** Espera en la fila de acceso a playa, posicionamiento bajo la grúa pórtico RTG y maniobra de enganche/carga/descarga del contenedor.
* **Estados del sistema:** Estado de cada grúa pórtico RTG (Libre / Ocupada en maniobra), cantidad de camiones esperando en la fila ($L_q$).
* **Eventos:** Arribo de un camión al acceso de playa, inicio de maniobra bajo la grúa pórtico, finalización de la transferencia del contenedor y salida del camión.
* **Denominación conceptual unificada:** Los camiones forman una única fila virtual FIFO, administrada por el sistema de llamado electrónico. La espera física se desarrolla en la playa de transferencia (cuya capacidad se asume ilimitada a efectos del modelo). Se emplea *fila de espera* para el concepto matemático y *playa de transferencia* para el espacio físico.

---

## 2. Resolución Analítica y Metodológica

### a) Diagnóstico de la Operación Actual en Playa ($M/M/c$)
* **Datos de entrada:**
  * Tasa media de arribos: $\lambda = 28\text{ camiones/hora}$
  * Tiempo medio de maniobra: $t_s = 7,5\text{ minutos} \implies \mu = \frac{60}{7,5} = 8\text{ camiones/hora}$ por grúa pórtico.
* **Intensidad de tráfico y condición de estabilidad:**
  $$r = \frac{\lambda}{\mu} = \frac{28}{8} = 3,50\text{ Erlangs} \quad \implies \quad c_{\min} = \lceil 3,50 \rceil = 4\text{ grúas pórtico para no colapsar } (\rho < 1)$$
* **Evaluación de valores enteros factibles de $c$ y tabla de discrepancia:**
  Se evalúan los enteros estables ($c \ge 4$) y se calcula el error absoluto respecto de los registros observados en la franja pico ($P_1^{\text{obs}} = 0,74$ y $P_2^{\text{obs}} = 0,50$):

  | Grúas ($c$) | $\rho$ | $P(W_q > 0)$ | $P(W_q > 6\text{ min})$ | $P(W_q \le 6\text{ min})$ | Error $|P(W_q>0) - 0,74|$ | Error $|P(W_q>6) - 0,50|$ | **Suma de Errores** |
  | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
  | **$c = 4$** | **$87,50\%$** | **$73,79\%$** | **$49,46\%$** | **$50,54\%$** | **$|0,7379 - 0,74| = 0,0021$** | **$|0,4946 - 0,50| = 0,0054$** | **$0,0075$ (Mínimo)** |
  | **$c = 5$** | $70,00\%$ | $37,78\%$ | $11,38\%$ | $88,62\%$ | $|0,3778 - 0,74| = 0,3622$ | $|0,1138 - 0,50| = 0,3862$ | **$0,7484$** |
  | **$c = 6$** | $58,33\%$ | $17,75\%$ | $2,40\%$ | $97,60\%$ | $|0,1775 - 0,74| = 0,5625$ | $|0,0240 - 0,50| = 0,4760$ | **$1,0385$** |

* **Justificación del criterio:**
  El criterio de mínima discrepancia conjunta (suma de errores absolutos $= 0,0075$) selecciona unívocamente a **$c = 4$**, reproduciendo fielmente los registros operativos de la terminal.
* **Conclusión parte a):** Actualmente la terminal opera con exactamente **$c = 4$ grúas pórtico RTG**.
  * Utilización media: $\rho = 87,50\%$
  * Probabilidad de esperar en fila: $P(W_q > 0) = 73,79\%$
  * Longitud media de fila: $L_q = 5,1650\text{ camiones}$ ($L = 8,6650\text{ camiones}$)
  * Tiempo medio de espera en fila: $W_q = 11,0679\text{ minutos}$ ($W = 18,5679\text{ minutos}$)
  * Porcentaje con espera menor o igual que $6\text{ minutos}$: $P(W_q \le 6\text{ min}) = 1 - 0,4946 = \mathbf{50,54\%}$

---

### b) Cumplimiento del Nivel de Servicio Establecido ($95\%$ espera menor a $6\text{ minutos}$)
Evaluando $P(W_q \le t) = 1 - P(W_q > 0) \cdot e^{-c \mu (1 - \rho) t}$ para $t = 6\text{ min} = 0,10\text{ horas}$:

| Grúas RTG ($c$) | Utilización ($\rho$) | $P(W_q > 0)$ | $L_q\text{ (cam)}$ | $W_q\text{ (min)}$ | $P(W_q > 6\text{ min})$ | **$P(W_q \le 6\text{ min})$** | ¿Cumple SLA $\ge 95\%$? |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **$c = 4$ (Actual)** | $87,50\%$ | $73,79\%$ | $5,1650$ | $11,0679$ | $49,46\%$ | **$50,54\%$** | No |
| **$c = 5$ (Una menos)**| $70,00\%$ | $37,78\%$ | $0,8816$ | $1,8892$ | $11,38\%$ | **$88,62\%$** | No (Faltan $6,38\%$) |
| **$c = 6$ (Mínimo)** | $58,33\%$ | $17,75\%$ | $0,2485$ | $0,5324$ | $2,40\%$ | **$97,60\%$** | **Sí ($\ge 95\%$)** |
| **$c = 7$** | $50,00\%$ | $7,62\%$ | $0,0762$ | $0,1633$ | $0,46\%$ | **$99,54\%$** | Sí |

* **Resultado esperado:**
  * El Gerente de Operaciones debe habilitar **$c = 6$ grúas pórtico RTG** para cumplir estrictamente con el SLA del $95\%$ ($97,60\%$).
  * **Evaluación con una grúa menos ($c = 5$):** Se alcanza un **$88,62\%$**, lo que reduce la congestión respecto a la situación base pero resulta insuficiente para cumplir la exigencia del $95\%$.

---

### c) Análisis de Sensibilidad: Sistema Smart-Gate vs. Modernización de Spreaders

#### Alternativa 1: Sistema de Citas "Smart-Gate" ($-25\%$ en hora pico $\implies \lambda' = 21\text{ cam/h}, \mu = 8\text{ cam/h}$)
* Nueva intensidad de tráfico: $r' = \frac{21}{8} = 2,625\text{ Erlangs}$ ($c_{\min} = 3$).
* Con $c = 4$ grúas (dotación actual): $\rho = 65,62\%$, $P(\text{espera}) = 36,33\%$, $W_q = 1,9818\text{ min}$, $P(W_q \le 6\text{ min}) = 87,91\%$ (no alcanza el 95%).
* Con **$c = 5$ grúas**:
  * Utilización: $\rho = 52,50\%$
  * Probabilidad de esperar: $P(W_q > 0) = 15,34\%$
  * Longitud de fila: $L_q = 0,1696\text{ camiones}$
  * Tiempo medio de espera: $W_q = 0,4846\text{ min}$ ($29,1\text{ segundos}$)
  * **Cumplimiento SLA:** **$P(W_q \le 6\text{ min}) = 97,70\% \ge 95\%$**.
* **Limitación del alcance:** El análisis se circunscribe a la franja pico. El informe del grupo debe explicitar como limitación que los 7 camiones/hora desplazados (el 25%) deben ser programados en la franja nocturna asegurando suficiente dotación en puertas y playa nocturna para no trasladar la saturación a dicho turno.

#### Alternativa 2: Modernización de Spreaders y Guiado Láser ($t_s = 6\text{ min} \implies \mu' = 10\text{ cam/h}, \lambda = 28\text{ cam/h}$)
* Nueva intensidad de tráfico: $r'' = \frac{28}{10} = 2,80\text{ Erlangs}$ ($c_{\min} = 3$).
* Con $c = 4$ grúas: $\rho = 70,00\%$, $P(\text{espera}) = 42,87\%$, $W_q = 2,1433\text{ min}$, $P(W_q \le 6\text{ min}) = 87,09\%$.
* Con **$c = 5$ grúas**:
  * Utilización: $\rho = 56,00\%$
  * Probabilidad de esperar: $P(W_q > 0) = 18,95\%$
  * Longitud de fila: $L_q = 0,2412\text{ camiones}$
  * Tiempo medio de espera: $W_q = 0,5169\text{ min}$ ($31,0\text{ segundos}$)
  * **Cumplimiento SLA:** **$P(W_q \le 6\text{ min}) = 97,90\% \ge 95\%$**.

#### Comparación Técnica y Recomendación
1. **Recomendación Técnica:**
   Ambas alternativas cumplen satisfactoriamente el SLA con **$c = 5$ grúas pórtico** en lugar de 6 (reduciendo una grúa respecto a la solución directa del inciso b). Desde el punto de vista puramente operativo:
   * **Smart-Gate** reduce la probabilidad de espera al $15,34\%$ y el tiempo medio a $29,1\text{ s}$, logrando un tráfico más predecible, pero requiere coordinar con transportistas y verificar el turno nocturno.
   * **Spreader Láser** no requiere desplazar camiones ni alterar hábitos de terceros, procesando la demanda pico completa en sitio con un cumplimiento de SLA del $97,90\%$.
2. **Información necesaria para una recomendación económica definitiva:**
   * **Inversión inicial (CAPEX):** Costo de plataforma de agendamiento web, tótems y lectores RFID/OCR en puertas (Smart-Gate) vs. costo de adquisición e instalación de spreaders telescópicos y posicionadores láser en las grúas de playa.
   * **Costo operativo corriente (OPEX):** Ahorro en combustible diésel y mantenimiento al operar 5 grúas en vez de 6 ($\text{USD } 180.000\text{ anuales}$ de ahorro base).
   * **Costos de incentivo:** Monto de las bonificaciones tarifarias necesarias para motivar a los choferes a acudir en franja nocturna.
   * **Sobrecosto de turno nocturno:** Costo laboral diferencial por horas nocturnas para operadores de balanza, grúa y seguridad portuaria.

---

## 3. Pautas para la Simulación en FlexSim y Procesamiento en Excel
* **Configuraciones experimentales obligatorias (4 modelos):**
  Se deberán modelar y evaluar en FlexSim 4 configuraciones: 1) Situación actual ($c = 4$), 2) Cumplimiento estricto del SLA ($c = 6$), 3) Alternativa 1: Sistema Smart-Gate ($c = 5, \lambda' = 21$) y 4) Alternativa 2: Modernización de Spreaders ($c = 5, \mu' = 10$).
* **Condiciones experimentales:** Mantener estrictamente constantes las condiciones experimentales entre escenarios, excepto el parámetro evaluado ($\lambda$, $\mu$ o $c$). Para favorecer la comparación entre alternativas, se recomienda utilizar semillas comunes entre escenarios equivalentes, manteniendo una semilla diferente para cada réplica.
* **Warm-up y réplicas:**
  * Realizar al menos **10 réplicas independientes, utilizando una semilla aleatoria diferente en cada réplica. Cada réplica deberá comprender un período de calentamiento, determinado y justificado mediante un procedimiento gráfico (método de Welch, series temporales acumuladas) o estadístico, seguido de 120 horas efectivas de recolección de resultados. El período de calentamiento no deberá incluirse dentro de esas 120 horas.**
  * Si se decide no utilizar calentamiento, deberá demostrarse que las condiciones iniciales representan adecuadamente el estado estacionario.
* **Procesamiento estadístico en Excel:**
  * Calcular intervalos de confianza del 95 % para, como mínimo, $W_q$, $W$, $L_q$, $L$, $P(W_q > 0)$ y el porcentaje de cumplimiento del SLA ($P(W_q \le 6\text{ min})$). La utilización de cada servidor deberá informarse mediante su media y desviación estándar entre réplicas.
  * Los intervalos de confianza deberán calcularse utilizando como observaciones las medias obtenidas en cada réplica independiente:
    $$\bar{X} = \frac{1}{n}\sum_{i=1}^{n} X_i, \quad s = \sqrt{\frac{1}{n-1}\sum_{i=1}^{n}(X_i - \bar{X})^2}, \quad \text{IC}_{95\%} = \left[\bar{X} - t_{9;\,0,025}\frac{s}{\sqrt{10}}, \; \bar{X} + t_{9;\,0,025}\frac{s}{\sqrt{10}}\right]$$
  * **Verificación de utilizaciones:** Contrastar la utilización media teórica del sistema $\rho = \frac{\lambda}{c\mu}$ con la utilización de cada una de las grúas individuales en FlexSim, verificando si la asignación de camiones genera cargas de trabajo balanceadas entre servidores homogéneos.
