# KUBECTL CHEAT SHEET

**Bootcamp DevOps 2026** | Autor: Carlos Palanca
Referencia rapida para estudiantes STEM

---

## Informacion del cluster

```bash
kubectl cluster-info                  # Ver URL del cluster y servicios principales
kubectl get nodes                     # Listar nodos del cluster y su estado
kubectl version --short               # Ver version del cliente y del servidor
```

---

## Pods

```bash
kubectl get pods                      # Listar pods en el namespace actual
kubectl get pods -o wide              # Listar pods con IP y nodo asignado
kubectl describe pod <nombre>         # Ver detalles completos de un pod
kubectl logs <nombre>                 # Ver logs de un pod
kubectl logs <nombre> -f              # Ver logs en tiempo real (follow)
kubectl exec -it <nombre> -- bash     # Abrir terminal dentro del pod
kubectl port-forward pod/<nombre> 8080:80   # Redirigir puerto local al pod
kubectl delete pod <nombre>           # Eliminar un pod
```

---

## Deployments

```bash
kubectl get deploy                    # Listar deployments
kubectl describe deploy <nombre>      # Ver detalles de un deployment
kubectl scale deploy <nombre> --replicas=3   # Escalar a 3 replicas
kubectl rollout status deploy/<nombre>       # Ver estado del despliegue
kubectl rollout undo deploy/<nombre>         # Revertir al despliegue anterior
kubectl set image deploy/<nombre> contenedor=imagen:tag   # Cambiar imagen
```

---

## Services

```bash
kubectl get svc                       # Listar servicios
kubectl describe svc <nombre>         # Ver detalles de un servicio
kubectl port-forward svc/<nombre> 8080:80   # Redirigir puerto local al servicio
```

---

## ConfigMaps y Secrets

```bash
kubectl create configmap <nombre> --from-literal=clave=valor   # Crear ConfigMap
kubectl get cm                        # Listar ConfigMaps
kubectl describe cm <nombre>          # Ver contenido de un ConfigMap
kubectl create secret generic <nombre> --from-literal=clave=valor   # Crear Secret
kubectl get secrets                   # Listar Secrets
```

---

## Namespaces

```bash
kubectl get ns                        # Listar namespaces
kubectl create ns <nombre>            # Crear un namespace
kubectl get pods -n <namespace>       # Listar pods en un namespace especifico
kubectl config set-context --current --namespace=<ns>   # Cambiar namespace por defecto
```

---

## Apply y Delete

```bash
kubectl apply -f archivo.yaml         # Crear o actualizar recursos desde YAML
kubectl delete -f archivo.yaml        # Eliminar recursos definidos en YAML
kubectl apply -f directorio/          # Aplicar todos los YAML de un directorio
```

---

## Debug rapido

```bash
kubectl get events --sort-by=.lastTimestamp   # Ver eventos recientes del cluster
kubectl describe pod <nombre>                 # Revisar Events y Conditions del pod
kubectl logs <nombre> --previous              # Ver logs del contenedor anterior (si crasheo)
```

---

## Flujo tipico

1. Escribir manifiesto YAML (Deployment + Service)
2. `kubectl apply -f mi-app.yaml`
3. `kubectl get pods` -- verificar que estan Running
4. `kubectl port-forward svc/mi-app 8080:80` -- probar localmente
5. Modificar el YAML (nueva imagen, replicas, config)
6. `kubectl apply -f mi-app.yaml` -- aplicar cambios
7. `kubectl rollout status deploy/mi-app` -- verificar despliegue
