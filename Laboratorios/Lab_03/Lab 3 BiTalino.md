<h1 align="center">Análisis de Señales EMG: Músculo Abductor Pollicis y Antebrazo</h1>
<p align="center"><em>Laboratorio 3 — Introducción a Señales Biomédicas</em></p>

## Índice
 
- [1. Introducción](#1-introducción)
- [2. Materiales y equipo](#2-materiales-y-equipo)
- [3. Protocolo de adquisición](#3-protocolo-de-adquisición)
- [4. Procedimiento](#4-procedimiento)
  - [4.1 Filtrado](#41-filtrado)
  - [4.2 Envolvente RMS](#42-envolvente-rms)
  - [4.3 Análisis espectral y frecuencia mediana (MDF)](#43-análisis-espectral-y-frecuencia-mediana-mdf)
- [5. Resultados](#5-resultados)
  - [5.1 Músculo abductor pollicis](#51-músculo-abductor-pollicis)
  - [5.2 Músculo del antebrazo](#52-músculo-del-antebrazo)
  - [5.3 Comparación entre estadios y músculos](#53-comparación-entre-estadios-y-músculos)
- [6. Discusión](#6-discusión)
- [7. Conclusiones](#7-conclusiones)
- [8. Referencias](#8-referencias)


# 1. Introducción
La electromiografía de superficie (sEMG) es una técnica no invasiva que permite registrar la actividad eléctrica generada por los músculos mediante sensores colocados sobre la piel, que captan los potenciales de acción de las fibras musculares que se contraen.
En este laboratorio se realizaron mediciones de señales EMG en dos grupos musculares: el músculo abductor pollicis brevis y el músculo flexor radial del carpo, bajo tres estadíos de actividad: en reposo, movimiento leve y movimiento con oposición, dejando 30 segundos de reposo entre cada medición. El objetivo del laboratorio es caracterizar la señal EMG conforme aumenta la intensidad de contracción muscular.


# 2. Materiales y equipo
| Equipo | Cantidad |
| :--- | :--- |
| Kit Bitalino | 1 |
| Cables y electrodos | 3 |
| Laptop | 1 |

# 3. Protocolo de adquisición
 
Previo a la colocación de los electrodos, se limpió la zona de la piel con alcohol isopropílico y se dejó secar para asegurar una mejor conductividad y minimizar el ruido en la señal. 
<p align="center">
  <img src="Imagenes/limpieza1.jpeg" alt="1.1" width="700"><br>
</p>
<p align="center">
  <img src="Imagenes/limpieza2.jpeg" alt="1.1" width="700"><br>
</p>

Los electrodos se colocaron siguiendo la guía de colocación de electrodos de BITalino, asegurando que los dos electrodos se colocaran en dirección a las fibras musuclares a medir, además del tercer electrodo colocado en el codo tomandolo como tierra para ambas mediciones musculares.
<p align="center">
  <img src="Imagenes/colocar1.jpeg" alt="1.1" width="700"><br>
</p>
<p align="center">
  <img src="Imagenes/colocar2.jpeg" alt="1.1" width="700"><br>
</p>

Las señales se adquirieron con un sensor BITalino (r)evolution (`EMGBITREV`), a una frecuencia de muestreo de 1000 Hz, mediante el software OpenSignals (r)evolution. Para cada músculo se registraron tres archivos:
 
- **Reposo**: 30 segundos sin actividad muscular voluntaria.
- **Movimiento controlado**: 3 repeticiones del movimiento (pulgar–meñique para el abductor pollicis), con 30 segundos de reposo entre cada repetición.
- **Movimiento de contrafuerza**: 3 repeticiones de contracción contra resistencia, con 30 segundos de reposo entre cada repetición.
La lectura de los archivos `.txt` (formato OpenSignals) y la conversión a milivoltios se realizó con la librería [`opensignalsreader`](https://github.com/PGomes92/opensignalsreader), que aplica automáticamente las funciones de transferencia oficiales de BITalino.


# 4. Procedimiento
## 4.1 Filtrado
 
A cada señal se le removió el offset DC (resta de la media) y se aplicó un filtro pasa-altos Butterworth de orden 4 con frecuencia de corte en 20 Hz, seguido de un filtro notch en 60 Hz (frecuencia de la red eléctrica en el Perú) para eliminar la interferencia de línea. Ambos filtros se aplicaron con fase cero (`filtfilt`) para no introducir desfases en la señal.
 
## 4.2 Envolvente RMS
 
La señal EMG cruda oscila muy rápido (decenas a cientos de Hz) alrededor de cero, por lo que a simple vista es difícil distinguir cuándo el músculo está realmente contraído y cuándo está en reposo, sobre todo en los registros largos de movimiento (97–139 s) que incluyen las 3 repeticiones separadas por pausas. Para resolver esto se calculó una **envolvente RMS**: en vez de mirar cada muestra individual, se obtiene la raíz media cuadrática (RMS) de la señal dentro de una ventana móvil de 200 ms, con un paso de 50 ms entre ventanas sucesivas. El resultado es una curva suave que sigue la intensidad de la contracción a lo largo del tiempo, sin el ruido de alta frecuencia de la señal cruda.
 
En las figuras de la sección 4, la envolvente se muestra como la **línea negra** superpuesta sobre la señal filtrada (en color): sube cuando el músculo se contrae y cae cerca de cero cuando vuelve al reposo. Gracias a esto, en los estadios de movimiento controlado y contrafuerza se distinguen visualmente los 3 "montículos" que corresponden a las 3 repeticiones del protocolo, separados por los tramos de reposo.
 
## 4.3 Análisis espectral y frecuencia mediana (MDF)
 
Se calculó el espectro de frecuencia mediante la Transformada Rápida de Fourier (FFT) con ventana de Hanning, expresado en decibelios. A partir de la potencia espectral acumulada se estimó la **frecuencia mediana (MDF)**, definida como la frecuencia por debajo de la cual se concentra el 50% de la potencia total de la señal. La MDF es un indicador comúnmente usado para caracterizar cambios en el patrón de reclutamiento de unidades motoras y fatiga muscular.


# 5. Resultados

## 5.1 Músculo abductor pollicis
<p align="center">
  <img src="Imagenes/pulgar.png" alt="1.1" width="700"><br>
</p>
Figura X. Extraído de “BITalino (r)evolution Lab Guide” [Online]. Available:https://support.pluxbiosignals.com/wp-content/uploads/2022/04/HomeGuide1_EMG.pdf 

<p align="center">
  <img src="Imagenes/pos_pul.jpeg" alt="1.1" width="700"><br>
</p>
De acuerdo con la Figura X, los electrodos se colocan para realizar la medición de la actividad del músculo abductor pollicis. Los dos electrodos de medición se ubican longitudinalmente sobre la base del dedo pulgar, con 2 cm de espaciado,  mientras que el electrodo de referencia, utilizado como tierra, se coloca a nivel del codo.

| **En reposo** | **Movimiento leve** | **Movimiento con oposición** |
|:------------------:|:----------------------:|:----------------------:|
| [▶️ Ver video](https://github.com/joseverameza/GRUPO4-ISB-2026-II/blob/main/Laboratorios/Lab_03/Imagenes/Carpo_reposo.mp4) | [▶️ Ver video](https://github.com/joseverameza/GRUPO4-ISB-2026-II/blob/main/Laboratorios/Lab_03/Imagenes/leve_carpo_f.mp4) | [▶️ Ver video](https://github.com/joseverameza/GRUPO4-ISB-2026-II/blob/main/Laboratorios/Lab_03/Imagenes/contra_carpo_f.mp4) |

### Señal en OpenSignals
En reposo
<p align="center">
  <img src="Imagenes/open_reposo_carpo.jpg" alt="1.1" width="700"><br>
</p>


Movimiento leve
<p align="center">
  <img src="Imagenes/open_leve_carpo.jpg" alt="1.1" width="700"><br>
</p>

Movimiento con opsición
<p align="center">
  <img src="Imagenes/open_contra_carpo.jpg" alt="1.1" width="700"><br>
</p>

### Ploteo en Phyton
En reposo
<p align="center">
  <img src="Imagenes/plot reposo.png" alt="1.1" width="700"><br>
</p>


Movimiento leve
<p align="center">
  <img src="Imagenes/plot movimiento leve.png" alt="1.1" width="700"><br>
</p>

Movimiento con opsición
<p align="center">
  <img src="Imagenes/movimiento en contra.png" alt="1.1" width="700"><br>
</p>


## 5.2 Músculo del antebrazo
<p align="center">
  <img src="Imagenes/1-tri.jpg" alt="1.1" width="700"><br>
</p>
Figura X. Extraído de “BITalino (r)evolution Lab Guide” [Online]. Available:https://support.pluxbiosignals.com/wp-content/uploads/2022/04/HomeGuide1_EMG.pdf 


<p align="center">
  <img src="Imagenes/2-tri.jpg" alt="1.1" width="700"><br>
</p>
De acuerdo con la Figura X, los electrodos se colocan para realizar la medición de la actividad del músculo flexor radial del carpo. Los dos electrodos de medición se ubican longitudinalmente sobre la fibra muscular, con 2 cm de espaciado,  mientras que el electrodo de referencia, utilizado como tierra, se coloca a nivel del codo.



| **En reposo** | **Movimiento leve** | **Movimiento con oposición** |
|:------------------:|:----------------------:|:----------------------:|
| [▶️ Ver video](https://github.com/joseverameza/GRUPO4-ISB-2026-II/blob/main/Laboratorios/Lab_03/Imagenes/Carpo_reposo.mp4) | [▶️ Ver video](https://github.com/joseverameza/GRUPO4-ISB-2026-II/blob/main/Laboratorios/Lab_03/Imagenes/leve_carpo_f.mp4) | [▶️ Ver video](https://github.com/joseverameza/GRUPO4-ISB-2026-II/blob/main/Laboratorios/Lab_03/Imagenes/contra_carpo_f.mp4) |

### Señal en OpenSignals
En reposo
<p align="center">
  <img src="Imagenes/open_reposo_carpo.jpg" alt="1.1" width="700"><br>
</p>


Movimiento leve
<p align="center">
  <img src="Imagenes/open_leve_carpo.jpg" alt="1.1" width="700"><br>
</p>

Movimiento con opsición
<p align="center">
  <img src="Imagenes/open_contra_carpo.jpg" alt="1.1" width="700"><br>
</p>


### Ploteo en Phyton
En reposo
<p align="center">
  <img src="Imagenes/plot reposo.png" alt="1.1" width="700"><br>
</p>


Movimiento leve
<p align="center">
  <img src="Imagenes/plot movimiento leve.png" alt="1.1" width="700"><br>
</p>

Movimiento con opsición
<p align="center">
  <img src="Imagenes/movimiento en contra.png" alt="1.1" width="700"><br>
</p>

### Análisis

## Cuestionario
**¿Cuáles son las frecuencias significativas para las adquisiciones de EMG? ¿Son las mismas en todas las áreas del cuerpo, como el área facial?**
> La señal EMG útil se concentra entre 20 y 450 Hz aproximadamente, con la mayor parte de la energía entre 50 y 150 Hz, tal como se aprecia en los espectros obtenidos en las tres condiciones (reposo, leve y con oposición). Sin embargo, no son exactamente las mismas en todo el cuerpo: en zonas como el rostro los músculos son más pequeños y delgados, lo que genera señales de menor amplitud y más sensibles al ruido, por lo que el contenido frecuencial puede variar un poco respecto a músculos grandes como el flexor radial del carpo.

**¿Qué tipo de filtro es esencial al trabajar con señales de EMG? ¿Por qué necesitamos aplicar dicho filtro?**
> Es fundamental usar un filtro pasa banda (entre 20 y 450 Hz aprox.) junto con un filtro notch en 50/60 Hz. El pasa banda elimina tanto los artefactos de movimiento en bajas frecuencias como el ruido de alta frecuencia que no corresponde a actividad muscular, mientras que el notch quita la interferencia de la red eléctrica, que se nota como picos puntuales en las FFT obtenidas (por ejemplo cerca de 60 Hz y sus armónicos). Sin este filtrado, la señal se contamina y es difícil distinguir la actividad muscular real.

**¿Cómo varía la amplitud en cada contracción muscular? ¿Hay diferencia según la ubicación en el cuerpo?**
> La amplitud aumenta conforme la contracción es más intensa: en reposo apenas se ve ruido de fondo (~0.02 mV), en movimiento leve sube a un rango de 0.2-0.4 mV, y en movimiento con oposición llega hasta 1.5 mV aproximadamente. Esto pasa porque al necesitar más fuerza, el cuerpo recluta más unidades motoras y estas disparan con mayor frecuencia. También influye la ubicación: músculos grandes y superficiales como el flexor radial del carpo dan señales más fuertes que músculos pequeños o más profundos, ya que hay más fibras activas cerca del electrodo y menos tejido que atenúe la señal.

**¿Equivale la amplitud de la EMG a la cantidad de fuerza que has generado con tu músculo?**
> No exactamente. Hay una relación entre ambas (a mayor fuerza, mayor amplitud), pero no es una equivalencia directa ni lineal, ya que factores como la posición de los electrodos, el grosor de la piel y tejido adiposo, la fatiga muscular y el crosstalk de músculos cercanos afectan la lectura. Para usar la EMG como estimador real de fuerza se necesitaría normalizar la señal respecto a una contracción máxima voluntaria, así que el valor en mV que se obtiene es más una referencia relativa que una medida calibrada de fuerza.
