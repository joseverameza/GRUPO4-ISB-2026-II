<h1 align="center">Preguntas y Respuestas — Laboratorios 1, 2 y 3</h1>
<p align="center"><strong>Introducción a Señales Biomédicas — Semana 2</strong></p>

> Documento de referencia rápida: reúne únicamente las preguntas de cada laboratorio junto con las respuestas entregadas en los notebooks, sin el resto de teoría, código ni gráficas.

## Índice

1. [Lab 1 — Introducción con PhysioNet](#1-lab-1--introducción-con-physionet)
2. [Lab 2 — Análisis de señales biomédicas con PhysioNet](#2-lab-2--análisis-de-señales-biomédicas-con-physionet)
3. [Lab 3 — Filtros FIR, IIR y Transformada Z](#3-lab-3--filtros-fir-iir-y-transformada-z)

---

# 1. Lab 1 — Introducción con PhysioNet

**Archivo:** `Lab001_Introduccion_Senales_Biomedicas_PhysioNet.ipynb`

### Pregunta inicial

**¿Qué cree que representa `CHANNEL = 0`?**
> Representa el subarreglo de la señal que vamos a analizar, en este caso el 0 (el primero).

### Preguntas de análisis 1

**1. ¿Cuál es la frecuencia de muestreo del registro?**
> La frecuencia de muestra del registro es 360 Hz.

**2. ¿Cuántos canales tiene?**
> Tiene 2 canales, 'MLII' y 'V5'.

**3. ¿Cómo se llama el canal seleccionado?**
> El canal seleccionado se llama 'MLII'.

**4. ¿Cuál es la duración total del registro?**
> La duración total del registro es 1085.56 segundos, aproximadamente 30 minutos.

**5. Si `fs = 360 Hz`, ¿cuántas muestras se adquieren en 1 segundo?**
> En 1 segundo, se adquieren 360 muestras.

**6. ¿Cuántas muestras corresponden a un segmento de 10 segundos?**
> Si en 1 segundo hay 360 muestras, en 10 segundos habrán 3600 muestras.

### Preguntas de análisis 2

**1. ¿La señal presenta una amplitud constante?**
> No hay amplitud constante, varía de -0.6 mV a 1 mV.

**2. ¿Se pueden identificar visualmente complejos QRS?**
> No se puede identificar visualmente complejos QRS debido al largo tiempo en el eje X.

**3. ¿La morfología permanece igual durante todo el registro?**
> Vemos que alrededor de los ~1500 s se ve un pico de amplitud -2.5 mV que no concuerda con la morfología de un ECG.

**4. ¿Se observa ruido?**
> Observando la diferencia de amplitudes durante el eje X, podemos inferir que esto es causado por el ruido ya que deberían estar centrados a 0.

**5. ¿Qué ventajas tiene visualizar una señal durante varios minutos?**
> Al visualizar la señal durante varios minutos podemos ver de manera macro cómo es la amplitud y morfología de la señal completa. Asimismo podemos identificar picos extraños como lo explicado en la pregunta 3.

**6. ¿Qué limitaciones tiene una gráfica demasiado extensa?**
> No nos permite visualizar el complejo de la señal, en este caso el complejo QRS con claridad.

### Preguntas de análisis 3

**1. ¿Dónde se concentra la mayor cantidad de muestras?**
> La mayor cantidad de muestras es entre el intervalo de [-0.4, -0.2] mV.

**2. ¿La distribución parece simétrica?**
> No, la distribución se encuentra más al lado izquierdo, de amplitud negativa.

**3. ¿Existen valores extremos?**
> Hay valores extremos en -0.6 mV y por 0.90 mV.

**4. ¿Qué podría producir valores de amplitud muy diferentes al valor central?**
> Pueden ser causados por ruidos del equipo o por artefactos.

**5. ¿El histograma conserva información temporal? Explique.**
> No, el histograma solo muestra la frecuencia de los valores de mV. No indica cuándo eso ocurre.

### Preguntas de análisis 4

**1. ¿Cuántas muestras existen por segundo?**
> De acuerdo al fs=360 Hz, hay 360 muestras por segundo.

**2. ¿Qué ocurre si aumentamos `fs`?**
> Si se aumenta fs, aumentamos la cantidad de muestras por segundo, significa una mayor resolución de la señal pero toma más tiempo y memoria.

**3. ¿Qué ocurre si disminuimos `fs`?**
> Pasa lo contrario, se pierde la resolución y hay peligro de que incumpla el teorema de Nyquist, que sea menor al doble de frecuencia máxima de la señal.

**4. ¿Qué representa el eje `n` en una señal discreta?**
> El eje "n" representa el índice de muestra en la señal discreta, es decir la posición entera en la secuencia discreta.

**5. ¿Cuál es la relación entre `n`, `fs` y `t`?**
> Se relacionan en la ecuación t[n]= n/fs. Explica el momento en el tiempo en donde la muestra se capturó.

### Preguntas de análisis 5

**1. ¿Puede escuchar claramente la señal?**
> Se escuchan los golpes repetitivos causados por el complejo QRS, pero la intensidad es baja lo que dificulta escucharlos claramente.

**2. ¿El sonido corresponde a un sonido fisiológico real?**
> No, porque hemos convertido la señal del ECG a un archivo de audio .wav. Solo obtenemos la sonificación de datos eléctricos, no acústicos.

**3. ¿Por qué un ECG puede convertirse a WAV?**
> Porque ambos (ECG y WAV) son series de números que cambian con el tiempo, se puede sonificar cualquier registro matemático.

**4. ¿Qué información se conserva al hacer la conversión?**
> Al hacer la conversión, se conserva la información estructural como la forma de onda (QRS) y su periodicidad, así como la frecuencia de muestreo.

**5. ¿Qué información fisiológica podría perderse al escucharla?**
> Se pierde toda información fisiológica de baja amplitud como las ondas P y T, alteraciones del segmento ST e incluso cambio morfológico del complejo QRS.

**6. ¿Qué diferencia existe entre representar un ECG gráficamente y reproducirlo como audio?**
> Al representar un ECG gráficamente se visualiza la escala exacta en gráficas de Amplitud vs tiempo, cuando la frecuencia de valores en el histograma. En reproducción de audio solo percibimos el ritmo del complejo QRS.

### Ejercicio 1 — Cambiar de registro (registro 200 vs. 100)

**Comparación de frecuencia de muestreo, canales y unidades:**
```python
RECORD_2 = "200"
record_2 = wfdb.rdrecord(
    RECORD_2,
    pn_dir=DATABASE
)
fs_2 = record_2.fs

# Frecuencia de muestreo, número de canales y nombre de canales
print("=" * 55)
print("INFORMACIÓN DEL REGISTRO 1")
print("=" * 55)

print(f"Base de datos       : {DATABASE}")
print(f"Registro            : {RECORD}")
print(f"Frecuencia muestreo : {record.fs} Hz")
print(f"Número de muestras  : {record.sig_len}")
print(f"Número de canales   : {record.n_sig}")
print(f"Canales             : {record.sig_name}")
print(f"Unidades            : {record.units}")

duration_total = record.sig_len / record.fs

print(f"Duración total      : {duration_total:.2f} segundos")
print("=" * 55)



print("=" * 55)
print("INFORMACIÓN DEL REGISTRO 2 ")
print("=" * 55)

print(f"Base de datos       : {DATABASE}")
print(f"Registro            : {RECORD_2}")
print(f"Frecuencia muestreo : {record_2.fs} Hz")
print(f"Número de muestras  : {record_2.sig_len}")
print(f"Número de canales   : {record_2.n_sig}")
print(f"Canales             : {record_2.sig_name}")
print(f"Unidades            : {record_2.units}")

duration_total_2 = record_2.sig_len / record_2.fs

print(f"Duración total      : {duration_total_2:.2f} segundos")
print("=" * 55)
```
> Comparando los valores de REGISTRO 1 con REGISTRO 2, vemos que el valor de frecuencia de muestreo, número de muestras, número de canales, unidades y duración total son las mismas. Lo que cambia son los nombres de los canales, en REGISTRO 1 es ['MLII', 'V5'] y en REGISTRO 2 es ['MLII', 'V1'].

**Comparación de morfología y amplitud:**
```python
# Morfología y Amplitud
signal_2 = record_2.p_signal[:, CHANNEL]
t_2 = np.arange(len(signal_2)) / fs_2
N_2 = int(DURATION * fs_2)
seg_2 = signal_2[:N_2]
t_2_seg = t_2[:N_2]

plt.figure(figsize=(15, 5))

plt.plot(t_segment, signal_segment)

plt.xlabel("Tiempo [s]")
plt.ylabel(f"Amplitud [{record.units[CHANNEL]}]")
plt.title(
    f"Segmento ECG — primeros {DURATION} segundos REGISTRO 1"
)

plt.grid(True)
plt.tight_layout()
plt.show()

plt.figure(figsize=(15, 5))

plt.plot(t_2_seg, seg_2)

plt.xlabel("Tiempo [s]")
plt.ylabel(f"Amplitud [{record.units[CHANNEL]}]")
plt.title(
    f"Segmento ECG — primeros {DURATION} segundos REGISTRO 2"
)

plt.grid(True)
plt.tight_layout()
plt.show()
```


> La morfología del REGISTRO 2 difiere en gran medida del REGISTRO 1, visualizamos las ondas P y T con mayor magnitud y ancho, lo que incrementa el intervalo entre cada complejo QRS. Vemos que la amplitud en el REGISTRO 2 es de -2.0 a 1.0 mV, mayor en comparación del -0.45 a 0.9 mV del REGISTRO 1.

**Comparación de distribución (histograma):**
> A comparación de la distribución en el REGISTRO 1 al lado izquierdo con mayor frecuencia en amplitudes negativas, en REGISTRO 2, es para el lado derecho entre el intervalo de -0.5 a 0 mV.

### Ejercicio 2 — Cambiar de canal (canal 1 del registro 200)

**1. ¿Qué nombre tiene el nuevo canal? / 2. ¿Tiene las mismas unidades?**
> El nombre del nuevo canal es 'V1', tiene las mismas unidades de mV.

**3. ¿La morfología es igual? / 4. ¿Qué diferencias observa?**
> Tiene similar morfología del canal 0, en el sentido que en el mismo tiempo se presentan los picos característicos. No obstante, en el canal 1 se presenta mayor ruido que distorsiona la morfología, lo que hace que no esté centrado en 0. Asimismo su rango de amplitud disminuyó a -0.1 a 0.2 mV.

### Ejercicio 3 — Cambiar la duración (5 s vs. 20 s)

**¿Por qué una duración demasiado pequeña o demasiado grande puede dificultar el análisis?**
> En una duración demasiado pequeña como en 5s se ven menos complejos QRS, lo que es mejor para el detalle pero no nos permite visualizar la variabilidad en el tiempo. Por otro lado, en un tiempo largo, es lo contrario, se dificulta la inspección visual fina pero mejor variabilidad.

### Ejercicio 4 — Frecuencia de muestreo ($f_s = 360$ Hz)

**a) Periodo de muestreo $T_s = 1/f_s$**
> Ts = 1/360 = 2.78 ms

**b) ¿Cuántas muestras existen en 5 segundos?**
> Si en 1 segundo hay 360 muestras, en 5 segundos hay 1800 muestras.

**c) ¿Cuántas muestras existen en 10 segundos?**
> Si en 1 segundo hay 360 muestras, en 10 segundos hay 3600 muestras.

