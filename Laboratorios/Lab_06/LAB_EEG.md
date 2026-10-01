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
## 3.1. Actividades realizadas

Se realizaron cinco actividades experimentales para observar las variaciones de la señal EEG ante diferentes condiciones visuales, cognitivas y auditivas. Inicialmente, se registró una señal basal en condiciones de mínima estimulación externa. Posteriormente, se evaluaron los cambios asociados a la apertura y cierre de ojos, la fijación de la mirada en un punto, la resolución de preguntas de diferente dificultad y la escucha de música de distintos estilos e intensidades.

## 3.2. Protocolo de adquisición

1. Conectar el BITalino y comprobar la comunicación con OpenSignals (r)evolution.
2. Conectar el sensor EEG y el cable de referencia a los canales correspondientes.
3. Colocar los electrodos de medición y referencia, verificando su correcta adhesión.
4. Iniciar la adquisición de la señal EEG y comprobar su estabilidad antes de comenzar las actividades.
5. Realizar las actividades experimentales en el orden establecido, registrando las señales correspondientes.
6. Guardar los registros obtenidos en cada condición para su posterior visualización y análisis.

 
# 4. Señal en OpenSignals
 
## 4.1 Lectura basal
 


## 4.2 Apertura y cierre de ojos
 


## 4.3 Mirada fija en un punto
 

## 4.4. Preguntas de dificultad variable
 
## 4.5. Música lofi y música estruendosa


# 5. Señal procesada en Python
 
## 5.1 Lectura basal
 


## 5.2 Apertura y cierre de ojos
 


## 5.3 Mirada fija en un punto
 

## 5.4. Preguntas de dificultad variable
 
## 5.5. Música lofi y música estruendosa


# 6. Análisis

## 6.1. Comparación entre condiciones visuales

[Comparar la señal basal, la apertura y cierre de ojos
y la mirada fija. Describir los cambios observados en
la amplitud, estabilidad y potencia alfa, si se calculó.]

## 6.2. Comparación entre condiciones cognitivas

[Comparar las señales obtenidas durante las preguntas
universitarias y las preguntas sencillas. Considerar
las limitaciones relacionadas con el cambio de BITalino.]

## 6.3. Comparación entre estímulos auditivos

[Comparar los registros obtenidos durante la música
lofi y la música estruendosa. Describir las diferencias
observadas y las posibles fuentes de variabilidad.]

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


