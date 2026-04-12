# GITHUB ACTIONS CHEAT SHEET

**Bootcamp DevOps 2026** | Autor: Carlos Palanca
Referencia rapida para estudiantes STEM

---

## Estructura de un workflow

Los workflows se definen en archivos YAML dentro de `.github/workflows/`.

```yaml
name: CI                        # Nombre del workflow
on: push                        # Evento que lo dispara
jobs:                           # Grupo de trabajos
  build:                        # Nombre del job
    runs-on: ubuntu-latest      # Maquina donde se ejecuta
    steps:                      # Pasos secuenciales
      - uses: actions/checkout@v4   # Usar una accion existente
      - run: echo "Hola mundo"      # Ejecutar un comando
```

---

## Triggers (eventos que disparan el workflow)

```yaml
on: push                                    # Cada push a cualquier rama
on:
  push:
    branches: [main]                        # Solo push a main
on:
  pull_request:
    branches: [main]                        # Cuando se abre PR hacia main
on:
  schedule:
    - cron: '0 8 * * 1'                     # Programado (lunes a las 8:00 UTC)
on: workflow_dispatch                       # Manual desde la interfaz de GitHub
```

---

## Acciones comunes

```yaml
- uses: actions/checkout@v4                 # Descargar el codigo del repo
- uses: actions/setup-node@v4              # Instalar Node.js
  with:
    node-version: '20'
- uses: actions/setup-python@v5            # Instalar Python
  with:
    python-version: '3.12'
- uses: actions/cache@v4                   # Cachear dependencias (acelera builds)
  with:
    path: ~/.npm
    key: ${{ runner.os }}-npm-${{ hashFiles('**/package-lock.json') }}
- uses: docker/build-push-action@v5        # Construir y subir imagen Docker
  with:
    push: true
    tags: usuario/app:latest
```

---

## Secrets y variables

```yaml
env:
  MI_VARIABLE: "valor"                              # Variable de entorno a nivel de step/job

steps:
  - run: echo ${{ secrets.GITHUB_TOKEN }}           # Token automatico del repo
  - run: echo ${{ secrets.MI_SECRET }}              # Secret definido en Settings > Secrets
  - run: echo ${{ vars.MI_VARIABLE }}               # Variable definida en Settings > Variables
```

**Donde configurar secrets:** GitHub > tu repo > Settings > Secrets and variables > Actions

---

## Expresiones utiles

```yaml
if: github.ref == 'refs/heads/main'                 # Ejecutar solo en rama main
if: github.event_name == 'pull_request'              # Ejecutar solo en PRs
if: success()                                        # Solo si los pasos anteriores pasaron
if: failure()                                        # Solo si algo fallo

needs: [build]                                       # Este job depende de otro

strategy:
  matrix:
    node-version: [18, 20, 22]                       # Ejecutar en multiples versiones
```

---

## Comandos utiles

```yaml
- run: npm install                          # Comando simple

- run: |                                    # Comando multi-linea
    npm install
    npm run build
    npm test

- name: Paso con nombre descriptivo         # Nombrar el paso para mejor lectura
  run: npm test
```

---

## Flujo tipico

```
1. Crear archivo .github/workflows/ci.yml     # Definir el workflow
2. Escribir los triggers, jobs y steps         # Configurar que hace y cuando
3. git add, commit y push                      # Subir cambios al repo
4. Ir a GitHub > pestaña "Actions"             # Ver ejecucion del workflow
5. Revisar logs si algo falla                  # Depurar errores
6. Corregir y push de nuevo                    # El workflow se re-ejecuta automaticamente
```

---

## Ejemplo minimo: CI basico

```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm install
      - run: npm test
```

Este workflow se ejecuta en cada push o PR a main: descarga el codigo, instala Node.js, instala dependencias y corre los tests.
