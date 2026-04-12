# HELM CHEAT SHEET

**Bootcamp DevOps 2026** | Autor: Carlos Palanca
Referencia rapida para estudiantes STEM

---

## Repositorios

```bash
helm repo add <nombre> <url>          # Agregar un repositorio de charts
helm repo update                      # Actualizar indice de todos los repos
helm repo list                        # Listar repositorios configurados
helm search repo <termino>            # Buscar charts en los repos locales
helm repo remove <nombre>             # Eliminar un repositorio
```

Ejemplo:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo nginx
```

---

## Instalar

```bash
helm install <release> <chart>                # Instalar un chart con nombre de release
helm install <release> <chart> -f values.yaml # Instalar con valores personalizados
helm install <release> <chart> --set key=val  # Instalar sobreescribiendo un valor
helm install <release> <chart> -n <namespace> # Instalar en un namespace especifico
```

Ejemplo:

```bash
helm install mi-nginx bitnami/nginx -f mis-valores.yaml
```

---

## Gestionar releases

```bash
helm list                             # Listar releases instalados
helm list -A                          # Listar releases en todos los namespaces
helm status <release>                 # Ver estado de un release
helm history <release>                # Ver historial de revisiones
helm upgrade <release> <chart>        # Actualizar un release
helm upgrade <release> <chart> -f values.yaml   # Actualizar con nuevos valores
helm rollback <release> <revision>    # Revertir a una revision anterior
helm uninstall <release>              # Desinstalar un release
```

---

## Inspeccionar charts

```bash
helm show chart <chart>               # Ver metadata del chart (Chart.yaml)
helm show values <chart>              # Ver valores por defecto del chart
helm template <release> <chart>       # Renderizar templates sin instalar (preview)
helm template <release> <chart> -f values.yaml   # Preview con valores custom
```

---

## ArtifactHub

ArtifactHub es el buscador central de charts de Helm:

- URL: **https://artifacthub.io**
- Buscar charts por nombre o categoria
- Ver documentacion, valores disponibles y ejemplos
- Copiar el comando `helm repo add` directamente desde la pagina del chart

---

## Flujo tipico

1. `helm repo add bitnami https://charts.bitnami.com/bitnami` -- agregar repo
2. `helm search repo <app>` -- buscar el chart deseado
3. `helm show values bitnami/<app>` -- revisar valores configurables
4. Crear archivo `values.yaml` con las personalizaciones
5. `helm install mi-release bitnami/<app> -f values.yaml` -- instalar
6. `helm list` -- verificar que el release esta desplegado
7. Modificar `values.yaml` segun sea necesario
8. `helm upgrade mi-release bitnami/<app> -f values.yaml` -- actualizar
9. `helm rollback mi-release 1` -- revertir si algo falla
