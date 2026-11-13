---
title: "11 - Segmentacion y DMZ industrial"
---

# 11. Segmentación y DMZ industrial

## 11.1 El control que más veces se nombra y menos veces se implementa bien

Si hubiera que resumir en un único control técnico todo lo visto hasta ahora —el modelo Purdue (tema 1), el FR5 de 62443-3-3 (tema 9), el patrón de incidentes por convergencia IT/OT (temas 4 y 5)— sería este: **ninguna comunicación debería cruzar directamente de la zona de manufactura a la zona de empresa sin pasar por una frontera controlada.** Esa frontera tiene nombre propio: la **DMZ industrial** (*Industrial Demilitarized Zone*, IDMZ), situada en el nivel 3.5 del modelo Purdue (tema 1).

## 11.2 Qué es, y qué no es, una DMZ industrial

Tomando como referencia la guía conjunta de Cisco y Rockwell Automation **CPwE** (*Converged Plantwide Ethernet*) — probablemente el documento de arquitectura de referencia más citado del sector para esta frontera —, la IDMZ se sitúa entre la zona de manufactura (OT) y la zona de empresa (IT), con dos objetivos que a veces se perciben como contradictorios pero que deben cumplirse ambos a la vez: **impedir la comunicación directa** entre OT e IT, y **seguir permitiendo** el intercambio de los datos y servicios que el negocio necesita (históricos de producción, OEE, mantenimiento predictivo) (Cisco/Rockwell, CPwE).

Técnicamente, esto se traduce casi siempre en **servicios duplicados o reflejados dentro de la propia DMZ**, no en abrir un puerto a través de ella: un servidor de históricos "espejo" vive en la DMZ, recibe datos desde OT, y es ese espejo —no el histórico original en la zona de control— el que la zona de empresa consulta. Si el servidor de empresa se compromete, el atacante llega como mucho al espejo, nunca al sistema de control real. La guía de Rockwell añade un matiz operativo importante: debe existir un mecanismo de **buffering** para los datos que van de OT a empresa en caso de que la conexión a través de la DMZ se caiga, de forma que una interrupción de red no obligue a elegir entre perder datos de proceso o saltarse la frontera de seguridad (Rockwell, 2022).

## 11.3 Lo que NO es una DMZ industrial

Tres errores de diseño frecuentes, todos ellos observados en auditorías reales del sector:

1. **Un firewall sin zona intermedia real.** Poner un firewall entre OT e IT con reglas de "permitir tráfico X desde IT hacia OT" sigue siendo comunicación directa punto a punto, solo que filtrada. El modelo CPwE exige una zona intermedia con sus propios activos, no solo una regla de firewall entre dos zonas que siguen hablando entre sí.
2. **Una VPN de mantenimiento del fabricante que bordea la DMZ.** Es, como se mencionó en el tema 8, una de las vías de entrada reales más documentadas: una conexión de soporte remoto permanente que conecta directamente con la zona de control, sin pasar por la frontera que existe precisamente para evitar ese escenario.
3. **Reglas de firewall que "temporalmente" se abren para un mantenimiento** y nunca se cierran — el patrón de degradación silenciosa descrito en el tema 8, aplicado específicamente a la pieza de arquitectura más crítica de toda la red OT.

## 11.4 Segmentación dentro de la propia zona OT

La DMZ resuelve la frontera OT/IT, pero 62443-3-2 (tema 9) exige ir más allá: segmentar también **dentro** de la zona de manufactura, en zonas y conductos más finos. Un patrón de referencia habitual:

- **Zona de seguridad (SIS):** aislada incluso de la propia red de control del proceso, con el SL-T más alto de toda la planta — es, literalmente, la zona que Triton/Trisis (tema 5) intentó comprometer precisamente porque romper su aislamiento desactivaba la última red de seguridad.
- **Zonas de célula/área (nivel 1-2):** agrupando PLC y HMI de una misma línea o unidad de proceso, de forma que un compromiso en una línea no se propague lateralmente a otra sin cruzar, de nuevo, un conducto controlado.
- **Zona de operaciones de planta (nivel 3):** históricos, MES, servidores de ingeniería — con su propio conducto de control hacia la DMZ, no hacia la zona de empresa directamente.

## 11.5 El principio que lo unifica todo

La segmentación —entre OT e IT, y dentro de la propia OT— no es un control aislado: es la implementación de red del **FR5 (flujo de datos restringido)** de 62443-3-3 visto en el tema 9, y es la precondición técnica que hace viable la monitorización pasiva del tema 12: solo se puede inspeccionar con criterio el tráfico que cruza fronteras bien definidas; en una red plana sin segmentación, un sensor de monitorización ve "todo mezclado con todo" y pierde buena parte de su capacidad de detectar anomalías de verdad.

## Bibliografía

- Cisco Systems / Rockwell Automation. *Converged Plantwide Ethernet (CPwE) Design and Implementation Guide*, capítulo de DMZ industrial.
- Rockwell Automation (2022). *Securely Traversing IACS Data across the IDMZ Using Cisco Firepower Threat Defense* (whitepaper, 7 de marzo de 2022).
- ISA / IEC. *IEC 62443-3-2 / 3-3* (FR5 — Restricted Data Flow).
- NIST (2023). *SP 800-82 Rev. 3*, sección de arquitectura de red y segmentación.
