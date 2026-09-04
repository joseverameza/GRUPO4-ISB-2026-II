<h1 align="center">Filtros Digitales, Transformada Z y Señales Biomédicas</h1>
<p align="center"><strong>Semana 2 — Introducción a Señales Biomédicas</strong></p>

## Índice

1. [Resumen de la clase](#1-resumen-de-la-clase)
   - [1.1 Extracción de características](#11-extracción-de-características)
   - [1.2 Concepto de filtro y delta de Kronecker](#12-concepto-de-filtro-y-delta-de-kronecker)
   - [1.3 Transformada bilineal (de *s* a *z*)](#13-transformada-bilineal-de-s-a-z)
   - [1.4 Diagrama de polos y ceros](#14-diagrama-de-polos-y-ceros)
   - [1.5 Frecuencia de Nyquist vs. teorema de Nyquist](#15-frecuencia-de-nyquist-vs-teorema-de-nyquist)
   - [1.6 Filtros FIR](#16-filtros-fir)
   - [1.7 Filtros IIR](#17-filtros-iir)
2. [Resumen de teoría de laboratorio](#2-resumen-de-teoría-de-laboratorio)
   - [2.1 Generación y muestreo de una señal senoidal](#21-generación-y-muestreo-de-una-señal-senoidal)
   - [2.2 Análisis en frecuencia (FFT)](#22-análisis-en-frecuencia-fft)
   - [2.3 Diseño de un filtro Butterworth (SciPy)](#23-diseño-de-un-filtro-butterworth-scipy)
   - [2.4 Introducción a PhysioNet](#24-introducción-a-physionet)
3. [Laboratorios trabajados](#3-laboratorios-trabajados)
   - [3.1 Lab 1 — Introducción con PhysioNet](#31-lab-1--introducción-con-physionet)
   - [3.2 Lab 2 — Análisis de señales biomédicas con PhysioNet](#32-lab-2--análisis-de-señales-biomédicas-con-physionet)
   - [3.3 Lab 3 — Filtros FIR, IIR y Transformada Z](#33-lab-3--filtros-fir-iir-y-transformada-z)
4. [Conclusiones](#4-conclusiones)

---

# Filtros Digitales, Transformada Z y Señales Biomédicas

**Semana 2 — Introducción a Señales Biomédicas**

## Índice

1. [Resumen de la clase](#1-resumen-de-la-clase)
   - [1.1 Extracción de características](#11-extracción-de-características)
   - [1.2 Concepto de filtro y delta de Kronecker](#12-concepto-de-filtro-y-delta-de-kronecker)
   - [1.3 Transformada bilineal (de s a z)](#13-transformada-bilineal-de-s-a-z)
   - [1.4 Diagrama de polos y ceros](#14-diagrama-de-polos-y-ceros)
   - [1.5 Frecuencia de Nyquist vs. teorema de Nyquist](#15-frecuencia-de-nyquist-vs-teorema-de-nyquist)
   - [1.6 Filtros FIR](#16-filtros-fir)
   - [1.7 Filtros IIR](#17-filtros-iir)
2. [Resumen de teoría de laboratorio](#2-resumen-de-teoría-de-laboratorio)
   - [2.1 Generación y muestreo de una señal senoidal](#21-generación-y-muestreo-de-una-señal-senoidal)
   - [2.2 Análisis en frecuencia (FFT)](#22-análisis-en-frecuencia-fft)
   - [2.3 Diseño de un filtro Butterworth (SciPy)](#23-diseño-de-un-filtro-butterworth-scipy)
   - [2.4 Introducción a PhysioNet](#24-introducción-a-physionet)
3. [Laboratorios trabajados](#3-laboratorios-trabajados)
   - [3.1 Lab 1 — Introducción con PhysioNet](#31-lab-1--introducción-con-physionet)
   - [3.2 Lab 2 — Análisis de señales biomédicas con PhysioNet](#32-lab-2--análisis-de-señales-biomédicas-con-physionet)
   - [3.3 Lab 3 — Filtros FIR, IIR y Transformada Z](#33-lab-3--filtros-fir-iir-y-transformada-z)
4. [Conclusiones](#4-conclusiones)

---

# 1. Resumen de la clase

## 1.1 Extracción de características

La extracción de características permite obtener información relevante a partir de una señal registrada. Estas características dependen del fenómeno fisiológico o movimiento que se desea estudiar y de la forma en que el sensor realiza la adquisición. Posteriormente, esta información puede utilizarse en etapas de procesamiento, clasificación o aprendizaje automático.

## 1.2 Concepto de filtro y delta de Kronecker

Una señal discreta puede representarse como una suma de impulsos delta de Kronecker desplazados y escalados. Esta representación resulta útil para estudiar el comportamiento de sistemas y filtros discretos.

En circuitos eléctricos, elementos como capacitores y bobinas modifican la respuesta del sistema en función de la frecuencia y pueden formar parte de diferentes tipos de filtros. Los diagramas de magnitud y fase permiten analizar cómo responde el sistema ante distintas componentes frecuenciales.

En Python, un filtro puede representarse mediante una función de transferencia:

```python
from scipy import signal
import numpy as np
import matplotlib.pyplot as plt

fc = 50
wc = 2 * np.pi * fc

num = [wc]
den = [1, wc]

h_s = signal.TransferFunction(num, den)
```

## 1.3 Transformada bilineal (de *s* a *z*)

La transformada bilineal permite convertir una función de transferencia definida en el dominio continuo $s$ a una representación equivalente en el dominio discreto $z$. Para realizar esta transformación se utiliza la siguiente sustitución:

$$
s = \frac{2}{T}\cdot\frac{1-z^{-1}}{1+z^{-1}}
$$

En Python:

```python
fs = 1e3
ts = 1 / fs

h_d = h_s.to_discrete(ts, method="bilinear")

b = np.squeeze(h_d.num)
a = np.squeeze(h_d.den)
```

En los sistemas discretos es importante analizar la estabilidad antes de implementar un filtro. Además, al aplicar la transformada Z, la convolución en el dominio temporal puede expresarse como una multiplicación en el dominio $z$, facilitando el análisis del sistema.

La respuesta al impulso también permite caracterizar completamente un sistema LTI. Para ello se utiliza como entrada un delta de Kronecker y se observa la salida generada por el sistema.

## 1.4 Diagrama de polos y ceros

Los polos y ceros permiten analizar diferentes características de un sistema discreto:

- En un sistema causal y racional, la estabilidad se cumple cuando todos los polos se encuentran dentro del círculo unitario.
- Los ceros pueden ubicarse en diferentes regiones del plano $z$.
- Los ceros cercanos al círculo unitario producen una mayor atenuación alrededor de determinadas frecuencias.
- Los polos cercanos al círculo unitario pueden aumentar la respuesta del sistema alrededor de determinadas frecuencias y producir resonancia.

```python
z, p, k = signal.tf2zpk(b, a)

plt.scatter(np.real(z), np.imag(z), marker='o', label='Ceros')
plt.scatter(np.real(p), np.imag(p), marker='x', label='Polos')

circle = plt.Circle((0, 0), 1, fill=False)
plt.gca().add_artist(circle)

plt.axhline(0)
plt.axvline(0)

plt.gca().set_aspect('equal', 'box')
plt.legend()
plt.title('Polos y ceros')
plt.show()
```

## 1.5 Frecuencia de Nyquist vs. teorema de Nyquist

Aunque ambos conceptos están relacionados con el proceso de muestreo, representan ideas diferentes:

- **Teorema de Nyquist-Shannon:** para evitar aliasing, la frecuencia de muestreo debe ser mayor que el doble de la frecuencia máxima presente en la señal.
- **Frecuencia de Nyquist ($f_{Nyquist}$):** corresponde a la mitad de la frecuencia de muestreo.

$$
f_{Nyquist} = \frac{f_s}{2}
$$

## 1.6 Filtros FIR

Un filtro FIR (*Finite Impulse Response*) genera cada muestra de salida mediante una combinación finita de muestras actuales y anteriores de la señal de entrada. A diferencia de un filtro IIR, no necesita utilizar muestras anteriores de la salida como realimentación.

De manera general, puede expresarse como:

$$
y[n] = \sum_{k=0}^{M} b_k x[n-k]
$$

**Fenómeno de Gibbs:** al aproximar una respuesta ideal mediante una cantidad finita de coeficientes pueden aparecer oscilaciones o *ripples* cerca de las discontinuidades de la respuesta frecuencial. En el diseño de filtros FIR mediante ventanas, una respuesta al impulso ideal de duración infinita debe truncarse para poder implementarse computacionalmente. Este truncamiento es una de las causas de las ondulaciones observadas en la respuesta.

## 1.7 Filtros IIR

Un filtro IIR (*Infinite Impulse Response*) utiliza tanto muestras de la señal de entrada como valores anteriores de la propia salida. Por esta razón, presenta realimentación.

Una ecuación general puede escribirse como:

$$
y[n] =
\sum_{k=0}^{M} b_k x[n-k]
-
\sum_{k=1}^{N} a_k y[n-k]
$$

- Los coeficientes $b$ están relacionados con las muestras de entrada.
- Los coeficientes $a$ están relacionados con la realimentación del sistema.

La presencia de realimentación hace que un filtro IIR pueda presentar problemas de estabilidad. Por ello, es importante verificar la ubicación de sus polos antes de aplicarlo sobre una señal.

---

# 2. Resumen de teoría de laboratorio

## 2.1 Generación y muestreo de una señal senoidal

Durante el laboratorio se generó una señal senoidal y posteriormente se realizó su muestreo utilizando una frecuencia de muestreo definida.

```python
import numpy as np
import matplotlib.pyplot as plt

f = 100
w = 2 * np.pi * f
A = 1
phi = 0

fs = 10 * f
ts = 1 / fs

n = np.arange(1000)
t = n * ts

x = A * np.sin(w * t + phi)

plt.plot(x)
plt.xlim(0, 50)
plt.show()
```

La frecuencia de muestreo debe elegirse de acuerdo con la frecuencia máxima presente en la señal para evitar el fenómeno de aliasing. Al aumentar $f_s$, se obtiene una mayor cantidad de muestras por período y, por lo tanto, una representación temporal más detallada de la señal.

## 2.2 Análisis en frecuencia (FFT)

Para estudiar las componentes frecuenciales de una señal se utiliza la Transformada Discreta de Fourier (DFT). En la práctica, este cálculo se realiza de forma eficiente mediante el algoritmo FFT (*Fast Fourier Transform*).

```python
plt.magnitude_spectrum(x, scale="dB", Fs=fs)
plt.show()
```

Al representar la señal en el dominio de la frecuencia se observan picos asociados con las distintas componentes frecuenciales presentes. Por esta razón, analizar el espectro mediante FFT permite identificar qué frecuencias predominan en una señal.

Si se combinan señales con diferentes frecuencias, el espectro muestra los picos correspondientes a cada una de sus componentes.

## 2.3 Diseño de un filtro Butterworth (SciPy)

Para reducir componentes frecuenciales no deseadas se pueden utilizar las herramientas disponibles en el módulo `scipy.signal`.

```python
from scipy import signal

sos = signal.butter(
    N=10,
    Wn=90,
    btype='lowpass',
    fs=1000,
    output='sos'
)

filtered = signal.sosfilt(sos, x3)

plt.plot(filtered)
plt.xlim(0, 650)
plt.show()
```

La función `signal.sosfilt()` permite aplicar un filtro representado mediante secciones de segundo orden sobre una señal.

Cuando dos componentes frecuenciales se encuentran muy próximas, puede ser más difícil separarlas completamente debido a la banda de transición del filtro. Si existe una mayor distancia entre las frecuencias, el filtro puede diferenciarlas con mayor facilidad.

El filtro Butterworth se caracteriza por presentar una respuesta de magnitud suave y sin ondulaciones dentro de la banda pasante. La pendiente de transición depende principalmente del orden seleccionado y de la frecuencia de corte.

## 2.4 Introducción a PhysioNet

PhysioNet es una plataforma que proporciona acceso a diferentes bases de datos de señales fisiológicas. Para trabajar con estos registros desde Google Colab se puede utilizar la librería `wfdb` junto con herramientas de procesamiento científico en Python.

```python
!pip install uv
!uv pip install wfdb scipy numpy matplotlib pandas
```

Para cargar una señal es necesario indicar la base de datos y el registro correspondiente. Cada registro contiene información propia, como la frecuencia de muestreo, número de canales, unidades y cantidad total de muestras.

La duración de una señal puede calcularse mediante:

$$
\text{duración} =
\text{número de muestras}\times T_s
=
\frac{\text{número de muestras}}{f_s}
$$

Una señal fisiológica sin procesar puede contener diferentes componentes, como desplazamientos de línea base, ruido o artefactos. La variación natural de amplitud de una señal fisiológica no implica por sí sola que se encuentre contaminada, por lo que es necesario analizarla antes de decidir qué procesamiento aplicar.

La **discretización** permite representar una señal continua mediante muestras tomadas en instantes determinados.

Para realizar una normalización básica puede retirarse primero la componente DC:

$$
x_{AC}[n] = x[n] - \bar{x}
$$

Posteriormente, la señal puede escalarse respecto a su amplitud máxima absoluta:

$$
x_{norm}[n] =
\frac{x_{AC}[n]}
{\max\left(|x_{AC}[n]|\right)}
$$

---

# 3. Laboratorios trabajados

Durante la semana se desarrollaron tres notebooks de Jupyter de manera progresiva. Primero se realizó la exploración de una señal biomédica obtenida desde PhysioNet. Luego se profundizó en su análisis temporal y frecuencial mediante FFT y STFT. Finalmente, se trabajó con el diseño, aplicación y evaluación de filtros digitales FIR e IIR.

## 3.1 Lab 1 — Introducción con PhysioNet

**Archivo:** `Lab001_Introduccion_Senales_Biomedicas_PhysioNet.ipynb`  
**Tema:** importación, exploración, visualización y exportación de un ECG real.  
**Dataset:** `mitdb` (MIT-BIH Arrhythmia Database), registro `100`, canales `MLII` y `V5`, $f_s = 360$ Hz.

### Actividades desarrolladas

- Se accedió a una base de datos fisiológica disponible en PhysioNet mediante la combinación **DATABASE + RECORD**, considerando que cada registro pertenece a una base de datos específica.
- Se utilizó `wfdb.rdrecord()` para importar el registro y revisar información como frecuencia de muestreo, cantidad de canales, nombres de los canales, unidades y duración.
- Se seleccionó un canal mediante `record.p_signal[:, CHANNEL]`.
- Se construyó el eje temporal utilizando:

$$
t[n] = \frac{n}{f_s}
$$

- Se realizaron cuatro representaciones principales: ECG completo, segmento ampliado de 10 segundos, histograma de amplitudes y representación discreta de las primeras 100 muestras.
- Se calcularon parámetros estadísticos básicos, incluyendo media, desviación estándar, mínimo, máximo y rango.
- Se convirtió la señal ECG a formato `.wav`, retirando previamente la componente DC, normalizando la amplitud y convirtiendo los datos a `int16`.
- La conversión a WAV se utilizó únicamente como una forma de reproducir la información contenida en la señal y no significa que el ECG sea originalmente una señal acústica.
- Se realizaron ejercicios modificando el registro, canal y duración del segmento analizado.
- Se reforzaron conceptos relacionados con PhysioNet, `DATABASE`, `RECORD`, `fs`, `CHANNEL` y las diferencias entre una señal continua y una señal discreta.

## 3.2 Lab 2 — Análisis de señales biomédicas con PhysioNet

**Archivo:** `Lab002_Introduccion_Senales_Biomedicas_PhysioNet.ipynb`  
**Tema:** representación temporal, FFT y STFT de señales fisiológicas.  
**Dataset:** `nsrdb` (MIT-BIH Normal Sinus Rhythm Database), registros `16265`, `16272` y `16420`, utilizando 3600 muestras de cada registro y el canal `0`.

### Actividades desarrolladas

- Se importaron los tres registros mediante `wfdb` junto con sus anotaciones `.atr`.
- Se identificaron parámetros como frecuencia de muestreo, número de muestras y cantidad de canales.
- Se representaron las tres señales en el dominio temporal.
- Se calculó la FFT de cada registro antes y después de retirar la componente DC.

La componente AC se obtuvo mediante:

$$
x_{AC}[n] = x[n] - \bar{x}
$$

- Se compararon los espectros antes y después de eliminar la media.
- Se identificó la frecuencia dominante de cada registro, excluyendo la componente de 0 Hz para evitar seleccionar nuevamente la componente DC.
- Se calculó la **STFT** (*Short-Time Fourier Transform*) y se representó el espectrograma de cada señal.
- Para los registros `16265` y `16420` se utilizó `nperseg=256`.
- Para el registro `16272` se utilizó `nperseg=32` con el objetivo de observar con mayor detalle eventos localizados temporalmente.
- Se estudió el compromiso entre resolución temporal y resolución frecuencial: una ventana más corta mejora la localización temporal, mientras que una ventana más larga proporciona una mejor resolución en frecuencia.
- Se compararon características como frecuencia de muestreo, número de muestras, amplitud, componente DC, frecuencias dominantes y comportamiento en la STFT.
- Se realizaron ejercicios comparando tamaños de ventana de `32`, `128` y `256` muestras.
- Finalmente, se analizaron las diferencias entre FFT y STFT, destacando que la FFT describe el contenido frecuencial global, mientras que la STFT permite observar cómo cambia dicho contenido a lo largo del tiempo.

## 3.3 Lab 3 — Filtros FIR, IIR y Transformada Z

**Archivo:** `Lab003_Introduccion_Filtros_FIR_IIR_Transformada_Z.ipynb`  
**Tema:** diseño, aplicación y validación de filtros digitales sobre una señal ECG.  
**Señal de trabajo:** ECG sintético generado mediante `neurokit2` con `duration=10 s`, `sampling_rate=250 Hz` y `heart_rate=70 bpm`.

El procesamiento desarrollado durante el laboratorio siguió la siguiente secuencia:

> Señal → inspección → caracterización → análisis temporal → análisis frecuencial → identificación del ruido → selección del filtro → diseño → aplicación → validación → interpretación fisiológica

### Ejercicio 1 — Análisis y caracterización de la señal

- Se realizó la inspección inicial de un archivo `.wav` o, en su defecto, de una señal ECG sintética.
- Se identificaron la frecuencia de muestreo, número de muestras y duración.
- Se realizó la representación de la señal en el dominio temporal.
- Posteriormente, se calculó la FFT utilizando:

```python
np.fft.rfft()
np.fft.rfftfreq()
```

- El análisis frecuencial permitió observar componentes fisiológicas de la señal e identificar posibles componentes de ruido.

### Ejercicio 2 — Diseño y comparación de filtros FIR e IIR

Se diseñó un filtro **FIR pasa-bajos** mediante `signal.firwin`, utilizando 101 coeficientes y una frecuencia de corte de 40 Hz.

También se diseñó un filtro **IIR Butterworth pasa-bajos** de cuarto orden con frecuencia de corte de 40 Hz. Para su implementación se utilizó la representación mediante secciones de segundo orden (**SOS**), que proporciona una mayor estabilidad numérica que la representación directa mediante coeficientes `(b, a)`, especialmente en filtros de mayor orden.

Los filtros se aplicaron mediante:

```python
signal.filtfilt()
signal.sosfiltfilt()
```

Estas funciones realizan filtrado hacia adelante y hacia atrás, evitando el desplazamiento de fase neto.

Los resultados obtenidos con ambos filtros se compararon visualmente y mediante el cálculo del **error cuadrático medio (MSE)**.

También se evaluaron sus respuestas en frecuencia mediante:

```python
signal.freqz()
signal.sosfreqz()
```

Las principales diferencias consideradas fueron:

| Característica | FIR | IIR |
|---|---|---|
| Realimentación | No | Sí |
| Respuesta al impulso | Finita | Infinita |
| Estabilidad | Generalmente estable | Debe verificarse |
| Fase lineal | Puede diseñarse con fase lineal | Generalmente no |
| Orden requerido | Mayor | Menor |
| Costo computacional | Generalmente mayor | Generalmente menor |

### Ejercicio 3 — Procesamiento de una señal contaminada

Se construyó una señal ECG contaminada mediante una interferencia senoidal:

$$
x[n] = ECG[n] + A\sin(2\pi f_{noise}t)
$$

La interferencia utilizada fue de 35 Hz.

- Se calculó la FFT de la señal contaminada para identificar la componente no deseada.
- Se realizó una búsqueda automática del pico dentro de la banda de 25 a 45 Hz.
- Una vez identificada la interferencia, se diseñó un filtro Butterworth pasa-bajos de cuarto orden con frecuencia de corte de 25 Hz.
- El filtrado se realizó mediante `sosfiltfilt`.
- La recuperación de la señal se evaluó cuantitativamente mediante MSE, RMSE y SNR.

### Validación cuantitativa

Se utilizaron las siguientes métricas:

**MSE:**

$$
MSE =
\frac{1}{N}
\sum_{n=0}^{N-1}
\left(x[n]-\hat{x}[n]\right)^2
$$

**RMSE:**

$$
RMSE = \sqrt{MSE}
$$

**SNR:**

$$
SNR =
10\log_{10}
\left(
\frac{P_{señal}}
{P_{ruido}}
\right)
$$

Se compararon los valores obtenidos antes y después del filtrado para determinar si la interferencia había sido reducida.

### Validación biomédica

Además de las métricas numéricas, se verificó que el procesamiento conservara características importantes de la señal ECG, especialmente:

- La morfología general del complejo QRS.
- La ubicación temporal de los picos R.
- La frecuencia cardíaca aproximada.

También se estudiaron dos errores de diseño.

**Frecuencia de corte demasiado baja:** al utilizar una frecuencia de corte de 5 Hz se eliminaron componentes importantes del ECG, produciendo un aplanamiento y ensanchamiento del complejo QRS.

**Desplazamiento de fase:** al utilizar `sosfilt` se observó un desplazamiento temporal de la señal filtrada. Este efecto modificó la posición de los picos R respecto a la señal original.

El problema se corrigió utilizando:

```python
signal.sosfiltfilt()
```

Este método permite aplicar un filtrado de fase cero y conservar la alineación temporal de las principales características de la señal.

---

# 4. Conclusiones

Los conceptos estudiados permitieron relacionar la teoría de sistemas discretos con el procesamiento de señales biomédicas. La representación mediante el delta de Kronecker facilita el análisis de señales discretas, mientras que la transformada bilineal permite convertir una función de transferencia del dominio continuo $s$ al dominio discreto $z$. Además, el análisis de polos y ceros permite evaluar la estabilidad del sistema y comprender cómo un filtro modifica diferentes componentes frecuenciales.

Los laboratorios desarrollados permitieron aplicar estos conceptos de manera progresiva. En el Lab 1 se trabajó con la importación, exploración y representación de señales fisiológicas obtenidas desde PhysioNet. En el Lab 2 se amplió este análisis utilizando herramientas como la FFT y la STFT para estudiar el contenido frecuencial y su variación en el tiempo. Finalmente, en el Lab 3 se diseñaron y compararon filtros FIR e IIR, evaluando su comportamiento mediante métricas cuantitativas y verificando la conservación de características fisiológicas relevantes de la señal ECG.

A partir de estos ejercicios se comprobó que el filtrado de una señal biomédica no consiste únicamente en seleccionar una función disponible en una librería. Es necesario analizar previamente las características de la señal, identificar las componentes que se desean conservar o eliminar y seleccionar correctamente el tipo de filtro, su orden y su frecuencia de corte. Asimismo, la validación debe considerar tanto métricas numéricas como la preservación de la información fisiológica de interés.
