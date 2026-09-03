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

# 1. Resumen de la clase

## 1.1 Extracción de características

La extracción de características es el paso más importante para el Deep Learning: las características se adquieren mediante un sensor, de acuerdo con cada movimiento o proceso fisiológico que se desea registrar.

## 1.2 Concepto de filtro y delta de Kronecker

Cualquier señal puede expresarse en función del delta de Kronecker. En un circuito, cada vez que aparece un capacitor o una bobina, es probable que ese componente esté actuando como un filtro. El diagrama de fase y el diagrama de magnitud indican el ancho de banda en el que va a trabajar el sistema.

En Python, un filtro puede definirse como una función de transferencia:

```python
from scipy import signal
import numpy as np
import matplotlib.pyplot as plt

fc = 50
wc = 2 * np.pi * fc
num = wc
den = [1, wc]
h_s = signal.TransferFunction(num, den)  # sistema LTI en tiempo continuo
```

## 1.3 Transformada bilineal (de *s* a *z*)

La transformada bilineal convierte la función de transferencia del dominio $s$ al dominio $z$, mediante el reemplazo:

$$s = \frac{2}{T}\cdot\frac{1-z^{-1}}{1+z^{-1}}$$

En Python:

```python
fs = 1e3   # 1 kHz
ts = 1 / fs
h_d = h_s.to_discrete(ts, method="bilinear")

a = -h_d.den[1:]
b = h_d.num
```

Todo sistema LTI debe ser **acotado** (estable); de lo contrario, una computadora no puede representarlo ni procesarlo. Al trabajar en el dominio $z$ la operación de convolución se convierte en una multiplicación, lo que simplifica el análisis. La forma más sencilla de caracterizar un filtro es observar su respuesta al impulso mediante la función delta de Kronecker: la salida obtenida ante ese impulso es, por definición, la función matemática del filtro.

## 1.4 Diagrama de polos y ceros

- Si todos los polos se encuentran dentro del círculo unitario, el sistema es **estable**.
- Los ceros pueden ubicarse en cualquier posición del plano.
- Los ceros **atenúan** frecuencias.
- Los polos **amplifican** frecuencias o generan **resonancia**.

```python
z, p, k = signal.tf2zpk(b, a)

plt.scatter(np.real(z), np.imag(z), marker='o', label='Ceros')
plt.scatter(np.real(p), np.imag(p), marker='x', label='Polos')

circle = plt.Circle((0, 0), 1, color='r', fill=False)
plt.gca().add_artist(circle)
plt.axhline(0)
plt.axvline(0)
plt.gca().set_aspect('equal', 'box')
plt.legend()
plt.title('Polos y ceros')
plt.show()
```

## 1.5 Frecuencia de Nyquist vs. teorema de Nyquist

No deben confundirse:

- **Teorema de Nyquist:** la frecuencia de muestreo debe ser mayor o igual al doble de la frecuencia máxima de la señal.
- **Frecuencia de Nyquist ($f_{Nyquist}$):** es la mitad de la frecuencia de muestreo.

## 1.6 Filtros FIR

Un filtro FIR corresponde a la sumatoria de muchas muestras desplazadas de la señal. Cada desplazamiento de $n$ muestras puede interpretarse como una derivada de orden $n$.

**Fenómeno de Gibbs:** la respuesta en magnitud aparece en forma de ondulaciones (*ripples*) cuando se aproxima, mediante un sistema discreto, una respuesta ideal que originalmente es continua/analógica. Los filtros FIR trabajan con el **método de ventana**: se parte de una respuesta al impulso ideal a una frecuencia de corte normalizada, pero como la computadora solo puede representar un número finito de muestras, esa respuesta ideal debe truncarse con una ventana. Al forzar una respuesta ideal sobre un componente discreto aparecen los ripples como consecuencia directa de ese truncamiento.

## 1.7 Filtros IIR

La ecuación de un filtro IIR relaciona la salida con muestras pasadas de la entrada **y** con muestras pasadas de la propia salida (realimentación):

