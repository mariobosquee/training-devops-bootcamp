# ARGOCD CHEAT SHEET

**Bootcamp DevOps 2026** | Autor: Carlos Palanca
Referencia rapida para estudiantes STEM

---

## Instalacion

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd            # Verificar que todos los pods estan Running
```

---

## Acceso a la UI

```bash
# Obtener password inicial del admin
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# Exponer la UI en localhost
kubectl port-forward svc/argocd-server -n argocd 8080:443

# Abrir en el navegador
# https://localhost:8080
# Usuario: admin
# Password: el obtenido arriba
```

---

## CLI

```bash
argocd login localhost:8080 --insecure          # Conectar CLI al servidor
argocd app list                                  # Listar todas las aplicaciones
argocd app get <nombre>                          # Ver detalles de una aplicacion
argocd app sync <nombre>                         # Sincronizar (desplegar) una app
argocd app delete <nombre>                       # Eliminar una aplicacion
argocd app diff <nombre>                         # Ver diferencias pendientes
```

---

## Crear Application via UI

1. Abrir la UI de ArgoCD en el navegador
2. Click en **New App**
3. Configurar:
   - **Application Name**: nombre de la app
   - **Project**: default
   - **Sync Policy**: Manual o Automatic
4. Source:
   - **Repository URL**: URL del repo Git
   - **Path**: ruta dentro del repo donde estan los manifiestos
5. Destination:
   - **Cluster URL**: https://kubernetes.default.svc (cluster local)
   - **Namespace**: namespace donde desplegar
6. Click en **Create**
7. Click en **Sync** para desplegar

---

## Crear Application via YAML

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: mi-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/usuario/repo.git
    targetRevision: main
    path: k8s/
  destination:
    server: https://kubernetes.default.svc
    namespace: mi-namespace
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

```bash
kubectl apply -f application.yaml     # Crear la Application en ArgoCD
```

---

## Sync y Rollback

```bash
argocd app sync <nombre>              # Sincronizar app con el estado del repo Git
argocd app sync <nombre> --force      # Forzar sincronizacion (reemplaza recursos)
argocd app rollback <nombre> <id>     # Revertir a una version anterior del despliegue
argocd app history <nombre>           # Ver historial de despliegues para elegir id
```

---

## Flujo tipico

1. Instalar ArgoCD en el cluster con `kubectl apply`
2. Obtener password y acceder a la UI o CLI
3. Crear una Application apuntando a un repo Git
4. `argocd app sync mi-app` -- sincronizar por primera vez
5. Hacer un cambio en el repo Git (push)
6. ArgoCD detecta el cambio (auto-sync o manual sync)
7. Verificar el estado en la UI o con `argocd app get mi-app`
