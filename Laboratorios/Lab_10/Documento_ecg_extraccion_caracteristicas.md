<h1 align="center">Extracción de Características para Clasificación de Señales ECG</h1>
<p align="center"><em>Laboratorio 11 — Introducción a Señales Biomédicas</em></p>

**Grupo 4 — 2026-II**  
**Integrantes:** 
- Solangel Macedo Pereria
- Astrid Sophia Mejía Barreto
- Geraldine Mayte Segura Villarreal
- José Vera Meza


## Índice

- [1. Introducción](#1-introducción)
- [2. Teoría de clase](#2-teoría-de-clase)
  - [2.1 Acondicionamiento y filtrado de la señal](#21-acondicionamiento-y-filtrado-de-la-señal)
  - [2.2 Extracción de características y machine learning](#22-extracción-de-características-y-machine-learning)
  - [2.3 Señal ECG: distancias y complejo QRS](#23-señal-ecg-distancias-y-complejo-qrs)
  - [2.4 Parámetros normales del electrocardiograma](#24-parámetros-normales-del-electrocardiograma)
  - [2.5 Familias de extracción de características](#25-familias-de-extracción-de-características)
- [3. Procedimiento](#3-procedimiento)
  - [3.1 Parte A — Conociendo los datos](#31-parte-a--conociendo-los-datos)
  - [3.2 Parte B — Extracción de características y clasificación](#32-parte-b--extracción-de-características-y-clasificación)
- [4. Resultados](#4-resultados)
  - [4.1 Parte A — Calidad de los registros y elección de derivación](#41-parte-a--calidad-de-los-registros-y-elección-de-derivación)
  - [4.2 Parte B — Características y clasificación](#42-parte-b--características-y-clasificación)
- [5. Cuestionario final](#5-cuestionario-final)
- [6. Conclusiones](#6-conclusiones)
- [7. Referencias](#7-referencias)

# 1. Introducción

En este laboratorio se procesó el ECG propio del grupo, registrado en las tres derivaciones de Einthoven (DI, DII, DIII) bajo cuatro condiciones fisiológicas (reposo, hiperventilación, hipoventilación y actividad aeróbica), para extraer características de cada latido y clasificarlos según la condición en que fueron registrados, siguiendo el flujo del artículo base (Subba y Chingtham, 2024).

**Objetivo:** extraer características morfológicas, estadísticas, de orden superior y espectrales (FFT y wavelet) de los latidos del ECG propio, y comparar el desempeño de cinco algoritmos de clasificación (Árbol de Decisión, Regresión Logística, Random Forest, KNN y SVM) para distinguir la condición fisiológica a partir de dichas características.

# 2. Teoría de clase

## 2.1 Acondicionamiento y filtrado de la señal

El acondicionamiento de la señal comprende el filtrado analógico, la amplificación y la adquisición de datos, previos al filtrado digital.

Para el filtrado digital se retoma lo visto en DSP: el uso de un circuito RC y el divisor de tensión, con el uso de impedancias. Usando el divisor de tensión para obtener $V_{out}$ a partir de $V_{in}$ y planteándolo en $s$ (con $s = j\omega$), se obtiene una función de transferencia de primer orden.

## 2.2 Extracción de características y machine learning

Como parte del procesamiento posterior al filtrado se encuentran las wavelets (extracción de características, *feature extraction*) y el machine learning, siguiendo el proceso CRISP-DM.

### Datos tabulares

Las características se pueden extraer de forma manual, organizándolas en una tabla, o de forma automática mediante machine learning. Para entender mejor los datos se pueden graficar hasta 2 o 3 características a la vez: por ejemplo, si se extraen 30 características, estas se pueden resumir a 2 o 3 mediante algoritmos de reducción de dimensión como **PCA** y **t-SNE**, visualizando así los patrones existentes. Al aplicar el modelo propiamente dicho (por ejemplo, una CNN de una dimensión), esto permite obtener una buena métrica.

## 2.3 Señal ECG: distancias y complejo QRS

Tradicionalmente se trabaja con hoja cuadriculada para medir distancias sobre la señal ECG; en la práctica esto lo usan más los cardiólogos y normalmente no se emplea en este curso. En su lugar, se puede usar la distancia entre picos R-R mediante la segunda derivada.

Otra característica es el complejo QRS, obtenido con el algoritmo phantom. La librería **NeuroKit** ya implementa este algoritmo para medir dichas distancias ([neuropsychology.github.io/NeuroKit/functions/ecg.html](https://neuropsychology.github.io/NeuroKit/functions/ecg.html)), y resume en sus funciones lo visto desde el inicio del curso sobre filtrado.

## 2.4 Parámetros normales del electrocardiograma

Al trabajar con una señal de ECG hay que verificar si los puntos característicos están dentro de rangos normales:

- **Intervalo PR prolongado (> 0.20 s):** puede indicar un bloqueo AV de primer grado.
- **Complejo QRS ensanchado (> 0.12 s):** sugiere un bloqueo de rama o arritmia ventricular.
- **Intervalo QT prolongado:** riesgo de torsades de pointes y otras arritmias.
- **Elevación del segmento ST:** puede indicar infarto agudo de miocardio.

Por ello, al trabajar con ECG conviene construir una tabla con las distancias de los distintos puntos y verificar que se encuentren en el rango normal. Además, para obtener información completa del paciente es necesario visualizar las 12 derivaciones.

## 2.5 Familias de extracción de características

Dentro de feature extraction se trabajan las siguientes familias:

- Estadísticas de orden superior (HOS).
- Extracción de características morfológicas (dominio temporal).
- CWT (transformada wavelet continua) y STFT (transformada de Fourier de tiempo corto).

# 3. Procedimiento



## 3.1 Parte A — Conociendo los datos



## 3.2 Parte B — Extracción de características y clasificación



# 4. Resultados

## 4.1 Parte A — Calidad de los registros y elección de derivación



## 4.2 Parte B — Características y clasificación



# 5. Cuestionario final



# 6. Conclusiones



# 7. Referencias

1. T. Subba y T. Chingtham, "Comparative Analysis of Machine Learning Algorithms With Advanced Feature Extraction for ECG Signal Classification", *IEEE Access*, 2024. doi: 10.1109/ACCESS.2024.3387041
2. J. Pan y W. J. Tompkins, "A Real-Time QRS Detection Algorithm", *IEEE Trans. Biomed. Eng.*, vol. BME-32, no. 3, pp. 230–236, 1985.
3. PLUX Wireless Biosignals, *BITalino (r)evolution — ECG Sensor Datasheet* (fórmula de conversión).
4. L. Sörnmo y P. Laguna, *Bioelectrical Signal Processing in Cardiac and Neurological Applications*, Elsevier, 2005.
5. NeuroKit2 — documentación de funciones ECG: [neuropsychology.github.io/NeuroKit/functions/ecg.html](https://neuropsychology.github.io/NeuroKit/functions/ecg.html)
6. *[Agregar cualquier otra referencia citada en el análisis]*