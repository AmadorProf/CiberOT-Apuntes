---
title: "04 - Por que OT no es IT - superficie de ataque y consecuencias fisicas"
---

# 4. Por qué OT no es IT: superficie de ataque y consecuencias físicas

## 4.1 La tríada invertida

En seguridad IT se enseña la tríada **CIA**: Confidencialidad, Integridad, Disponibilidad, casi siempre en ese orden de prioridad. En OT, el orden práctico se invierte casi por completo: **Disponibilidad e Integridad por delante de la Confidencialidad**. A nadie le importa demasiado si un atacante lee la temperatura de un horno; sí importa, y mucho, si ese horno deja de responder a las órdenes de parada de emergencia, o si responde a órdenes que nadie dio.

Esta inversión no es una preferencia cultural de los ingenieros de planta: es consecuencia directa de qué hay al final de la cadena. En IT, el peor caso de un fallo de disponibilidad es que un servicio deje de facturar durante unas horas. En OT, el peor caso de un fallo de disponibilidad —o de integridad— puede ser una explosión, un vertido tóxico, o la caída de una red eléctrica regional. Esa es la razón por la que normas como ISA/IEC 62443 hablan de **seguridad (safety)** y **ciberseguridad (security)** casi como sinónimos entrelazados: proteger el sistema de control es, en última instancia, proteger a las personas y al entorno.

## 4.2 Por qué "simplemente parchear" no funciona

Cuatro fricciones estructurales explican por qué la higiene básica de TI (parchear, poner antivirus, reiniciar cuando haga falta) no se traslada sin más a OT:

1. **Disponibilidad 24/7/365 no negociable.** Una refinería o una planta química no para para instalar un parche de martes. Las ventanas de mantenimiento se planifican con meses de antelación.
2. **Certificación del fabricante.** Muchos sistemas de control industrial requieren que el propio fabricante certifique cualquier cambio de software, incluidos los parches del sistema operativo subyacente, antes de aplicarlo — de lo contrario se pierde la garantía o la certificación regulatoria del proceso.
3. **Hardware con recursos mínimos y sin margen.** Un PLC de hace 15 años puede no tener memoria ni CPU de sobra para ejecutar un agente de seguridad moderno, y un reinicio no planificado de un controlador en producción puede ser, en sí mismo, el incidente.
4. **Determinismo frente a actualización.** Cualquier cambio de software en un lazo de control de tiempo real introduce el riesgo de alterar la temporización que ese lazo necesita para ser estable. "Funciona, no lo toques" no es pereza: es una estrategia de gestión de riesgo racional en ese contexto.

## 4.3 De bits a física: la cadena de consecuencias

La diferencia categórica con IT es esta cadena causal:

```
Compromiso lógico  →  Manipulación de lógica de control o de sensores  →  Acción física incorrecta  →  Daño material / a personas / al entorno
```

Triton/Trisis (2017) es el ejemplo de manual: el objetivo no era el sistema de control del proceso sino el **Sistema Instrumentado de Seguridad (SIS)** —los controladores Triconex de Schneider Electric cuya única función es llevar la planta a un estado seguro si algo va mal—. Comprometer el SIS no para la planta: **desactiva el último mecanismo que la detendría si algo falla**, con el proceso principal siguiendo funcionando con normalidad y sin red de seguridad (Dark Reading, s.f.). Según Trellix/McAfee, el ataque a la planta petroquímica saudí no llegó a causar el daño físico buscado, aparentemente por un error en el propio malware, no por una defensa que lo detuviera (Trellix, s.f.).

## 4.4 Superficie de ataque: lo que es distinto en OT

| Dimensión | IT típico | OT típico |
|---|---|---|
| Objetivo del atacante | Datos, dinero, extorsión | Proceso físico, disrupción, a veces daño deliberado |
| Vida útil de los activos | 3-5 años | 15-25 años |
| Ventanas de parcheo | Mensuales/semanales | Anuales, ligadas a paradas de planta |
| Prioridad CIA | Confidencialidad primero | Disponibilidad e Integridad primero |
| Consecuencia del peor caso | Pérdida económica, reputacional | Daño físico, ambiental, a personas |
| Visibilidad típica del defensor | Alta (EDR, logs, SIEM maduros) | Baja (protocolos propietarios, monitorización histórica escasa) |

## 4.5 Convergencia IT/OT: por qué el problema ha empeorado, no mejorado

Durante décadas, el "air gap" —el aislamiento físico de la red OT respecto a cualquier otra red— fue la defensa de facto. Ese aislamiento nunca fue tan real como se creía (memorias USB, portátiles de mantenimiento, módems de soporte remoto de fabricantes), y hoy es directamente incompatible con las exigencias del negocio: mantenimiento predictivo, telemetría a la nube, integración con ERP. Según Waterfall Security, es precisamente ese aumento de la conectividad IT-OT lo que ha hecho posibles los incidentes transversales a gran escala de los últimos años, muchos de ellos ransomware que ni siquiera apuntaba a OT en origen pero que obligó a parar la producción por precaución —el caso típico es una parada de varios días en una infraestructura crítica para evitar que el malware saltara desde sistemas de facturación comprometidos hacia la red de control (Waterfall Security, s.f.).

## Bibliografía

- Dark Reading (s.f.). *A Brief History of ICS-Tailored Attacks*. https://www.darkreading.com/cyberattacks-data-breaches/brief-history-of-ics-tailored-attacks
- Trellix (s.f.). *Triton Malware Spearheads Latest Attacks on Industrial Systems*. https://www.trellix.com/blogs/research/triton-malware-spearheads-latest-generation-of-attacks-on-industrial-systems
- Waterfall Security (s.f.). *OT Cybersecurity: The Top 10 Attacks Since 2020*. https://waterfall-security.com/ot-insights-center/ot-cybersecurity-insights-center/ot-cybersecurity-the-top-10-attacks-since-2020/
- ISA/IEC 62443-1-1 — *Terminology, concepts and models* (define los conceptos de safety vs. security en contexto IACS).
- NIST (2023). *SP 800-82 Rev. 3*.
