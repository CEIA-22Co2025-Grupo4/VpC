# TP2 - Detector de Máximo Enfoque

## Descripción

Este trabajo práctico implementa un detector de máximo enfoque sobre video aplicando técnicas de análisis espectral, similar a los sistemas utilizados en cámaras digitales modernas para autofocus.

## Consigna

**Objetivo**: Implementar un algoritmo que detecte automáticamente el punto de máximo enfoque en un video aplicando técnicas de análisis espectral.

### Experimentos Requeridos

1. **Medición sobre todo el frame**
   - Calcular la métrica de enfoque para cada frame completo del video
   - Visualizar la evolución de la métrica frame a frame
   - Detectar automáticamente el frame con máximo enfoque

2. **Medición sobre ROI central**
   - Calcular la métrica sobre una región de interés (ROI) ubicada en el centro del frame
   - Área de la ROI: 5% o 10% del área total del frame
   - Comparar resultados entre ambas configuraciones

3. **Medición sobre matriz de enfoque** (Opcional)
   - Dividir cada frame en una matriz de N×M regiones rectangulares equiespaciadas
   - Calcular la métrica para cada región
   - Probar con al menos 3 configuraciones diferentes (ej: 3×3, 7×5, 5×7)
   - Analizar promedio y máximo de la matriz

### Requisitos Adicionales

- **Métrica principal**: Implementar la métrica propuesta en el paper "Image Sharpness Measure for Blurred Images in Frequency Domain"
- **Métrica alternativa**: Cambiar la métrica eligiendo un algoritmo del apéndice de "Analysis of focus measure operators in shape-from-focus" (se implementó Varianza del Laplaciano)
- **Detección automática**: El algoritmo debe detectar y devolver los puntos de máximo enfoque de manera automática
- **Bonus**: Aplicar unsharp masking para expandir la zona de enfoque y recalcular la métrica

**Archivo de entrada**: `focus_video.mov`

## Estructura del Notebook

### 1. Imports y Configuración
- Librerías: NumPy, OpenCV, Matplotlib, SciPy (FFT)

### 2. Implementaciones de Métricas de Enfoque

#### 2.1. Frequency Domain Sharpness Measure
- **Función**: `frequency_domain_sharpness()`
- **Paper**: "Image Sharpness Measure for Blurred Images in Frequency Domain"
- **Método**: Analiza componentes de alta frecuencia en el dominio de Fourier
- **Métrica**: Ratio entre energía de alta frecuencia y energía total

#### 2.2. Variance of Laplacian
- **Función**: `variance_of_laplacian()`
- **Paper**: "Analysis of focus measure operators in shape-from-focus"
- **Método**: Calcula la varianza del operador Laplaciano aplicado a la imagen
- **Ventajas**: Más eficiente computacionalmente, opera en dominio espacial

#### 2.3. Tenengrad
- **Función**: `tenengrad()`
- **Método**: Suma de gradientes al cuadrado usando operadores Sobel
- **Uso**: Métrica adicional disponible para comparación

### 3. Funciones de Procesamiento de Video

- `extract_roi_center()`: Extrae región de interés del centro de la imagen
- `create_focus_matrix()`: Crea matriz de mediciones de enfoque
- `detect_maximum_focus()`: Detecta automáticamente el frame con máximo enfoque
- `apply_unsharp_masking()`: Aplica unsharp masking para mejorar nitidez

### 4. Funciones de Detección de Región Temporal

- `find_focus_region_by_threshold()`: Encuentra el rango de frames donde el enfoque está por encima de un umbral (basado en percentiles)
- `find_longest_focus_region()`: Encuentra la región continua más larga donde el enfoque está por encima del umbral
- `visualize_focus_region_temporal()`: Visualiza la curva de enfoque con la región detectada resaltada

### 5. Experimentos

#### Experimento 1: Medición sobre Todo el Frame
- `process_video_full_frame()`: Procesa video frame completo
- Visualización de curva de evolución de la métrica
- Detección automática del máximo

#### Experimento 2: Medición sobre ROI Central
- `process_video_roi()`: Procesa video con ROI central
- Configuraciones: 5% y 10% del área total
- Comparación de resultados

#### Experimento 3: Matriz de Enfoque
- `process_video_focus_matrix()`: Procesa video con matriz de regiones
- Configuraciones: 3×3, 7×5, 5×7
- Análisis de promedio y máximo por frame

### 6. Métrica Alternativa
- Comparación entre Frequency Domain y Variance of Laplacian
- Visualización de ambas métricas lado a lado

### 7. Bonus: Unsharp Masking
- `process_video_with_unsharp_masking()`: Procesa video con mejora de nitidez
- Comparación de métricas antes y después del procesamiento
- Análisis de la diferencia

### 8. Detección de Región Temporal de Máximo Enfoque
- `find_focus_region_by_threshold()`: Detecta el rango completo de frames enfocados usando umbral por percentil
- `find_longest_focus_region()`: Encuentra la región continua más larga de frames enfocados
- Visualización de regiones temporales detectadas con umbrales y marcas visuales

### 9. Resumen y Resultados
- Tabla resumen con todos los frames de máximo enfoque detectados
- Comparación entre diferentes experimentos y métricas

