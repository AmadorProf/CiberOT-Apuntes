---
title: "05 - Historia de los ciberataques a infraestructuras criticas"
---

# 5. Historia de los ciberataques a infraestructuras críticas

Repasar esta historia no es coleccionismo morboso: cada incidente cambió de forma medible cómo se diseñan las defensas OT actuales. Este tema sigue el orden cronológico y, para cada hito, señala qué lección de arquitectura o de normativa nació (o se reforzó) a raíz de él.

## 5.1 Maroochy Water Services (Australia, 2000)

Antes de que existiera el término "ciberataque a infraestructura crítica" en el imaginario popular, un ex-contratista con conocimiento del sistema SCADA de la planta de tratamiento de aguas residuales de Maroochy Shire utilizó equipo de radio robado y software de control para emitir órdenes no autorizadas a las estaciones de bombeo, provocando el vertido de cientos de miles de litros de aguas residuales sin tratar al entorno. Es uno de los primeros incidentes documentados de **sabotaje OT por un insider** con conocimiento legítimo del sistema, y anticipó un patrón que se repite: el atacante no necesitaba "hackear" nada sofisticado, solo tenía las credenciales y el conocimiento de dominio que ya tenía por haber trabajado allí.

## 5.2 Stuxnet (Irán, descubierto en 2010)

**Stuxnet** es, según la práctica totalidad de las fuentes del sector, el primer malware dirigido específicamente a ICS con repercusión mundial (Forescout, s.f.; Dark Reading, s.f.). Su objetivo: las centrifugadoras de enriquecimiento de uranio de la planta de Natanz, controladas por PLC Siemens S7. En vez de destruirlas de forma evidente, el malware alteraba sutilmente su velocidad de giro hasta dañarlas mecánicamente, mientras **reproducía a los operarios, a través de la HMI, lecturas de funcionamiento normal grabadas previamente** (Plant Engineering, s.f.). Es el caso de manual del ataque a la **integridad de la visualización**: el operario no estaba ciego, estaba viendo una mentira coherente.

Lección de arquitectura: demostró que un air gap físico no es una defensa suficiente (el malware se propagó vía USB) y disparó la adopción masiva, a partir de esa década, de listas blancas de aplicaciones y control de dispositivos extraíbles en entornos OT.

## 5.3 BlackEnergy y el primer apagón de Ucrania (2015)

En diciembre de 2015, el malware **BlackEnergy** —usado junto con herramientas de acceso remoto y limpieza de rastro (KillDisk)— permitió a atacantes tomar control de las estaciones de trabajo SCADA de varias compañías eléctricas ucranianas y abrir disyuntores de forma remota, dejando sin suministro a cerca de 230.000 personas (Honeywell, s.f.). Fue el primer apagón documentado causado deliberadamente por un ciberataque.

## 5.4 Industroyer / CrashOverride (Ucrania, 2016)

Un año después, un malware mucho más especializado — **Industroyer**, también conocido como CrashOverride — provocó un segundo corte en Kiev. A diferencia de BlackEnergy, que dependía de acceso remoto manual a las estaciones de trabajo, Industroyer **hablaba de forma nativa los protocolos de las subestaciones** (incluyendo IEC 60870-5-104 y variantes de IEC 61850) y podía **controlar disyuntores y conmutadores de subestación directamente**, sin intervención humana en tiempo real (Dark Reading, s.f.; McAfee, s.f.). Fue el primer malware diseñado explícitamente como una "caja de herramientas" modular para automatizar ataques a la red eléctrica a nivel de protocolo.

## 5.5 Triton / Trisis (Arabia Saudí, 2017)

Ya descrito en el tema 4: el primer malware documentado dirigido específicamente contra un **Sistema Instrumentado de Seguridad** (los controladores Triconex de Schneider Electric), con el objetivo de desactivar la última capa de protección física de una planta petroquímica (Dark Reading, s.f.). El ataque no logró su objetivo final, aparentemente por un fallo del propio malware, no por una detección exitosa (Trellix, s.f.) — un matiz incómodo que recuerda que "no pasó nada grave" y "la defensa funcionó" no son siempre la misma frase.

## 5.6 Industroyer2 (Ucrania, 2022)

En el contexto de la invasión rusa de Ucrania, se detectó una variante evolucionada construida sobre el código fuente del Industroyer original, dirigida de nuevo contra infraestructura eléctrica ucraniana. Fue neutralizada antes de causar el corte que buscaba, en parte gracias a la inteligencia de amenazas acumulada desde 2016 — un ejemplo real de cómo la atribución y el análisis de malware de incidentes anteriores llega a pagar dividendos defensivos años después.

## 5.7 El patrón post-2020: menos malware a medida, más ransomware oportunista

Según el resumen de Waterfall Security de los diez principales incidentes OT desde 2020, la mayoría no son malware diseñado específicamente para ICS (como Stuxnet o Industroyer) sino **ransomware genérico de IT que termina afectando a OT por precaución o por conectividad excesiva** entre ambos mundos: el ejemplo citado es una parada de producción de varios días no porque el ransomware hubiera llegado a los controladores, sino para evitar que lo hiciera desde sistemas de facturación comprometidos (Waterfall Security, s.f.). El informe de Honeywell añade que, desde la guerra de Ucrania en 2022, los **hacktivistas** han mostrado un interés marcadamente mayor en infraestructura crítica y OT como objetivo simbólico, no solo los actores estatales tradicionales.

## 5.8 Línea temporal resumida

| Año | Incidente | Novedad que introdujo |
|---|---|---|
| 2000 | Maroochy Water | Sabotaje OT por insider con conocimiento legítimo |
| 2010 | Stuxnet | Primer malware ICS global; manipulación de HMI + air gap no es suficiente |
| 2015 | BlackEnergy (Ucrania) | Primer apagón causado por ciberataque documentado |
| 2016 | Industroyer | Malware que habla protocolos de subestación nativamente |
| 2017 | Triton/Trisis | Primer ataque dirigido a un Sistema Instrumentado de Seguridad |
| 2022 | Industroyer2 | Reutilización de código + neutralización gracias a inteligencia previa |
| 2020-hoy | Ransomware oportunista | OT afectada "de rebote" por la convergencia IT/OT |

## Bibliografía

- Forescout (s.f.). *Since Stuxnet: A Brief History of Critical Infrastructure Attacks*. https://www.forescout.com/blog/since-stuxnet-a-brief-history-of-critical-infrastructure-attacks
- Dark Reading (s.f.). *A Brief History of ICS-Tailored Attacks*. https://www.darkreading.com/cyberattacks-data-breaches/brief-history-of-ics-tailored-attacks
- Plant Engineering (s.f.). *The Most Infamous Cyber-Attacks on Industrial Systems*. https://www.plantengineering.com/the-most-infamous-cyber-attacks-on-industrial-systems
- Honeywell (s.f.). *The Cyber Physical Six*. https://hcenews.honeywell.com/rs/093-RAU-212/images/The_Cyber_Physical_Six.pdf
- Trellix (s.f.). *Triton Malware Spearheads Latest Generation of Attacks on Industrial Systems*.
- Waterfall Security (s.f.). *OT Cybersecurity: The Top 10 Attacks Since 2020*.
- McAfee (s.f.). Cobertura sobre Industroyer y el segundo corte de Ucrania.
