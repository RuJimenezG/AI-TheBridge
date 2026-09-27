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


# Project break

Tiempo 100 % para el trabajo (sin clases).

Las semanas del 7/9 y 14/9 no hay clase normal, pero tenemos:
- 7/9 para revisar la práctica de SQL
- 9/9 microcredencial

Volvemos el 21/9, el sprint 11 se libera el miércoles 16/9.
24/9 presentaciones del project break, presentación obligatoria.

----

Falta:
6. Chunking
- Experimento variando CHUNK_SIZE y CHUNK_OVERLAP

15. Grounding y abstención
- Evaluación completa

20. Evaluación de generación
- Cerrar resultados y analizarlos

24. README
- ¿Capturas?

25. Reproducibilidad del README

- Documentar en informe_decisiones.md 
  - Paso de los más de 22000 chunks a la versión actual, comentando los problemas iniciales y las ventajas de la versión actual
- Arquitectura del proyecto.
- Logging
- Limitaciones conocidad
- Proveedores de embeddings

- documentar decisión de pasar de configurar ciertas variables de .env a config.py

- control de límite de peticiones para embedding con openai

python main.py --query "¿Qué es la Tarjeta Azul?" --k 3

Evaluar cómo afecta el tamaño de fragmentación a las fuentes documentales —FAQ, Markdown y PDF— manteniendo fija la estrategia específica del CSV.

python -c "import json, statistics; from pathlib import Path; d=json.loads(Path('output/chunks.json').read_text(encoding='utf-8')); t=[len(c['text']) for c in d['chunks'] if Path(str(c.get('metadata',{}).get('source',''))).name != 'Paradas CRTM.csv']; print('Chunks documentales:',len(t)); print('Media:',round(statistics.mean(t),2)); print('Mínimo:',min(t)); print('Máximo:',max(t)); print('Mediana:',statistics.median(t))"


python main.py --query "¿Qué es la Tarjeta Azul y en qué medios de transporte es válida?" --k 4

python main.py --query "¿Cuántas zonas tarifarias existen en el sistema de transporte de Madrid?" --k 4

python main.py --query "¿Qué pasa si cumplo 26 años mientras tengo cargado un abono de 30 días?" --k 4

python main.py --query "¿Qué bonificaciones se mantienen en 2026 sobre el Abono Transporte y la Tarjeta Azul?" --k 4

python main.py --query "Según la resolución oficial del CRTM, ¿se mantienen los precios del transporte público en 2026?" --k 4

python main.py --query "¿Qué incremento se aplica a las tarifas del transporte público en 2026 y qué excepciones hay?" --k 4

python main.py --query "¿En qué zona tarifaria se encuentra la parada de Plaza de Castilla?" --k 4



python main.py --ask "¿Qué es la Tarjeta Azul y en qué medios de transporte es válida?"

python main.py --ask "¿Qué pasa si cumplo 65 años mientras tengo cargado un abono de 30 días?"
