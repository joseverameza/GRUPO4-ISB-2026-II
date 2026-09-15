<h1 align="center">Electrocardiograma (ECG): Reposo, Hiperventilación, Hipoventilación y Actividad Aeróbica</h1>
<p align="center"><em>Laboratorio 4 — Introducción a Señales Biomédicas</em></p>
## Índice
 
- [1. Introducción](#1-introducción)
- [2. Materiales](#2-materiales)
- [3. Procedimiento de toma de muestras](#3-procedimiento-de-toma-de-muestras)
  - [3.1 Actividades realizadas](#31-actividades-realizadas)
  - [3.2 Protocolo de adquisición por derivación](#32-protocolo-de-adquisición-por-derivación)
- [4. Ubicación de electrodos por derivación](#4-ubicación-de-electrodos-por-derivación)
- [5. Señal en OpenSignals](#5-señal-en-opensignals)
  - [5.1 Reposo](#51-reposo)
  - [5.2 Hiperventilación](#52-hiperventilación)
  - [5.3 Hipoventilación](#53-hipoventilación)
  - [5.4 Actividad aeróbica](#54-actividad-aeróbica)
- [6. Video de muestra](#6-video-de-muestra)
- [7. Señal procesada en Python](#7-señal-procesada-en-python)
  - [7.1 Reposo](#71-reposo)
  - [7.2 Hiperventilación](#72-hiperventilación)
  - [7.3 Hipoventilación](#73-hipoventilación)
  - [7.4 Actividad aeróbica](#74-actividad-aeróbica)
- [8. Análisis](#8-análisis)
- [9. Respuestas al cuestionario](#9-respuestas-al-cuestionario)
- [10. Referencias](#10-referencias)
# 1. Introducción
 
*[Completar: objetivo del laboratorio — adquirir señales de electrocardiograma (ECG) con el sensor BITalino, explorando las tres derivaciones bipolares de Einthoven (DI, DII, DIII) bajo cuatro condiciones fisiológicas distintas (reposo, hiperventilación, hipoventilación y actividad aeróbica), y relacionar los cambios observados con la fisiología cardíaca y respiratoria vista en clase.]*
 
# 2. Materiales
 
- OpenSignals (r)evolution (software de adquisición)
- 1x BITalino (r)evolution Assembled Core BT
- 1x Sensor de Electrocardiografía (ECG) ensamblado
- 3x Electrodos desechables autoadhesivos gelificados Ag/AgCl
- 1x Dongle Bluetooth
# 3. Procedimiento de toma de muestras
 
## 3.1 Actividades realizadas
 
Para cada una de las siguientes condiciones se registró la señal ECG en las tres derivaciones (DI, DII, DIII), recolocando los electrodos entre cada derivación:
 
| Actividad | DI | DII | DIII |
|---|---|---|---|
| Reposo (30 s) | ☐ | ☐ | ☐ |
| Hiperventilación | ☐ | ☐ | ☐ |
| Hipoventilación | ☐ | ☐ | ☐ |
| Actividad aeróbica | ☐ | ☐ | ☐ |
 
## 3.2 Protocolo de adquisición por derivación
 
1. Colocar los electrodos según la posición correspondiente a la derivación en turno (ver sección 4).
2. **Reposo:** iniciar grabación y mantener 30 s sin hablar ni moverse, en postura cómoda.
3. **Hiperventilación:** iniciar grabación y, *durante* los 30 s de registro, realizar el ciclo inhalar–retener–exhalar de forma repetida (la maniobra se realiza mientras se graba, no antes).
4. **Hipoventilación:** iniciar grabación y retener la respiración el mayor tiempo posible *durante* el registro.
5. **Actividad aeróbica:** realizar la actividad física indicada, sentarse rápidamente y grabar de inmediato para capturar la frecuencia cardíaca elevada.
6. Entre cada derivación (DI → DII → DIII) descansar ~30 s–1 min para permitir recolocar los electrodos y que el ritmo cardíaco se estabilice antes de la siguiente toma.
*[Completar con cualquier detalle adicional propio de la sesión: hora, duración exacta de cada actividad aeróbica, observaciones del participante, etc.]*
 
# 4. Ubicación de electrodos por derivación
 
El electrodo de referencia (REF, blanco) se coloca siempre en el punto que **no** se usa como medición en esa derivación, siguiendo el triángulo de Einthoven:
 
| Derivación | IN+ (rojo) | IN− (negro) | REF (blanco) |
|---|---|---|---|
| **DI** | LA (brazo/muñeca izq.) | RA (brazo/muñeca der.) | LF (pierna/tobillo izq.) |
| **DII** | LF (pierna/tobillo izq.) | RA (brazo/muñeca der.) | LA (brazo/muñeca izq.) |
| **DIII** | LF (pierna/tobillo izq.) | LA (brazo/muñeca izq.) | RA (brazo/muñeca der.) |
 
*[Completar: justificar por qué se colocaron los electrodos en huesos/prominencias óseas (muñecas, tobillo) en vez de sobre tejido muscular — reduce el ruido por artefactos de movimiento muscular (EMG) que se superpone a la señal ECG — y por qué esta ubicación específica (muñecas/tobillo vs. clavículas/cresta ilíaca vs. pecho) fue la elegida para esta sesión.]*
 
# 5. Señal en OpenSignals
 
## 5.1 Reposo
 
**DI:**
<p align="center"><img src="images/opensignals_reposo_DI.png" alt="OpenSignals Reposo DI" width="700"><br><em>Fig 1. Señal ECG en reposo, derivación DI, vista en OpenSignals.</em></p>
**DII:**
<p align="center"><img src="images/opensignals_reposo_DII.png" alt="OpenSignals Reposo DII" width="700"><br><em>Fig 2. Señal ECG en reposo, derivación DII, vista en OpenSignals.</em></p>
**DIII:**
<p align="center"><img src="images/opensignals_reposo_DIII.png" alt="OpenSignals Reposo DIII" width="700"><br><em>Fig 3. Señal ECG en reposo, derivación DIII, vista en OpenSignals.</em></p>
## 5.2 Hiperventilación
 
**DI:**
<p align="center"><img src="images/opensignals_hiperventilacion_DI.png" alt="OpenSignals Hiperventilación DI" width="700"><br><em>Fig 4. Señal ECG durante hiperventilación, derivación DI, vista en OpenSignals.</em></p>
**DII:**
<p align="center"><img src="images/opensignals_hiperventilacion_DII.png" alt="OpenSignals Hiperventilación DII" width="700"><br><em>Fig 5. Señal ECG durante hiperventilación, derivación DII, vista en OpenSignals.</em></p>
**DIII:**
<p align="center"><img src="images/opensignals_hiperventilacion_DIII.png" alt="OpenSignals Hiperventilación DIII" width="700"><br><em>Fig 6. Señal ECG durante hiperventilación, derivación DIII, vista en OpenSignals.</em></p>
## 5.3 Hipoventilación
 
**DI:**
<p align="center"><img src="images/opensignals_hipoventilacion_DI.png" alt="OpenSignals Hipoventilación DI" width="700"><br><em>Fig 7. Señal ECG durante hipoventilación, derivación DI, vista en OpenSignals.</em></p>
**DII:**
<p align="center"><img src="images/opensignals_hipoventilacion_DII.png" alt="OpenSignals Hipoventilación DII" width="700"><br><em>Fig 8. Señal ECG durante hipoventilación, derivación DII, vista en OpenSignals.</em></p>
**DIII:**
<p align="center"><img src="images/opensignals_hipoventilacion_DIII.png" alt="OpenSignals Hipoventilación DIII" width="700"><br><em>Fig 9. Señal ECG durante hipoventilación, derivación DIII, vista en OpenSignals.</em></p>
## 5.4 Actividad aeróbica
 
**DI:**
<p align="center"><img src="images/opensignals_aerobica_DI.png" alt="OpenSignals Actividad aeróbica DI" width="700"><br><em>Fig 10. Señal ECG tras actividad aeróbica, derivación DI, vista en OpenSignals.</em></p>
**DII:**
<p align="center"><img src="images/opensignals_aerobica_DII.png" alt="OpenSignals Actividad aeróbica DII" width="700"><br><em>Fig 11. Señal ECG tras actividad aeróbica, derivación DII, vista en OpenSignals.</em></p>
**DIII:**
<p align="center"><img src="images/opensignals_aerobica_DIII.png" alt="OpenSignals Actividad aeróbica DIII" width="700"><br><em>Fig 12. Señal ECG tras actividad aeróbica, derivación DIII, vista en OpenSignals.</em></p>
# 6. Video de muestra
 
📹 *[Insertar enlace o miniatura del video de muestra de la toma de datos, p. ej. colocación de electrodos y/o registro en vivo]*
 
`[Ver video de muestra](videos/video_muestra_lab04.mp4)`
 
# 7. Señal procesada en Python
 
## 7.1 Reposo
 
**DI:**
<p align="center"><img src="images/python_reposo_DI.png" alt="Python Reposo DI" width="700"><br><em>Fig 13. Señal ECG en reposo, derivación DI, procesada en Python.</em></p>
**DII:**
<p align="center"><img src="images/python_reposo_DII.png" alt="Python Reposo DII" width="700"><br><em>Fig 14. Señal ECG en reposo, derivación DII, procesada en Python.</em></p>
**DIII:**
<p align="center"><img src="images/python_reposo_DIII.png" alt="Python Reposo DIII" width="700"><br><em>Fig 15. Señal ECG en reposo, derivación DIII, procesada en Python.</em></p>
## 7.2 Hiperventilación
 
**DI:**
<p align="center"><img src="images/python_hiperventilacion_DI.png" alt="Python Hiperventilación DI" width="700"><br><em>Fig 16. Señal ECG durante hiperventilación, derivación DI, procesada en Python.</em></p>
**DII:**
<p align="center"><img src="images/python_hiperventilacion_DII.png" alt="Python Hiperventilación DII" width="700"><br><em>Fig 17. Señal ECG durante hiperventilación, derivación DII, procesada en Python.</em></p>
**DIII:**
<p align="center"><img src="images/python_hiperventilacion_DIII.png" alt="Python Hiperventilación DIII" width="700"><br><em>Fig 18. Señal ECG durante hiperventilación, derivación DIII, procesada en Python.</em></p>
## 7.3 Hipoventilación
 
**DI:**
<p align="center"><img src="images/python_hipoventilacion_DI.png" alt="Python Hipoventilación DI" width="700"><br><em>Fig 19. Señal ECG durante hipoventilación, derivación DI, procesada en Python.</em></p>
**DII:**
<p align="center"><img src="images/python_hipoventilacion_DII.png" alt="Python Hipoventilación DII" width="700"><br><em>Fig 20. Señal ECG durante hipoventilación, derivación DII, procesada en Python.</em></p>
**DIII:**
<p align="center"><img src="images/python_hipoventilacion_DIII.png" alt="Python Hipoventilación DIII" width="700"><br><em>Fig 21. Señal ECG durante hipoventilación, derivación DIII, procesada en Python.</em></p>
## 7.4 Actividad aeróbica
 
**DI:**
<p align="center"><img src="images/python_aerobica_DI.png" alt="Python Actividad aeróbica DI" width="700"><br><em>Fig 22. Señal ECG tras actividad aeróbica, derivación DI, procesada en Python.</em></p>
**DII:**
<p align="center"><img src="images/python_aerobica_DII.png" alt="Python Actividad aeróbica DII" width="700"><br><em>Fig 23. Señal ECG tras actividad aeróbica, derivación DII, procesada en Python.</em></p>
**DIII:**
<p align="center"><img src="images/python_aerobica_DIII.png" alt="Python Actividad aeróbica DIII" width="700"><br><em>Fig 24. Señal ECG tras actividad aeróbica, derivación DIII, procesada en Python.</em></p>
# 8. Análisis
 
## 8.1 Comparación entre derivaciones (DI, DII, DIII)
 
*[Completar: para cada actividad, comparar la amplitud y morfología del complejo QRS entre las tres derivaciones. Relacionar con el ángulo de cada derivación respecto al vector de despolarización (triángulo de Einthoven) y con la Ley de Einthoven (DII = DI + DIII).]*
 
## 8.2 Comparación entre actividades (reposo, hiperventilación, hipoventilación, actividad aeróbica)
 
*[Completar: comparar frecuencia cardíaca (a partir de los intervalos R-R), presencia de artefactos de movimiento o ruido, y cualquier cambio visible en la morfología de la señal (p. ej. arritmia sinusal respiratoria durante hiperventilación, elevación de la frecuencia cardíaca tras la actividad aeróbica).]*
 
# 9. Respuestas al cuestionario
 
**1. ¿Cuáles son las fuentes de ruido más típicas que afectan al ECG?**
 
*[Respuesta]*
 
**2. ¿Por qué cambia la señal de ECG al cambiar la posición de los sensores (Lead I–III)? ¿Cómo cambian sus componentes?**
 
*[Respuesta]*
 
**3. Describe si hay diferencias importantes en la señal al adquirirla desde distintas ubicaciones corporales (p. ej. muñeca/clavícula/pecho). ¿Cuál podría ser la causa? ¿Esperabas estos cambios?**
 
*[Respuesta]*
 
**4. Los sistemas cardíaco y respiratorio están interconectados. ¿Esperas que distintos tipos de respiración (más rápida, más profunda) influyan en la señal de ECG? Muestra capturas de las señales en las distintas circunstancias respiratorias y describe las variaciones si las hay.**
 
*[Respuesta]*
 
**5. En el Home-Guide #1 (EMG) se vio que distintos niveles de fuerza generan distintas amplitudes en la señal muscular. ¿Cómo influye el movimiento en la señal de ECG?**
 
*[Respuesta]*
 
**6. Según lo aprendido, ¿cómo se puede detectar bradicardia y taquicardia en la señal de ECG?**
 
*[Respuesta]*
 
# 10. Referencias
 
- PLUX Wireless Biosignals. *BITalino (r)evolution Home Guide #2 — Electrocardiography (ECG)*. OD.LB.03.04, 2021.
- Meza, M.; Cáceres, J.A. *Electrocardiograma: Anatomía del corazón, Ondas del ECG, Derivaciones, Características y Arritmias* (material de clase, Introducción a Señales Biomédicas).
- *[Agregar cualquier otra referencia citada en el análisis]*
