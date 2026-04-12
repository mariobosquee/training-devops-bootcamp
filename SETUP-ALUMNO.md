# Bootcamp DevOps 2026 — Preparacion del entorno

**Autor:** Carlos Palanca | T-Systems
**Objetivo:** Tener el entorno listo ANTES del primer dia del bootcamp
**Tiempo estimado:** 20-30 minutos
**Sistema operativo:** Windows 10/11

---

## Por que WSL2

En este bootcamp usamos herramientas que funcionan en Linux (Docker, Kubernetes, Terraform, etc.). En vez de instalar todo en Windows y lidiar con incompatibilidades, vamos a instalar **WSL2** (Windows Subsystem for Linux). Esto os da una terminal Ubuntu real dentro de Windows, sin necesidad de maquinas virtuales.

**Ventajas:**
- Todos los comandos del bootcamp funcionan tal cual, sin adaptaciones
- Docker Desktop se integra automaticamente
- VS Code funciona perfecto con WSL2
- Aprendeis Linux de propina (que es donde corren los servidores de verdad)

---

## Paso 1: Instalar WSL2 con Ubuntu (5 min)

Abrid **PowerShell como Administrador** (clic derecho en el icono de PowerShell > "Ejecutar como administrador") y ejecutad:

```powershell
wsl --install -d Ubuntu
```

Cuando termine, **reiniciad el ordenador** si os lo pide.

Al reiniciar, se abrira una ventana de Ubuntu pidiendo que creeis un usuario y contrasena. Elegid algo sencillo que recordeis (por ejemplo, vuestro nombre en minusculas y una contrasena corta). Esta contrasena es solo para el Ubuntu local, no tiene que ser super segura.

**Verificar que funciona:**

Abrid una terminal (Windows Terminal o CMD) y escribid:

```powershell
wsl --version
```

Debeis ver algo como `WSL version: 2.x.x`. Si veis version 1, actualizad con `wsl --update`.

Para entrar en Ubuntu en cualquier momento, abrid una terminal y escribid:

```powershell
wsl
```

> **A partir de aqui, TODOS los comandos se ejecutan dentro de Ubuntu (WSL2).** Cuando veais `$` al principio de la linea, estais dentro.

---

## Paso 2: Actualizar Ubuntu (2 min)

Dentro de Ubuntu:

```bash
sudo apt update && sudo apt upgrade -y
```

Os pedira la contrasena que creasteis en el paso anterior.

---

## Paso 3: Instalar Docker Desktop (5 min)

1. Descargad Docker Desktop desde https://www.docker.com/products/docker-desktop/
2. Instaladlo normalmente en Windows
3. **Importante:** Durante la instalacion (o despues en Settings), aseguraos de que esta marcada la opcion **"Use the WSL 2 based engine"**
4. Despues de instalar, abrid Docker Desktop
5. Id a **Settings > Resources > WSL Integration** y activad la integracion con **Ubuntu**
6. Pulsad **Apply & Restart**

**Verificar que funciona** (dentro de Ubuntu/WSL2):

```bash
docker version
```

Debeis ver dos secciones: `Client` y `Server`. Si solo veis `Client` o da error, comprobad que Docker Desktop esta abierto en Windows y que la integracion WSL esta activada.

```bash
docker run --rm hello-world
```

Debeis ver el mensaje `Hello from Docker!`.

---

## Paso 4: Instalar Git y configurarlo (2 min)

```bash
sudo apt install -y git

git config --global user.name "Tu Nombre Apellido"
git config --global user.email "tu@email.com"
```

**Verificar:**

```bash
git --version
```

Debeis ver `git version 2.x.x`.

> Necesitais tambien una cuenta en https://github.com. Si no la teneis, creadla (es gratis).

---

## Paso 5: Instalar kubectl, kind y Helm (3 min)

Estas herramientas las usaremos a partir del Dia 3, pero las instalamos ahora para no perder tiempo despues.

**kubectl** (gestionar Kubernetes):

```bash
curl -LO "https://dl.k8s.io/release/$(curl -sL https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

**kind** (crear clusters Kubernetes locales):

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-amd64
chmod +x ./kind && sudo mv ./kind /usr/local/bin/kind
```

**Helm** (instalar apps en Kubernetes):

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

**Verificar las tres:**

```bash
kubectl version --client
kind version
helm version
```

Cada comando debe mostrar la version sin errores.

---

## Paso 6: Instalar Terraform (2 min)

```bash
wget -qO- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install -y terraform
```

**Verificar:**

```bash
terraform version
```

Debeis ver `Terraform v1.x.x`.

---

## Paso 7: Instalar Node.js (2 min)

