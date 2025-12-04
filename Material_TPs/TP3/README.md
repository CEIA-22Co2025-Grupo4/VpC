# TP3 - Detección de Logotipos con Template Matching

## Descripción

Este trabajo práctico implementa técnicas avanzadas de detección de objetos mediante Template Matching, aplicando diferentes estrategias según las características de cada imagen. El objetivo es detectar el logotipo de Coca-Cola en diversas imágenes con diferentes condiciones de iluminación, perspectiva y cantidad de instancias.

## Consigna

**Objetivo**: Encontrar el logotipo de la gaseosa dentro de las imágenes provistas en `Material_TPs/TP3/images` a partir del template `Material_TPs/TP3/template`.

### Tareas

1. **(4 puntos) Obtener una detección del logo en cada imagen sin falsos positivos**
   - Procesar todas las imágenes en la carpeta `images/`
   - Seleccionar el método más adecuado para cada imagen
   - Visualizar resultados con bounding boxes y nivel de confianza

2. **(4 puntos) Plantear y validar un algoritmo para múltiples detecciones en la imagen**
   - Implementar detección múltiple en `coca_multi.png` usando el mismo template del ítem 1
   - Reducir falsos positivos mediante validaciones de forma, color y posición
   - Visualizar todas las detecciones con bounding boxes numerados

3. **(2 puntos) Generalizar el algoritmo del ítem 2 para todas las imágenes**
   - Adaptar el algoritmo de detección múltiple para funcionar con todas las imágenes
   - Manejar casos de una sola detección y múltiples detecciones
   - Visualizar resultados unificados

**Archivos utilizados**: 
- Template: `Material_TPs/TP3/template/` (logo de Coca-Cola)
- Imágenes: `coca_logo_1.png`, `coca_logo_2.png`, `coca_multi.png`, `coca_retro_1.png`, `coca_retro_2.png`, `COCA-COLA-LOGO.jpg`, `logo_1.png`

## Estructura del Notebook

### 1. Imports y Configuración
- Librerías: NumPy, OpenCV, Matplotlib
- Configuración de visualización con backend Qt para ventanas interactivas
- Type hints para mejor documentación del código

### 2. Funciones Auxiliares
- **`load_image()`**: Carga imagen y retorna versiones en RGB, escala de grises y BGR con redimensionamiento automático
- **`load_template()`**: Carga template en escala de grises
- **`preprocess_image()`**: Aplica preprocesamiento (CLAHE, suavizado, ecualización)
- **`create_template_variants()`**: Crea variantes del template (normal, CLAHE, invertido)
- **`rotate_template()`**: Rota template para detectar logos con orientación variable
- **`compute_scale_range()`**: Calcula rango de escalas basado en ratios esperados del logo
- **`compute_iou()`**: Calcula Intersection over Union entre dos bounding boxes
- **`nms_global()`**: Aplica Non-Maximum Suppression globalmente
- **`draw_bboxes()`**: Dibuja bounding boxes con numeración opcional

### 3. Funciones de Detección Core

#### 3.1. Template Matching Multi-escala
- **Función**: `template_match_multiscale()`
- **Método**: Template Matching clásico con múltiples escalas y variantes
- **Características**: 
  - Umbral adaptativo basado en estadísticas de correlación
  - Filtros de aspecto y tamaño mínimo
  - NMS para eliminar detecciones redundantes
- **Uso**: Imágenes con buena iluminación y contraste

#### 3.2. Template Matching basado en Bordes
- **Función**: `template_match_edges()`
- **Método**: Matching sobre bordes detectados con Canny
- **Características**:
  - Robusto ante inversión de contraste
  - Dilatación de bordes para mayor tolerancia
  - Restricción opcional por región vertical
- **Uso**: Imágenes con fondo limpio y bordes preservados

#### 3.3. Detección SIFT con Homografía
- **Función**: `detect_by_sift()`
- **Método**: Feature matching con SIFT + RANSAC
- **Características**:
  - Invariante a rotación, escala y perspectiva
  - Validación por número mínimo de matches
  - Filtros de tamaño de bounding box relativo
