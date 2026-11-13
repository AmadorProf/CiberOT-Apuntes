---
title: "06 - MITRE ATTACK for ICS y modelado de amenazas"
---

# 6. MITRE ATT&CK for ICS y modelado de amenazas

## 6.1 Qué resuelve un marco de conocimiento de adversarios

Después del repaso histórico del tema anterior, surge una pregunta operativa: ¿cómo se traduce "lo que hicieron los atacantes de Industroyer" en algo que un equipo de seguridad pueda usar para priorizar defensas en su propia planta? La respuesta de la industria, igual que ocurrió antes en IT con **MITRE ATT&CK Enterprise**, fue construir un **lenguaje común y catalogado de comportamientos adversarios**, pero adaptado a las metas y limitaciones específicas de un entorno de control industrial.

**MITRE ATT&CK for ICS** se publicó en enero de 2020, desarrollado por MITRE en colaboración con la empresa de inteligencia de amenazas OT **Dragos**, con el objetivo explícito de catalogar comportamientos maliciosos **documentados públicamente** contra infraestructura OT crítica (Help Net Security, 2020). La palabra "documentados" importa: a diferencia de un modelo teórico, cada técnica del marco está respaldada por evidencia de incidentes o investigación real, no por especulación.

## 6.2 En qué se diferencia de ATT&CK Enterprise

La diferencia de fondo no es solo de catálogo sino de **objetivo del adversario**. En IT, las tácticas terminan casi siempre en exfiltración de datos o impacto económico/reputacional. En ATT&CK for ICS, el marco incorpora dos tácticas que no tienen equivalente directo en el mundo IT, porque describen metas que solo tienen sentido cuando el objetivo final es un proceso físico:

- **Inhibit Response Function** (inhibir la función de respuesta): impedir que los mecanismos de protección y respuesta —alarmas, disyuntores de protección, sistemas instrumentados de seguridad— actúen cuando deberían. Es exactamente lo que perseguía Triton/Trisis contra el SIS saudí (tema 4 y 5).
- **Impair Process Control** (dañar el control del proceso): manipular directamente el proceso físico mediante la modificación de la lógica de control, de los parámetros, o de las lecturas que ve el operario — la categoría donde encaja la manipulación de la HMI en Stuxnet.

Las otras nueve tácticas sí tienen nombre compartido con Enterprise ATT&CK —Initial Access, Execution, Persistence, Privilege Escalation, Evasion, Discovery, Lateral Movement, Collection y Command and Control— aunque las técnicas concretas que las implementan están adaptadas al contexto ICS (por ejemplo, el acceso inicial a través de estaciones de ingeniería o de proveedores con VPN de soporte remoto, en vez de a través de un correo de phishing corporativo genérico). En conjunto, el marco cataloga más de ochenta técnicas de comportamiento específicas de ICS, organizadas por táctica y, en las matrices más recientes, también por plataforma (Windows, Linux, sistemas embebidos) (industrialcyber.co, s.f.; attack.mitre.org).

## 6.3 Para qué sirve en la práctica

ATT&CK for ICS no es un producto que se instala: es un **vocabulario y un mapa de cobertura**. Sus usos reales más frecuentes:

1. **Evaluar huecos de visibilidad.** Un equipo de seguridad OT puede marcar, técnica por técnica, cuáles detectaría hoy su instrumentación (SIEM, sensores pasivos del tema 13, logs de PLC) y cuáles no vería en absoluto. El resultado es un mapa honesto de puntos ciegos, no una lista de productos a comprar.
2. **Estructurar ejercicios de modelado de amenazas.** En vez de preguntar genéricamente "¿qué podría pasar?", el marco permite preguntar: "si un adversario ya tiene acceso a la estación de ingeniería, ¿qué técnicas de *Impair Process Control* tiene disponibles, y las detectaríamos?"
3. **Dar contexto a la inteligencia de amenazas.** Cuando un informe de Dragos o de CISA describe el comportamiento de un grupo o malware concreto, suele mapearlo contra técnicas ATT&CK for ICS, lo que permite comparar campañas distintas sobre una base común en vez de depender de descripciones en prosa no estructuradas.

## 6.4 Relación con el resto del temario

ATT&CK for ICS no sustituye a la gestión de riesgos de ISA/IEC 62443 (temas 7-10): responde a una pregunta distinta. 62443 pregunta "¿qué nivel de seguridad necesita esta zona?" (un ejercicio de diseño, hecho antes del ataque). ATT&CK for ICS pregunta "si me atacan, ¿con qué técnicas concretas lo harán, y lo vería?" (un ejercicio de detección y respuesta, orientado al día después del diseño). Un programa de seguridad OT maduro usa ambos: 62443 para decidir dónde poner las defensas, ATT&CK for ICS para comprobar si esas defensas, una vez puestas, realmente detectan algo.

## Bibliografía

- MITRE (2020-). *ATT&CK for ICS*. https://attack.mitre.org/matrices/ics/
- Help Net Security (2020). *ATT&CK for ICS: Knowledge base of techniques used by cyber adversaries*. https://www.helpnetsecurity.com/2020/01/08/attck-for-ics/
- Industrial Cyber (s.f.). *MITRE Reveals OT/ICS Attack Tactics and Techniques*. https://industrialcyber.co/reports/mitre-reveals-ot-ics-attack-tactics-and-techniques/
- WaterISAC (2020). *MITRE Releases ATT&CK™ for ICS as Common Lexicon for Industrial Cyber Defense Strategy*.
- Dragos (colaborador original en el desarrollo del marco junto con MITRE).