**d) ¿Qué sucedería si la frecuencia de muestreo fuese reducida significativamente?**
> Si se reduce mucho, se corre el riesgo de que pierda resolución y que sea menor al límite de Nyquist, lo que causaría distorsión a la señal.

### Ejercicio 5 — Análisis de amplitud

**Media, desviación estándar, máximo, mínimo y rango — ¿qué información proporciona cada parámetro?**
> - Media: Nivel promedio de la señal.
> - Desviación estándar: Dispersión del ciclo cardiaco con el ruido.
> - Máximo: Pico de onda R.
> - Mínimo: Pico de onda S.
> - Rango: El rango total del complejo QRS.
>
> *(Valores calculados: Media = 0.0632, Desv. estándar = 0.4947, Mínimo = -4.8900, Máximo = 4.9400, Rango = 9.8300)*

### Preguntas conceptuales

> ⚠️ **Esta sección quedó incompleta en el notebook entregado.** Solo la pregunta 1 tiene una respuesta, y aparece cortada; las preguntas 2 a 10 no tienen respuesta escrita. Se transcribe tal como está en el archivo para que puedan completarla antes de entregarlo.

**1. ¿Qué es PhysioNet?**
> PhysioNet es una base d *(respuesta incompleta en el notebook)*

**2. ¿Qué diferencia existe entre `DATABASE` y `RECORD`?**
> *(sin respuesta)*

