.gitignore típico

# Entornos virtuales
venv/
# Carpeta de dependencias (si instalas localmente en el proyecto)
packages/
lib/
lib64/
# Bytecode generado al ejecutar scripts
__pycache__/
*.pyc
*.pyo
*.pyd
*.so
# Configuraciones de VS Code
.vscode/
# Carpetas de Mac / Windows
.DS_Store
Thumbs.db
# SECRETOS
.env



https://developer.themoviedb.org/reference/authentication-how-do-i-generate-a-session-id

https://www.postman.com/

https://ai.google.dev/gemini-api/docs?hl=es-419

# Crear entorno virtual
python -m venv venv

Activar
source venv/Scripts/activate

Desactivar
deactivate

# Variables de entorno
crear un archivo llamado .env
añadir los secretos y variables necesarias (¿mayúsculas?)
pip install dotenv
import os
from dotenv import load_dotenv
después usar VARIABLE = os.getenv('VARIABLE dentro del archivo')

# Entrega del Team Challenge del sprint 3 el 2 de julio
Equipos: https://docs.google.com/spreadsheets/d/1SjZo-LYYY3UdOYthKE4zksahOnUahYlXAR5au_Si9Fs/edit?pli=1&gid=0#gid=0
Equipo 02:
Rubén Jiménez Gutiérrez
Miguel Gerardo López Iraheta
Guzmán López Barceló
David Cruz Puri