- **Uso**: Logos distorsionados, curvas, diferentes perspectivas

#### 3.4. Detección Múltiple
- **Función**: `detect_multiple_logos()`
- **Método**: Template matching con detección de máximos locales y validaciones
- **Características**:
  - Filtro por banda horizontal (y_ranges) para zonas válidas
  - Validación de consistencia de color (dominancia roja)
  - Filtros de forma relativos (aspect ratio, ancho, alto)
  - Test de aislamiento de picos para rechazar ruido
  - NMS para eliminar duplicados

### 4. Estrategia de Selección de Método

Tabla de decisión implementada según características de cada imagen:

| Imagen | Método | Justificación |
|--------|--------|---------------|
| coca_logo_1.png | Edge TM | Fondo limpio, bordes preservados tras Canny |
| coca_logo_2.png | SIFT | Superficie curva, distorsión de perspectiva |
| coca_multi.png | TM invertido | Múltiples logos, contraste invertido |
| coca_retro_1.png | SIFT | Logo retro estructuralmente diferente |
| coca_retro_2.png | SIFT | Logo curvado en emblema circular |
| COCA-COLA-LOGO.jpg | SIFT | Logo grande, fondo complejo |
| logo_1.png | TM invertido | Reflejos en vidrio, variaciones de iluminación |

### 5. Experimentos

#### Assignment 1: Detección Única por Imagen
- Procesamiento de todas las imágenes con método específico
- Visualización con bounding boxes y scores de confianza
- Tabla resumen con métricas de detección

#### Assignment 2: Detección Múltiple en coca_multi.png
- Detección de todos los logos en imagen con múltiples instancias
- Filtros por posición horizontal (estantes superior/inferior)
- Validación de color rojo dominante
- Numeración de detecciones por ubicación

#### Assignment 3: Generalización a Todas las Imágenes
- Aplicación del algoritmo de detección múltiple a todas las imágenes
- Manejo unificado de casos con una o múltiples instancias
- Visualización consolidada de resultados

### 6. Visualización de Resultados
- Gráficos comparativos con imágenes originales y detecciones
- Guardado automático de resultados en carpeta `results/`
- Tablas resumen con estadísticas de detección

## Ejecución