**3. ¿Qué representa `fs`?**
> *(sin respuesta)*

**4. ¿Qué representa `CHANNEL`?**
> *(sin respuesta)*

**5. ¿Qué representa `record.p_signal`?**
> *(sin respuesta)*

**6. ¿Por qué necesitamos construir un eje temporal?**
> *(sin respuesta)*

**7. ¿Qué diferencia existe entre una señal continua y una señal discreta?**
> *(sin respuesta)*

**8. ¿Por qué no debemos interpretar directamente un archivo WAV como si fuera una señal ECG?**
> *(sin respuesta)*

**9. ¿Qué ventajas ofrece trabajar con señales biomédicas reales en lugar de señales sintéticas?**
> *(sin respuesta)*

**10. ¿Qué dificultades encontró durante el laboratorio?**
> *(sin respuesta)*

---

# 2. Lab 2 — Análisis de señales biomédicas con PhysioNet

**Archivo:** `Lab002_Introduccion_Senales_Biomedicas_PhysioNet.ipynb`

### Pregunta inicial

**¿Qué diferencia espera encontrar entre analizar una señal en el dominio del tiempo y analizarla en el dominio de la frecuencia?**
> En el dominio del tiempo podemos analizar cómo se comporta la señal a lo largo del tiempo, mientras que en el dominio de la frecuencia se analiza qué componentes de frecuencia están presentes y cuál es su importancia en la señal.

### Preguntas de análisis 1

**1. ¿Cuál es la frecuencia de muestreo de cada registro?**
> Registro 16265: 128 Hz, Registro 16272: 128 Hz, Registro 16420: 128 Hz.

**2. ¿Los tres registros tienen la misma frecuencia de muestreo?**
> Sí.

**3. ¿Cuántos canales posee cada registro?**
> Todos los registros tienen 2 canales.

**4. ¿Qué señal corresponde al canal `0`?**
> A la señal ECG1.

**5. ¿Cuántas muestras contiene cada registro?**
> Todos los registros contienen 3600 muestras.

**6. ¿Por qué `3600` muestras corresponden aproximadamente a 10 segundos cuando `fs = 360 Hz`?**
> La frecuencia de muestreo es la cantidad de muestras que se toman por segundo, para hallar el tiempo realizamos el cálculo #muestras/fs = 3600/360 = 10 s.

### Preguntas de análisis 2

**1. ¿Las tres señales presentan una morfología similar?**
> Sí, las tres señales presentan una morfología bastante similar.

**2. ¿Cuál presenta mayor variación de amplitud?**
> La señal 16265 con una variación aproximada de amplitud desde -1.00 mV hasta 3.00 mV.

**3. ¿Se observan patrones periódicos?**
> Sí, las 3 señales presentan patrones periódicos.

**4. ¿Se identifican eventos o cambios particulares?**
> Sí. La señal 16272 presenta cambios más notorios en su línea base, especialmente alrededor de 18 s y hacia el final de la señal. En 16265 también aparecen algunos picos de menor amplitud alrededor de 15 s y 23-24 s. La señal 16420 presenta picos de amplitud variada.

**5. ¿Qué información puede obtenerse fácilmente desde la gráfica temporal?**
> La amplitud de los picos, duración de la señal, periodicidad, separación entre eventos, variaciones de la línea base y posibles eventos anormales o cambios puntuales.

### Preguntas de análisis 3

**1. ¿Qué ocurre con el componente cercano a 0 Hz después de eliminar la media?**
> El componente en 0 Hz disminuye considerablemente o desaparece, ya que representa la componente DC asociada al valor medio de la señal.

**2. ¿Qué diferencias observa entre los espectros de los tres registros?**
> Los tres presentan distribuciones diferentes de magnitud. El registro 16265 presenta picos de mayor magnitud y varias componentes importantes en bajas y medias frecuencias. El 16272 concentra componentes importantes principalmente en frecuencias bajas, mientras que el 16420 presenta un espectro más distribuido, especialmente aproximadamente entre 5 y 35 Hz. En 16420 se observa además claramente el efecto de eliminar la componente DC.

**3. ¿En qué intervalo de frecuencias se concentra la mayor parte de la energía?**
> En los tres registros, la mayor parte de la energía se concentra aproximadamente por debajo de 30-35 Hz, siendo especialmente importante la zona de bajas frecuencias.

