<h1 align="center">Electroencefalografía (EEG)</h1>
<p align="center"><em>Laboratorio 6 — Introducción a Señales Biomédicas</em></p>

# Índice
 


# 1. Introducción
La electroencefalografía (EEG) registra la actividad eléctrica del cerebro mediante
electrodos colocados sobre el cuero cabelludo. La señal proviene principalmente de las
neuronas piramidales de la corteza, cuya orientación perpendicular a la superficie
cortical hace que sus potenciales postsinápticos sean lo bastante intensos como para
detectarse desde el cuero cabelludo. Por ello, cada electrodo refleja la actividad de
la región cerebral que tiene debajo.

La señal EEG se analiza por bandas de frecuencia:

| Banda | Frecuencia (Hz) | Asociación típica |
|-------|-----------------|-------------------|
| Delta | 0 – 4 | Sueño profundo |
| Theta | 4 – 8 | Somnolencia, carga cognitiva (p. ej. tarea N-back) |
| Alpha | 8 – 12 | Relajación con ojos cerrados; se suprime al abrir los ojos o con actividad mental |
| Beta | 12 – 25 | Mente activa, concentración |
| Gamma | > 25 | Resolución de problemas, concentración |

Las posiciones de los electrodos se describen con el **sistema internacional 10-20**,
donde la letra indica el lóbulo (F: frontal, T: temporal, C: central, P: parietal,
O: occipital), los números impares corresponden al hemisferio izquierdo, los pares al
derecho y la "z" a la línea media.

**Objetivos del laboratorio:**
- Realizar adquisiciones EEG en tiempo real con el sistema BITalino.
- Observar cómo cambia la señal según el estado o la tarea (ojos abiertos/cerrados,
  carga cognitiva, estímulo auditivo).
- Familiarizarse con las bandas de frecuencia de interés, en particular alpha y beta.
- Identificar los artefactos que afectan al registro y cómo minimizarlos.

 
# 2. Materiales
- Software OpenSignals (r)evolution
- BITalino (r)evolution Core BT
- Sensor EEG ensamblado (configuración bipolar, pines IN+ e IN−)
- Electrodos autoadhesivos desechables de Ag/AgCl con gel (2 para el sensor, 1 para la
  referencia)
- Antifaz/gafas para cubrir los ojos, audífonos


# 3. Procedimiento
### 3.1 Configuración experimental

1. Se conectó el BITalino Core BT a OpenSignals (r)evolution y se verificó la conexión.
2. Se conectaron el sensor EEG y el cable de referencia a dos canales analógicos.
3. Se limpió la piel con alcohol para retirar partículas y mejorar la conductividad,
   y se colocaron los electrodos con gel en los dos snaps del sensor y en la referencia.
4. El sensor se ubicó en la frente, sobre la posición [FP1 / FP2 / O2: completar]
   del sistema 10-20, y la referencia sobre una zona ósea detrás de la oreja.


### 3.2 Control del entorno

Dado que la señal es muy sensible a artefactos, se tomaron estas medidas:

- Se apagaron las luces y el participante se ubicó de espaldas a la fuente de luz,
  para eliminar estímulos visuales externos. Asimismo se le tapo los ojos y se colocó audifonos en el odio. 
- El participante no habló ni movió la boca o la mandíbula, evitando artefactos EMG.
- Se evitaron movimientos oculares rápidos y parpadeos.


### 3.3 Fases de la dinámica

| Fase | Descripción |
|------|-------------|
| Lectura basal | Registro en reposo, ojos cerrados, sin movimiento, audífonos colocados|
| Apertura y cierre de ojos | 5 ciclos de 5 s por estado, ojos tapados, audífonos colocados |
| Mirada fija en un punto| 30 s, ojos destapados, audifonos colocados|
| Preguntas de dificultad variable | Preguntas susurradas (nivel universitario y luego fáciles), 1 oreja libre |
| Música | Lofi vs. música estruendosa (1–1:30 min por canción), ojos tapados, audífonos colocados |

 
# 4. Señal en OpenSignals
 
## 4.1 Lectura basal
 
<p align="center"><img src="images/opensignals_basal.jpeg" alt="OpenSignals Basal" width="700"><br><em>Fig X. Señal EEG en basal, vista en OpenSignals. </em></p>
 


## 4.2 Apertura y cierre de ojos
 


## 4.3 Mirada fija en un punto
 

## 4.4. Preguntas de dificultad variable
 
## 4.5. Música lofi y música estruendosa


# 5. Señal procesada en Python
 
## 5.1 Lectura basal
**Tiempo**
<p align="center"><img src="images/phyton_basal_tiempo.png" alt="Phyton Basal Tiempo" width="700"><br><em>Fig X. Señal EEG en basal, Amplitud vs. Tiempo, procesada en Python. </em></p>

**Espectrograma**
<p align="center"><img src="images/phyton_basal_espectograma.png" alt="Phyton Basal Espectrograma" width="700"><br><em>Fig X. Señal EEG en basal, Espectrograma, procesada en Python. </em></p>


## 5.2 Apertura y cierre de ojos
 


## 5.3 Mirada fija en un punto
 

## 5.4. Preguntas de dificultad variable
 
## 5.5. Música lofi y música estruendosa


# 6. Análisis

<p align="center"><img src="images/phyton_frecuencias.png" alt="Phyton Basal Espectrograma" width="700"><br><em>Fig X. Densidad espectral de potencia de las señales, procesada en Python. </em></p>

<p align="center"><img src="images/phyton_bandpower.png" alt="Phyton Basal Espectrograma" width="700"><br><em>Fig X. Porcentakes de banda en señales, procesada en Python. </em></p>




## 6.1. Lectura basal



## 6.2. Apertura y cierre de ojos


## 6.3. Mirada fija en un punto


## 6.4 Preguntas de dificultad variable

## 6.5 Música lofi y música estruendosa

## 6.4. Comparación general

[Integrar los hallazgos de las diferentes actividades,
identificando los cambios observados en las señales
EEG y las limitaciones experimentales.]


# 7. Respuestas al cuestionario
 
**1. ¿Cuáles son las frecuencias significativas para las adquisiciones de EEG? ¿Son las mismas en todas las áreas cerebrales?**
 

 
**2. ¿Qué tipo de filtro es esencial al trabajar con señales de EEG? ¿Por qué es necesario aplicar dicho filtro?**
 

 
**3. ¿Es posible influir en la señal de EEG mediante los pensamientos? ¿Qué acción se puede realizar para activar una banda de frecuencia específica? ¿Fue posible visualizar el cambio en la señal?**
 

 
**4. Muestre una captura de pantalla de una parte relevante de los datos de EEG obtenidos en el experimento propuesto. ¿Corresponde esta señal a lo que esperaba? ¿Por qué?**
 


**5. ¿Existe alguna diferencia en la señal entre las dos ubicaciones, FP1 y FP2?**
 

 
**6. ¿Qué frecuencias deberían cambiar durante las tareas asignadas? ¿Se pueden observar cambios específicos en la señal en bruto (RAW)? Describa lo que observa.**
 

**7. Según su criterio, ¿la amplitud del EEG se corresponde con el nivel de concentración aplicado?**



# 8. Referencias

- PLUX Wireless Biosignals, *BITalino (r)evolution Home Guide #3 — Electroencephalography (EEG): Exploring Brain Signals*, OD.LB.04.05, 2021.