- Las **entradas** siempre se asocian con los coeficientes $b$.
- Las **salidas** (realimentación) se asocian con los coeficientes $a$.

Los filtros IIR agregan un riesgo de **inestabilidad**; por ello siempre se debe verificar el diagrama de polos y ceros antes de aplicarlos.

# 2. Resumen de teoría de laboratorio

## 2.1 Generación y muestreo de una señal senoidal

En Google Colab se trabajó primero la generación de una señal senoidal y su muestreo:

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

La frecuencia de muestreo debe ser, como mínimo, el doble de la frecuencia máxima de la señal (teorema de Nyquist). Al aumentar la frecuencia de muestreo mejora la resolución de la gráfica de la señal senoidal.

## 2.2 Análisis en frecuencia (FFT)

Se utiliza la Transformada Discreta de Fourier (DFT) —calculada de forma eficiente mediante el algoritmo FFT— para observar las características espectrales de la señal, recorriendo desde la muestra 0 hasta la última muestra disponible:

```python
plt.magnitude_spectrum(x, scale="dB", Fs=fs)
plt.show()
```

En el dominio frecuencial se identifica un pico correspondiente a la frecuencia de la señal. El primer paso frente a cualquier señal nueva es siempre observar su espectro (FFT) y analizar sus componentes. Al sumar una segunda señal, en el `magnitude_spectrum` aparecen ambos picos correspondientes a cada frecuencia presente.

## 2.3 Diseño de un filtro Butterworth (SciPy)

Para eliminar componentes de frecuencia no deseadas se utiliza el paquete `scipy.signal`:

```python
from scipy import signal

sos = signal.butter(N=10, Wn=90, btype='lp', fs=1000, output='sos')
filtered = signal.sosfilt(sos, x3)
plt.plot(filtered)
plt.xlim(0, 650)
```

`signal.sosfilt(filtro, señal)` aplica el filtro diseñado sobre la señal.

**Observación importante:** si dos componentes de frecuencia están muy cercanas entre sí (por ejemplo, un pico en 100 Hz y otro en 80 Hz, con el filtro cortando en 80 Hz), el filtro deja pasar parte de la componente no deseada de 100 Hz, afectando el resultado. Cuando la separación entre frecuencias es mayor (por ejemplo, 120 Hz y 20 Hz), el filtro las separa de forma mucho más efectiva y la señal resultante se aproxima a una sinusoidal pura.

La elección del tipo de filtro depende de la pendiente de transición deseada y de qué tan cerca se necesita estar de un filtro ideal. El filtro Butterworth utilizado deja pasar ciertas potencias residuales cerca de la frecuencia de corte; su objetivo es eliminar componentes de frecuencia no deseadas de la señal.

## 2.4 Introducción a PhysioNet

PhysioNet es una plataforma con bases de datos de señales fisiológicas reales. Se instala mediante:

```bash
!pip install uv
!uv pip install wfdb scipy numpy matplotlib pandas
```

Para cargar una señal es necesario definir la base de datos y el *record* correspondiente, además de indicar el canal, la duración y la amplitud de interés. Al cargar un registro, este trae asociada su propia información (frecuencia de muestreo, canales, etc.).

$$\text{tiempo de la señal} = \text{número de muestras} \times T_s = \frac{\text{número de muestras}}{f_s}$$

Al graficar la señal cruda se observa que no tiene amplitud constante —está contaminada—, por lo que debe manipularse para mejorarla. Se trabaja también el concepto de **discretización**: digitalizar la señal seleccionando un número finito de muestras a partir de la señal continua.

**Normalización de una señal:** quitar la componente DC (restar la media) y dividir entre el valor máximo de la señal.

# 3. Laboratorios trabajados

Durante la semana se trabajaron tres notebooks de Jupyter, pensados como una progresión: de la exploración básica de una señal real (Lab 1), pasando por su caracterización en frecuencia y tiempo-frecuencia (Lab 2), hasta el diseño y validación de filtros digitales (Lab 3).

## 3.1 Lab 1 — Introducción con PhysioNet