**4. ¿Por qué la FFT no conserva directamente la información temporal?**
> Porque la FFT transforma la señal del dominio del tiempo al dominio de la frecuencia e indica qué frecuencias están presentes y con qué magnitud.

**5. ¿Qué ventaja ofrece observar una señal en el dominio frecuencial?**
> Permite identificar fácilmente las frecuencias dominantes, componentes periódicas y posibles componentes de ruido, lo que puede ser difícil de identificar en el dominio del tiempo.

*(Frecuencias dominantes calculadas: Registro 16265 = 3.200 Hz, Registro 16272 = 0.142 Hz, Registro 16420 = 9.458 Hz)*

### Preguntas de análisis 4 (STFT)

**1. ¿Qué información adicional proporciona la STFT respecto a la FFT?**
> La STFT permite observar qué frecuencias aparecen y en qué momento aparecen, mientras que la FFT solo muestra qué frecuencias están presentes en toda la señal, sin indicar cuándo ocurren.

**2. ¿En qué momentos se observan cambios de energía?**
> En 16265 la energía es más intensa al inicio y presenta variaciones entre aproximadamente 10-28 s. En 16272 se observan eventos puntuales de alta energía distribuidos en casi todo el registro. En 16420 la energía es más continua, aunque cambia ligeramente alrededor de 20-27 s.

**3. ¿Qué diferencias observa entre los tres espectrogramas?**
> El registro 16265 concentra mayor energía principalmente por debajo de 25-30 Hz, con bandas horizontales bien definidas. El 16272 muestra eventos más breves y localizados en el tiempo, representados por líneas verticales; la energía se concentra principalmente en bajas frecuencias. El 16420 presenta una distribución más amplia y continua de energía, aproximadamente hasta 35-40 Hz, con varias bandas horizontales.

**4. ¿Por qué el registro `16272` utiliza una ventana de 32 muestras?**
> Porque presenta cambios rápidos y eventos cortos en el tiempo. Una ventana pequeña de 32 muestras mejora la resolución temporal, permitiendo localizar mejor cuándo ocurren esos eventos.

**5. ¿Qué podría ocurrir si se utiliza una ventana demasiado grande?**
> Se obtiene una mejor resolución en frecuencia, pero se pierde resolución temporal. Los eventos rápidos podrían aparecer difuminados o mezclados, dificultando identificar el momento exacto en que ocurren.

**6. ¿Qué podría ocurrir si se utiliza una ventana demasiado pequeña?**
> Se mejora la resolución temporal, pero empeora la resolución en frecuencia. Las frecuencias pueden verse menos definidas o más anchas, haciendo más difícil distinguir componentes cercanas entre sí.

### Discusión — Comparación de los tres registros

**¿Qué registro presenta mayor variabilidad?**
> El registro 16272 presenta la mayor variabilidad global debido a los desplazamientos significativos de su línea base (*drift*) y a la presencia de transitorios o artefactos de corta duración.

**¿Qué diferencias aparecen en el dominio temporal?**
> Mientras que 16265 presenta picos QRS bien definidos y de alta amplitud, 16272 muestra fluctuaciones marcadas en el nivel medio, y 16420 exhibe una amplitud más regular y uniforme con variaciones moderadas entre ciclos.

**¿Qué diferencias aparecen en el dominio frecuencial?**
> 16265 concentra picos de gran amplitud en bajas y medias frecuencias (alrededor de 3.2 Hz); 16272 acumula casi toda su energía en una frecuencia muy baja (0.142 Hz), reflejando la deriva de la línea base; 16420 distribuye su energía en un ancho de banda mayor (entre 5 Hz y 35 Hz).

**¿Qué información adicional aporta la STFT?**
> La FFT promedia el contenido frecuencial de toda la señal, perdiendo la coordenada del tiempo. La STFT resuelve esto al mapear el contenido frecuencial en función del tiempo, permitiendo localizar en qué segundo exacto ocurren los cambios bruscos de energía o ruidos transitorios.

**¿Cómo influye el tamaño de ventana?**
> Existe un compromiso (*trade-off*) regido por el principio de incertidumbre: una ventana grande ($n_{perseg}$ alto) mejora la resolución en frecuencia pero pierde resolución temporal; una ventana pequeña ($n_{perseg}$ bajo) precisa el instante exacto de un evento pero ensancha las componentes en el espectro.

### Ejercicios propuestos

**Ejercicio 2.1 — Cambiar el segmento temporal**

1. *¿La morfología cambia?* → La morfología general del complejo QRS no cambia drásticamente porque corresponde al mismo paciente, pero sí pueden variar la amplitud de los picos R, la forma de la onda T o la inclinación de la línea base debido a ligeros artefactos de movimiento o respiración en el nuevo segmento.
2. *¿El espectro cambia?* → Sí, cambia. Aunque el rango general de frecuencias se mantiene similar, la distribución de amplitudes varía según la frecuencia cardíaca instantánea y el nivel de deriva (*drift*) presente en ese tramo específico.
3. *¿La STFT muestra nuevos eventos?* → Sí, la STFT muestra cómo las concentraciones de energía se desplazan a los nuevos instantes de tiempo en los que ocurren los latidos o los artefactos propios de ese tramo.
4. *¿Qué diferencias encuentra respecto al segmento original?* → El segmento original suele presentar una fase de estabilización o mayores variaciones en la componente DC al inicio del registro, mientras que un segmento posterior suele mostrar una señal continua con un ritmo más estable o con diferentes artefactos puntuales.

**Ejercicio 2.2 — Cambiar el tamaño de ventana (registro 16272)**

