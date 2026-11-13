---
title: "12 - Monitorizacion pasiva y analisis de trafico OT"
---

# 12. Monitorización pasiva y análisis de tráfico OT

## 12.1 Por qué "monitorizar OT" no es "poner un SIEM más"

Un SIEM corporativo entiende logs de Windows, de firewalls, de aplicaciones web. No entiende, de serie, qué significa un código de función 6 de Modbus escribiendo en el registro 4012, ni si ese valor concreto es una orden de proceso normal o el inicio de una manipulación como la de Stuxnet. Monitorizar OT con criterio exige **entender los protocolos del tema 3 a nivel semántico**, no solo verlos como paquetes TCP/IP genéricos. Esa necesidad es la que da origen a toda una categoría de producto: la **monitorización pasiva de redes OT**, también llamada *OT Network Detection and Response (NDR)*.

## 12.2 Por qué "pasiva" y no "activa"

En TI es normal escanear activamente una red para inventariar activos: enviar paquetes de sondeo, interrogar servicios, a veces incluso explotar vulnerabilidades de forma controlada para verificarlas. En OT, ese enfoque es directamente peligroso: muchos PLC y RTU antiguos tienen pilas de red tan frágiles que un escaneo de puertos agresivo, trivial para un servidor moderno, puede colgar el dispositivo o provocar un comportamiento errático en el proceso que controla. La respuesta del sector fue construir herramientas que observan **copias del tráfico** —mediante puertos espejo (SPAN) o TAP de red— sin inyectar jamás un solo paquete en la red de producción: **inspección profunda de paquetes (DPI)** aplicada específicamente a protocolos industriales, de forma enteramente pasiva.

Esto no es absoluto en todas las plataformas: algunos productos, descritos más adelante, complementan la monitorización pasiva con mecanismos de **consulta controlada** (a veces llamada *smart polling* o *active querying*) que envían peticiones de solo lectura, cuidadosamente limitadas, para enriquecer el inventario de activos sin modificar nunca su estado — una distinción importante que conviene verificar siempre contra la documentación técnica del fabricante antes de desplegarla en un entorno sensible.

## 12.3 Qué puede detectar realmente un sensor de monitorización OT

Entendiendo el contenido semántico del protocolo (no solo las cabeceras de red), un sensor de este tipo puede, en principio:

1. **Construir un inventario de activos pasivo**: qué PLC, HMI, RTU y estaciones de ingeniería existen, su fabricante, modelo y versión de firmware, simplemente observando cómo se comunican —algo que muchas plantas no tienen documentado de forma fiable por otros medios, como se mencionó en el tema 8.
2. **Detectar cambios de lógica de control**: alguien está descargando un nuevo programa a un PLC fuera de la ventana de mantenimiento planificada.
3. **Detectar comandos anómalos a nivel de protocolo**: una escritura Modbus a un registro que históricamente nunca se escribe desde esa dirección IP, o un código de función que ese dispositivo concreto nunca ha usado.
4. **Detectar activos nuevos o no autorizados** apareciendo en la red —el escenario típico de un portátil de mantenimiento conectado directamente a una zona de control sin pasar por el proceso de gestión de cambios.
5. **Correlacionar con inteligencia de amenazas conocida**, incluyendo indicadores asociados a malware ICS documentado como el del tema 5, y mapear comportamientos observados contra las técnicas de MITRE ATT&CK for ICS del tema 6.

## 12.4 Los límites honestos de la monitorización pasiva

Dos limitaciones que conviene tener presentes para no sobrevender lo que esta tecnología puede ofrecer:

- **No previene, detecta.** Un sensor pasivo, por definición, no está en línea con el tráfico y no puede bloquearlo en tiempo real (eso exigiría un dispositivo activo en línea, con el riesgo operativo que eso implica en redes deterministas). Su valor está en la visibilidad y la respuesta, no en la prevención directa.
- **Depende de una buena segmentación previa (tema 11).** Un sensor colocado en un punto de la red donde el tráfico de diez zonas distintas se mezcla sin control genera tanto ruido que la detección de anomalías pierde buena parte de su valor — la monitorización OT no sustituye a la arquitectura de red, la complementa.

## 12.5 De la detección a la plataforma

Lo descrito en este tema —DPI de protocolos industriales, inventario pasivo, detección de anomalías— es exactamente la capacidad base sobre la que se construyen las plataformas comerciales de detección y respuesta OT que se comparan en el tema 13: Nozomi Networks, Claroty, Dragos y Armis parten todas de este mismo principio de inspección pasiva, y se diferencian sobre todo en qué construyen encima (analítica, inteligencia de amenazas, integración con IT, alcance de activos) más que en el principio de captura de tráfico en sí.

## Bibliografía

- Knapp, E. D. (2024). *Industrial Network Security* (3.ª ed.). Elsevier/Syngress — capítulos sobre monitorización y detección de intrusiones en redes ICS.
- NIST (2023). *SP 800-82 Rev. 3*, sección de detección y monitorización continua.
- MITRE. *ATT&CK for ICS* (tema 6) — base para correlacionar detecciones con técnicas de adversario conocidas.
- Documentación técnica pública de plataformas de OT NDR (Nozomi Networks, Claroty, Dragos, Armis) sobre arquitectura de sensores pasivos.
