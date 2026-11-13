---
title: "10 - Seguridad de producto y componente - comparativa con NIST y NERC CIP"
---

# 10. Seguridad de producto y componente — comparativa con NIST SP 800-82 y NERC CIP

## 10.1 El último eslabón de la cadena: el fabricante

Cerrando el recorrido por 62443 que empezó en el tema 7 ("cuatro normas, cuatro audiencias"), llegamos al grupo 4: los requisitos dirigidos al **fabricante** del componente — el PLC, la HMI, el switch industrial, la pasarela de protocolo — antes incluso de que ese componente llegue a integrarse en ningún sistema.

**IEC 62443-4-1** (*Product security development life-cycle requirements*, 2018) no evalúa el producto terminado: evalúa **el proceso con el que el fabricante lo construye**. Exige prácticas de desarrollo seguro a lo largo de todo el ciclo de vida: gestión de requisitos de seguridad desde el diseño, modelado de amenazas durante la arquitectura, revisión de código y pruebas de seguridad antes del lanzamiento, y —un punto que la industria OT tiende a subestimar mucho más que la IT— un **proceso de gestión de vulnerabilidades posterior al lanzamiento**, con un canal definido para que investigadores y clientes reporten fallos y una cadencia real de parches durante años, no solo en el primer año de vida comercial del producto.

**IEC 62443-4-2** (*Technical security requirements for IACS components*, 2018) es la contraparte técnica: toma los mismos **siete Requisitos Fundamentales** del tema 9 (FR1-FR7) y los traduce en requisitos verificables a nivel de componente individual — por ejemplo, qué mecanismos de autenticación debe soportar nativamente un PLC para poder integrarse en una zona con un SL-T determinado. Es la pieza que cierra el círculo: un integrador de sistemas (62443-3-3) solo puede alcanzar un SL-T concreto en una zona si los componentes que usa (62443-4-2) son, de origen, capaces de ese SL-C.

## 10.2 NIST SP 800-82: la alternativa (complementaria) made in USA

Mientras 62443 es una familia de normas internacionales orientada a certificación de productos y programas, **NIST SP 800-82** es una **guía** —no una norma certificable— publicada por el instituto de estándares estadounidense, pensada originalmente para sistemas de control industrial y ampliada en su revisión más reciente a todo el espectro de **tecnología operativa (OT)**, incluyendo sectores como edificios automatizados, agua/saneamiento y ferrocarril de mercancías (NIST, 2023; techxplore, 2023).

La **Revisión 3**, publicada en septiembre de 2023, es la edición final vigente en el momento de escribir estas notas. Su aportación más citada es el **OT overlay** sobre los controles de **NIST SP 800-53 Rev. 5**: en vez de reinventar un catálogo de controles desde cero, SP 800-82r3 adapta el catálogo general de seguridad de la información de NIST a las particularidades OT descritas en el tema 4, y ofrece líneas base diferenciadas para sistemas OT de impacto bajo, moderado y alto (NIST, 2023). Hay además una **Revisión 4** en fase de borrador público en el momento de redactar este tema, con comentarios abiertos hasta finales de noviembre de 2026, que amplía aún más la cobertura sectorial y reestructura la gestión de riesgo en torno a la función *Govern* del NIST Cybersecurity Framework 2.0 — conviene revisar el estado de publicación de esta revisión antes de citarla como vigente, porque mientras no se publique en firme, **la Revisión 3 sigue siendo la edición de referencia**.

## 10.3 NERC CIP: la vía regulatoria obligatoria del sector eléctrico norteamericano

A diferencia de 62443 (voluntaria, de adopción internacional) y de NIST SP 800-82 (guía, no mandato legal en sí misma), **NERC CIP** (*Critical Infrastructure Protection*) es un conjunto de estándares de cumplimiento **obligatorio** para las entidades que operan el sistema eléctrico masivo (*Bulk Electric System*) en Norteamérica, con sanciones económicas reales por incumplimiento. Entre los estándares más citados:

- **CIP-002:** identificación y clasificación de activos cibernéticos críticos (*BES Cyber Systems*).
- **CIP-003:** controles de gestión de seguridad, incluida la gestión de personal con acceso autorizado.
- **CIP-005:** el **perímetro de seguridad electrónico** — el requisito más directamente comparable a la segmentación por zonas de 62443 y a la DMZ industrial del tema 11.
- **CIP-007:** gestión de seguridad de sistemas — descrito por varias fuentes del sector como el estándar que más violaciones de cumplimiento acumula en la práctica, a menudo por gestión deficiente de parches y puertos/servicios innecesarios activos.
- **CIP-008:** identificación, clasificación y notificación de incidentes de ciberseguridad.
- **CIP-011:** protección de la información de los sistemas BES Cyber (cifrado, control de acceso, gestión del ciclo de vida de la información).
- **CIP-014:** seguridad física de subestaciones y centros de control críticos.

El número exacto de estándares vigentes varía según la fuente consultada y la fecha de la versión de cumplimiento aplicable (las cifras citadas en fuentes del sector oscilan entre 11 y una docena larga, con decenas de sub-requisitos), así que para un proyecto de cumplimiento real la referencia autoritativa es siempre la página de *Reliability Standards* de NERC, no un resumen de terceros.

## 10.4 Cómo encajan los tres marcos entre sí

| | ISA/IEC 62443 | NIST SP 800-82 | NERC CIP |
|---|---|---|---|
| Naturaleza | Familia de normas internacionales | Guía técnica (no normativa en sí) | Estándares de cumplimiento obligatorio |
| Ámbito | IACS en general, cualquier sector | OT en general (ampliado en Rev. 3) | Sistema eléctrico masivo de Norteamérica |
| Unidad de análisis | Zonas y conductos, SL por componente/sistema | Controles de SP 800-53 adaptados (overlay OT) | Activos cibernéticos del BES clasificados por criticidad |
| Vía de adopción | Voluntaria, con certificación de producto/sistema posible | Voluntaria, orientativa | Obligatoria con sanción económica |
| Mejor uso práctico | Diseñar y especificar seguridad de producto y sistema | Evaluar postura de seguridad y seleccionar controles | Demostrar cumplimiento regulatorio en el sector eléctrico de EE. UU./Canadá |

En la práctica, una organización madura no elige "uno de los tres": usa 62443 para el diseño y la especificación de compra, NIST SP 800-82 como referencia de controles y evaluación de madurez, y —si opera en el sector eléctrico norteamericano— NERC CIP como el suelo mínimo de cumplimiento legal que no es negociable, por encima del cual los otros dos marcos pueden seguir aportando profundidad adicional.

## Bibliografía

- IEC (2018). *IEC 62443-4-1 — Product security development life-cycle requirements*.
- IEC (2018). *IEC 62443-4-2 — Technical security requirements for IACS components*.
- NIST (2023). *SP 800-82 Rev. 3 — Guide to Operational Technology (OT) Security*. https://csrc.nist.gov/pubs/sp/800/82/r3/final
- NIST (2026). *SP 800-82 Rev. 4 (Initial Public Draft)*. https://csrc.nist.gov/pubs/sp/800/82/r4/ipd
- techxplore (2023). *New revised guide to operational technology (OT) security published*.
- NERC. *Reliability Standards* (CIP-002 a CIP-014 y sucesivas). https://www.nerc.com
- RSI Security (s.f.). *NERC CIP Standards: What You Need To Know*. https://blog.rsisecurity.com/nerc-cip-standards-what-you-need-to-know/