1. *¿Cuál permite observar mejor los eventos temporales?* → $n_{perseg} = 32$: al ser una ventana más corta, ofrece mayor resolución temporal, lo que permite precisar el momento exacto en que ocurren los eventos o transitorios rápidos.
2. *¿Cuál proporciona mayor detalle en frecuencia?* → $n_{perseg} = 256$: al incluir más muestras por ventana, ofrece mayor resolución frecuencial, permitiendo diferenciar componentes de frecuencia muy cercanas entre sí.
3. *¿Cuál considera más apropiada para este registro? Justifique.* → Para el registro 16272 es más apropiada la ventana de $n_{perseg} = 32$. Dado que esta señal presenta variaciones rápidas, artefactos de corta duración y cambios en la línea base, la prioridad es tener una alta resolución temporal para identificar la ocurrencia de estos eventos sin que se diluyan en el tiempo.

**Ejercicio 2.3 — Comparación FFT vs. STFT**
> La FFT global calcula el contenido frecuencial integrando la señal completa a lo largo de todo el tiempo de registro. Por esta razón, indica qué frecuencias están presentes y con qué magnitud, pero pierde por completo la información de *cuándo* aparecieron. Por el contrario, la STFT divide la señal en pequeños segmentos de tiempo utilizando una ventana deslizante y calcula una FFT para cada uno. Esto genera una representación bidimensional (tiempo-frecuencia) que permite ubicar con exactitud el instante de tiempo en el que aparece, cambia o desaparece cada componente de frecuencia.

### Preguntas conceptuales

**1. ¿Qué es PhysioNet?**
> Es una plataforma y repositorio de acceso abierto respaldado por el MIT que proporciona acceso a bases de datos de señales fisiológicas digitalizadas (como ECG, EEG, etc.) y herramientas de software libre para la investigación biomédica.

**2. ¿Qué información proporciona `record.p_signal`?**
> Es un arreglo bidimensional de NumPy que contiene las muestras procesadas de la señal biomédica expresadas en unidades físicas reales (por ejemplo, milivoltios, mV). La estructura es `[muestras, canales]`.

**3. ¿Qué representa la frecuencia de muestreo `fs`?**
> Representa la cantidad de muestras o lecturas discretas tomadas de la señal analógica por cada segundo de tiempo. Su unidad de medida es el Hertz (Hz).

**4. ¿Qué representa la componente DC de una señal?**
> Representa el valor promedio o valor medio de la señal a lo largo del tiempo. En el espectro de frecuencias corresponde a la amplitud presente en 0 Hz.

**5. ¿Por qué se resta la media antes de analizar el espectro AC?**
> Porque la componente DC suele tener una magnitud muy grande en comparación con las variaciones de la señal. Si no se resta, el pico en 0 Hz distorsiona la escala vertical del gráfico e impide visualizar adecuadamente las componentes frecuenciales de interés.

**6. ¿Qué información proporciona la FFT?**
> Proporciona el espectro de magnitudes de la señal, indicando qué componentes frecuenciales están presentes y cuánta energía tiene cada una en todo el registro, pero sin indicar en qué momento ocurren.

**7. ¿Qué información adicional proporciona la STFT?**
> Proporciona una representación tiempo-frecuencia que permite observar cómo evoluciona el contenido frecuencial de la señal a lo largo del tiempo, indicando qué frecuencias aparecen y en qué instantes exactos ocurren.

**8. ¿Qué relación existe entre `fs`, número de muestras y duración?**
> $\text{Duración } (t) = \dfrac{\text{Número de muestras } (N)}{f_s}$

**9. ¿Qué efecto tiene aumentar el tamaño de ventana de la STFT?**
> Mejora la resolución en frecuencia (permite distinguir componentes de frecuencia muy cercanas), pero empeora la resolución temporal (se difuminan los instantes en que ocurren eventos rápidos).

**10. ¿Por qué no es suficiente utilizar únicamente una representación temporal para analizar completamente una señal biomédica?**
> Porque la representación temporal $x(t)$ superpone todas las componentes en una sola gráfica. Fenómenos como interferencias de la red eléctrica (50/60 Hz), ruidos de baja frecuencia o variaciones espectrales específicas son difíciles de identificar a simple vista en el tiempo y requieren del dominio frecuencial para su correcto análisis y filtrado.

### Reto final — Desarrollo completo (registro 16265)

1. **Registro seleccionado:** 16265.
2. **Frecuencia de muestreo:** 128 Hz.
3. **Canal analizado:** Canal 0, que corresponde a la señal ECG1 (medida en mV).
4. **Gráfica temporal:** al graficar la señal en el tiempo, se ve bastante limpia y estable durante los 28.12 segundos (3600 muestras). Los picos R son muy claros, alcanzan amplitudes cercanas a los 3 mV y mantienen un ritmo constante sin cortes ni saltos raros.
5. **FFT con DC:** al sacar la FFT directa de la señal original, el pico en 0 Hz es gigantesco debido al valor medio (la componente DC) y termina aplastando visualmente al resto de las frecuencias en la gráfica.
6. **FFT sin DC:** al restarle la media a la señal, la gráfica cambia por completo. La componente en 0 Hz desaparece y deja ver claramente que la energía principal del corazón está concentrada en los 3.2 Hz (el ritmo del pulso) y en sus primeros armónicos. Después de los 30 Hz, la energía cae casi a cero.
7. **Espectrograma STFT:** usando una ventana de 256 muestras, el espectrograma muestra franjas horizontales bien continuas y parejas por debajo de los 25-30 Hz. No hay cortes bruscos ni manchas verticales raras, lo que confirma que el contenido de frecuencia es uniforme a lo largo de todo el tiempo analizado.
8. **Comparación de resultados:** el análisis en el tiempo nos deja ver la forma y tamaño del latido, pero oculta qué frecuencias componen la señal. La FFT nos muestra todo el rango de frecuencias presente, pero pierde el tiempo en el que ocurrieron. Finalmente, la STFT junta ambos mundos y nos confirma que esas frecuencias del ECG se mantuvieron estables segundo a segundo sin interferencias.
9. **Respuestas a las preguntas de análisis:**
   - *¿Qué aporta el dominio temporal?* Nos sirve para medir la amplitud de los picos, revisar la forma del latido y darnos cuenta a simple vista si hay desplazamientos en la línea base.
   - *¿Por qué quitamos la componente DC?* Porque su magnitud es enorme comparada con la señal del ECG. Si no se la restamos, el gráfico se satura en 0 Hz y no nos deja analizar el espectro real que nos interesa.
   - *¿Qué ventaja nos da la STFT?* Nos permite localizar exactamente cuándo ocurren los cambios o ruidos en el espectro a lo largo del tiempo, algo que la FFT normal no es capaz de hacer porque promedia todo.
