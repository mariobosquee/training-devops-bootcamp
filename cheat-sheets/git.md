# GIT CHEAT SHEET

**Bootcamp DevOps 2026** | Autor: Carlos Palanca
Referencia rapida para estudiantes STEM

---

## Configuracion inicial

```bash
git config --global user.name "Tu Nombre"       # Establecer nombre de usuario
git config --global user.email "tu@email.com"    # Establecer email
git config --global init.defaultBranch main      # Rama por defecto: main
git config --list                                # Ver configuracion actual
```

---

## Crear / clonar repositorio

```bash
git init                        # Crear repositorio nuevo en carpeta actual
git clone <url>                 # Descargar repositorio remoto completo
```

---

## Flujo basico

```bash
git status                      # Ver estado de archivos (modificados, staged, etc.)
git add <archivo>               # Agregar archivo al area de staging
git add .                       # Agregar TODOS los cambios al staging
git commit -m "mensaje"         # Guardar cambios con un mensaje descriptivo
git push                        # Subir commits al repositorio remoto
git pull                        # Descargar y aplicar cambios del remoto
git log --oneline               # Ver historial de commits en formato corto
git diff                        # Ver diferencias entre archivos modificados y staging
```

---

## Ramas

```bash
git branch                      # Listar ramas locales
git branch <nombre>             # Crear rama nueva
git switch <nombre>             # Cambiar a otra rama
git switch -c <nombre>          # Crear rama nueva y cambiar a ella
git merge <rama>                # Fusionar <rama> en la rama actual
git branch -d <nombre>          # Eliminar rama (ya fusionada)
```

---

## Resolver conflictos (paso a paso)

1. Ejecutar `git merge <rama>` o `git pull` -- aparece mensaje de conflicto
2. Abrir los archivos marcados con conflicto
3. Buscar los marcadores `<<<<<<<`, `=======`, `>>>>>>>`
4. Editar el archivo dejando solo el codigo correcto
5. `git add <archivo>` -- marcar conflicto como resuelto
6. `git commit` -- finalizar el merge

---

## GitHub (trabajo remoto)

```bash
git remote -v                           # Ver repositorios remotos configurados
git remote add origin <url>             # Conectar repo local con GitHub
git push -u origin main                 # Primer push (establece tracking)
git push origin <rama>                  # Subir una rama al remoto
```

**Crear Pull Request (PR):**

1. Subir tu rama con `git push origin <tu-rama>`
2. Ir a GitHub > pestaña "Pull requests" > "New pull request"
3. Seleccionar tu rama como source y main como destino
4. Escribir titulo y descripcion del cambio
5. Pedir revision y hacer merge cuando este aprobado

---

## Deshacer cambios

```bash
git restore <archivo>           # Descartar cambios en archivo (volver al ultimo commit)
git restore --staged <archivo>  # Sacar archivo del staging (sin perder cambios)
git reset --soft HEAD~1         # Deshacer ultimo commit (mantiene cambios en staging)
git reset --hard HEAD~1         # Deshacer ultimo commit (ELIMINA cambios, cuidado)
```

---

## Flujo tipico

```
1. git clone <url>              # Descargar el proyecto
2. git switch -c mi-feature     # Crear rama para tu cambio
3. (editar archivos)            # Hacer tus cambios en el codigo
4. git add .                    # Preparar cambios
5. git commit -m "descripcion"  # Guardar cambios
6. git push origin mi-feature   # Subir rama a GitHub
7. Crear Pull Request en GitHub # Solicitar revision
8. Merge del PR                 # Integrar cambios a main
```
