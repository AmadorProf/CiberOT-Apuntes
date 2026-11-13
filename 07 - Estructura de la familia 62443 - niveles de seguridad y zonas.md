---
title: "07 - Estructura de la familia 62443 - niveles de seguridad y zonas"
---

# 7. Estructura de la familia ISA/IEC 62443

## 7.1 De dónde viene y por qué tiene dos nombres

La serie nació como **ISA99**, un comité de la *International Society of Automation* constituido para abordar la ciberseguridad de los sistemas de automatización y control industrial (IACS). Posteriormente la Comisión Electrotécnica Internacional adoptó y renumeró la serie como **IEC 62443**, por lo que hoy es habitual verla citada indistintamente como "ISA/IEC 62443", "ISA99" o solo "62443" (Wikipedia, s.f.; cavac.at, s.f.). A diferencia de normas más monolíticas, 62443 es una **familia de documentos** —actualmente del orden de una docena de estándares, especificaciones técnicas e informes técnicos— agrupados en cuatro niveles según a quién van dirigidos.

## 7.2 Los cuatro grupos de documentos

| Grupo | A quién se dirige | Pregunta que responde | Documentos representativos |
|---|---|---|---|
| **1 — General** | Todo el mundo | Terminología, conceptos, modelo de madurez | 62443-1-1 (*Terminology, concepts and models*), 62443-1-5 (*Scheme for security profiles*, 2023) |
| **2 — Políticas y procedimientos** | El propietario/operador del activo (*asset owner*) | ¿Cómo gestiono la seguridad como programa continuo? | 62443-2-1 (*Security program requirements for IACS asset owners*), 62443-2-3 (gestión de parches) |
| **3 — Sistema** | El integrador de sistemas | ¿Cómo diseño y evalúo el riesgo de este sistema concreto? | 62443-3-2 (*Security risk assessment for system design*, 2020), 62443-3-3 (*System security requirements and security levels*) |
| **4 — Componente / producto** | El fabricante | ¿Cómo construyo un producto (PLC, HMI, switch industrial...) que pueda integrarse de forma segura? | 62443-4-1 (*Product security development life-cycle requirements*, 2018), 62443-4-2 (*Technical security requirements for IACS components*, 2018) |

Esta tabla es la razón por la que 62443 cuesta de explicar en una frase: **no es una norma, son cuatro normas que hablan con cuatro audiencias distintas sobre la misma cadena de responsabilidad**. El fabricante construye un componente conforme a 62443-4-2; el integrador lo combina con otros en un sistema conforme a 62443-3-3; el propietario del activo opera ese sistema dentro de un programa de seguridad conforme a 62443-2-1. Si cualquiera de los tres eslabones falla, la cadena completa es tan débil como el eslabón roto, por mucho que los otros dos estén certificados.

## 7.3 Niveles de seguridad (SL): midiendo contra el adversario, no contra una lista de casillas

El concepto más influyente que introduce la serie —y el que más se cita fuera de 62443, incluso en comparaciones con otros marcos— es el de **Nivel de Seguridad (*Security Level*, SL)**. A diferencia de una checklist binaria ("cumple / no cumple"), un SL mide la **capacidad del adversario** frente a la que un sistema está diseñado para resistir:

- **SL 0:** Sin requisitos ni protección especial.
- **SL 1:** Protección frente a violación **casual o accidental**.
- **SL 2:** Protección frente a violación **intencionada, usando medios simples**, con pocos recursos, habilidades genéricas y motivación baja.
- **SL 3:** Protección frente a violación **intencionada, usando medios sofisticados**, con recursos moderados, conocimiento específico de IACS y motivación moderada.
- **SL 4:** Protección frente a violación **intencionada, usando medios sofisticados**, con recursos extendidos, conocimiento específico de IACS profundo y motivación alta — el nivel pensado, en la práctica, para adversarios con capacidades de estado-nación.

Esta escala se aplica en tres variantes que conviene no confundir, y que son el puente directo hacia los temas 8 y 9:

- **SL-T (*Target*):** el nivel que **se necesita** para una zona o conducto concreto, determinado mediante el proceso de evaluación de riesgos de 62443-3-2.
- **SL-C (*Capability*):** el nivel que un producto o sistema **es capaz de alcanzar** por diseño, tal como lo certifica el fabricante o el integrador.
- **SL-A (*Achieved*):** el nivel que el sistema **alcanza realmente en operación**, una vez instalado, configurado y mantenido en su contexto real — que puede ser inferior al SL-C si la configuración o el mantenimiento no están a la altura.

La relación entre los tres es el resumen más honesto de por qué un sistema "certificado SL 3" puede seguir siendo vulnerable en la práctica: **SL-C es una propiedad del producto; SL-A es una propiedad de cómo se opera ese producto**. Un PLC con SL-C 3 operado con contraseñas por defecto tiene un SL-A mucho más bajo.

## 7.4 Zonas y conductos: la unidad de análisis

62443 no evalúa el riesgo de "la planta" como un todo monolítico, sino de **zonas** (agrupaciones de activos con requisitos de seguridad homogéneos) conectadas por **conductos** (los canales de comunicación entre zonas, incluyendo lo que fluye por ellos). Esto es, en esencia, una versión formalizada y más flexible de los niveles de Purdue vistos en el tema 1: en vez de asumir que todo lo que está en el "nivel 1" necesita el mismo SL, 62443 permite declarar, por ejemplo, que la zona del sistema instrumentado de seguridad necesita SL-T 3 mientras que la zona de un lazo de control no crítico adyacente solo necesita SL-T 1 — y diseñar el conducto entre ambas (el firewall o diodo de datos que las separa) en consecuencia. Cómo se llega a ese número concreto de SL-T es el contenido del tema 9.

## Bibliografía

- ISA / IEC. *ISA/IEC 62443 series* (múltiples partes, ISA99 / IEC TC65/WG10).
- Wikipedia (s.f.). *IEC 62443*. https://en.wikipedia.org/wiki/IEC_62443
- cavac.at (s.f.). *IEC 62443*. https://cavac.at/cavacopedia/IEC%2062443
- IEC (2023). *IEC 62443-1-5 — Scheme for IEC 62443 security profiles*, Ed. 1.0.
- IEC (2018). *IEC 62443-4-1 — Product security development life-cycle requirements*.
- IEC (2018). *IEC 62443-4-2 — Technical security requirements for IACS components*.
- Control Engineering (s.f.). *Structure of IEC 62443*. https://www.controleng.com/structure-of-iec-62443
