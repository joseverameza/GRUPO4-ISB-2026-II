<h1 align="center">Extracción de características y clasificación de la señal sEMG</h1>
<p align="center"><em>Laboratorio 10 — Introducción a Señales Biomédicas</em></p>

**Grupo 4 — 2026-II**  
**Integrantes:** Astrid Sophia Mejía Barreto, Geraldine Mayte Segura Villarreal, José Fernando Vera Meza y Solangel Macedo Pereira.  
**Fecha de laboratorio:** 04/09/2026.

## Índice

- [1. Introducción](#1-introducción)
- [2. Fundamento teórico del laboratorio](#2-fundamento-teorico-del-laboratorio)
- [3. Procedimiento](#3-procedimiento)
- [4. Resultados](#4-resultados)
  - [4.1 Adquisición y saturación](#41-adquisición-y-saturación)
  - [4.2 Filtrado](#42-filtrado)
  - [4.3 Detección de contracciones](#43-detección-de-contracciones)
  - [4.4 Características](#44-características)
  - [4.5 Clasificación con LDA, SVM y CNN](#45-clasificación-con-lda-svm-y-cnn)
  - [4.6 Análisis de fatiga](#46-análisis-de-fatiga)
- [5. Conclusiones](#5-conclusiones)
- [6. Referencias](#6-referencias)

# 1. Introducción


# 2. Fundamento teórico del laboratorio
# 3. Procedimiento



# 4. Resultados

## 4.1 Adquisición y saturación

| Parámetro | Reposo | Contracciones |
|---|---:|---:|
| Duración | 48.3 s | 122.55 s |
| Rango ADC | 504 a 514 | 0 a 1022 |
| Muestras saturadas | 0 | 32 (0.026 %) |

El rango medible del sensor fue de ±1.632 mV. El valor 1022 se repitió 113 veces, lo que indica que el techo real del conversor está ahí; contándolo, la saturación sube a **145 muestras (0.118 %)**. Todas las ventanas saturadas fueron de contracción.

<p align="center"><img src="./images/senal_mV.png" alt="Señal sEMG en mV" width="700"><br><em>Fig 1. Registros de reposo y de contracciones en mV. Las líneas punteadas marcan el rango del sensor.</em></p>

## 4.2 Filtrado

El filtrado eliminó la interferencia de red sin alterar la banda útil. La potencia en 60 Hz respecto a sus frecuencias vecinas pasó de 6.4 a 0.11 veces en el reposo.

| Medida | Valor |
|---|---:|
| RMS del reposo filtrado (piso de ruido) | 3.60 µV |
| RMS de la prueba filtrada | 141.02 µV |
| SNR aproximada | 31.9 dB |
| Frecuencia mediana en contracción | 75 Hz |
| Potencia de la contracción en 20–150 Hz | 92.6 % |

<p align="center"><img src="./images/psd_tiempo_filtrado.png" alt="Señal antes y después del filtrado" width="700"><br><em>Fig 2. PSD antes y después del filtrado (izquierda) y zoom temporal entre 5 y 9 s (derecha).</em></p>

## 4.3 Detección de contracciones

Con el umbral intra-registro (3.8 µV) se detectaron **11 contracciones** de 4.21 ± 6.03 s. Tres de ellas fueron largas y claras; las demás fueron segmentos cortos con amplitud de ruido, porque el umbral quedó muy cerca del piso de ruido (3.60 µV). Se obtuvieron **979 ventanas**: 588 de reposo, 356 de contracción y 35 de transición.

<p align="center"><img src="./images/contracciones_detectadas.png" alt="Contracciones detectadas" width="700"><br><em>Fig 3. Señal filtrada, envolvente RMS, umbral y contracciones detectadas.</em></p>

## 4.4 Características

| Característica | Reposo | Contracción |
|---|---:|---:|
| RMS (mV) | 0.0035 | 0.1238 |
| ZC | 3.80 | 28.86 |
| WAMP | 3.87 | 109.35 |
| MNF (Hz) | 214.9 | 134.5 |
| MDF (Hz) | 201.3 | 117.1 |
| Potencia 150–450 Hz | 0.641 | 0.303 |

Según la razón de Fisher, las más discriminantes fueron **MNF (2.37), MDF (2.21) y P 150–450 (2.12)**, mientras que las de amplitud quedaron abajo (RMS 0.37). Esto se debe a que las falsas contracciones aumentaron la dispersión del RMS en esa clase. Las características de amplitud fueron redundantes entre sí (correlación de 0.950 a 1.000).

<p align="center"><img src="./images/fisher_boxplot.png" alt="Razón de Fisher y boxplots" width="700"><br><em>Fig 4. Razón de Fisher y distribución de cinco características por clase.</em></p>

## 4.5 Clasificación con LDA, SVM y CNN

| Modelo | Exactitud balanceada |
|---|---:|
| LDA, 15 características | 0.729 ± 0.188 |
| SVM, 15 características | 0.735 ± 0.193 |
| SVM sin amplitud | 0.728 ± 0.188 |
| Un solo umbral en RMS | 0.807 ± 0.193 |
| CNN con espectrogramas (500 ms) | 0.857 ± 0.113 |

La validación por ciclos fue necesaria: el mismo modelo dio **0.871 con KFold aleatorio** y 0.728 con GroupKFold. La CNN obtuvo el mayor promedio, pero usó ventanas más largas y uno de sus pliegues tuvo una sola clase.

<p align="center"><img src="./images/espectrograma_completo.png" alt="Espectrograma del registro completo" width="700"><br><em>Fig 5. Espectrograma del registro de contracciones.</em></p>

<p align="center"><img src="./images/cnn_perdida_confusion.png" alt="Pérdida y matriz de confusión de la CNN" width="700"><br><em>Fig 6. Pérdida de entrenamiento y matriz de confusión de la CNN.</em></p>

## 4.6 Análisis de fatiga

Las tres contracciones reales tuvieron valores muy similares (RMS de 0.213 a 0.282 mV y MDF de 70.3 a 78.1 Hz). Con todas las contracciones analizadas no hubo tendencia significativa (MDF: ρ = 0.16, p = 0.726), por lo que no se evidencia fatiga.
 

<p align="center"><img src="./images/fatiga_rms_mnf_mdf.png" alt="RMS, MNF y MDF por contracción" width="700"><br><em>Fig 7. RMS, MNF y MDF en función del número de contracción.</em></p>

# 5. Conclusiones


# 6. Respuestas al cuestionario

Bloque 1. Fundamentos fisiológicos

1. Con 1 unidad motora, ¿puedes distinguir cada disparo? ¿Y con 60?


2. ¿Qué mecanismo fisiológico representa pasar de 5 a 60 unidades?


3. Si la señal con 60 UM "parece ruido", ¿por qué sigue siendo útil?


Bloque 2. Instrumentación y saturación

4. ¿Qué porcentaje de muestras está saturado? ¿Te parece aceptable?

5. ¿Qué cambiarías en la adquisición para evitarlo?


6. ¿Por qué el baseline sirve como "registro de ruido" del sistema?

Bloque 3. Preprocesamiento

7. ¿Era visible el pico de 60 Hz en el espectro crudo? ¿Desapareció tras el notch?

8. ¿En qué rango de frecuencias se concentra la energía durante la contracción? ¿Coincide con la teoría?

9. ¿Por qué no se recomienda usar un pasa-bajos de 50 Hz para "limpiar" la sEMG?

Bloque 5. Características

10. ¿Qué características quedaron arriba en el ranking de Fisher? ¿ZC, SSC y WAMP miden solo frecuencia?

11. ¿Por qué RMS, MAV, iEMG y VAR obtienen rankings parecidos? ¿Aportan información distinta?

12. ¿Cómo cambia la curtosis en las ventanas saturadas? ¿Qué harías con esas ventanas?
En nuestros datos la curtosis media fue mayor en las ventanas saturadas (2.149 frente a 1.567), lo contrario a lo que predice la teoría, según la cual el recorte de los extremos la reduce. Con solo 25 ventanas saturadas y 32 muestras recortadas, el recorte afecta a muy pocas muestras por ventana, y además estas ventanas son las de mayor intensidad, por lo que no se puede separar el efecto de la saturación del de la intensidad. En el Ejercicio 1, quitar esas ventanas apenas cambió la curtosis media (1.61 a 1.57). En un estudio real se deberían marcar y reportar, y comparar los resultados con y sin ellas, sin eliminarlas sin más, porque corresponden a las contracciones más fuertes.

Bloque 6. Clasificación

13. ¿Bastaría un umbral en RMS? ¿Para qué sirve un vector de muchas características?
En principio sí, y en nuestros datos un solo umbral en RMS (0.807) superó al LDA y la SVM con 15 características (0.729 y 0.735). No llegó a valores cercanos a 1 por las falsas detecciones en las etiquetas. El vector de muchas características se vuelve necesario en tareas más difíciles, como distinguir varios gestos, donde todos implican contracción y la amplitud sola no alcanza.

14. Sin las características de amplitud, ¿sigue habiendo información para separar las clases?
Sí. Sin amplitud se obtuvo 0.715 (LDA) y 0.728 (SVM), prácticamente igual que con todas las características. El espectro de la contracción es claramente distinto del reposo: MDF de 117.1 Hz frente a 201.3 Hz, y 30 % frente a 64 % de potencia en 150–450 Hz.

15. Con solo RMS el LDA rinde peor que la SVM. ¿Qué ocurre con log(RMS)?
El LDA mejoró de 0.576 a 0.663 ± 0.142. El LDA supone clases gaussianas con igual covarianza, pero el RMS del reposo es muy estable (0.004 mV) y el de la contracción muy disperso (desviación 0.199 mV, asimetría 1.71). El logaritmo comprime los valores grandes y acerca los datos a ese supuesto, aunque sin igualar al umbral directo.

16. ¿Hubo diferencia entre KFold aleatorio y GroupKFold? ¿Por qué reportar la validación por grupos?
Sí: 0.871 con KFold frente a 0.728 con GroupKFold. La diferencia de 0.14 muestra la sobreestimación por fuga de datos. La validación por grupos debe reportarse siempre porque es la única que evalúa el modelo con datos realmente nuevos; en estudios con varias personas, el equivalente es dividir por sujeto.

Bloque 7. CNN

17. ¿La CNN superó al LDA? ¿Cuál elegirías para esta tarea?
Numéricamente sí (0.857 frente a 0.729), pero con ventanas de 500 ms en lugar de 250 ms, mucha variación entre pliegues (± 0.113) y un pliegue con una sola clase. Por costo e interpretabilidad elegiríamos el enfoque clásico, pero antes corregiríamos el etiquetado, porque es lo que más limitó el desempeño.

18. ¿Qué pasaría con la CNN si probáramos en otra persona?
No se puede medir con estos datos, que son de un solo sujeto. Es esperable que el desempeño baje, porque la amplitud y el espectro de la sEMG dependen de la anatomía, el tejido adiposo y la posición de los electrodos. Para comprobarlo habría que registrar varias personas y evaluar dividiendo por sujeto.

19. ¿Qué ventaja y qué desventaja tendría una 1D-CNN sobre la señal cruda?
La ventaja es que aprendería directamente de la señal, sin depender de los parámetros de la STFT. La desventaja es que tendría que descubrir por sí sola la estructura en frecuencia, lo que suele exigir más datos y más parámetros.

Para pensar

20. ¿Qué características usarías para una prótesis que debe decidir en menos de 300 ms?
Características rápidas de calcular y estables con ventanas cortas. En el Ejercicio 1, con 100 ms, ZC (1.65) y MDF (1.43) mantuvieron una separación razonable; sin embargo, la MDF fue la más sensible al tamaño de ventana (1.43 a 3.68), por lo que con ventanas cortas conviene apoyarse más en las de complejidad y amplitud.

21. ¿Cómo cambiaría el análisis con varios canales?
El vector de características se calcularía para cada canal y se concatenaría. Esto permite distinguir gestos según qué músculos se activan y con qué intensidad.

22. ¿Qué haría falta para que el modelo funcione en otro sujeto?
Registrar varios sujetos, normalizar la amplitud (por ejemplo, %MVC), estandarizar la colocación de electrodos según SENIAM y evaluar con validación por sujeto.

# 7. Referencias

- Universidad Peruana Cayetano Heredia. *Laboratorio 10 — Extracción de características y clasificación de la sEMG*. Cuaderno práctico del curso Introducción a Señales Biomédicas, 2026.
- PLUX Wireless Biosignals. *BITalino (r)evolution — EMG Sensor Datasheet*.
- B. Hudgins, P. Parker, and R. N. Scott, "A new strategy for multifunction myoelectric control," *IEEE Trans. Biomed. Eng.*, vol. 40, no. 1, pp. 82–94, 1993.
- M. Cifrek, V. Medved, S. Tonković, and S. Ostojić, "Surface EMG based muscle fatigue evaluation in biomechanics," *Clin. Biomech.*, vol. 24, no. 4, pp. 327–340, 2009.
