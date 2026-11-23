# Análisis de sentimiento en Bluesky — Elecciones generales 29-N

Proyecto del CEIABD para ejecutar **en tu propio equipo** con VSCode.

## Contenido de la carpeta

| Archivo | Para qué sirve |
|---|---|
| `sentimiento_29N_bluesky.ipynb` | El cuaderno con todo el trabajo, explicado bloque a bloque |
| `requirements.txt` | Lista de librerías de Python que hay que instalar |
| `.env.example` | Plantilla para tus credenciales (se copia como `.env`) |
| `.gitignore` | Evita subir credenciales y datos a un repositorio por error |
| `datos/` | Se crea al ejecutar el cuaderno; ahí se guardan los CSV |

## 1. Requisitos

- **Python 3.10 o superior** (probado con 3.13). Comprueba tu versión con `python --version` (en Windows también `py --version`).
- **VSCode** con las extensiones **Python** y **Jupyter** de Microsoft.
- Unos **3 GB libres** en disco (PyTorch y el modelo de lenguaje ocupan bastante).
- Una cuenta gratuita de **Bluesky** con una **contraseña de aplicación** (Ajustes → Privacidad y seguridad → App Passwords).

## 2. Crear el entorno e instalar las librerías

### Opción A — desde VSCode (la más sencilla)

1. Abre la carpeta del proyecto: **Archivo → Abrir carpeta…**
2. Pulsa `Ctrl+Mayús+P` (`Cmd+Mayús+P` en Mac) y escribe **Python: Create Environment**.
3. Elige **Venv**, después tu versión de Python, y marca **requirements.txt** cuando te pregunte qué dependencias instalar.
4. Espera a que termine (varios minutos la primera vez).

### Opción B — desde la terminal

```bash
# Windows (PowerShell)
py -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt

# macOS / Linux
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

> **Windows:** si PowerShell no deja activar el entorno, ejecuta una vez
> `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` y vuelve a intentarlo.
>
> **Linux sin GPU NVIDIA:** para no descargar ~2 GB de librerías CUDA, instala antes PyTorch para CPU:
> `pip install torch --index-url https://download.pytorch.org/whl/cpu` y después `pip install -r requirements.txt`.

## 3. Configurar las credenciales

1. Copia `.env.example` y llama a la copia `.env` (en la misma carpeta).
2. Genera la sal secreta del grupo:
   ```bash
   python -c "import secrets; print(secrets.token_hex(16))"
   ```
3. Abre `.env` y rellena:
   ```
   BSKY_HANDLE=nombre.bsky.social
   BSKY_APP_PASSWORD=xxxx-xxxx-xxxx-xxxx
   HASH_SAL=la_cadena_que_has_generado
   ```
   Sin comillas ni espacios alrededor del `=`.

## 4. Abrir y ejecutar el cuaderno

1. Abre `sentimiento_29N_bluesky.ipynb` en VSCode.
2. Arriba a la derecha, pulsa **Seleccionar kernel** y elige el entorno **.venv** de esta carpeta.
3. Ejecuta las celdas en orden con `Mayús+Intro`.

La primera vez que llegues al bloque 7, se descargará el modelo de lenguaje (~435 MB). Se guarda en caché y no vuelve a descargarse.

En el bloque 10 (anotación manual), el cuadro para escribir aparece **en la parte superior de la ventana de VSCode**.

## 5. Problemas frecuentes

| Problema | Solución |
|---|---|
| `ModuleNotFoundError` | El kernel no es el de `.venv`. Cámbialo arriba a la derecha. |
| `RuntimeError: Faltan en el archivo .env` | El archivo se llama `.env.example` en vez de `.env`, o falta algún valor. |
| `Invalid identifier or password` | Usuario mal escrito (sin `@`) o estás usando la contraseña principal en vez de la de aplicación. |
| Error 429 al recoger | Demasiadas peticiones seguidas. Espera unos minutos y aumenta la pausa `time.sleep`. |
| Aviso sobre `HF_TOKEN` | Es informativo. Se puede ignorar. |
| La clasificación tarda mucho | Normal sin GPU. Prueba primero con menos términos o menos días. |

## 6. Normas

- No difundir resultados fuera del aula antes de las **20:00 del 29 de noviembre** (art. 69.7 LOREG).
- Los resultados describen la conversación en una red social, **no** la intención de voto.
- No compartir `.env` ni la carpeta `datos/`. Al terminar, borrad los datos en bruto, la sal y la contraseña de aplicación.
