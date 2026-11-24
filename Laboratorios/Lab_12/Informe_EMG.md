# Detección de fatiga muscular mediante sEMG

**Proyecto privado:** [fatiga-semg-grupo4](https://studio.edgeimpulse.com/studio/1171872)

## Datos y control de integridad

El objetivo fue evaluar una CNN 1D capaz de distinguir repeticiones de curl de bíceps con y sin fatiga a partir de un canal de electromiografía superficial. El protocolo del equipo incluyó reposo, MVC antes y después del ejercicio y registro del esfuerzo percibido con OMNI-RES. Según la guía, la clase fatiga corresponde al nivel 2 de Cerqueira y a OMNI-RES 9 en los sujetos del laboratorio.

| Conjunto | Origen | Sin fatiga | Con fatiga | Total |
|---|---|---:|---:|---:|
| Training | Cerqueira, 13 sujetos (C01–C13) | 989 | 762 | 1751 |
| Testing | BITalino, 4 sujetos (S01–S04) | 89 | 64 | 153 |

Se verificaron los 1904 CSV. Todos contienen las columnas timestamp y emg, 4000 valores numéricos finitos y tiempos enteros consecutivos de 0 a 3999 ms. El intervalo de 1 ms corresponde a 1000 Hz. Los 3999 ms son el tiempo entre la primera y la última muestra, y 4000 ms es la duración de la ventana de 4000 puntos a 1000 Hz. Se comprobó la unicidad del identificador de repetición y de la secuencia EMG mediante SHA-256. El inventario detalla la revisión por archivo.

El filtrado previo de 20–450 Hz y las unidades en mV se consignan según la guía del equipo. La auditoría del ZIP verifica la estructura de las señales y las etiquetas de los nombres. Para contrastar cada etiqueta con el criterio experimental harían falta los registros de etiquetado originales.

## Configuración y estado en Edge Impulse

Edge Impulse mostró 1751 registros en Training y 153 en Test. La carga se realizó por separado con etiquetas inferidas del nombre de archivo y con la división automática desactivada. El CSV Wizard quedó configurado para series temporales con timestamp en milisegundos, un canal emg y un registro por archivo.

| Parámetro | Valor |
|---|---|
| Canal | emg |
| Frecuencia detectada | 1000 Hz |
| Tamaño y paso de ventana | 4000 ms y 4000 ms, respectivamente |
| Relleno con ceros | Activado en el editor del impulso |
| Procesamiento | Raw Data |
| Bloque de aprendizaje | Classification, modo Expert (Keras) según la guía |
| CNN 1D | Conv1D de 16, 32 y 32 filtros, kernels 25, 9 y 9, batch normalization y max pooling de factor 4 en los dos primeros bloques, global average pooling, dropout 0.3, capa densa de 16 y softmax de 2 clases |
| Tasa de aprendizaje y épocas | 0.001 y aproximadamente 30, especificadas por la guía |
| Estado | Raw Data añadido en el editor. El guardado final del impulso y la aplicación del código en Expert no se pudieron verificar. |

El archivo CNN1D_Keras.py contiene el bloque de arquitectura de la guía y sus importaciones. Está destinado a sustituir la definición del modelo en la plantilla Expert, que proporciona input_length y classes. El entrenamiento, la validación interna y Model Testing quedaron pendientes. No hay métricas ni matriz de confusión de Edge Impulse.

## Tabla de métricas

| Evaluación | Exactitud | Exactitud balanceada | Precisión | Recall | F1 |
|---|---:|---:|---:|---:|---:|
| Validación interna CNN 1D (Edge Impulse) | Pendiente | Pendiente | Pendiente | Pendiente | Pendiente |
| Prueba CNN 1D (Edge Impulse, 153 repeticiones) | Pendiente | Pendiente | Pendiente | Pendiente | Pendiente |
| CNN previa en Python (referencia de la guía) | — | 54 % | — | — | — |
| LDA con características relativas (referencia de la guía) | — | 71 % | — | — | — |
| Predicción constante de «sin fatiga» (referencia calculada) | 58.2 % | 50 % | — | 0 % para fatiga | 0 % para fatiga |

La exactitud balanceada es el promedio del recall de las dos clases. Es la métrica comparable con los valores de referencia de Python. Con la matriz de confusión, se debe reportar precisión, recall y F1 por clase y aclarar si las cifras agregadas son macro. La precisión de fatiga es indefinida si el modelo nunca predice esa clase.

## Interpretación y limitaciones

El conjunto de prueba permite evaluar la transferencia desde Cerqueira hacia una adquisición independiente con BITalino. Un resultado modesto podría estar asociado a diferencias de instrumentación, colocación de electrodos, intensidad del ejercicio y criterio de etiquetado. La CNN de la guía recibe cada ventana de forma aislada y carece de una referencia individual de las primeras repeticiones. Esto limita su capacidad para medir cambios relativos de una persona a lo largo de la serie.

Los 153 registros corresponden a cuatro sujetos y, por tanto, las repeticiones de una misma persona comparten condiciones de adquisición. S02 aporta solo dos repeticiones con fatiga, frente a entre 20 y 21 para cada uno de los otros tres sujetos. Una validación interna que distribuya repeticiones del mismo sujeto entre entrenamiento y validación puede sobreestimar la generalización. La evaluación independiente en Testing tiene mayor peso para interpretar el desempeño.