### 10. Conclusiones
- Análisis de consistencia entre diferentes métricas y configuraciones
- Hallazgos principales sobre ROI, matrices de enfoque y unsharp masking
- Comparación directa entre Frequency Domain y Variance of Laplacian
- Recomendaciones prácticas según caso de uso (tiempo real, offline, multi-zona, focus stacking)
- Conclusión final sobre efectividad y aplicabilidad de los métodos implementados

## Ejecución

> **Nota**: Si es la primera vez que ejecutas este proyecto, asegúrate de seguir los pasos de configuración inicial descritos en el [README principal](../../README.md#configuración-del-proyecto).

### Pasos para Ejecutar este TP

1. **Verifica que tienes el archivo de video necesario**:
   - `focus_video.mov` debe estar en el directorio `TP2/`

2. **Asegúrate de tener el entorno virtual activo**:
   ```bash
   source .venv/bin/activate  # macOS/Linux
   # o
   .venv\Scripts\activate    # Windows
   ```

3. **Inicia Jupyter Notebook** (desde cualquier directorio):
   ```bash
   jupyter notebook
   ```

4. **Abre el notebook** `TP2.ipynb` y selecciona el kernel "Python (VpC)":
   - En Jupyter: Kernel → Change Kernel → Python (VpC)
   - En JupyterLab: Click en el kernel actual (arriba a la derecha) → Select Kernel → Python (VpC)

5. **Ejecuta las celdas en orden secuencial**:
   - **Primero**: Celdas de imports y funciones de métricas (celdas 1-12)
   - **Luego**: Experimento 1 - Medición sobre todo el frame (celdas 13-16)
   - **Después**: Experimento 2 - ROI central (celdas 17-21)
   - **Siguiente**: Experimento 3 - Matriz de enfoque (celdas 22-25)
   - **Luego**: Métrica alternativa - Varianza del Laplaciano (celdas 26-28)
   - **Después**: Bonus - Unsharp Masking (celdas 29-32)
   - **Siguiente**: Detección de región temporal (celdas 33-40)
   - **Luego**: Resumen y resultados (celdas 41-42)
   - **Finalmente**: Conclusiones (celdas 43-48)

**Nota**: El procesamiento del video puede tardar varios minutos dependiendo de la longitud del video y la potencia de tu computadora.

## Resultados Esperados

### Gráficos de Evolución
- Curvas que muestran la métrica de enfoque a lo largo de todos los frames
- Línea vertical marcando el frame con máximo enfoque detectado automáticamente
- Comparaciones entre diferentes configuraciones (ROI 5% vs 10%, diferentes matrices)

### Detección Automática
- Identificación del frame número con máximo enfoque para cada experimento
- Comparación entre métricas (Frequency Domain vs Laplacian)
- Análisis del efecto del unsharp masking

### Detección de Región Temporal
- Identificación de rangos de frames donde el video está enfocado (no solo un punto)
- Dos métodos: umbral por percentil y región continua más larga
- Visualización de regiones temporales con umbrales y marcas visuales
- Útil para segmentación de video y análisis de períodos de enfoque

### Análisis de Matriz de Enfoque
- Visualización de cómo varía el enfoque en diferentes regiones del frame
- Identificación de zonas con mejor/menor enfoque
- Promedio y máximo de la matriz para cada frame

## Métricas Implementadas

### Frequency Domain Sharpness Measure
- **Principio**: Las imágenes nítidas tienen más energía en altas frecuencias
- **Cálculo**: Ratio entre energía de alta frecuencia (>10% del tamaño mínimo) y energía total
- **Ventajas**: Robusto ante variaciones de iluminación, no requiere referencia

### Variance of Laplacian
- **Principio**: Las imágenes nítidas tienen mayor varianza en la respuesta del Laplaciano
- **Cálculo**: Varianza del operador Laplaciano aplicado a la imagen
- **Ventajas**: Más rápido computacionalmente, fácil de interpretar

## Notas Técnicas

- El umbral de alta frecuencia se calcula como `min(rows, cols) * 0.1` según el paper
- La detección automática usa una ventana deslizante para encontrar máximos locales
- El unsharp masking puede expandir la zona de enfoque pero también puede introducir artefactos
- Las diferentes métricas pueden dar resultados ligeramente diferentes según las características de la imagen
- La detección de región temporal permite identificar períodos completos de enfoque, no solo puntos aislados
- Los umbrales por percentil (85-90%) son configurables según las necesidades del análisis

## Referencias

- **"Image Sharpness Measure for Blurred Images in Frequency Domain"**
  - De, K., & Masilamani, V. (2013). Procedia Engineering, 64, 149–158.
  - Disponible en: [ScienceDirect](https://www.sciencedirect.com/science/article/pii/S1877705813000203) (acceso abierto)
  - Alternativa: [Scribd](https://www.scribd.com/document/669309825/1-s2-0-S1877705813016007-main)

- **"Analysis of focus measure operators in shape-from-focus"**
  - Nayar, S. K., & Nakagawa, Y. (1994). IEEE Transactions on Pattern Analysis and Machine Intelligence, 16(8), 824-831.
  - Disponible en: [IEEE Xplore](https://ieeexplore.ieee.org/document/308610) (puede requerir acceso institucional)
  - Alternativa: [ResearchGate](https://www.researchgate.net/publication/224377585_Analysis_of_focus_measure_operators_in_shape-from-focus) (acceso abierto)

