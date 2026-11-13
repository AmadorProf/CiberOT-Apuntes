---
title: "09 - Analisis de riesgos y requisitos de sistema - 62443-3-2 y 62443-3-3"
---

# 9. Análisis de riesgos y requisitos de sistema (62443-3-2 y 62443-3-3)

## 9.1 De la organización al diseño concreto

Si 62443-2-1 (tema 8) responde "¿cómo gestiono la seguridad como programa?", **62443-3-2** y **62443-3-3** responden la pregunta que ese programa necesita resolver para cada sistema concreto: "¿qué nivel de seguridad necesita exactamente esta zona, y qué tiene que cumplir técnicamente para alcanzarlo?"

## 9.2 62443-3-2: el proceso de evaluación de riesgos para el diseño del sistema

**IEC 62443-3-2:2020**, titulada *Security risk assessment for system design*, formaliza el proceso para llegar de "tengo un sistema" a "sé qué SL-T necesita cada parte de ese sistema". Según la documentación del estándar, el proceso sigue, a grandes rasgos, esta secuencia (IEC, 2020; fuente secundaria de cumplimiento, 2024):

1. **Definir el sistema bajo consideración (SuC).** Delimitar qué entra y qué no entra en el análisis — sin este paso, cualquier evaluación de riesgo posterior carece de alcance claro.
2. **Particionar en zonas y conductos**, tal como se introdujo en el tema 7: agrupar activos con necesidades de seguridad homogéneas, e identificar los canales de comunicación entre esos grupos.
3. **Realizar una evaluación de riesgo inicial ("high-level")** por zona, para identificar rápidamente qué zonas son candidatas a necesitar un análisis más profundo por su criticidad para la seguridad física (*safety*), el impacto financiero o el cumplimiento regulatorio.
4. **Realizar una evaluación de riesgo detallada** en las zonas que lo requieran: identificar amenazas, vulnerabilidades, contramedidas existentes, probabilidad y consecuencia, para calcular un riesgo que se compara contra el **riesgo tolerable** definido por la organización.
5. **Determinar el SL-T** de cada zona y conducto a partir de esa comparación: si el riesgo detallado supera el tolerable, el SL-T sube (y con él, los requisitos técnicos que tendrá que cumplir esa zona según 62443-3-3); si no lo supera, el SL-T se mantiene bajo y se evitan controles desproporcionados al riesgo real.
6. **Documentar los requisitos de seguridad resultantes**, que alimentan tanto las decisiones de arquitectura (temas 11-12) como las especificaciones de compra de equipos (tema 10).

El matiz importante, y el que distingue a 62443-3-2 de un checklist genérico, es que **el riesgo tolerable no es un número universal**: una refinería y una planta de embotellado de agua no tienen el mismo riesgo tolerable para el mismo tipo de incidente, y el propio estándar deja esa calibración en manos de la organización, dentro del marco de su CSMS (tema 8).

## 9.3 62443-3-3: los siete requisitos fundamentales (FR) y los SL por sistema

Una vez fijado el SL-T de una zona, **62443-3-3** (*System security requirements and security levels*) traduce ese número en requisitos técnicos concretos, organizados en **siete Requisitos Fundamentales (Foundational Requirements, FR)** que estructuran prácticamente todo el vocabulario técnico del resto de la familia:

| FR | Nombre | Qué cubre |
|---|---|---|
| FR1 | Identificación y control de autenticación | Quién —persona, dispositivo o software— puede identificarse y autenticarse en el sistema |
| FR2 | Control de uso | Qué puede hacer cada identidad autenticada una vez dentro (autorización) |
| FR3 | Integridad del sistema | Proteger el sistema y la información frente a manipulación no autorizada |
| FR4 | Confidencialidad de los datos | Proteger la información frente a divulgación no autorizada, en tránsito y en reposo |
| FR5 | Flujo de datos restringido | Segmentación: limitar qué datos cruzan qué fronteras de zona (el fundamento técnico de la DMZ del tema 11) |
| FR6 | Respuesta oportuna a eventos | Capacidad de detectar y responder a incidentes de seguridad en tiempos razonables (el fundamento de la monitorización del tema 12) |
| FR7 | Disponibilidad de recursos | Garantizar que el sistema sigue disponible frente a degradación o denegación de servicio — coherente con la prioridad de disponibilidad del tema 4 |

Para cada FR, la norma define requisitos concretos y los escala en función del SL-T de la zona: un requisito de FR1 para SL-T 1 puede ser tan simple como "requerir autenticación de usuario humano", mientras que el mismo FR1 para SL-T 3 puede exigir autenticación multifactor resistente a repetición y gestión centralizada de identidades. Esta es la mecánica concreta que convierte un número abstracto (SL-T 3) en una lista de controles verificables.

## 9.4 Por qué este orden de los dos documentos importa

Un error frecuente en programas de seguridad OT jóvenes es empezar por 62443-3-3 —"vamos a implementar los siete FR al máximo nivel en todo"— sin haber pasado antes por 62443-3-2. El resultado es previsible: sobre-ingeniería en zonas de bajo riesgo (coste y fricción operativa innecesarios) y, paradójicamente, posibles huecos en zonas de alto riesgo que no se identificaron correctamente por no haberse evaluado de forma diferenciada. El orden correcto —evaluar primero (3-2), implementar requisitos escalados después (3-3)— es la aplicación práctica del principio de que la seguridad OT debe ser proporcional al riesgo real, no uniforme por decreto.

## Bibliografía

- IEC (2020). *IEC 62443-3-2:2020 — Security risk assessment for system design*. https://webstore.iec.ch/en/publication/30727
- ISA / IEC. *IEC 62443-3-3 — System security requirements and security levels*.
- theartofservice (s.f.). *62443-3-2 ZCR 4* (resumen secundario del proceso de evaluación por zonas y conductos). https://compliance.theartofservice.com/controls/iec-62443/62443-3-2-zcr-4
- AFNOR (2020). *IEC 62443-3-2:2020 — Security risk assessment for system design*. https://www.boutique.afnor.org/en-gb/standard/iec-62443322020/
