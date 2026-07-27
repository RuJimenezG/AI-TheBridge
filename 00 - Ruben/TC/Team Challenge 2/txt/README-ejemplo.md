# Team Challenge - Pelis (Team Challenge Sprint 03)

Este proyecto implementa un catálogo de películas mediante la ingesta y análisis exploratorio con Pandas de un conjunto de datos del sector cinematográfico (películas, valoraciones, etiquetas y enlaces).
Posteriormente, se conecta con la API de TMDB para enriquecer el catálogo obteniendo información adicional de cada película mediante peticiones HTTP.
Por último, utiliza los modelos de lenguaje de Gemini para traducir y generar las sinopsis en español.

---

## Comenzando

Sigue estas instrucciones para obtener una copia del proyecto operativa en tu máquina local para el desarrollo y pruebas.

### Prerrequisitos

Antes de empezar, asegúrate de tener instalado lo siguiente:
* [Python 3.10+](https://www.python.org/)
* [Git](https://git-scm.com/)

---

## Instalación y Configuración

Sigue estos pasos en tu terminal (por ejemplo, en PowerShell de VS Code) para configurar tu entorno de trabajo:

### 1. Clonar el repositorio
Clona el proyecto en tu máquina local y accede a la carpeta del repositorio:
```bash
git clone https://github.com/dcruzpuri/team_challenge_pelis.git
cd team_challenge_pelis
```

### 2. Crear y activar un entorno virtual
Es recomendable utilizar un entorno virtual para aislar las dependencias del proyecto:

En Linux/macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

En Windows (Command Prompt):

```DOS
python -m venv venv
venv\Scripts\activate
```

### 3. Instalar las dependencias
Instala todas las librerías de Python necesarias ejecutando:

```Bash
pip install -r requirements.txt
```

Nota: Asegúrate de que pandas, requests y google-genai (o el SDK correspondiente de Gemini) estén incluidos en tu archivo requirements.txt.

### 4. Configurar las claves de entorno
El proyecto requiere credenciales de API para conectarse con TMDB y Gemini. Crea un archivo llamado .env en la raíz del proyecto y añade tus claves de la siguiente manera:



```
# Clave de API de TMDB (The Movie Database)
TMDB_API_KEY=tu_api_key_de_tmdb_aqui
```

```
# Clave de API de Google Gemini
GEMINI_API_KEY=tu_api_key_de_gemini_aqui
```

---

## Ejecución del Proyecto
Una vez configurado el entorno y las variables, puedes ejecutar las celdas de código del notebook en /notebooks/Team_Challenge_sprint03_04.ipynb por orden.

---

## Estructura del Proceso
Ingesta y Análisis de Datos: Carga de los archivos CSV (movies, ratings, tags, links) y análisis exploratorio con Pandas.

Enriquecimiento (API TMDB): Consultas HTTP automatizadas utilizando los enlaces (imdbId / tmdbId) para recuperar sinopsis y web de la película.

Procesamiento de Texto (Gemini): Traducción y generación inteligente de las sinopsis en español optimizando la calidad lingüística del catálogo.