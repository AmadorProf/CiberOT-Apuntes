---
title: "01 - Arquitectura OT - el modelo Purdue y la norma ISA-95"
---

# 1. Arquitectura OT: el modelo Purdue y la norma ISA-95

## 1.1 Por qué hace falta un modelo de referencia

Una planta industrial no es una red plana. Hay sensores midiendo temperatura cada pocos milisegundos, controladores tomando decisiones en tiempo real, estaciones donde un operario supervisa el proceso, sistemas que planifican la producción del día, y sistemas corporativos que facturan lo que se produjo. Cada capa tiene requisitos de latencia, disponibilidad y seguridad completamente distintos: un retraso de 200 ms es intrascendente en facturación y catastrófico en el lazo de control de una turbina.

Para razonar sobre esa heterogeneidad sin perderse, la industria usa desde los años 90 un modelo de referencia jerárquico conocido como **PERA** (*Purdue Enterprise Reference Architecture*), desarrollado en el Laboratorio de Control Industrial Aplicado de la Universidad Purdue. La norma **ISA-95** (adoptada internacionalmente como **IEC 62264**, *Enterprise-control system integration*) formalizó más tarde esa jerarquía como un estándar de integración entre los sistemas de planta y los de empresa. En la práctica, cuando alguien en ciberseguridad OT dice "modelo Purdue", normalmente se refiere a esta síntesis PERA + ISA-95 (SentinelOne, s.f.; Opsio, s.f.).

Es importante no tratarlo como una ley física: es una arquitectura de referencia, no una norma obligatoria, y muchas instalaciones reales no encajan limpiamente en sus niveles. Aun así, sigue siendo el lenguaje común para hablar de segmentación OT, y es el punto de partida que usan tanto el NIST SP 800-82 como la propia familia ISA/IEC 62443 para situar sus controles (NIST, 2023).

## 1.2 Los niveles, de abajo arriba

| Nivel | Nombre | Qué vive ahí | Ejemplos típicos |
|---|---|---|---|
| 0 | Proceso físico | El proceso real: el fluido, el motor, la pieza | Válvulas, motores, sensores de campo |
| 1 | Control básico | Lazos de control en tiempo real | PLC, RTU, controladores de lazo (PID) |
| 2 | Supervisión de área | Supervisión y control humano del proceso | HMI, estaciones SCADA locales, históricos de planta |
| 3 | Operaciones de planta | Gestión del flujo de trabajo de producción | MES (*Manufacturing Execution System*), historiadores centrales |
| 3.5 | DMZ industrial | Zona de intercambio controlado entre OT e IT | Proxies de datos, replicación unidireccional, firewalls dedicados |
| 4 | Logística de planta / empresa | Planificación y sistemas de negocio | ERP, correo, aplicaciones corporativas |
| 5 | Red corporativa / externa | Conectividad externa | Internet, VPN de proveedores, la nube |

El **nivel 3.5**, la DMZ industrial, no apareció en la formulación original de PERA: es una adición posterior que se volvió práctica estándar precisamente para evitar que el tráfico cruzara directamente entre el nivel 3 (OT) y el nivel 4 (IT) sin control intermedio (SentinelOne, s.f.). Volveremos sobre ella en el tema 11, porque es la pieza de arquitectura más citada en cualquier auditoría de seguridad OT.

Una lectura útil de la tabla: **cuanto más bajo es el nivel, más determinista y menos tolerante a cambios es el entorno**. Un PLC en el nivel 1 puede llevar el mismo firmware quince años; un servidor de nivel 4 se parchea cada mes. Esa asimetría de ritmos — y no la tecnología en sí — es la raíz de buena parte de los problemas de seguridad OT, como veremos en el tema 4.

## 1.3 Zonas: la lectura "de ciberseguridad" del modelo

Más allá de los niveles numéricos, el modelo se suele agrupar en tres **zonas** funcionales:

- **Zona empresarial (niveles 4-5):** TI convencional, con sus prácticas habituales de parcheo, antivirus y gestión de identidades.
- **DMZ industrial (nivel 3.5):** la frontera controlada.
- **Zona de manufactura / OT (niveles 0-3):** el dominio de la disponibilidad y la seguridad física por encima de la confidencialidad.

Esta lectura por zonas es también la que adopta **IEC 62443-3-2**, aunque con una diferencia importante que veremos en el tema 9: en vez de fijar fronteras únicas basadas en los niveles de Purdue, 62443 define **zonas y conductos** a medida del sistema concreto, con un nivel de seguridad objetivo (SL-T) propio para cada uno. Varias fuentes describen precisamente esa flexibilidad de zonas/conductos como la evolución natural frente a la rigidez de los niveles de Purdue cuando la arquitectura real no encaja en seis capas limpias (fuente secundaria sobre 62443, 2024).

## 1.4 Qué cambia en la práctica

Tres consecuencias prácticas de pensar en niveles:

1. **La segmentación de red debe reflejar los niveles**, no la topología física de los armarios eléctricos. Dos PLC en el mismo edificio pueden pertenecer a zonas de seguridad distintas si controlan procesos independientes.
2. **El tráfico entre zona de manufactura y zona empresarial siempre debería pasar por la DMZ**, nunca de forma directa. Es la regla de oro que repiten tanto las guías de Cisco/Rockwell (tema 11) como el propio NIST SP 800-82.
3. **Los niveles inferiores (0-1) rara vez necesitan "seguridad IT clásica"** (antivirus, parches frecuentes): necesitan aislamiento, redundancia y control de acceso físico. Confundir ambos mundos es el error de diseño más repetido en auditorías OT.

## Bibliografía

- NIST (2023). *SP 800-82 Rev. 3 — Guide to Operational Technology (OT) Security*. National Institute of Standards and Technology. https://csrc.nist.gov/pubs/sp/800/82/r3/final
- International Society of Automation / IEC. *ISA-95 / IEC 62264 — Enterprise-Control System Integration*.
- SentinelOne (s.f.). *What Is the Purdue Model?* https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-the-purdue-model/
- Opsio (s.f.). *¿Qué es el modelo Purdue en ICS?* https://opsiocloud.com/knowledge-base/what-is-purdue-model-ics/
- IEC (2020). *IEC 62443-3-2:2020 — Security risk assessment for system design*. https://webstore.iec.ch/en/publication/30727