Necesitamos Node.js para probar las apps en local.

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

**Verificar:**

```bash
node --version
npm --version
```

Debeis ver `v20.x.x` y `10.x.x` respectivamente.

---

## Paso 8: Configurar VS Code con WSL2 (3 min)

1. Abrid **VS Code** en Windows
2. Instalad la extension **"WSL"** (de Microsoft) desde el marketplace de extensiones
3. Pulsad `Ctrl+Shift+P` y escribid "WSL: Connect to WSL"
4. VS Code se reabrira conectado a Ubuntu

A partir de ahora, cuando abrais una terminal en VS Code (Ctrl+`), sera una terminal Ubuntu. Y cuando editeis archivos, estareis editando dentro del sistema de archivos de WSL2.

**Tip:** Para abrir VS Code directamente desde Ubuntu, navegad a una carpeta y escribid:

```bash
code .
```

---

## Paso 9: Verificacion final

Ejecutad todos estos comandos seguidos para confirmar que todo esta instalado:

```bash
echo "=== Verificacion del entorno ==="
echo ""
echo "Docker:    $(docker --version 2>&1)"
echo "Git:       $(git --version 2>&1)"
echo "kubectl:   $(kubectl version --client --short 2>&1)"
echo "kind:      $(kind version 2>&1)"
echo "Helm:      $(helm version --short 2>&1)"
echo "Terraform: $(terraform version -json 2>&1 | head -1)"
echo "Node.js:   $(node --version 2>&1)"
echo "npm:       $(npm --version 2>&1)"
echo ""
echo "=== Todo listo para el bootcamp! ==="
```

Si todos los comandos muestran una version y ninguno dice "command not found", estais listos.

---

## Resumen de lo instalado

| Herramienta | Para que | Dia |
|-------------|----------|-----|
| WSL2 + Ubuntu | Terminal Linux en Windows | Todos |
| Docker Desktop | Contenedores | Todos |
| Git | Control de versiones | Todos |
| VS Code + extension WSL | Editor de codigo | Todos |
| Node.js + npm | Probar apps en local | Dia 1-2 |
| kubectl | Gestionar Kubernetes | Dia 3-4 |
| kind | Clusters K8s locales | Dia 3-4 |
| Helm | Instalar apps en K8s | Dia 3-4 |
| Terraform | Infraestructura como codigo | Dia 4 |

---

## Problemas frecuentes

| Problema | Solucion |
|----------|----------|
| `wsl --install` da error | Comprobad que teneis Windows 10 version 2004+ o Windows 11. Actualizad Windows si es necesario. |
| WSL dice "version 1" en vez de 2 | Ejecutad `wsl --set-default-version 2` en PowerShell como admin. |
| Docker no funciona dentro de WSL2 | Abrid Docker Desktop en Windows. Id a Settings > Resources > WSL Integration y activad Ubuntu. |
| `docker version` solo muestra Client | Docker Desktop no esta corriendo o la integracion WSL no esta activada. |
| `sudo` pide contrasena y no la acepta | Es la contrasena que creasteis al instalar Ubuntu en WSL, no la de Windows. |
| `curl` o `wget` fallan con error de red | Comprobad vuestra conexion a internet. Si estais detras de un proxy corporativo, preguntad al instructor. |
| VS Code no se conecta a WSL | Comprobad que la extension "WSL" esta instalada. Reiniciad VS Code. |
| "No space left on device" | WSL2 comparte disco con Windows. Liberad espacio en C:. |

---

## Paso 10: Clonar el repositorio del bootcamp

Este es el repositorio con las apps en las que trabajareis los 4 dias. El instructor os dara la URL exacta. El formato es:

```bash
mkdir -p ~/bootcamp
cd ~/bootcamp
git clone https://github.com/ORGANIZACION/training-devops-bootcamp.git
cd training-devops-bootcamp
```

Verificad que veis las apps:

```bash
ls apps/
```

Debeis ver: `campuseats`, `monitoring`, `myfitness`, `quizbattle`.

Abrid VS Code:

```bash
code .
```

---

## Donde trabajar dentro de WSL2

Trabajad siempre dentro de `~/bootcamp/training-devops-bootcamp/`. Es la carpeta del repositorio dentro de Ubuntu.

**No trabajeis en `/mnt/c/...`** (que es vuestro disco C: de Windows). El rendimiento es mucho peor. Trabajad siempre dentro de `~/` (la carpeta home de Ubuntu).

---

**Si teneis dudas con la instalacion, traedlas al primer dia y las resolveremos juntos. Lo importante es que WSL2 y Docker Desktop esten funcionando.**
