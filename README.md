# Modelos y Simulación — Trabajos Prácticos Finales Regulares 2026

**Universidad Católica de Santiago del Estero (UCSE) — Departamento Académico San Salvador**  
**Facultad de Ciencias de la Computación / Carrera:** Ingeniería en Informática  
**Cátedra:** Modelos y Simulación (Ciclo Lectivo 2026)  
**Equipo Docente:**
* **Profesora Asociada:** Ing. María Fernanda Vázquez
* **Jefe de Trabajos Prácticos:** Ing. Fabio Damián Argañaraz Azua

🌐 **Portal Web Oficial del Trabajo Final (GitHub Pages):**  
👉 **[https://fabiodrizzt.github.io/modelos-y-simulacion-trabajos-finales-2026/](https://fabiodrizzt.github.io/modelos-y-simulacion-trabajos-finales-2026/)**

---

## 📋 Descripción del Trabajo Práctico Final Integrador
El presente repositorio contiene los enunciados interactivos, especificaciones técnicas y lineamientos de modelado para el Trabajo Práctico Final Integrador de la cátedra **Modelos y Simulación (2026)**.

Cada grupo de estudiantes desarrollará el análisis estocástico combinando la formulación matemática analítica de **Teoría de Colas** con la modelización computacional, experimentación estadística y simulación 3D en **FlexSim**.

---

## 🗂️ Casos de Estudio Asignados

| Grupo / Caso | Organización / Entorno | Modelo Teórico de Colas | Enlace al Enunciado |
| :---: | :--- | :--- | :--- |
| **Caso 1** | **Aeroparque / Compañía Aérea**<br>Operaciones de Check-in y dimensionamiento de butacas | $M/M/4$, $M/M/1 + M/M/3$, $M/M/2 + M/M/1$ | [`tf-grupo1-lapa.html`](tf-grupo1-lapa.html) |
| **Caso 2** | **FAdeA: Fábrica Argentina de Aviones**<br>Red tándem de conformado e inspección con WIP | Red en Serie $M/M/10 \to M/M/1$ y $M/G/1$ (Gamma) | [`tf-grupo2-fadea.html`](tf-grupo2-fadea.html) |
| **Caso 3** | **Guardia Central — Clínica San Martín**<br>Dimensionamiento de boxes médicos y SLA del 95% | $M/M/c$ con distribución $P(W_q \le 3\text{ min}) \ge 0,95$ | [`tf-grupo3-clinica.html`](tf-grupo3-clinica.html) |
| **Caso 4** | **Terminal Portuaria del Plata**<br>Grúas pórtico RTG, Smart-Gate y SLA portuario | $M/M/c$ con distribución $P(W_q \le 6\text{ min}) \ge 0,95$ | [`tf-grupo4-puerto.html`](tf-grupo4-puerto.html) |

---

## 📦 Paquete de Entregables Exigidos
Para la defensa y demostración (Demo), cada grupo deberá presentar los siguientes cuatro artefactos obligatorios:
1. **Informe Administrativo y Técnico en Word (`.docx`):** Identificación de los 5 elementos del sistema, formulación analítica paso a paso, análisis crítico y recomendación fundamentada.
2. **Memoria de Cálculo y Planilla de Datos en Excel (`.xlsx`):** Formulación teórica, recolección estadística de al menos 10 réplicas independientes de 120 horas efectivas por escenario, medias, desvíos estándar e intervalos de confianza del 95 %.
3. **Modelos Computacionales Ejecutables en FlexSim (`.fsm`):** Modelos 3D funcionales del caso base y de cada alternativa de mejora con sus respectivos *dashboards*.
4. **Presentación Ejecutiva para el Día de la Demo (`.pptx` / PDF):** Diapositivas de soporte visual para la defensa oral de 20 minutos.

---

## 🗓️ Condiciones Formales de Entrega
* **Fecha y hora límite:** Viernes 20 de noviembre de 2026, 23:59 hs (hora de Argentina).
* **Medio de entrega:** Buzón de tareas oficial en el Campus Virtual UCSE (Moodle).
* **Nomenclatura oficial:**
  * `MS2026_TF_Grupo[X]_Informe.docx`
  * `MS2026_TF_Grupo[X]_Datos.xlsx`
  * `MS2026_TF_Grupo[X]_ModelosFlexSim.zip` (o `.fsm`)
  * `MS2026_TF_Grupo[X]_Presentacion.pptx` / `.pdf`
