<h1 align="center">LABORATORIO 3: BiTalino</h1>

## Introducción



## Materiales y equipo
| Equipo | Cantidad |
| :--- | :--- |
| Kit Bitalino | 1 |
| Cables y electrodos | 3 |
| Laptop | 1 |



## Procedimiento



## Medición del abductor pollicis brevis (pulgar)
<p align="center">
  <img src="Imagenes/pulgar_1.png" alt="1.1" width="700"><br>
</p>
Figura 1. Extraído de “BITalino (r)evolution Lab Guide” [Online]. Available:https://support.pluxbiosignals.com/wp-content/uploads/2022/04/HomeGuide1_EMG.pdf 

<p align="center">
  <img src="Imagenes/pulgar_2.jpeg" alt="1.1" width="700"><br>
</p>



| **En reposo** | **Movimiento leve** | **Movimiento con oposición** |
|:------------------:|:----------------------:|:----------------------:|
| [▶️ Ver video](https://github.com/joseverameza/GRUPO4-ISB-2026-II/blob/main/Laboratorios/Lab_03/Imagenes/pulgar_reposo.mp4) | [▶️ Ver video](https://github.com/joseverameza/GRUPO4-ISB-2026-II/blob/main/Laboratorios/Lab_03/Imagenes/pulgar_m_leve.mp4) | [▶️ Ver video](https://github.com/joseverameza/GRUPO4-ISB-2026-II/blob/main/Laboratorios/Lab_03/Imagenes/pulgar_cont.mp4) |
## Medición del músculo flexor radial del carpo
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

Como se puede ver en las tres gráficas, hay una relación bastante clara entre el nivel de esfuerzo y la señal EMG registrada.

En reposo, la señal se mantiene prácticamente plana, oscilando apenas entre -0.02 y 0.01 mV, sin ninguna ráfaga o pico que indique contracción muscular. En el espectro de frecuencia de esta parte se nota un pico bien marcado cerca de los 60 Hz y otro alrededor de los 400 Hz, que son compatibles con interferencia eléctrica o ruido de la línea eléctrica y no a actividad muscular real, lo cual tiene sentido porque en teoría el músculo no debería estar activo.

En movimiento leve la cosa cambia bastante: aparecen tres ráfagas claras de activación (alrededor de los segundos 5, 47 y 95), con una amplitud que llega hasta 0.4 mV en el pico más alto. Fuera de esas ráfagas la señal vuelve a estar cerca de cero, lo que confirma que cada una corresponde a una contracción puntual del músculo. En el espectro se ve que la energía se concentra más que nada entre 0 y 150 Hz, con una caída conforme sube la frecuencia, un patrón bastante típico de EMG.

Con oposición, la amplitud sube muchísimo más, llegando hasta ±1.5 mV, casi cuatro veces lo que se vio en movimiento leve. Las tres ráfagas (cerca de los segundos 10, 60 y 115) también se ven más anchas y sostenidas, como si la contracción durara más tiempo y con más fuerza, que es justo lo que se espera cuando el músculo tiene que vencer resistencia. El espectro también sube de nivel en general, con magnitudes entre -60 y -80 dB en las frecuencias más bajas, comparado con los -100 dB que se veían en movimiento leve, aunque la forma general del espectro (concentrado en frecuencias bajas y medias) se mantiene parecida.

En resumen, se ve claramente cómo a mayor esfuerzo o resistencia aumenta tanto la amplitud como la duración de la actividad muscular, mientras que en reposo lo único presente es ruido de fondo sin ningún patrón de contracción.

## Cuestionario
**¿Cuáles son las frecuencias significativas para las adquisiciones de EMG? ¿Son las mismas en todas las áreas del cuerpo, como el área facial?**
> La señal EMG útil se concentra entre 20 y 450 Hz aproximadamente, con la mayor parte de la energía entre 50 y 150 Hz, tal como se aprecia en los espectros obtenidos en las tres condiciones (reposo, leve y con oposición). Sin embargo, no son exactamente las mismas en todo el cuerpo: en zonas como el rostro los músculos son más pequeños y delgados, lo que genera señales de menor amplitud y más sensibles al ruido, por lo que el contenido frecuencial puede variar un poco respecto a músculos grandes como el flexor radial del carpo.

**¿Qué tipo de filtro es esencial al trabajar con señales de EMG? ¿Por qué necesitamos aplicar dicho filtro?**
> Es fundamental usar un filtro pasa banda (entre 20 y 450 Hz aprox.) junto con un filtro notch en 50/60 Hz. El pasa banda elimina tanto los artefactos de movimiento en bajas frecuencias como el ruido de alta frecuencia que no corresponde a actividad muscular, mientras que el notch quita la interferencia de la red eléctrica, que se nota como picos puntuales en las FFT obtenidas (por ejemplo cerca de 60 Hz y sus armónicos). Sin este filtrado, la señal se contamina y es difícil distinguir la actividad muscular real.

**¿Cómo varía la amplitud en cada contracción muscular? ¿Hay diferencia según la ubicación en el cuerpo?**
> La amplitud aumenta conforme la contracción es más intensa: en reposo apenas se ve ruido de fondo (~0.02 mV), en movimiento leve sube a un rango de 0.2-0.4 mV, y en movimiento con oposición llega hasta 1.5 mV aproximadamente. Esto pasa porque al necesitar más fuerza, el cuerpo recluta más unidades motoras y estas disparan con mayor frecuencia. También influye la ubicación: músculos grandes y superficiales como el flexor radial del carpo dan señales más fuertes que músculos pequeños o más profundos, ya que hay más fibras activas cerca del electrodo y menos tejido que atenúe la señal.

**¿Equivale la amplitud de la EMG a la cantidad de fuerza que has generado con tu músculo?**
> No exactamente. Hay una relación entre ambas (a mayor fuerza, mayor amplitud), pero no es una equivalencia directa ni lineal, ya que factores como la posición de los electrodos, el grosor de la piel y tejido adiposo, la fatiga muscular y el crosstalk de músculos cercanos afectan la lectura. Para usar la EMG como estimador real de fuerza se necesitaría normalizar la señal respecto a una contracción máxima voluntaria, así que el valor en mV que se obtiene es más una referencia relativa que una medida calibrada de fuerza.
