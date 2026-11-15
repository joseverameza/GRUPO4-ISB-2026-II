<h1 align="center">Tratamiento de la señal EEG, ritmos cerebrales, filtro ICA y extracción de características</h1>
<p align="center"><em>Laboratorio 12 — Introducción a Señales Biomédicas</em></p>

**Grupo 4 — 2026-II**  
**Integrantes:** 
- Solangel Macedo Pereria
- Astrid Sophia Mejía Barreto
- Geraldine Mayte Segura Villarreal
- José Vera Meza


## Índice

- [1. Introducción](#1-introducción)
- [2. Teoría de clase](#2-teoría-de-clase)

- [3. Procedimiento](#3-procedimiento)

- [4. Resultados](#4-resultados)
- [5. Cuestionario final](#5-cuestionario-final)
- [6. Conclusiones](#6-conclusiones)
- [7. Referencias](#7-referencias)

# 1. Introducción

El electroencefalograma (EEG) es una técnica no invasiva que registra la actividad eléctrica del cerebro mediante electrodos colocados sobre el cuero cabelludo. Su principal ventaja es la resolución temporal, es útil para estudiar el sueño, la carga cognitiva, la epilepsia y las interfaces cerebro-computadora (BCI) [1]. No obstante, la señal es de apenas unos microvoltios y está expuesta a artefactos de mayor amplitud que la actividad cerebral, por lo que requiere un procesamiento cuidadoso antes de ser interpretada.

En este laboratorio se procesa las señales EEG adquiridas en el LAB 06 con el sistema BITalino bajo diferentes condiciones:lectura basal, mirada fija en un punto (ojos abiertos), apertura y cierre de ajos, música relajante y estresante. El objetivo es limpiar las señales EEG, analizar sus ritmos cerebrales y de este manera, extraer características que permitan distinguir el estado de la persona. 


# 2. Teoría de clase

## 2.1 Origen y características de la señal EEG

El EEG refleja la suma de potenciales postsinápticos que ocurren de forma sincronizada en poblaciones de neuronas piramidales de la corteza cerebral [1]. Un electrodo no capta neuronas individuales, sino la actividad coherente de grandes grupos. Antes de llegar al sensor, la señal atraviesa meninges, líquido cefalorraquídeo, cráneo y piel; por ello, su amplitud es muy pequeña, del orden de microvoltios [2].

Asimismo, el EEG es una señal no estacionaria. A diferencia del ciclo P-QRS-T del ECG, que se repite con una morfología casi constante, sus propiedades estadísticas cambian en el tiempo según la actividad mental. Por ello, se analiza en ventanas cortas en las que la señal puede considerarse casi estacionaria [3].


## 2.2 Bandas de frecuencia del EEG

Separar la señal por bandas permite asociar cada rango de oscilación con un estado funcional distinto [4].

| Banda | Frecuencia (Hz) | Asociación típica |
|-------|-----------------|-------------------|
| Delta | 0.5 – 4 | Sueño profundo |
| Theta | 4 – 8 | Somnolencia, carga cognitiva |
| Alfa | 8 – 12 | Relajación con ojos cerrados; se suprime al abrir los ojos o con actividad mental |
| Beta | 12 – 25 | Mente activa, concentración |
| Gamma | > 25 | Integración sensorial y percepción consciente |



## 2.3 Artefactos

Los artefactos son señales no cerebrales que contaminan el registro y suelen superar en amplitud al EEG [5]:

- **Ocular (EOG):** parpadeos y movimientos oculares; afectan sobre todo a los canales frontales y se confunden con actividad lenta (delta/theta).
- **Muscular (EMG):** contracción de mandíbula, cara y cuello; aporta energía de alta frecuencia que contamina beta y gamma.
- **Cardíaco (ECG):** espigas periódicas asociadas al latido.
- **Red eléctrica:** interferencia a 60 Hz en Perú.


## 2.4 Preprocesamiento y filtrado

Se usan filtros pasa-altas (0.5–1 Hz, elimina la deriva de línea base), pasa-bajas (40–45 Hz, recorta el ruido muscular y térmico) y notch (60 Hz, rechaza la red eléctrica). En EEG se aplica filtrado de fase cero para evitar retardos temporales en la señal [6].

Cuando el artefacto comparte banda con el EEG (por ejemplo, parpadeos y ondas delta/theta), un filtro en frecuencia no basta y se usa el análisis de componentes independientes (ICA), que separa la señal en fuentes independientes para eliminar las de origen no cerebral [7]. Los filtros espaciales CAR (resta el promedio de todos los electrodos) y Laplaciano (resta el promedio de los vecinos) requieren varios canales [8].


## 2.5 Densidad espectral de potencia (PSD)

La PSD indica cómo se distribuye la potencia de la señal en cada frecuencia (µV²/Hz). Una FFT simple de todo el registro es un estimador ruidoso, por lo que se usa el método de Welch [9]: se divide la señal en segmentos superpuestos, se aplica una ventana a cada uno, se calcula la FFT, se obtiene el periodograma modificado y se promedian los periodogramas, lo que reduce la varianza del espectro.

Para visualizarla se expresa en decibelios, ya que las frecuencias lentas tienen mucha más potencia que las rápidas y en escala lineal estas últimas quedarían ocultas:

$$P_{dB}(\omega) = 10 \log_{10}\big(P(\omega)\big)$$

## 2.6 Extracción de características para Machine Learning

Dar al clasificador miles de puntos crudos (canales × frecuencia de muestreo × tiempo) provoca sobreajuste, por lo que se comprime cada ventana en un vector compacto de descriptores [10]. Las características se agrupan en cinco dominios [11]:

| Dominio | Pregunta que responde | Ejemplos |
|---|---|---|
| Tiempo | ¿Qué amplitud o forma tiene la onda? | Media, varianza, cruces por cero, parámetros de Hjorth |
| Frecuencia | ¿Cuánta energía hay en cada ritmo? | Potencia absoluta y relativa (Welch) |
| Tiempo-frecuencia | ¿Cuándo ocurre un cambio de frecuencia? | Wavelets (DWT, CWT), STFT |
| Espacial / red | ¿Qué zonas se activan o sincronizan? | CSP, coherencia, PLV, PLI |
| No lineal | ¿Qué tan compleja es la señal? | Entropía diferencial, entropía muestral, dimensión fractal de Higuchi |







# 3. Procedimiento



# 4. Resultados


# 5. Cuestionario final

# 6. Conclusiones



# 7. Referencias


[1] Lotte, F., Bougrain, L., Cichocki, A., et al. (2018). A review of classification algorithms for EEG-based brain–computer interfaces: a 10 year update. *Journal of Neural Engineering*, 15(3), 031005. https://doi.org/10.1088/1741-2552/aab2f2

[2] Nunez, P. L., & Srinivasan, R. (2006). *Electric Fields of the Brain: The Neurophysics of EEG* (2.ª ed.). Oxford University Press.

[3] Kaplan, A. Y., Fingelkurts, A. A., Fingelkurts, A. A., Borisov, S. V., & Darkhovsky, B. S. (2005). Nonstationary nature of the brain activity as revealed by EEG/MEG. *Signal Processing*, 85(11), 2190–2212.

[4] Klimesch, W. (1999). EEG alpha and theta oscillations reflect cognitive and memory performance: a review and analysis. *Brain Research Reviews*, 29(2–3), 169–195.

[5] Urigüen, J. A., & Garcia-Zapirain, B. (2015). EEG artifact removal—state-of-the-art and guidelines. *Journal of Neural Engineering*, 12(3), 031001.

[6] Widmann, A., Schröger, E., & Maess, B. (2015). Digital filter design for electrophysiological data. *Journal of Neuroscience Methods*, 250, 34–46.

[7] Makeig, S., Bell, A. J., Jung, T.-P., & Sejnowski, T. J. (1996). Independent component analysis of electroencephalographic data. *Advances in Neural Information Processing Systems 8*, 145–151.

[8] McFarland, D. J., McCane, L. M., David, S. V., & Wolpaw, J. R. (1997). Spatial filter selection for EEG-based communication. *Electroencephalography and Clinical Neurophysiology*, 103(3), 386–394.

[9] Welch, P. D. (1967). The use of fast Fourier transform for the estimation of power spectra. *IEEE Transactions on Audio and Electroacoustics*, 15(2), 70–73.

[10] Jenke, R., Peer, A., & Buss, M. (2014). Feature extraction and selection for emotion recognition from EEG. *IEEE Transactions on Affective Computing*, 5(3), 327–339.

[11] Boonyakitanont, P., Lek-uthai, A., Chomtho, K., & Songsiri, J. (2020). A review of feature extraction and performance evaluation in epileptic seizure detection using EEG. *Biomedical Signal Processing and Control*, 57, 101702.
