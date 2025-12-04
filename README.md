# VpC - Visión por Computadora

Repositorio de Trabajos Prácticos de Visión por Computadora.

## Estructura del Proyecto

```
VpC-main/
├── Material_TPs/
│   ├── TP1/          # White Patch y Análisis de Histogramas
│   ├── TP2/          # Detector de Máximo Enfoque
│   └── TP3/          # Detección de Logotipos con Template Matching
├── pyproject.toml    # Configuración del proyecto
├── .python-version   # Versión de Python
└── README.md         # Este archivo
```

## Requisitos Previos

- Python 3.10 o superior
- [uv](https://github.com/astral-sh/uv) instalado

### Instalación de uv

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# O con pip
pip install uv
```

## Configuración del Proyecto

### 1. Inicializar el entorno virtual con uv

```bash
# Crear y activar el entorno virtual
uv venv

# En macOS/Linux
source .venv/bin/activate

# En Windows
.venv\Scripts\activate
```

### 2. Instalar dependencias

```bash
# Instalar todas las dependencias del proyecto
uv pip install -e .

# O instalar directamente desde pyproject.toml
uv sync
```

### 3. Configurar Jupyter

```bash
# Instalar el kernel de Jupyter para el entorno virtual
uv pip install ipykernel
python -m ipykernel install --user --name=vpc --display-name "Python (VpC)"
```

## Uso

### Ejecutar los notebooks

1. Activa el entorno virtual:
   ```bash
   source .venv/bin/activate  # macOS/Linux
   # o
   .venv\Scripts\activate     # Windows
   ```

2. Inicia Jupyter:
   ```bash
   jupyter notebook
   ```

3. Selecciona el kernel "Python (VpC)" en los notebooks

### Trabajos Prácticos

- **TP1**: Corrección de color con White Patch y análisis de histogramas
  - Ver [TP1/README.md](Material_TPs/TP1/README.md) para más detalles

- **TP2**: Detector de máximo enfoque en video
  - Ver [TP2/README.md](Material_TPs/TP2/README.md) para más detalles

- **TP3**: Detección de logotipos con Template Matching
  - Ver [TP3/README.md](Material_TPs/TP3/README.md) para más detalles

## Dependencias Principales

- `numpy`: Operaciones numéricas y arrays
- `opencv-python`: Procesamiento de imágenes y video
- `matplotlib`: Visualización y gráficos
- `scipy`: Operaciones científicas (FFT, etc.)
- `jupyter`: Entorno de notebooks

## Comandos Útiles de uv

```bash
# Ver dependencias instaladas
uv pip list

# Ver información del proyecto
uv pip show vpc

# Actualizar dependencias
uv pip install --upgrade -e .

# Agregar una nueva dependencia
uv pip install nombre-paquete

# Agregar dependencia de desarrollo
uv pip install --dev nombre-paquete

# Sincronizar con pyproject.toml (instala todas las dependencias)
uv sync

# Crear entorno virtual con versión específica de Python
uv venv --python 3.11

# Ejecutar comandos dentro del entorno virtual sin activarlo
uv run python script.py
uv run jupyter notebook

# Verificar que el entorno está activo
which python  # Debe apuntar a .venv/bin/python
```

## Notas

- Los archivos de video (`.mov`, `.mp4`) están en `.gitignore` por tamaño
- Los notebooks deben ejecutarse con el kernel configurado para el entorno virtual
- Asegúrate de tener todas las imágenes/videos necesarios en los directorios correspondientes antes de ejecutar los notebooks
