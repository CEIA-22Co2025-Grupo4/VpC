# TP1 - Visión por Computadora

## Descripción

Este trabajo práctico implementa técnicas fundamentales de procesamiento de imágenes, enfocándose en dos áreas principales:

1. **Corrección de color mediante White Patch**
2. **Análisis de histogramas para clasificación**

## Consigna

### Parte 1: Algoritmo White Patch

**Objetivo**: Implementar el algoritmo White Patch para corregir las diferencias de color causadas por variaciones en la iluminación.

**Tareas**:
- Implementar el algoritmo White Patch que normaliza los niveles de color basándose en el punto más brillante de la imagen
- Procesar todas las imágenes en la carpeta `/white_patch`
- Mostrar los resultados obtenidos (imágenes originales vs corregidas)
- Analizar las posibles fallas y limitaciones del método

**Archivos utilizados**: Todas las imágenes en `white_patch/` (test_blue.png, test_green.png, test_red.png, wp_blue.jpg, wp_green.png, wp_green2.jpg, wp_red.png, wp_red2.jpg)

### Parte 2: Análisis de Histogramas

**Objetivo**: Analizar histogramas de imágenes en escala de grises para entender su distribución tonal y evaluar su utilidad como features para clasificación.

**Tareas**:
- Leer las imágenes `img1_tp.png` y `img2_tp.png` con OpenCV en escala de grises
- Visualizar las imágenes
- Elegir un número apropiado de bins y graficar los histogramas
- Comparar los histogramas entre sí
- Explicar las observaciones
- Responder: ¿Es útil usar histogramas como features para entrenar modelos de clasificación/detección de imágenes?

**Archivos utilizados**: `img1_tp.png`, `img2_tp.png`

## Estructura del Notebook

### Imports y Configuración
- Importación de librerías: NumPy, OpenCV, Matplotlib
- Configuración de visualización inline

### Funciones Auxiliares
- `show_images()`: Función para mostrar múltiples imágenes con subplots

### Parte 1: White Patch
- `get_max_pixel_values()`: Obtiene los valores máximos de cada canal RGB
- `apply_gain()`: Aplica la ganancia a cada canal según los valores máximos
- `white_patch_correction()`: Función principal que aplica la corrección White Patch
- Procesamiento y visualización de todas las imágenes en `white_patch/`
- Análisis y conclusiones sobre las limitaciones del método

### Parte 2: Histogramas
- Carga de imágenes en escala de grises
- Visualización de las imágenes
- `plot_images_with_histograms()`: Función para visualizar imágenes con sus histogramas para diferentes números de bins
- Comparación de histogramas con bins: 64, 128, 256, 512
- Análisis y conclusiones sobre la utilidad de histogramas como features

## Ejecución

> **Nota**: Si es la primera vez que ejecutas este proyecto, asegúrate de seguir los pasos de configuración inicial descritos en el [README principal](../../README.md#configuración-del-proyecto).

### Pasos para Ejecutar este TP

1. **Verifica que tienes todas las imágenes necesarias**:
   - `white_patch/` con todas las imágenes de prueba
   - `img1_tp.png` y `img2_tp.png` en el directorio raíz de TP1

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

4. **Abre el notebook** `TP1.ipynb` y selecciona el kernel "Python (VpC)":
   - En Jupyter: Kernel → Change Kernel → Python (VpC)
   - En JupyterLab: Click en el kernel actual (arriba a la derecha) → Select Kernel → Python (VpC)

5. **Ejecuta las celdas en orden secuencial**:
   - Primero las celdas de imports y funciones auxiliares
   - Luego la Parte 1 (White Patch)
   - Finalmente la Parte 2 (Histogramas)

## Resultados Esperados

### Parte 1
- Corrección de color en imágenes con diferentes iluminaciones
- Identificación de limitaciones: necesidad de áreas blancas en la imagen, posibles distorsiones en imágenes sin blancos, saturación en zonas claras

### Parte 2
- Visualización de histogramas que muestran la distribución tonal de cada imagen
- Comparación que revela diferencias en complejidad y distribución
- Conclusión sobre la utilidad limitada de histogramas como features (útil para clasificación global, no para detección específica debido a la pérdida de información espacial)

## Notas Técnicas

- El algoritmo White Patch asume que el punto más brillante es blanco puro
- Se puede mejorar usando percentiles (ej: 95%) en lugar del máximo absoluto
- Los histogramas son útiles para características globales pero no conservan información espacial