10. **Conclusiones:** el registro 16265 es una señal muy estable, con latidos periódicos claros y casi toda su energía útil concentrada por debajo de los 30 Hz. Quitar la media de la señal es un paso obligatorio antes de hacer cualquier análisis de frecuencias para no saturar los resultados con la componente DC. Es necesario combinar el tiempo, la FFT y la STFT para entender la señal de forma completa, ya que cada método muestra cosas que los otros no pueden ver por sí solos.

---

# 3. Lab 3 — Filtros FIR, IIR y Transformada Z

**Archivo:** `Lab003_Introduccion_Filtros_FIR_IIR_Transformada_Z.ipynb`

### Actividad inicial

**1. ¿Cuál es la frecuencia de muestreo?**
> Frecuencia de muestreo de 250 Hz.

**2. ¿Cuántas muestras contiene la señal?**
> Tiene 2500 muestras, se calcula como duración × tasa de muestreo (N = 10 × 250).

**3. ¿Cuál es su duración?**
> Duración de 10 segundos, calculada como N/fs = 2500/250 = 10 s.

**4. ¿Qué elementos de la morfología ECG puedes reconocer?**
> Con fs = 250 Hz y 70 lpm se observan aproximadamente 11-12 latidos en los 10 s de registro.
> - Onda P: despolarización auricular, se aprecia como pequeña deflexión positiva inicial.
> - Complejo QRS: despolarización ventricular, se aprecia un pico principal de gran amplitud con las deflexiones Q, R y S.
> - Onda T: repolarización ventricular, se aprecia deflexión positiva posterior al complejo QRS.
> - Segmentos e intervalos: segmento PR y segmento ST, líneas isoeléctricas entre P-QRS y QRS-T, intervalo PR e intervalo QT.

**5. ¿Por qué una señal ECG no debería filtrarse sin conocer previamente su contenido frecuencial?**
> Filtrar una señal sin conocer su contenido frecuencial podría distorsionar la señal o eliminar información clínica crítica; es fundamental analizar el espectro de frecuencia para identificar dónde están los componentes de la señal y dónde el ruido (60 Hz de la red eléctrica) antes de diseñar los filtros. La señal ECG concentra su energía en un rango aproximado de 0.5 Hz - 250 Hz, por lo que los filtros diseñados deberían considerar mantener este ancho de banda para conservar detalles morfológicos importantes en la señal. La FFT es la herramienta que permite decidir un corte razonado en vez de arbitrario.

### Preguntas de análisis — Ejercicio 1

**1. ¿Qué frecuencias predominan?**
> La mayor parte de la energía se concentra por debajo de ~15 Hz (aprox. 95 % de la energía espectral acumulada), con picos marcados alrededor de 1-12 Hz.

**2. ¿Qué frecuencia parece corresponder al contenido fisiológico?**
> El rango 0.5-40 Hz corresponde al contenido fisiológico del ECG; en particular el complejo QRS concentra energía entre ~10-25 Hz y las ondas P/T entre 0.5-10 Hz.

**3. ¿Qué frecuencia podría corresponder a ruido?**
> En esta señal sintética limpia no hay una componente de ruido dominante (es una señal "ideal"); en un registro real, componentes por encima de 40-50 Hz (interferencia muscular/EMG) o exactamente en 50/60 Hz (interferencia de línea eléctrica) suelen ser ruido.

**4. ¿Qué frecuencia de corte propondrías?**
> Un filtro pasa-bajos con corte entre 35-40 Hz conserva más del 99 % de la energía fisiológica sin dejar pasar ruido de alta frecuencia innecesario.

**5. ¿Por qué?**
> Ese corte preserva el QRS (que requiere hasta ~25-40 Hz de ancho de banda para no distorsionar su forma) mientras atenúa el ruido de alta frecuencia.

**6. ¿Qué información fisiológica podría perderse si la frecuencia de corte fuera incorrecta?**
> Un corte demasiado bajo (p. ej. 10 Hz) aplanaría y ensancharía el QRS, perdiendo su pendiente característica y afectando la detección de picos R; un corte demasiado alto dejaría pasar ruido sin mejorar la señal.

**7. ¿Qué relación existe entre $f_s$ y la frecuencia máxima observable?**
> Por el teorema de muestreo de Nyquist-Shannon, la frecuencia máxima que puede representarse sin aliasing es fs/2 (frecuencia de Nyquist). Con fs = 250 Hz, el límite es 125 Hz.

### Preguntas de análisis — Ejercicio 2 (FIR vs. IIR)

**1. ¿Por qué seleccionaste ese tipo de filtro?**
> Un pasa-bajos, porque el ruido de interés en este laboratorio (interferencia de alta frecuencia) está por encima de la banda fisiológica del ECG (0.5-40 Hz aprox.).

