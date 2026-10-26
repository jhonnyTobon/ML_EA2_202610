# ML_EA2_202610
se crea este repositorio con el fin de hacer la entrega de la segunda evidencia de aprendizaje para la asignatura de Machine Learning

jhonny arley tobon acevedo 1017222491
javier hernan martinez ceballos 13015595

## Descripción del notebook de regresión lineal

El notebook [src/sincronicas/Semana_2SINC.ipynb](./src/sincronicas/Semana_2SINC.ipynb) tiene como objetivo desarrollar y evaluar modelos de regresión lineal para predecir el precio de viviendas en Madrid a partir de variables como el tamaño en metros cuadrados, el número de habitaciones y otros atributos del conjunto de datos disponible.

En este trabajo se carga un dataset de viviendas desde Hugging Face, se exploran los datos, se prepara la información para entrenamiento y prueba, y se entrenan modelos de regresión simple y múltiple usando scikit-learn. Además, el notebook incluye visualizaciones de la relación entre los datos y las predicciones, compara el rendimiento de los modelos mediante métricas como MAE, MSE y R², y revisa la calidad del ajuste a través de gráficos y diagnósticos de error. También se hace uso de una clase auxiliar (`RegresionLineal`) para encapsular la lógica de preparación, entrenamiento, evaluación y predicción del modelo.

En resumen, el notebook sirve como una práctica de Machine Learning aplicada a un problema de regresión, mostrando el flujo completo de análisis: carga de datos, preparación, modelado, validación y evaluación de resultados.

Uso
cd /workspaces/RP_AD2026Q4
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
pip install pyarrow openpyxl  # opcional: parquet y xlsx
python scripts/build_notebooks.py   # opcional: regenerar notebooks
jupyter lab
En Bash, el entorno virtual debe activarse con source (o con un punto):

source .venv/bin/activate
# equivalente:
. .venv/bin/activate
No se debe ejecutar .venv/bin/activate directamente, porque activate es un script que modifica la sesión actual de la terminal. Cuando se activa correctamente, el prompt muestra (.venv).

Cada ejecución del modelo genera un resumen textual en la carpeta `reportes/` con la configuración, métricas y diagnóstico del ajuste.

Autenticación en Hugging Face
Para descargar datasets públicos normalmente no se necesita un token. Si el dataset es privado o requiere aceptación de condiciones de uso, configura un token de Hugging Face siguiendo estos pasos.

1. Crear un token
Inicia sesión en Hugging Face.
Abre Settings > Access Tokens.
Selecciona Create new token.
Asigna un nombre descriptivo, por ejemplo RP_AD2026Q4.
Selecciona el permiso mínimo necesario: Read.
Presiona Create token y copia el token. Normalmente comienza por hf_.
Guarda el token de forma segura y no lo incluyas en el código fuente.

2. Instalar la CLI e iniciar sesión
Con el entorno virtual activado, instala o actualiza la librería de Hugging Face:

python -m pip install --upgrade huggingface_hub
Inicia sesión desde la terminal:

hf auth login
Cuando la terminal solicite el token, pégalo y presiona Enter. Es normal que no se muestren caracteres mientras lo escribes o pegas.

La sesión queda guardada localmente para que HFDatasetLoader pueda usarla automáticamente. Comprueba que la autenticación funcionó:

hf auth whoami
Si tu instalación usa la CLI antigua, el comando equivalente es:

huggingface-cli login
3. Cargar el dataset autenticado
Después de iniciar sesión, no es necesario pasar el token al constructor:

from rp_utils import HFDatasetLoader

loader = HFDatasetLoader(
    "Fast_Food_Restaurants",
    filename="datos/fast_food.csv",
)
df = loader.load()
No escribas tokens directamente en archivos Python ni los subas a GitHub. Si un token se expone accidentalmente, revócalo desde Access Tokens y crea uno nuevo.

Carga de un dataset como DataFrame
HFDatasetLoader recibe el identificador del dataset, descarga la división indicada y devuelve un pandas.DataFrame. filename y file_type son opcionales; si se proporciona filename, el DataFrame también se guarda en csv, json, parquet o xlsx.

from rp_utils import HFDatasetLoader

loader = HFDatasetLoader(
    "Fast_Food_Restaurants",
    filename="datos/fast_food.csv",
)
df = loader.load()
Para ejecutar este ejemplo manualmente, crea un archivo prueba_loader.py en la raíz del proyecto con el código anterior y ejecuta:

PYTHONPATH=src python prueba_loader.py
También se puede ejecutar directamente desde la terminal:

PYTHONPATH=src python -c '
from rp_utils import HFDatasetLoader

df = HFDatasetLoader(
    "Fast_Food_Restaurants",
    filename="datos/fast_food.csv",
).load()

print(df.head())
'
También se puede indicar el formato explícitamente:

loader = HFDatasetLoader(
    "Fast_Food_Restaurants",
    filename="datos/fast_food",
    file_type="parquet",
)
df = loader.load()
