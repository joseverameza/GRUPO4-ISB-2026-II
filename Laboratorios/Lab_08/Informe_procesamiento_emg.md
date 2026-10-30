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



<p align="center"><img src="./images/fatiga_rms_mnf_mdf.png" alt="RMS, MNF y MDF por contracción" width="700"><br><em>Fig 7. RMS, MNF y MDF en función del número de contracción.</em></p>

# 5. Conclusiones



# 6. Referencias

- Universidad Peruana Cayetano Heredia. *Laboratorio 10 — Extracción de características y clasificación de la sEMG*. Cuaderno práctico del curso Introducción a Señales Biomédicas, 2026.
- PLUX Wireless Biosignals. *BITalino (r)evolution — EMG Sensor Datasheet*.
- B. Hudgins, P. Parker, and R. N. Scott, "A new strategy for multifunction myoelectric control," *IEEE Trans. Biomed. Eng.*, vol. 40, no. 1, pp. 82–94, 1993.
- M. Cifrek, V. Medved, S. Tonković, and S. Ostojić, "Surface EMG based muscle fatigue evaluation in biomechanics," *Clin. Biomech.*, vol. 24, no. 4, pp. 327–340, 2009.