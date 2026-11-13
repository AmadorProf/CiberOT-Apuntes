---
title: "02 - Dispositivos de campo y control - PLC RTU DCS SCADA HMI"
---

# 2. Dispositivos de campo y control: PLC, RTU, DCS, SCADA, HMI

## 2.1 El vocabulario que hay que dominar antes de hablar de seguridad

Es imposible discutir con criterio la seguridad de un entorno OT sin saber qué hace cada caja. Son términos que se usan de forma intercambiable en la prensa generalista y que en realidad designan cosas distintas, con superficies de ataque distintas.

### PLC — Controlador Lógico Programable

Un **PLC** (*Programmable Logic Controller*) es un ordenador industrial endurecido que ejecuta, en un bucle de scan determinista, lógica de control sobre entradas y salidas físicas (sensores y actuadores). Se programa típicamente en lenguajes normalizados por **IEC 61131-3** (Ladder Diagram, Structured Text, Function Block Diagram, entre otros). Es el nivel 1 del modelo Purdue por excelencia.

Desde el punto de vista de seguridad, lo crítico de un PLC es que su **lógica de control** es, en sí misma, el activo a proteger: Stuxnet no "robó datos" de las centrifugadoras iraníes, **reescribió la lógica del PLC** para que éstas giraran a velocidades destructivas mientras mostraba lecturas normales al operario (tema 5).

### RTU — Unidad Terminal Remota

Una **RTU** (*Remote Terminal Unit*) cumple una función parecida a un PLC pero está pensada para **telemetría y control en ubicaciones remotas y dispersas**: subestaciones eléctricas, estaciones de bombeo, pozos. Suele depender de enlaces de comunicación de menor ancho de banda (radio, satélite, líneas dedicadas) y de protocolos orientados a esa realidad, como **DNP3** (tema 3). La frontera entre PLC y RTU se ha difuminado con los años, pero la distinción histórica importa porque explica por qué muchas RTU siguen operando con hardware y protocolos de los años 90: su reemplazo físico implica desplegar personal en ubicaciones remotas, lo que encarece y ralentiza cualquier modernización.

### DCS — Sistema de Control Distribuido

Un **DCS** (*Distributed Control System*) es la arquitectura típica de procesos continuos y complejos — refinerías, plantas químicas, generación eléctrica — donde el control no reside en un único controlador sino que se **distribuye** entre múltiples controladores interconectados que comparten una base de datos de proceso común. A diferencia de un PLC (pensado originalmente para control discreto/secuencial), un DCS está diseñado desde el origen para regulación continua de cientos o miles de lazos simultáneos.

### SCADA — Supervisión, Control y Adquisición de Datos

**SCADA** (*Supervisory Control and Data Acquisition*) no es un dispositivo sino una **arquitectura de sistema**: el conjunto de software y comunicaciones que permite supervisar y controlar de forma centralizada procesos geográficamente dispersos, típicamente PLC y RTU en campo reportando a un centro de control. Un sistema SCADA eléctrico nacional puede supervisar miles de subestaciones; cada una con su propia RTU, todas reportando al centro de control a través de la red de telemetría.

### HMI — Interfaz Hombre-Máquina

La **HMI** (*Human-Machine Interface*) es la pantalla —física o en un PC— donde el operario ve el estado del proceso y emite órdenes. Es, por diseño, el punto de confianza del operario: si la HMI miente (como en Stuxnet), el operario no tiene forma de saberlo sin instrumentación independiente. Las HMI modernas corren a menudo sobre Windows embebido o Linux, lo que las convierte en el punto OT con más similitudes— y por tanto más vulnerabilidades heredadas— con el mundo IT convencional.

## 2.2 Cómo encajan estas piezas en una arquitectura real

```
Nivel 2  [HMI] <---> [Servidor SCADA] <---> [Histórico de planta]
                            |
Nivel 1  [PLC #1]      [PLC #2]      [PLC #3]
            |              |              |
Nivel 0  Sensores/      Sensores/      Sensores/
         actuadores     actuadores     actuadores
```

En un DCS, el "Servidor SCADA" de este esquema se sustituye por una red de controladores redundantes que comparten estado entre sí, con mucha más densidad de E/S por controlador y mecanismos de tolerancia a fallos integrados — es, en esencia, la diferencia filosófica entre "varios cerebros pequeños coordinados por un supervisor" (PLC + SCADA) y "un cerebro distribuido y redundante" (DCS).

## 2.3 Por qué esto importa para la seguridad

Tres implicaciones directas:

1. **La superficie de ataque cambia según el dispositivo.** Un PLC suele exponer su propio protocolo de programación (a menudo sin autenticación) además del protocolo de control; una HMI expone lo que exponga su sistema operativo subyacente.
2. **El tiempo de vida es radicalmente distinto al de TI.** Un PLC o RTU puede seguir en producción 15-25 años. Diseñar controles que asuman ciclos de parcheo de TI (mensuales) es, sencillamente, ignorar la realidad operativa de estos activos.
3. **El impacto de un compromiso es físico, no solo lógico.** Modificar la lógica de un PLC no "borra datos": puede abrir una válvula que no debía abrirse. Esto es lo que diferencia categóricamente el riesgo OT del riesgo IT, y es el hilo central del tema 4.

## Bibliografía

- IEC (2013). *IEC 61131-3 — Programmable controllers — Part 3: Programming languages*.
- Knapp, E. D. (2024). *Industrial Network Security* (3.ª ed.). Elsevier/Syngress.
- Macaulay, T. y Singer, B. L. (2016). *Cybersecurity for Industrial Control Systems: SCADA, DCS, PLC, HMI, and SIS*. Auerbach Publications.
- NIST (2023). *SP 800-82 Rev. 3 — Guide to Operational Technology (OT) Security*.