**2. ¿Cómo determinaste la frecuencia de corte?**
> Se determinó a partir de la FFT de la señal limpia (Ejercicio 1), eligiendo 40 Hz porque conserva más del 99 % de la energía fisiológica.

**3. ¿Qué orden elegiste y por qué?**
> FIR con 101 taps (compromiso entre selectividad y retardo de grupo); IIR Butterworth de orden 4 (suficiente pendiente de atenuación sin comprometer la estabilidad).

**4. ¿Cuál presenta mayor atenuación en la región de rechazo?**
> Para un orden "comparable", el IIR Butterworth logra una transición más abrupta que el FIR, es decir, atenúa más rápido después del corte.

**5. ¿Cómo se comporta la fase?**
> El FIR diseñado con `firwin` tiene fase lineal (por la simetría de sus coeficientes), por lo que introduce solo un retardo constante; el IIR tiene fase no lineal, pero al aplicarlo con `sosfiltfilt` (filtrado bidireccional) se cancela la distorsión de fase, a costa de no poder usarse en tiempo real.

**6. ¿Qué diferencia estructural existe entre FIR e IIR?**
> El FIR calcula la salida solo a partir de entradas pasadas (sin realimentación), mientras que el IIR usa realimentación de salidas pasadas, lo que le da una respuesta al impulso teóricamente infinita.

**7. ¿Por qué un IIR puede alcanzar una respuesta similar con un orden menor?**
> La realimentación permite generar polos que producen pendientes de atenuación pronunciadas con pocos coeficientes, mientras que un FIR necesita muchos más coeficientes (taps) para aproximar la misma pendiente sin retroalimentación.

**8. ¿Qué relación existe entre polos, ceros y respuesta en frecuencia?**
> Los ceros producen atenuaciones (mínimos) en la respuesta en frecuencia cerca de su ubicación angular en el círculo unitario, y los polos producen resonancias/amplificaciones; su ubicación (radio y ángulo) determina la forma completa de |H(e^{jω})|.

**9. ¿Por qué la Transformada Z es útil para estudiar estos filtros?**
> Permite estudiar la estabilidad (polos dentro del círculo unitario), diseñar la función de transferencia H(z)=B(z)/A(z), y obtener la respuesta en frecuencia como caso particular al evaluar H(z) en z=e^{jω}, unificando el análisis temporal y frecuencial del filtro.

*(Resultado cuantitativo: MSE FIR vs. original = 1.385e-06, MSE IIR vs. original = 8.710e-07)*

### Validación biomédica (Ejercicio 3 — señal contaminada a 35 Hz, filtro Butterworth pasa-bajos de 25 Hz)

**1. ¿Disminuyó la interferencia?**
> Sí: el SNR aumentó de aproximadamente 5 dB (señal contaminada) a más de 25 dB después del filtrado, y el pico espectral cercano a 35 Hz desaparece en el espectro de la señal recuperada.

**2. ¿Se conservó la morfología ECG?**
> Sí; visualmente el complejo QRS, la onda P y la onda T se mantienen reconocibles en la señal recuperada, y el MSE/RMSE respecto de `ecg_clean` son muy pequeños en relación con la amplitud típica de la señal.

**3. ¿La FFT confirma la reducción del ruido?**
> Sí: el espectro de la señal recuperada ya no muestra el pico en la banda 25-45 Hz que sí estaba presente en el espectro de la señal contaminada.

**4. ¿El filtro introdujo una distorsión apreciable?**
> Una distorsión mínima y esperable: al usar `sosfiltfilt` no hay desplazamiento de fase, pero al eliminar frecuencias por encima de 25 Hz se suaviza ligeramente el pico más agudo del complejo QRS.

**5. ¿La frecuencia cardíaca aproximada se mantiene?**
> Sí; al detectar los picos R en la señal limpia y en la recuperada (con `nk.ecg_findpeaks`) se obtiene el mismo número de latidos y, por lo tanto, la misma frecuencia cardíaca aproximada, coherente con el valor configurado de 70 lpm.

**6. ¿Qué información fisiológica podría haberse perdido?**
> Con un corte de 25 Hz se atenúan las componentes de muy alta frecuencia del QRS (su pendiente más abrupta); en registros reales esto podría afectar levemente la amplitud máxima del pico R o el análisis de muescas/melladuras finas del QRS, así como el análisis de potenciales tardíos, que requieren mayor ancho de banda.

*(Métricas: MSE = 9.338e-05, RMSE = 0.00966, SNR antes = 5.15 dB, SNR después = 28.45 dB)*

### Análisis de errores de diseño

**Error 1 — Frecuencia de corte demasiado baja (5 Hz)**
1. *Qué se observa:* el complejo QRS se aplana y se ensancha notablemente, perdiendo su forma característica y su pico agudo; el MSE respecto de la señal original aumenta de forma importante comparado con el filtro bien diseñado (MSE = 3.029e-02).
2. *Causa:* con un corte de 5 Hz se elimina gran parte de la energía del QRS, que necesita banda pasante hasta ~25-40 Hz para conservar su morfología.
3. *Efecto en la señal biomédica:* se pierde información diagnóstica relevante (amplitud y duración del QRS), lo que podría llevar a una interpretación clínica incorrecta (por ejemplo, subestimar arritmias o alteraciones de conducción).
4. *Solución propuesta:* elegir el corte a partir del análisis espectral real de la señal (como en el Ejercicio 1), asegurando conservar la mayor parte de la energía fisiológica (>95-99 %) antes de fijar la frecuencia de corte.

