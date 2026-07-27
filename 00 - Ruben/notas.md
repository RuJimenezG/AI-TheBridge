## .gitignore típico

```
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
```

## Enlaces de interés

https://developer.themoviedb.org/reference/authentication-how-do-i-generate-a-session-id

https://www.postman.com/

https://ai.google.dev/gemini-api/docs?hl=es-419

## Python: Crear entorno virtual
python -m venv venv

### Activar
source .venv/Scripts/activate (bash)
.\.venv\Scripts\Activate.ps1 (powershell)

### Desactivar
deactivate

## Variables de entorno
crear un archivo llamado .env
añadir los secretos y variables necesarias (¿mayúsculas?)
pip install dotenv
import os
from dotenv import load_dotenv
después usar VARIABLE = os.getenv('VARIABLE dentro del archivo')

## TEAM CHALLENGE 1. Sprint 3 y 4 . Entrega el 2 de julio
Equipos: https://docs.google.com/spreadsheets/d/1SjZo-LYYY3UdOYthKE4zksahOnUahYlXAR5au_Si9Fs/edit?pli=1&gid=0#gid=0
Equipo 02:
Rubén Jiménez Gutiérrez
Miguel Gerardo López Iraheta
Guzmán López Barceló
David Cruz Puri

## TEAM CHALLENGE 2. Sprint 5 a 7. Entrega el 2 de julio

Grupo 2:

• Rubén Jiménez Gutiérrez.
• Miguel Gerardo Lopez Iraheta.
• Guzmán López Barceló.
• David Cruz Puri.


# Auto Deberes para agosto
- Investigar workspaces de Visual Studio Code
- Revisar el primer sprint de modularización (Sprint 2, Unidad 2).
- Revisar en detalle los 3 proyectos del sprint 5.