> **Nota**: Si es la primera vez que ejecutas este proyecto, asegúrate de seguir los pasos de configuración inicial descritos en el [README principal](../../README.md#configuración-del-proyecto).

### Pasos para Ejecutar este TP

1. **Verifica que tienes todas las imágenes y templates necesarios**:
   - Carpeta `images/` con todas las imágenes de prueba
   - Carpeta `template/` con el template del logo de Coca-Cola
   - Carpeta `results/` (se crea automáticamente si no existe)

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

4. **Abre el notebook** `TP3.ipynb` y selecciona el kernel "Python (VpC)":
   - En Jupyter: Kernel → Change Kernel → Python (VpC)
   - En JupyterLab: Click en el kernel actual (arriba a la derecha) → Select Kernel → Python (VpC)

5. **Ejecuta las celdas en orden secuencial**:
   - **Primero**: Celdas de imports y funciones auxiliares (celdas 1-4)
   - **Luego**: Funciones de detección core (celdas 5-6)
   - **Después**: Cargar template (celda 7)
   - **Siguiente**: Assignment 1 - Detección única (celdas 8-25)
   - **Luego**: Assignment 2 - Detección múltiple en coca_multi.png (celdas 26-28)
   - **Siguiente**: Assignment 3 - Generalización (celdas 29-35)

**Nota sobre visualización**: El notebook usa `%matplotlib qt` para mostrar imágenes en ventanas separadas. Si prefieres visualización inline, cambia a `%matplotlib inline` en la primera celda.

## Resultados Esperados

### Assignment 1: Detección Única
- Una detección por imagen sin falsos positivos
- Bounding boxes verdes con scores de confianza
- Tabla resumen mostrando:
  - Imagen procesada
  - Método utilizado
  - Score/número de matches
  - Justificación de la elección del método

### Assignment 2: Detección Múltiple
- Detección de múltiples logos en `coca_multi.png`
- Reducción de falsos positivos mediante:
  - Filtros de posición (bandas horizontales)
  - Validación de color (dominancia roja R > 110, R > G+25, R > B+25)
  - Filtros de forma (aspect ratio 2.0-3.8, tamaños relativos)
- Bounding boxes numerados por ubicación
- Conteo separado por estante (superior/inferior)

### Assignment 3: Generalización
- Algoritmo unificado que funciona para todas las imágenes
- Manejo correcto de casos con una o múltiples instancias
- Visualización consolidada con todas las detecciones

### Archivos Generados
- `results/TP3-assignment1.png`: Grid con resultados de detección única
- `results/TP3-assignment2.png`: Detecciones múltiples en coca_multi.png
- `results/TP3-assignment3-*.png`: Resultados individuales de generalización

## Técnicas Implementadas

### Template Matching
- **Métrica**: Correlación normalizada (`TM_CCOEFF_NORMED`)
- **Multi-escala**: Búsqueda en 15-25 escalas diferentes
- **Variantes**: Normal, CLAHE, invertido, invertido+CLAHE
- **Umbral adaptativo**: `mean + 2.5 × std` para robustez

### Detección por Bordes
- **Detector**: Canny con dilatación morfológica
- **Ventajas**: Invariante a inversión de contraste
- **Kernel**: 2×2 para robustez ante ruido

### SIFT + Homografía
- **Features**: Hasta 2000 puntos SIFT
- **Matching**: Lowe's ratio test (0.75)
- **Validación**: RANSAC con mínimo 8 matches
- **Perspectiva**: Maneja transformaciones proyectivas

### Reducción de Falsos Positivos
1. **Non-Maximum Suppression**: IoU threshold 0.2-0.3
2. **Filtros de forma**: Aspect ratio y tamaños relativos
3. **Filtros espaciales**: Bandas horizontales válidas
4. **Validación de color**: Consistencia de dominancia roja
5. **Test de aislamiento**: Rechazo de picos de correlación aislados

## Notas Técnicas

### Parámetros Relativos
- Todos los filtros de tamaño son relativos al tamaño de la imagen
- Ancho mínimo: 3-5% del ancho de imagen
- Alto mínimo: 2% del alto de imagen
- Permite generalización a diferentes resoluciones

### Preprocesamiento
- **CLAHE**: Mejora contraste local (clipLimit=2.0, tileGridSize=8×8)
- **Gaussian Blur**: Reduce ruido (kernel 3×3)
- **Histogram Equalization**: Normaliza iluminación global

### Validación de Color
- **Rojo dominante**: R > 110-120
- **Separación de canales**: R > G+25-30, R > B+25-30
- **Muestreo**: Promedio sobre toda la región detectada

### Optimizaciones
- Redimensionamiento automático de imágenes grandes (max 1200px)
- Templates limitados a 400px para eficiencia
- Salto de escalas inválidas (demasiado grandes/pequeñas)
- Cache de variantes de template

## Referencias

### Material Teórico
- **Clase 4**: Template Matching y Feature Detection
  - Archivo: `Material_TPs/TP3/Clase4.pdf`
  - Temas: Correlación, SIFT, RANSAC, Homografía

### Documentación OpenCV
- **Template Matching**: [cv2.matchTemplate()](https://docs.opencv.org/4.x/d4/dc6/tutorial_py_template_matching.html)
- **SIFT**: [cv2.SIFT_create()](https://docs.opencv.org/4.x/da/df5/tutorial_py_sift_intro.html)
- **Feature Matching**: [cv2.BFMatcher](https://docs.opencv.org/4.x/dc/dc3/tutorial_py_matcher.html)
- **Homography**: [cv2.findHomography()](https://docs.opencv.org/4.x/d9/dab/tutorial_homography.html)

### Papers de Referencia
- **SIFT**: Lowe, D. G. (2004). "Distinctive Image Features from Scale-Invariant Keypoints"
- **RANSAC**: Fischler, M. A., & Bolles, R. C. (1981). "Random Sample Consensus: A Paradigm for Model Fitting"