**Archivo:** `Lab001_Introduccion_Senales_Biomedicas_PhysioNet.ipynb`
**Tema:** importación, exploración, visualización y exportación de un ECG real.
**Dataset:** `mitdb` (MIT-BIH Arrhythmia Database), registro `100`, canales `MLII` y `V5`, $f_s = 360$ Hz.

Objetivos y contenido:

- Acceder a una base de datos fisiológica de PhysioNet mediante el concepto **DATABASE + RECORD** (el mismo número de registro puede significar algo distinto según la base de datos).
- Cargar el registro con `wfdb.rdrecord()` e identificar $f_s$, número de canales, nombres de canales, unidades y duración total.
- Extraer un canal específico desde `record.p_signal[:, CHANNEL]` y construir el eje temporal $t[n] = n/f_s$.
- Cuatro gráficas de análisis: (1) ECG completo, (2) segmento de 10 s ampliado, (3) histograma de amplitudes, (4) representación discreta de las primeras 100 muestras ($x[n]$ con marcadores).
- Estadística básica del segmento: media, desviación estándar, mínimo, máximo y rango.
- Conversión del ECG a formato `.wav`: eliminar la componente DC, normalizar y convertir a `int16`, para luego reproducirlo y descargarlo — dejando explícito que el WAV es solo una representación reproducible, no que el ECG sea originalmente una señal acústica.
- Ejercicios propuestos: cambiar de registro, cambiar de canal, cambiar la duración analizada, y ejercicios de cálculo sobre $f_s$ y estadística de amplitud.
- Preguntas conceptuales sobre PhysioNet, `DATABASE` vs. `RECORD`, `fs`, `CHANNEL`, señal continua vs. discreta, y por qué un WAV no debe interpretarse como si fuera el ECG original.

## 3.2 Lab 2 — Análisis de señales biomédicas con PhysioNet

**Archivo:** `Lab002_Introduccion_Senales_Biomedicas_PhysioNet.ipynb`
**Tema:** representación temporal, FFT y STFT de señales fisiológicas.
**Dataset:** `nsrdb` (Normal Sinus Rhythm Database), tres registros (`16265`, `16272`, `16420`), 3600 muestras (10 s) por registro, canal `0`.

Objetivos y contenido:

- Importar los tres registros y sus anotaciones (`.atr`) mediante `wfdb`, e identificar $f_s$, número de muestras y canales de cada uno.
- Representar las tres señales en el **dominio temporal**.
- Calcular la **FFT** de cada registro en dos versiones: con la componente DC presente y luego de restar la media (`x_AC[n] = x[n] - x̄`), comparando ambos espectros.
- Identificar la **frecuencia dominante** de cada registro (ignorando 0 Hz para no volver a capturar la componente DC).
- Calcular la **STFT** (Transformada de Fourier de Tiempo Corto) y graficar el espectrograma de cada registro, usando `nperseg=256` para los registros `16265`/`16420` y `nperseg=32` para `16272` (para conservar mejor eventos localizados en el tiempo) — introduciendo el compromiso entre resolución temporal y resolución frecuencial según el tamaño de ventana.
- Tabla comparativa de los tres registros ($f_s$, muestras, canales, amplitud, componente DC, frecuencias dominantes, comportamiento en la STFT).
- Ejercicios propuestos: cambiar el segmento temporal analizado, comparar distintos tamaños de ventana (`32`, `128`, `256`) para el registro `16272`, y explicar por qué la FFT global puede ocultar *cuándo* ocurre un evento mientras que la STFT sí puede localizarlo.
- Preguntas conceptuales sobre PhysioNet, `record.p_signal`, `fs`, la componente DC, y la relación entre FFT y STFT.

## 3.3 Lab 3 — Filtros FIR, IIR y Transformada Z

**Archivo:** `Lab003_Introduccion_Filtros_FIR_IIR_Transformada_Z.ipynb`
**Tema:** diseño, aplicación y validación de filtros digitales sobre una señal ECG.
**Señal de trabajo:** ECG sintético generado con `neurokit2` (`duration=10 s`, `sampling_rate=250 Hz`, `heart_rate=70 bpm`).

