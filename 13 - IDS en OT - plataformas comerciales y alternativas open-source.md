---
title: "13 - IDS en OT - plataformas comerciales y alternativas open-source"
---

# 13. IDS en OT: plataformas comerciales y alternativas open-source

## 13.1 De la teoría de detección a las opciones reales del mercado

El tema 12 explicó el principio técnico (DPI pasiva de protocolos industriales). Este tema cierra el recorrido mirando **quién implementa ese principio hoy**, tanto en el mercado comercial como en el mundo abierto, para que la elección de herramienta no sea un ejercicio de marketing sino una decisión informada sobre qué necesita realmente cada organización.

## 13.2 Las cuatro plataformas comerciales de referencia

Comparando la documentación pública y análisis de mercado disponibles (Technology Match; otcybersecuritysoftware.pages.dev; Elisity; búsqueda agregada, 2026), el panorama comercial se puede resumir así:

| Plataforma | Enfoque declarado | Diferenciador habitual citado |
|---|---|---|
| **Dragos** | DPI pasiva + inteligencia de amenazas ICS propia | Descrita de forma consistente como la de mayor profundidad en inteligencia de amenazas ICS y capacidad de respuesta a incidentes en infraestructura crítica; nació de la misma comunidad que contribuyó a MITRE ATT&CK for ICS (tema 6) |
| **Nozomi Networks** | DPI pasiva + *smart polling* opcional | Fuerte en analítica (detección de anomalías asistida por IA), escalado a operaciones multi-sitio distribuidas, y monitorización de espectro inalámbrico |
| **Claroty** | DPI pasiva, con variantes de producto para distintos tamaños de despliegue | Posicionado de forma competitiva en coste y alcance frente a Dragos para volúmenes de activos comparables; fuerte integración con flujos de trabajo de gestión de vulnerabilidades |
| **Armis** | Modelo de integraciones y colectores pasivos, agnóstico de agente | Cobertura más amplia de tipos de activos (IoT, IT y OT a la vez), pensado para organizaciones que quieren un único inventario convergente en vez de una herramienta solo-OT |

Dos matices necesarios antes de tomar cualquiera de estas descripciones como verdad absoluta: primero, buena parte de las fuentes públicas que comparan estas plataformas son blogs de consultoras o de los propios competidores, con sesgos evidentes — cualquier decisión de compra real debería apoyarse en una prueba de concepto sobre el propio tráfico de la planta, no en comparativas de terceros. Segundo, analistas independientes de mercado (como Gartner o Forrester) sitúan habitualmente a Nozomi y Dragos entre los líderes del segmento en sus informes anuales, pero esas posiciones cambian de edición a edición y conviene consultar siempre el informe más reciente, no una cita de segunda mano.

## 13.3 Qué tienen en común, más allá del marketing

Pese a las diferencias de posicionamiento, las cuatro comparten la arquitectura de fondo descrita en el tema 12: sensores que observan tráfico espejado o replicado, parsers de protocolo específicos para Modbus/DNP3/OPC-UA/PROFINET y variantes propietarias de cada fabricante de automatización, y un motor de analítica que compara lo observado contra una base de comportamiento normal o contra reglas e indicadores de amenaza conocidos. La disputa real entre ellas no es "pasivo sí o no" sino **qué tan bien parsean protocolos propietarios poco documentados**, **qué tan buena es su analítica para reducir falsos positivos** (el problema número uno citado por analistas de SOC que trabajan con estas plataformas) y **qué tan integrada queda su telemetría con el SIEM/SOC de IT corporativo** sin forzar a los analistas OT a cambiar de herramienta.

## 13.4 Alternativas open-source: no partir siempre de cero

Para organizaciones con presupuesto limitado, o para quien quiera entender el problema desde dentro antes de comprar una plataforma, existen bases abiertas reales, no solo herramientas IT genéricas forzadas a un contexto que no entienden:

- **Zeek** (anteriormente *Bro*): motor de análisis de tráfico de red open-source, ampliamente usado en IT, que cuenta con **analizadores de protocolo específicos para ICS** desarrollados por la comunidad — el proyecto **ICSNPP** (*ICS Network Protocol Parsers*) del Idaho National Laboratory es la referencia más citada, con parsers para Modbus, DNP3, BACnet, Profinet y otros protocolos industriales integrables directamente en Zeek.
- **Suricata**: motor de detección de intrusos/IDS open-source con soporte de reglas para algunos protocolos industriales, normalmente usado en combinación con conjuntos de reglas específicos de ICS (incluyendo reglas derivadas de inteligencia de amenazas pública sobre el malware del tema 5).
- **GRASSMARLIN**: herramienta de mapeo pasivo de redes ICS desarrollada originalmente por la NSA y liberada como código abierto; aunque su desarrollo activo se ha ralentizado con los años, sigue siendo citada como referencia histórica de mapeo pasivo gratuito y es útil para entender los conceptos antes de evaluar una plataforma comercial.

La honestidad que hay que mantener con estas alternativas: ninguna llega, de fábrica, a la cobertura de parsers propietarios ni a la inteligencia de amenazas curada de una plataforma comercial madura. Son excelentes para **aprender el problema, hacer una prueba de concepto de bajo coste, o cubrir una zona de criticidad baja** — no sustituyen, en la mayoría de los casos, a una plataforma dedicada en las zonas de SL-T más alto de una planta crítica.

## 13.5 Cierre del temario

Con esto se completa el recorrido: de la arquitectura (tema 1) a los dispositivos y protocolos (temas 2-3), de ahí al porqué del riesgo y su historia real (temas 4-6), después a la normativa que formaliza cómo gestionarlo (temas 7-10), y por último a las dos capas de defensa que hoy sostienen la mayoría de los programas de seguridad OT maduros: la arquitectura de red (tema 11) y la detección (temas 12-13). Ninguna de las dos capas sustituye a la otra, y ninguna sustituye tampoco al programa de gestión continuo del tema 8: la tecnología ayuda, pero el riesgo se gestiona con proceso, no solo con sensores.

## Bibliografía

- Technology Match (s.f.). *Claroty vs Nozomi Networks vs Dragos vs Armis: OT Industrial Security*. https://technologymatch.com/blog/claroty-vs-nozomi-networks-vs-dragos-vs-armis-ot-industrial-security
- Elisity (s.f.). *Leading Vendors Securing OT/Industrial Control Systems*. https://www.elisity.com/blog/leading-vendors-securing-ot-industrial-control-systems
- Idaho National Laboratory. *ICSNPP — ICS Network Protocol Parsers for Zeek*. https://github.com/cisagov/icsnpp (proyecto mantenido en colaboración con CISA).
- Zeek Project. *Zeek Network Security Monitor*. https://zeek.org
- OISF. *Suricata*. https://suricata.io
- NSA (histórico). *GRASSMARLIN*.