**Error 2 — Filtrado con desplazamiento de fase (`sosfilt` en vez de `sosfiltfilt`)**
1. *Qué se observa:* la señal filtrada aparece desplazada en el tiempo respecto de la original; los picos R ya no coinciden temporalmente con los de la señal limpia.
2. *Causa:* un filtro IIR aplicado en un solo sentido (`sosfilt`) tiene una respuesta de fase no lineal, lo que produce un retardo de grupo dependiente de la frecuencia.
3. *Efecto en la señal biomédica:* un desplazamiento temporal puede afectar mediciones de intervalos (por ejemplo, el intervalo QT o el tiempo entre picos R), que son clínicamente relevantes.
4. *Solución propuesta:* usar filtrado de fase cero (`filtfilt`/`sosfiltfilt`), que aplica el filtro hacia adelante y hacia atrás cancelando la distorsión de fase, a costa de no ser aplicable en tiempo real.

### Preguntas integradoras

**1. ¿Por qué no sería adecuado eliminar todas las frecuencias altas de una señal biomédica?**
> Porque parte de la información fisiológica relevante (por ejemplo, la pendiente y el pico del complejo QRS, o transitorios rápidos) se ubica en frecuencias relativamente altas dentro de la banda 0.5-40 Hz; eliminarlas todas distorsionaría o borraría características diagnósticas.

**2. ¿Cómo determinaste la frecuencia de corte?**
> Analizando la FFT de la señal (limpia y/o contaminada) para identificar dónde se concentra la energía fisiológica y dónde aparece la interferencia, y eligiendo un corte que preserve la mayor parte de la energía fisiológica mientras excluye el ruido identificado.

**3. ¿Qué información obtuviste de la FFT?**
> Permitió ver qué componentes de frecuencia dominan la señal, distinguir el contenido fisiológico del ruido (en este caso, un tono de interferencia cerca de 35 Hz) y verificar, después del filtrado, que dicho pico desaparece del espectro.

**4. ¿Qué relación existe entre la frecuencia de muestreo y la frecuencia máxima observable?**
> Por el teorema de Nyquist-Shannon, la frecuencia máxima representable sin aliasing es fs/2; con fs = 250 Hz, el límite es 125 Hz.

**5. ¿Qué ocurre si se utiliza una frecuencia de corte demasiado baja?**
> Se distorsiona/aplana la morfología del ECG (especialmente el QRS), pudiendo perderse información diagnóstica relevante, tal como se observó en la demostración de errores de diseño.

**6. ¿Qué ocurre si se utiliza una frecuencia de corte demasiado alta?**
> El filtro deja pasar ruido de alta frecuencia sin atenuarlo suficientemente, por lo que la señal sigue contaminada y el propósito del filtrado no se cumple.

**7. ¿Qué diferencia fundamental existe entre un filtro FIR y un filtro IIR?**
> El FIR no tiene realimentación (respuesta al impulso finita) y puede diseñarse con fase exactamente lineal; el IIR usa realimentación (respuesta al impulso infinita), lo que le permite alcanzar pendientes de atenuación pronunciadas con menor orden, pero requiere analizar su estabilidad y su fase no es lineal en general.

**8. ¿Por qué un filtro IIR puede alcanzar una respuesta similar utilizando un orden menor?**
> Porque incorpora polos (mediante la realimentación) que generan una atenuación pronunciada cerca de la frecuencia de corte con pocos coeficientes, mientras que un FIR necesita muchos más taps (mayor orden) para lograr una pendiente de transición comparable sin realimentación.

**9. ¿Qué relación existe entre polos, ceros y respuesta en frecuencia?**
> Al evaluar H(z) sobre el círculo unitario (z = e^{jω}), los ceros cercanos al círculo unitario producen atenuaciones/mínimos de magnitud en esas frecuencias, y los polos cercanos al círculo unitario producen picos/resonancias; su distribución conjunta determina toda la forma de |H(e^{jω})|.

**10. ¿Por qué la Transformada Z es útil para estudiar filtros digitales?**
> Porque generaliza el análisis de Fourier a sistemas discretos, permitiendo estudiar la función de transferencia H(z)=B(z)/A(z), evaluar la estabilidad del filtro (polos dentro del círculo unitario) y obtener la respuesta en frecuencia como caso particular (z sobre el círculo unitario).

**11. ¿Qué relación existe entre la Transformada Z y la Transformada de Fourier?**
> La Transformada de Fourier de tiempo discreto es la Transformada Z evaluada exactamente sobre el círculo unitario (z = e^{jω}); es decir, la Transformada Z es una generalización que también captura información sobre estabilidad y comportamiento transitorio, no solo la respuesta en frecuencia en estado estacionario.

**12. ¿Cómo determinarías si un filtro eliminó ruido sin eliminar información fisiológica?**
> Combinando: (a) inspección visual en el dominio temporal (morfología del QRS conservada), (b) comparación de espectros antes/después (desaparición del pico de ruido, energía fisiológica preservada), (c) métricas cuantitativas frente a una referencia limpia cuando existe (MSE, RMSE, SNR), y (d) validación fisiológica específica (frecuencia cardíaca aproximada y presencia de los complejos QRS mantenidas).

### Reflexión final

**"Primero inspecciono la señal, luego ____, después ____, finalmente ____."**
> Primero inspecciono la señal, luego **la analizo en el dominio temporal y frecuencial (FFT) para identificar el contenido fisiológico y el posible ruido**, después **diseño y aplico el filtro (FIR o IIR) con parámetros justificados a partir de ese análisis**, finalmente **valido el resultado comparando señales y espectros, calculando métricas cuantitativas (MSE, RMSE, SNR) y verificando que la información fisiológica relevante (morfología del QRS, frecuencia cardíaca) se conserve**.
>
> En síntesis: el flujo Fourier → espectro → identificación del ruido → diseño del filtro → Transformada Z → función de transferencia → FIR/IIR → implementación → validación biomédica no es una secuencia mecánica de funciones de Python, sino una cadena de decisiones justificadas en las características reales de la señal.