El flujo de trabajo propuesto en el laboratorio es:

> Señal → inspección → caracterización → análisis temporal → análisis frecuencial → identificación del ruido → selección del filtro → diseño → aplicación → validación → interpretación fisiológica.

Objetivos y contenido, organizado en tres ejercicios:

**Ejercicio 1 — Análisis y caracterización de la señal**
- Inspección robusta de un archivo `.wav` (o, en su defecto, de la señal ECG sintética): frecuencia de muestreo, número de muestras, duración.
- Análisis temporal y cálculo de la FFT (`np.fft.rfft` / `np.fft.rfftfreq`) para identificar componentes fisiológicas y posible ruido.

**Ejercicio 2 — Diseño y comparación de filtros FIR e IIR**
- Diseño de un filtro **FIR** pasa-bajos con `signal.firwin` (101 taps, corte en 40 Hz).
- Diseño de un filtro **IIR** Butterworth pasa-bajos en forma **SOS** (`signal.butter`, orden 4, corte en 40 Hz), por su mayor estabilidad numérica frente a la forma `(b, a)`.
- Aplicación de ambos filtros (`filtfilt` para el FIR, `sosfiltfilt` para el IIR, ambos de fase cero) y comparación visual y por **MSE** contra la señal original.
- Comparación de la respuesta en frecuencia (`freqz` para el FIR, `sosfreqz` para el IIR) y tabla conceptual FIR vs. IIR (realimentación, estabilidad, fase lineal, orden requerido, costo computacional).

**Ejercicio 3 — Procesamiento de una señal contaminada**
- Construcción de un problema experimental: $x[n] = ECG[n] + A\sin(2\pi f_{noise}t)$, con una interferencia de 35 Hz.
- Identificación de la interferencia mediante FFT (sin conocer de antemano la frecuencia de corte) y detección automática del pico en la banda 25–45 Hz.
- Diseño y aplicación de un filtro Butterworth pasa-bajos (corte en 25 Hz, orden 4, `sosfiltfilt`) para recuperar la señal.
- **Validación cuantitativa:** MSE, RMSE y SNR antes/después del filtrado.
- **Validación biomédica:** verificación de que se conserva la morfología del QRS y la frecuencia cardíaca aproximada.
- Demostración experimental de **dos errores de diseño**: (1) frecuencia de corte demasiado baja (5 Hz), que aplana y ensancha el QRS perdiendo información diagnóstica; (2) filtrado con `sosfilt` en lugar de `sosfiltfilt`, que introduce **desplazamiento de fase** y desalinea temporalmente los picos R — resuelto usando filtrado de fase cero.
- El laboratorio cierra con preguntas integradoras y una rúbrica de evaluación de 20 puntos (análisis de la señal, diseño del filtro, implementación, validación, interpretación biomédica y conclusiones).

# 4. Conclusiones

La semana conecta la teoría de sistemas discretos con su aplicación directa sobre señales biomédicas reales y sintéticas: toda señal puede describirse en función del delta de Kronecker; la transformada bilineal permite pasar una función de transferencia continua ($s$) a su equivalente discreta ($z$); el diagrama de polos y ceros determina la estabilidad de un filtro y anticipa su efecto sobre la señal (atenuación por ceros, resonancia por polos); y la elección entre un filtro FIR o IIR implica un compromiso entre fase lineal/estabilidad garantizada (FIR) y menor orden/costo computacional (IIR, a cambio de tener que verificar su estabilidad).

Los tres laboratorios llevan ese marco teórico a la práctica de forma progresiva: el Lab 1 enseña a importar, explorar y exportar una señal biomédica real desde PhysioNet; el Lab 2 profundiza en su caracterización en frecuencia y tiempo-frecuencia (FFT y STFT); y el Lab 3 cierra el ciclo diseñando, aplicando y validando —cuantitativa y fisiológicamente— filtros FIR e IIR sobre una señal contaminada, evidenciando que diseñar un filtro no es solo elegir una función de una librería, sino tomar decisiones justificadas en las características reales de la señal.