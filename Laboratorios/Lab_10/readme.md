.
# Extracción de características y clasificación de señales ECG

*Laboratorio 11 — Introducción a Señales Biomédicas*

**Universidad Peruana Cayetano Heredia**

**Grupo 4 — 2026-II**

**Integrantes:** Astrid Sophia Mejía Barreto, Geraldine Mayte Segura Villarreal, José Fernando Vera Meza y Solangel Macedo Pereira.

**Fecha de laboratorio:** 30/10/2026

**Herramientas:** BITalino (r)evolution, OpenSignals, Google Colab y Python.

---

## Índice

1. [Introducción](#1-introducción)
2. [Fundamento teórico del laboratorio](#2-fundamento-teórico-del-laboratorio)
3. [Procedimiento](#3-procedimiento)
4. [Resultados](#4-resultados)
5. [Análisis y discusión](#5-análisis-y-discusión)
6. [Conclusiones](#6-conclusiones)
7. [Referencias](#7-referencias)
8. [Archivos del laboratorio](#8-archivos-del-laboratorio)

---

## 1. Introducción

El electrocardiograma (ECG) permite registrar la actividad eléctrica del corazón mediante electrodos colocados sobre la superficie corporal. Su análisis proporciona información sobre el ritmo cardíaco y la morfología de las ondas que componen cada ciclo cardíaco. Sin embargo, para obtener información cuantitativa confiable, es necesario procesar adecuadamente los registros y distinguir la actividad fisiológica de posibles artefactos o interferencias.

En este laboratorio se trabajó con señales ECG adquiridas mediante BITalino (r)evolution y OpenSignals, empleando Python en Google Colab para su procesamiento y análisis. Se consideraron cuatro condiciones fisiológicas: basal, hiperventilación, hipoventilación y posterior a actividad aeróbica. Para cada condición se registraron las derivaciones DI, DII y DIII, obteniendo un total de 12 archivos.

El procesamiento incluyó la revisión de la calidad de los registros, el filtrado digital, la detección de los complejos QRS y la segmentación de latidos individuales. Posteriormente, se extrajeron características temporales, morfológicas, estadísticas, espectrales y wavelet, utilizadas como entradas para distintos algoritmos de aprendizaje automático.

El objetivo principal fue evaluar si las características extraídas del ECG permiten diferenciar las condiciones fisiológicas estudiadas y comparar el desempeño de cinco clasificadores mediante técnicas de validación cruzada.

### Objetivos específicos

- Evaluar la calidad de los registros ECG obtenidos en las tres derivaciones.
- Aplicar técnicas de filtrado digital para reducir interferencias y artefactos.
- Identificar automáticamente los picos R y calcular los intervalos RR.
- Segmentar los latidos y extraer características relevantes.
- Comparar cinco algoritmos de clasificación mediante métricas de desempeño.
- Analizar la influencia de las características del ritmo y de la morfología sobre la clasificación.

---

## 2. Fundamento teórico del laboratorio

### 2.1. Adquisición y digitalización de la señal ECG

La adquisición del electrocardiograma consiste en medir las diferencias de potencial eléctrico generadas por la actividad cardíaca. Estas diferencias de potencial son captadas mediante electrodos y posteriormente acondicionadas para su conversión a una señal digital.

En el laboratorio se utilizó BITalino (r)evolution junto con el software OpenSignals. Los registros fueron almacenados en archivos de texto que contienen las muestras digitalizadas y metadatos de adquisición, como la frecuencia de muestreo, la resolución del conversor y el canal utilizado.

Se trabajó con las tres derivaciones bipolares de Einthoven:

| Derivación | Diferencia de potencial |
|---|---|
| DI | LA - RA |
| DII | LL - RA |
| DIII | LL - LA |

Donde RA corresponde al electrodo derecho, LA al izquierdo y LL al electrodo inferior izquierdo.

Estas derivaciones permiten observar la misma actividad cardíaca desde diferentes orientaciones. Por ello, la amplitud y polaridad de las ondas pueden variar entre registros sin que esto represente necesariamente una alteración fisiológica.

En el notebook se empleó una frecuencia de muestreo de 1000 Hz y una resolución de 10 bits. La frecuencia de muestreo determina la cantidad de muestras adquiridas por segundo:

$$
f_s=\frac{1}{T_s}
$$

Donde:

- \(f_s\): frecuencia de muestreo, en Hz.
- \(T_s\): período de muestreo, en segundos.

Para una frecuencia de 1000 Hz, el intervalo entre muestras es de 1 ms. Esto permite representar cambios temporales rápidos, como los asociados al complejo QRS.

#### Conversión de las cuentas ADC a milivoltios

Los valores almacenados por el conversor analógico-digital (ADC) deben transformarse a unidades físicas antes de analizar sus amplitudes.

En el laboratorio se utilizó la siguiente expresión:

$$
ECG_{mV}=
\frac{\left(\frac{ADC}{2^n}-\frac{1}{2}\right)V_{CC}}{G_{ECG}}\times1000
$$

Donde:

- \(ADC\): valor digital registrado.
- \(n\): resolución del conversor, igual a 10 bits.
- \(V_{CC}\): tensión de alimentación considerada, 3.3 V.
- \(G_{ECG}\): ganancia del sensor, igual a 1100.

Esta conversión permite expresar la señal en milivoltios y realizar comparaciones cuantitativas de amplitud entre los registros.

### 2.2. Calidad de adquisición y control de artefactos

Antes del procesamiento resulta necesario verificar que los registros tengan una calidad suficiente. Una señal ECG puede contener alteraciones producidas por movimiento, interferencia eléctrica, mal contacto de los electrodos o problemas durante la transmisión de datos.

En este laboratorio se evaluaron principalmente tres aspectos.

**Pérdida de paquetes:** los archivos de OpenSignals incluyen un contador de secuencia denominado `nSeq`, que avanza cíclicamente de 0 a 15. Los saltos inesperados permiten identificar posibles pérdidas durante la transmisión.

**Saturación:** ocurre cuando la amplitud de la señal alcanza los límites del conversor ADC. En estas condiciones se pierde información de la amplitud original y pueden aparecer deformaciones que dificultan la identificación de ondas.

**Intervalos RR anómalos:** se identificaron intervalos que presentaban desviaciones superiores al 35 % respecto de una mediana local, calculada a partir de una ventana de nueve intervalos RR.

El criterio empleado fue:

$$
|RR_i-\widetilde{RR}_{local}|>
0.35\,\widetilde{RR}_{local}
$$

Donde \(\widetilde{RR}_{local}\) representa la mediana local de los intervalos RR.

La presencia de intervalos anómalos puede indicar artefactos, detecciones incorrectas o latidos no identificados. Por ello, estos intervalos no deben interpretarse automáticamente como anomalías cardíacas.

### 2.3. Filtrado digital de la señal ECG

El filtrado digital permite atenuar componentes que interfieren con el análisis sin eliminar, en lo posible, la información fisiológica de interés.

Entre las principales fuentes de interferencia se encuentran la deriva de línea base, el ruido muscular y la interferencia de la red eléctrica.

| Interferencia | Característica | Tratamiento |
|---|---|---|
| Deriva de línea base | Variaciones lentas asociadas a respiración y movimiento | Filtro pasaaltos |
| Ruido de alta frecuencia | Actividad muscular y ruido electrónico | Filtro pasabajos |
| Interferencia eléctrica | Componente asociada a la red de 60 Hz | Filtro notch |

Para el procesamiento se implementó un filtro Butterworth pasabanda de tercer orden, con frecuencias de corte de 0.5 y 40 Hz, seguido de un filtro notch centrado en 60 Hz.

La banda de trabajo seleccionada fue:

$$
0.5\ \text{Hz}\leq f\leq40\ \text{Hz}
$$

El filtro pasabanda busca reducir tanto las variaciones lentas de línea base como las componentes de alta frecuencia que dificultan la identificación de los complejos cardíacos.

Por su parte, el filtro notch atenúa selectivamente la interferencia asociada a la frecuencia de la red eléctrica. En el notebook se utilizó un factor de calidad de 30 para este filtro.

El filtrado se aplicó mediante las funciones `sosfiltfilt` y `filtfilt` de SciPy. Estas realizan el procesamiento en ambas direcciones temporales, evitando el desplazamiento de fase que podría alterar la ubicación de los picos R.

Esto es importante porque una modificación de los tiempos de aparición de las ondas afectaría el cálculo de los intervalos RR y otras características temporales.

### 2.4. Detección automática de los picos R

La detección de los picos R constituye una etapa fundamental del análisis ECG, ya que permite localizar cada ciclo cardíaco y calcular los intervalos entre latidos consecutivos.

En el laboratorio se implementó una versión simplificada del algoritmo de Pan-Tompkins. Este método aprovecha las características del complejo QRS, especialmente sus pendientes pronunciadas y su contenido frecuencial.

El procedimiento comprendió seis etapas:

1. **Filtrado pasabanda de 5 a 15 Hz:** resalta las componentes asociadas al complejo QRS y atenúa parte de las ondas más lentas.
2. **Derivación:** permite destacar los cambios rápidos de amplitud de la señal.
3. **Elevación al cuadrado:** convierte los valores en positivos y aumenta la diferencia entre pendientes pronunciadas y variaciones pequeñas.
4. **Integración mediante ventana móvil de 150 ms:** suaviza la señal transformada y facilita la identificación de regiones compatibles con un complejo QRS.
5. **Aplicación de umbral y distancia mínima:** identifica candidatos que superan un umbral y evita detecciones demasiado próximas.
6. **Localización del pico R:** busca el máximo de la señal ECG filtrada dentro de una ventana de aproximadamente ±100 ms alrededor de cada candidato.

En la implementación se utilizó un umbral equivalente a 0.3 veces el percentil 99 de la señal integrada y una separación mínima de 250 ms entre candidatos.

La derivada discreta puede representarse aproximadamente como:

$$
d[n]=f_s\left(x[n]-x[n-1]\right)
$$

Posteriormente, el cuadrado de esta señal se expresa como:

$$
q[n]=d[n]^2
$$

La integración por ventana móvil permite obtener una representación suavizada de la actividad del QRS:

$$
y[n]=\frac{1}{N}\sum_{k=0}^{N-1}q[n-k]
$$

Donde \(N\) corresponde al número de muestras incluidas en la ventana.

La implementación de la práctica emplea operaciones numéricas equivalentes para resaltar los complejos QRS.

### 2.5. Intervalos RR y frecuencia cardíaca

El intervalo RR corresponde al tiempo transcurrido entre dos picos R consecutivos. A partir de esta medida es posible estimar la frecuencia cardíaca.

$$
RR_i=t_{R,i+1}-t_{R,i}
$$

La frecuencia cardíaca instantánea se calcula mediante:

$$
FC_i=\frac{60}{RR_i}
$$

Donde:

- \(RR_i\): intervalo entre latidos consecutivos, en segundos.
- \(FC_i\): frecuencia cardíaca, en latidos por minuto.

En el procesamiento se utilizaron únicamente los intervalos RR considerados válidos para calcular las frecuencias cardíacas representativas de cada condición.

También se evaluaron los cambios de frecuencia cardíaca entre el inicio y el final de los registros mediante los primeros y últimos cinco intervalos disponibles.

Este procedimiento permite analizar variaciones del ritmo relacionadas con las condiciones experimentales, aunque dichas variaciones deben interpretarse considerando el momento de adquisición y la calidad de cada registro.

### 2.6. Segmentación de los latidos

Una vez detectados los picos R, la señal se divide en segmentos individuales alineados temporalmente respecto de cada pico.

Para el laboratorio se utilizó una ventana de 600 ms:

- 200 ms anteriores al pico R.
- 400 ms posteriores al pico R.

Con una frecuencia de muestreo de 1000 Hz, cada segmento contiene 600 muestras.

Matemáticamente, puede expresarse como:

$$
x_i[k]=x[n_{R_i}+k]
$$

Para:

$$
-200\leq k<400
$$

Donde \(n_{R_i}\) corresponde al índice del pico R de cada latido.

Esta segmentación permite comparar las formas de los latidos y calcular características individuales utilizando una referencia temporal común.

Se excluyeron los primeros y últimos latidos cuando no disponían de intervalos RR a ambos lados. También se descartaron los segmentos asociados a intervalos anómalos o que no contaban con una ventana completa.

### 2.7. Extracción de características del ECG

La extracción de características consiste en representar cada segmento mediante un conjunto reducido de valores numéricos que describen propiedades relevantes de la señal.

El notebook considera 24 características organizadas en cinco familias.

| Familia | Número de características | Información |
|---|---:|---|
| Ritmo cardíaco | 3 | Intervalos RR y frecuencia cardíaca |
| Morfológicas | 7 | Amplitudes y tiempos de las ondas |
| Estadísticas básicas | 5 | Distribución de los valores |
| Estadísticas de orden superior | 2 | Asimetría y curtosis |
| Frecuenciales | 7 | FFT y transformada wavelet |
| **Total** | **24** | **Descripción de cada latido** |

#### 2.7.1. Características del ritmo

Se calcularon tres variables:

- `RR_prev_ms`: intervalo RR anterior.
- `RR_next_ms`: intervalo RR posterior.
- `FC_lpm`: frecuencia cardíaca instantánea.

Estas variables describen la separación temporal entre los latidos y permiten identificar cambios en el ritmo cardíaco.

#### 2.7.2. Características morfológicas

Se extrajeron siete medidas relacionadas con la forma del ciclo cardíaco:

| Característica | Descripción |
|---|---|
| `amp_P` | Amplitud estimada de la onda P |
| `amp_Q` | Amplitud estimada de la onda Q |
| `amp_R` | Amplitud del pico R |
| `amp_S` | Amplitud estimada de la onda S |
| `amp_T` | Amplitud estimada de la onda T |
| `dur_QS_ms` | Separación temporal entre los puntos Q y S |
| `t_RT_ms` | Tiempo transcurrido desde R hasta T |

Las amplitudes se calcularon respecto a una línea isoeléctrica estimada entre 100 y 60 ms antes del pico R.

La identificación de las ondas se realizó mediante ventanas temporales predefinidas alrededor de R. Por ello, estas mediciones deben considerarse aproximaciones computacionales y no equivalentes a una delineación clínica completa del ECG.

#### 2.7.3. Características estadísticas básicas

Las características estadísticas permiten resumir el comportamiento de la amplitud dentro de cada ventana.

Se utilizaron la media, mediana, desviación estándar y percentiles 25 y 75.

La media se define como:

$$
\mu=\frac{1}{N}\sum_{i=1}^{N}x_i
$$

La desviación estándar poblacional se calcula mediante:

$$
\sigma=\sqrt{\frac{1}{N}\sum_{i=1}^{N}(x_i-\mu)^2}
$$

Estas medidas permiten cuantificar la dispersión de los valores y complementar la información morfológica.

#### 2.7.4. Estadísticas de orden superior

Se calcularon la asimetría (*skewness*) y la curtosis.

La asimetría describe el grado de simetría de la distribución de amplitudes, mientras que la curtosis permite caracterizar la presencia de valores extremos y la forma de dicha distribución.

Estas características pueden resultar útiles para diferenciar latidos con picos pronunciados de aquellos que presentan una distribución de amplitudes menos concentrada.

#### 2.7.5. Características espectrales mediante FFT

La transformada rápida de Fourier (FFT) permite analizar el contenido frecuencial de una señal discreta.

La transformada discreta de Fourier se define como:

$$
X[k]=\sum_{n=0}^{N-1}x[n]e^{-j2\pi kn/N}
$$

En el notebook se aplicó una ventana de Hann antes del cálculo de la FFT.

Posteriormente, se obtuvieron tres características:

- `f_dominante_Hz`: frecuencia con mayor potencia dentro de la banda de análisis.
- `potencia_0.5_40`: potencia espectral calculada entre 0.5 y 40 Hz.
- `frac_5_15Hz`: fracción de potencia correspondiente a la banda de 5 a 15 Hz.

Estas variables permiten comparar la distribución de energía espectral de los latidos y complementar las características obtenidas en el dominio temporal.

#### 2.7.6. Características mediante transformada wavelet

La transformada wavelet permite estudiar una señal considerando simultáneamente su evolución temporal y las diferentes escalas frecuenciales.

A diferencia de una representación exclusivamente espectral, este método resulta especialmente útil para analizar señales no estacionarias, como el ECG, que presentan eventos transitorios de corta duración.

En el laboratorio se utilizó la transformada wavelet discreta (DWT) con la familia Daubechies 4 (`db4`) y seis niveles de descomposición.

Las bandas aproximadas consideradas fueron:

| Coeficiente | Banda aproximada |
|---|---|
| cA6 | 0–7.8 Hz |
| cD6 | 7.8–15.6 Hz |
| cD5 | 15.6–31 Hz |
| cD4 | 31–62.5 Hz |

Para cada nivel se calculó una energía relativa respecto de la energía total de los coeficientes de la descomposición:

$$
E_j=\frac{\sum_k|c_j[k]|^2}{\sum_m\sum_k|c_m[k]|^2}
$$

Donde \(c_j[k]\) representa los coeficientes del nivel correspondiente.

Este procedimiento permite representar cómo se distribuye la energía del latido entre distintas escalas. En particular, las bandas asociadas a cD6 y cD5 pueden aportar información sobre componentes rápidas del complejo QRS.

### 2.8. Clasificación mediante aprendizaje automático

Una vez extraídas las características, cada latido se representa como un vector de 24 variables numéricas.

La finalidad del clasificador es aprender patrones asociados a las cuatro condiciones estudiadas: basal, hiperventilación, hipoventilación y posterior a actividad aeróbica.

Se compararon cinco algoritmos:

| Algoritmo | Fundamento |
|---|---|
| Árbol de Decisión (DT) | Divide los datos mediante reglas sucesivas |
| Regresión Logística (LR) | Estima probabilidades de pertenencia a una clase |
| Random Forest (RF) | Combina múltiples árboles de decisión |
| KNN | Clasifica según la proximidad a ejemplos conocidos |
| SVM | Busca fronteras de separación entre clases |

Para la comparación se utilizó un proceso de estandarización mediante `StandardScaler`, incorporado dentro del pipeline de cada modelo.

#### Validación cruzada por bloques

Los latidos consecutivos de un mismo registro presentan una elevada dependencia temporal. Por esta razón, dividirlos aleatoriamente entre entrenamiento y prueba puede generar resultados demasiado optimistas.

Para reducir esta fuga de información se utilizó `StratifiedGroupKFold`, agrupando los latidos en bloques de 10 segundos.

De esta forma, los latidos pertenecientes a un mismo bloque permanecen juntos durante la separación de entrenamiento y prueba.

Este procedimiento permite realizar una evaluación más exigente que la división aleatoria de latidos individuales.

#### Métricas de evaluación

Se utilizaron la exactitud (*accuracy*) y el F1-score macro.

La exactitud corresponde a la proporción de predicciones correctas:

$$
Accuracy=\frac{\text{Predicciones correctas}}{\text{Total de predicciones}}
$$

El F1-score combina precisión y sensibilidad:

$$
F1=2\frac{Precision\cdot Recall}{Precision+Recall}
$$

El F1 macro corresponde al promedio de los valores F1 calculados para cada clase:

$$
F1_{macro}=\frac{1}{K}\sum_{i=1}^{K}F1_i
$$

Donde \(K\) representa el número de clases.

Esta última métrica es especialmente importante porque el número de latidos válidos no fue igual en las cuatro condiciones.

---

## 3. Procedimiento

### 3.1. Organización de los registros

Se utilizaron 12 archivos ECG adquiridos en cuatro condiciones fisiológicas y tres derivaciones.

Los archivos se importaron a Google Colab para identificar automáticamente su condición y derivación. Posteriormente, se revisaron los metadatos de adquisición, la duración de cada registro y las posibles pérdidas de datos.

### 3.2. Procesamiento y selección de señales

Se realizó una inspección visual de las señales ECG y se aplicaron filtros digitales para reducir interferencias.

Posteriormente, se evaluó la calidad de cada registro mediante el porcentaje de intervalos RR anómalos y las muestras saturadas.

A partir de este análisis se seleccionó la derivación DII para la etapa de clasificación, debido a que presentó registros completos en las cuatro condiciones y una calidad comparativamente favorable.

### 3.3. Identificación y segmentación de latidos

Se aplicó el detector de picos R basado en Pan-Tompkins simplificado.

Con los picos detectados se calcularon los intervalos RR y se identificaron los latidos válidos.

Cada latido fue segmentado utilizando una ventana de 600 ms, obteniendo registros individuales alineados respecto al pico R.

### 3.4. Extracción de características

Para cada latido válido se calcularon 24 características distribuidas en cinco familias.

Los resultados fueron organizados en una tabla, donde cada fila representa un latido y cada columna contiene una característica.

Se incorporó además la condición fisiológica correspondiente a cada registro como etiqueta de clasificación.

### 3.5. Entrenamiento y evaluación de clasificadores

Se implementaron cinco modelos de aprendizaje automático: Árbol de Decisión, Regresión Logística, Random Forest, KNN y SVM.

Los modelos se evaluaron mediante validación cruzada por bloques de 10 segundos, utilizando exactitud y F1 macro como métricas de comparación.

Finalmente, se comparó el rendimiento obtenido utilizando todas las características, únicamente las relacionadas con el ritmo y el conjunto que excluye dichas variables.

---

## 4. Resultados

Los siguientes resultados corresponden a las salidas guardadas en el notebook de ECG del grupo, utilizando los registros cargados y la configuración seleccionada.

### 4.1. Evaluación inicial de los registros

Se identificaron 12 archivos correspondientes a las cuatro condiciones y tres derivaciones.

Los registros presentaron duraciones superiores a 30 segundos, con una duración máxima reportada de aproximadamente 43 segundos.

No se identificaron pérdidas de paquetes mediante el contador de secuencia. Sin embargo, se detectaron 274 muestras saturadas en el registro aeróbico de D1.

La evaluación de calidad señaló los siguientes registros que requerían revisión:

| Registro | RR anómalos | Estado |
|---|---:|---|
| Aeróbica D1 | 55.6 % | Problemático |
| Hiperventilación D3 | 39.4 % | Problemático |
| Hipoventilación D3 | 15.8 % | Problemático |
| Aeróbica D3 | 19.0 % | Problemático |
| Hipoventilación D2 | 2.3 % | Revisar |
| Basal D3 | 1.8 % | Revisar |

La derivación D2 presentó tres registros sin intervalos RR anómalos y uno con un porcentaje reducido, correspondiente a hipoventilación.

Esto respaldó su selección para continuar con el procesamiento y clasificación.

### 4.2. Frecuencia cardíaca por condición

A partir de los intervalos RR considerados válidos se obtuvieron las siguientes frecuencias cardíacas medias.

| Condición | D1 (lpm) | D2 (lpm) | D3 (lpm) |
|---|---:|---:|---:|
| Basal | 91.8 | 88.2 | 90.5 |
| Hiperventilación | 103.0 | 115.5 | 88.0 |
| Hipoventilación | 87.7 | 81.4 | 83.1 |
| Aeróbica | 140.8 | 146.4 | 133.7 |

La frecuencia cardíaca más elevada se observó después de la actividad aeróbica, mientras que los valores más bajos correspondieron principalmente a la condición de hipoventilación.

En D2, la frecuencia cardíaca media aumentó desde 88.2 lpm en basal hasta 146.4 lpm en condición aeróbica.

Las diferencias entre derivaciones deben interpretarse considerando que los registros se adquirieron de manera secuencial y no simultánea.

### 4.3. Selección de la derivación DII

Para el procesamiento posterior se seleccionaron los archivos de D2 correspondientes a las cuatro condiciones.

El análisis automático produjo los siguientes resultados:

| Condición | Picos R detectados | Latidos válidos |
|---|---:|---:|
| Basal | 56 | 54 |
| Hiperventilación | 61 | 59 |
| Hipoventilación | 44 | 40 |
| Aeróbica | 75 | 73 |
| **Total** | **236** | **226** |

La exclusión de latidos permitió trabajar con segmentos que cumplían los criterios temporales y de calidad establecidos en el procesamiento.

### 4.4. Segmentación y características extraídas

A partir de los registros de D2 se obtuvo una matriz de 226 latidos válidos, cada uno compuesto por 600 muestras.

Posteriormente, se calcularon 24 características por latido.

La tabla final contiene 226 filas y 27 columnas: 24 características, una etiqueta de clase, el archivo de procedencia y el tiempo del pico R.

Entre los promedios obtenidos destacan:

| Característica | Basal | Hiperventilación | Hipoventilación | Aeróbica |
|---|---:|---:|---:|---:|
| FC instantánea (lpm) | 88.393 | 116.623 | 82.049 | 146.501 |
| Amplitud R (mV) | 0.516 | 0.526 | 0.554 | 0.644 |
| Amplitud T (mV) | 0.133 | 0.167 | 0.136 | 0.521 |
| Tiempo R-T (ms) | 215.130 | 297.915 | 229.725 | 199.000 |
| Energía wavelet cD6 | 0.522 | 0.440 | 0.498 | 0.249 |

Los resultados muestran diferencias cuantitativas en el ritmo y en las características de los latidos.

Estas diferencias constituyen la información utilizada posteriormente por los algoritmos de clasificación.

### 4.5. Comparación de clasificadores

La evaluación se realizó sobre 226 latidos distribuidos en 15 bloques temporales, empleando tres pliegues de validación cruzada.

Los resultados registrados fueron:

| Modelo | Exactitud | F1 macro | Desv. F1 |
|---|---:|---:|---:|
| Árbol de Decisión | 0.805 | 0.646 | 0.126 |
| Regresión Logística | 0.834 | 0.754 | 0.158 |
| Random Forest | 0.877 | 0.764 | 0.160 |
| KNN (k=5) | 0.821 | 0.669 | 0.190 |
| SVM (RBF) | 0.826 | 0.674 | 0.179 |

Random Forest presentó la mayor exactitud y el mayor F1 macro medio, con valores de 0.877 y 0.764, respectivamente.

La Regresión Logística obtuvo un F1 macro de 0.754, muy cercano al de Random Forest.

Por tanto, aunque Random Forest presentó el mejor valor medio, la diferencia entre ambos modelos fue pequeña respecto a la variabilidad observada entre los pliegues.

### 4.6. Influencia de las características del ritmo

Se evaluó el efecto de utilizar diferentes grupos de características.

| Modelo | F1 todas | F1 solo ritmo | F1 sin ritmo |
|---|---:|---:|---:|
| Árbol de Decisión | 0.646 | 0.711 | 0.531 |
| Regresión Logística | 0.754 | 0.708 | 0.646 |
| Random Forest | 0.764 | 0.720 | 0.615 |
| KNN | 0.669 | 0.660 | 0.648 |
| SVM | 0.674 | 0.652 | 0.665 |

En Random Forest, el F1 macro disminuyó de 0.764 a 0.615 al excluir las características relacionadas con el ritmo.

Esto indica que dichas características contribuyeron de manera importante al desempeño del modelo en estos registros.

Sin embargo, la clasificación también conservó un rendimiento superior a la referencia de azar cuando se utilizaron las características restantes.

### 4.7. Efecto del procedimiento de validación

Para analizar la posible fuga de información se compararon dos estrategias de validación sobre Random Forest utilizando las características sin ritmo.

| Procedimiento | F1 macro |
|---|---:|
| División aleatoria de latidos | 0.940 |
| Validación por bloques de 10 s | 0.615 |

La diferencia muestra que la división aleatoria produjo una estimación considerablemente mayor del rendimiento.

Debido a la similitud entre latidos próximos de un mismo registro, esta estrategia puede introducir dependencia entre los conjuntos de entrenamiento y prueba.

Por ello, se consideró más apropiada la validación por bloques para interpretar el desempeño del clasificador.

### 4.8. Experimentos complementarios pendientes

El notebook incluye un cuestionario final que propone comparar derivaciones, polaridad, pares de condiciones, sensibilidad del detector y reducción del número de características.

En las salidas guardadas se observa una ejecución de referencia con D2, inversión automática, umbral de 0.3 y 24 características.

Para completar esta etapa deben ejecutarse las comparaciones adicionales solicitadas en el cuestionario y registrar sus resultados.

No se presentan valores de experimentos que todavía no aparecen ejecutados en el cuaderno.

---

## 5. Análisis y discusión

El procesamiento de las señales ECG permitió identificar diferencias entre las condiciones fisiológicas estudiadas. La variación más evidente correspondió a la frecuencia cardíaca, que presentó valores elevados después de la actividad aeróbica y menores durante la condición de hipoventilación.

La evaluación inicial también evidenció que la calidad de adquisición influye directamente en la detección automática de latidos. El registro aeróbico de D1 presentó saturación y un porcentaje elevado de intervalos RR anómalos, lo que limitó su utilidad para el análisis posterior. En contraste, D2 mostró una calidad más favorable en las cuatro condiciones.

La segmentación de los registros en ventanas de 600 ms permitió representar cada latido mediante características cuantitativas. Además del ritmo, se observaron diferencias en amplitudes morfológicas, tiempos y distribución de energía wavelet. No obstante, algunas de estas variaciones pueden estar relacionadas con cambios de adquisición, posición de electrodos o ruido residual, por lo que deben interpretarse con cautela.

En la clasificación, Random Forest alcanzó el mejor F1 macro medio con 0.764, seguido de Regresión Logística con 0.754. Esta diferencia no resulta suficiente para afirmar una superioridad consistente, considerando la variabilidad entre los pliegues de validación.

Un resultado importante fue la reducción del rendimiento de Random Forest cuando se excluyeron las características del ritmo. Esto sugiere que la frecuencia cardíaca y los intervalos RR contienen información relevante para distinguir las condiciones experimentales. Sin embargo, las características restantes también aportaron capacidad de clasificación.

Asimismo, la comparación entre validación aleatoria y validación por bloques evidenció el riesgo de obtener resultados demasiado optimistas cuando los latidos de entrenamiento y prueba provienen de segmentos temporalmente próximos.

Finalmente, el conjunto de datos corresponde a registros de un participante y una sesión experimental. En consecuencia, el desempeño observado representa la capacidad de los modelos para diferenciar las condiciones de estos registros y no demuestra su aplicabilidad general a otros participantes ni su utilidad diagnóstica.

---

## 6. Conclusiones

1. Se procesaron 12 registros ECG correspondientes a cuatro condiciones fisiológicas y tres derivaciones bipolares. La evaluación inicial permitió identificar problemas de calidad asociados principalmente a intervalos RR anómalos y saturación de muestras.

2. La derivación DII presentó condiciones favorables para continuar el análisis. A partir de sus cuatro registros se identificaron 236 picos R, de los cuales 226 latidos cumplieron los criterios establecidos para la extracción de características.

3. El filtrado digital y la detección de picos R mediante Pan-Tompkins simplificado permitieron construir segmentos individuales de 600 muestras. De cada segmento se extrajeron 24 características relacionadas con el ritmo, la morfología, la distribución estadística y el contenido frecuencial.

4. Random Forest obtuvo el mejor desempeño medio entre los cinco clasificadores evaluados, con una exactitud de 0.877 y un F1 macro de 0.764. No obstante, la cercanía de este resultado con Regresión Logística y la variabilidad entre pliegues impiden establecer una superioridad concluyente.

5. Las características del ritmo contribuyeron de manera importante a la clasificación. Además, la validación por bloques redujo el optimismo observado con la división aleatoria de latidos, destacando la necesidad de considerar la dependencia temporal de las señales biomédicas.

En conjunto, el laboratorio permitió integrar las etapas de adquisición, acondicionamiento, análisis y clasificación del ECG, mostrando tanto las posibilidades del procesamiento digital como las limitaciones asociadas a la calidad y representatividad de los datos.

---

## 7. Referencias

[1] T. Subba y T. Chingtham, "Comparative Analysis of Machine Learning Algorithms With Advanced Feature Extraction for ECG Signal Classification", *IEEE Access*, 2024. DOI: [10.1109/ACCESS.2024.3387041](https://doi.org/10.1109/ACCESS.2024.3387041).

[2] J. Pan y W. J. Tompkins, "A Real-Time QRS Detection Algorithm", *IEEE Transactions on Biomedical Engineering*, vol. BME-32, no. 3, pp. 230–236, 1985.

[3] PLUX Wireless Biosignals, *BITalino (r)evolution — ECG Sensor Datasheet*. Documentación técnica del sensor ECG y conversión de las muestras del ADC.

[4] L. Sörnmo y P. Laguna, *Bioelectrical Signal Processing in Cardiac and Neurological Applications*. Elsevier Academic Press, 2005.

[5] M. Meza, L. De La Cruz y J. A. Cáceres, *Electrocardiograma: Anatomía del corazón, ondas del ECG, derivaciones, características y arritmias*. Material de clase, Universidad Peruana Cayetano Heredia.

[6] M. Meza, *Introducción a la Transformada Wavelet: Multirate, bancos de filtros y aplicaciones biomédicas*. Material de clase, Universidad Peruana Cayetano Heredia.

---

## 8. Archivos del laboratorio

El laboratorio se desarrolló mediante un notebook de Google Colab que contiene los procedimientos de adquisición, procesamiento, extracción de características y clasificación.

**Archivo principal:**

`LAB11_ECG_Extraccion_Caracteristicas_30102026.ipynb`

**Archivo de características generado:**

`caracteristicas_ecg.csv`

El notebook incluye las implementaciones en Python, las visualizaciones de las señales y las tablas de evaluación de los clasificadores.

Para reproducir el procesamiento es necesario disponer también de los archivos originales `.txt` correspondientes a las condiciones y derivaciones registradas mediante OpenSignals.

Se recomienda ejecutar nuevamente el notebook con los archivos originales antes de actualizar los resultados del informe.

