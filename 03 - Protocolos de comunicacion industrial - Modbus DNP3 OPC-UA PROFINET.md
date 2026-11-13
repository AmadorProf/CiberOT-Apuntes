---
title: "03 - Protocolos de comunicacion industrial - Modbus DNP3 OPC-UA PROFINET"
---

# 3. Protocolos de comunicación industrial

## 3.1 El pecado original: diseñados para redes aisladas

Casi todos los protocolos industriales en uso hoy se diseñaron entre los años 70 y 90, cuando la premisa de diseño era "esta red nunca va a tocar Internet". La prioridad era el determinismo y la tolerancia a interferencia eléctrica, no la autenticación ni el cifrado. Cuando a partir de los 2000 estas redes empezaron a conectarse —primero a redes corporativas, después a la nube— esa ausencia de seguridad por diseño se convirtió en el problema estructural que sigue arrastrando el sector treinta años después (Stormshield, s.f.; OPSWAT, s.f.).

## 3.2 Modbus

**Modbus** (Modicon, 1979) es probablemente el protocolo industrial más desplegado del mundo por pura antigüedad y simplicidad. Existe en variantes serie (RTU/ASCII) y sobre TCP/IP (Modbus TCP, puerto 502). Su modelo es maestro/esclavo: el maestro lee y escribe registros en el esclavo mediante códigos de función muy simples (leer/escribir bobinas, registros de entrada, registros de retención).

**No tiene ningún mecanismo de autenticación ni cifrado nativo.** Cualquier equipo que pueda enviar tramas Modbus TCP válidas a un dispositivo puede, en principio, leer o escribir sus registros — es decir, puede impersonar al maestro legítimo y cambiar el valor de un actuador (Automation Forum, s.f.). La mitigación habitual no es "securizar Modbus" (no se puede, sin romper interoperabilidad) sino rodearlo: firewalls industriales que filtran por lista blanca de códigos de función y direcciones de registro permitidas, y segmentación que impida que un atacante llegue siquiera a enviar esas tramas.

## 3.3 DNP3

**DNP3** (*Distributed Network Protocol*) nació en el sector eléctrico y es el protocolo dominante en RTU de distribución, subestaciones y SCADA de utilities, especialmente en Norteamérica (complementado por IEC 60870-5-104 en Europa para el mismo dominio). A diferencia de Modbus, DNP3 incluye conceptos más sofisticados de reporte por excepción y marcas de tiempo, pensados para enlaces de bajo ancho de banda con muchos puntos remotos.

En su modo "no seguro", DNP3 es vulnerable a **ataques de repetición e inyección**, y carece de comprobaciones de integridad robustas (resultado de búsqueda, 2026). La respuesta de la industria fue **DNP3 Secure Authentication (DNP3-SA)**, una extensión que añade autenticación criptográfica de los mensajes críticos. El problema práctico, igual que con otras extensiones seguras de protocolos legacy, es la adopción: muchas instalaciones siguen operando en modo no seguro porque activar DNP3-SA exige actualizar firmware en ambos extremos de la comunicación, algo costoso en una red con miles de RTU de distintos fabricantes y antigüedades.

## 3.4 OPC-UA

**OPC Unified Architecture** es, de los cuatro protocolos de este tema, el único diseñado desde cero (2006 en adelante) pensando en seguridad: soporta autenticación de usuario y de aplicación, firma y cifrado de mensajes, y un modelo de información orientado a objetos mucho más rico que el de Modbus o DNP3 (arXiv, 2020). Es también el protocolo de referencia de **Industria 4.0** para interoperabilidad vertical (de sensor a nube).

El matiz importante: OPC-UA **no es seguro por defecto**, es *seguro si se configura*. Su especificación permite un modo "Security Policy: None" sin cifrado ni firma, y en la práctica un número significativo de despliegues reales se encuentra funcionando en ese modo por comodidad de integración o por fallos de configuración (Ayinedjimi Consultants, s.f.). Herramientas públicas de reconocimiento pueden localizar servidores OPC-UA expuestos, probar credenciales débiles o por defecto, y contrastar su versión contra CVE conocidas (arXiv, 2020). Además, al ejecutarse sobre TCP/IP convencional, hereda toda la superficie de ataque de la pila de red subyacente (Stormshield, s.f.).

## 3.5 PROFINET

**PROFINET** es el estándar de Ethernet industrial más usado en automatización de fábrica en Europa (impulsado por Siemens y la organización PROFIBUS). Ofrece tres clases de rendimiento, hasta tiempo real estricto (Isochronous Real-Time) para aplicaciones de movimiento sincronizado. Su seguridad es, según las fuentes consultadas, mínima u opcional: poca integridad o confidencialidad nativa, y al correr directamente sobre Ethernet queda expuesto a ataques de intermediario (*man-in-the-middle*) y denegación de servicio a nivel de enlace.

## 3.6 Comparativa

| Protocolo | Año / origen | Transporte típico | Autenticación nativa | Cifrado nativo | Extensión segura |
|---|---|---|---|---|---|
| Modbus | 1979, Modicon | Serie / TCP 502 | No | No | No existe (se mitiga con firewall) |
| DNP3 | años 90, sector eléctrico | Serie / TCP/UDP 20000 | No (modo base) | No (modo base) | DNP3-SA |
| OPC-UA | 2006, OPC Foundation | TCP / HTTPS | Sí (si se configura) | Sí (si se configura) | Es la propia especificación base |
| PROFINET | 2003, PROFIBUS&PROFINET Int. | Ethernet | Mínima/opcional | Mínima/opcional | PROFINET Security Class (parcial) |

## 3.7 La conclusión práctica

Ningún firewall convencional de TI entiende de forma nativa estos protocolos: un firewall de perímetro ve "tráfico TCP al puerto 502" pero no sabe si el código de función que viaja dentro es una lectura inofensiva o una escritura que abre una válvula. Esa es precisamente la razón de ser de los **firewalls industriales con inspección profunda de protocolo (DPI-OT)** y de las plataformas de monitorización pasiva que veremos en los temas 11 y 13: la seguridad de estos protocolos, al no poder añadirse de origen en la mayoría del parque instalado, tiene que añadirse alrededor.

## Bibliografía

- Stormshield (s.f.). *How can the security of industrial protocols be controlled?* https://stormshield.com/news/how-can-the-security-of-industrial-protocols-be-controlled
- OPSWAT (s.f.). *Why Insecure Industrial Protocols Threaten Critical Infrastructure*. https://www.opswat.com/blog/the-unseen-risk-why-insecure-industrial-protocols-threaten-critical-infrastructure
- Automation Forum (s.f.). *Role of Protocols and Standards in Industrial Automation*. https://automationforum.co/protocols-and-standards-in-industrial-automation-a-guide-to-ot-cybersecurity/
- arXiv (2020). *Assessing the Security of OPC UA Deployments*. https://arxiv.org/pdf/2003.12341
- Ayinedjimi Consultants (s.f.). *Protocoles industriels vulnérables: Modbus*. https://ayinedjimi-consultants.fr/articles/protocoles-industriels-vulnerables-modbus
- IEC 60870-5-104 — *Telecontrol equipment and systems* (protocolo europeo equivalente a DNP3 en distribución eléctrica).
