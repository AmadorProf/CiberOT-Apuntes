---
title: "08 - El CSMS y el programa de seguridad del propietario del activo"
---

# 8. El CSMS y el programa de seguridad del propietario del activo

## 8.1 De la norma al programa: por qué 62443-2-1 es el documento que más se ignora

De toda la familia 62443, **62443-2-1** (*Security program requirements for IACS asset owners*) es probablemente la parte peor entendida, porque no habla de tecnología: habla de **gestión**. Define los requisitos de lo que la norma llama **CSMS — Cyber Security Management System**, el marco organizativo con el que un propietario de activos (una fábrica, una utility, un operador de infraestructura) gestiona la ciberseguridad de sus IACS como un **proceso continuo**, no como un proyecto con fecha de entrega.

Esta distinción no es semántica. Una auditoría de seguridad puntual, por exhaustiva que sea, es una fotografía: describe el estado el día de la auditoría. Un CSMS es una película: describe cómo el estado de seguridad se mantiene, se revisa y se mejora a lo largo del tiempo, incluso cuando cambian el personal, los proveedores o las amenazas. La mayoría de los incidentes OT documentados en el tema 5 no ocurrieron por ausencia total de controles técnicos, sino por **degradación no detectada** de controles que en algún momento existieron: contraseñas que nunca se rotaron, cuentas de contratistas que nunca se desactivaron, segmentación de red que se "temporalmente" desactivó para un mantenimiento y nunca se restauró.

## 8.2 Las fases de un CSMS

Aunque la redacción exacta de 62443-2-1 organiza el contenido en categorías más detalladas, el ciclo de vida de un CSMS se entiende mejor como una variación del clásico Plan-Do-Check-Act aplicado a IACS:

1. **Iniciar el programa:** obtener patrocinio de la dirección, definir el alcance (qué plantas, qué sistemas), formar un equipo con representación tanto de operaciones (ingenieros de proceso) como de seguridad (TI/ciberseguridad) — la ausencia de uno de los dos perfiles es la causa más común de que un CSMS fracase en la práctica, porque termina escribiendo políticas que o bien ignoran las restricciones operativas reales, o bien ignoran las amenazas reales.
2. **Evaluar el riesgo y categorizar activos:** inventariar qué sistemas existen (sorprendentemente, muchas organizaciones no tienen un inventario fiable de sus propios PLC y RTU) y aplicar el proceso de evaluación de riesgos de 62443-3-2 (tema 9) para asignar criticidad.
3. **Desarrollar e implementar contramedidas:** políticas de control de acceso, gestión de parches adaptada a ventanas de mantenimiento OT, segmentación de red, gestión de cambios, respuesta a incidentes — las contramedidas concretas se seleccionan para alcanzar el SL-T de cada zona, no de forma genérica.
4. **Mantener el programa:** auditorías periódicas, revisión tras incidentes, actualización del inventario y de la evaluación de riesgo cuando cambia la arquitectura (se añade un sistema, se conecta una nueva integración con la nube, se incorpora un proveedor nuevo).

## 8.3 El elemento más subestimado: la gestión de terceros y proveedores

Una parte sustancial de 62443-2-1 —y de su variante centrada en proveedores de servicios, 62443-2-4— trata la gestión de **integradores de sistemas y proveedores de mantenimiento remoto**. No es casualidad: buena parte de las vulnerabilidades reales en IACS no entran por la red de Internet corporativa sino por **el módem o la VPN que el fabricante del PLC usa para dar soporte remoto**, o por el portátil de un integrador externo que se conecta directamente a la red de control durante una puesta en marcha. Un CSMS maduro exige que esos accesos de terceros estén inventariados, limitados en el tiempo, auditados y, siempre que sea posible, mediados por la DMZ industrial del tema 11 — nunca con una conexión directa y permanente a la zona de control.

## 8.4 Relación con el resto de la familia

62443-2-1 es, en cierto sentido, el documento "paraguas": define el programa dentro del cual se usan el resto de las partes de la familia como herramientas. La evaluación de riesgos de 62443-3-2 (tema 9) alimenta las decisiones de priorización del CSMS; los requisitos técnicos de 62443-3-3 y 62443-4-2 (temas 9 y 10) son las contramedidas concretas que el programa implementa; y la gestión de parches de 62443-2-3 formaliza uno de los procesos operativos recurrentes del propio CSMS. Sin un CSMS que los sostenga, los controles técnicos de las otras partes de la familia tienden a degradarse con el tiempo exactamente de la forma descrita al principio de este tema: no por un único fallo dramático, sino por erosión silenciosa.

## Bibliografía

- ISA / IEC. *ISA/IEC 62443-2-1 — Security program requirements for IACS asset owners*.
- ISA / IEC. *ISA/IEC 62443-2-3 — Patch management in the IACS environment*.
- ISA / IEC. *ISA/IEC 62443-2-4 — Security program requirements for IACS service providers*.
- Knapp, E. D. (2024). *Industrial Network Security* (3.ª ed.). Elsevier/Syngress.
- Macaulay, T. y Singer, B. L. (2016). *Cybersecurity for Industrial Control Systems*. Auerbach Publications.
