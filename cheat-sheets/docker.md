# DOCKER CHEAT SHEET

**Bootcamp DevOps 2026** | Autor: Carlos Palanca
Referencia rapida para estudiantes STEM

---

## Imagenes

```bash
docker pull <imagen>                    # Descargar imagen de Docker Hub
docker build -t <nombre>:<tag> .        # Construir imagen desde Dockerfile
docker images                           # Listar imagenes locales
docker rmi <imagen>                     # Eliminar imagen
docker tag <imagen> <nuevo-nombre>      # Renombrar / etiquetar imagen
docker push <imagen>                    # Subir imagen a un registry
```

---

## Contenedores

```bash
docker run <imagen>                     # Crear y ejecutar contenedor
docker ps                               # Listar contenedores en ejecucion
docker ps -a                            # Listar TODOS los contenedores (incluye parados)
docker stop <contenedor>                # Detener contenedor
docker start <contenedor>               # Iniciar contenedor detenido
docker rm <contenedor>                  # Eliminar contenedor
docker logs <contenedor>                # Ver logs del contenedor
docker logs -f <contenedor>             # Ver logs en tiempo real
docker exec -it <contenedor> bash       # Abrir terminal dentro del contenedor
```

---

## Flags importantes de docker run

```bash
docker run -d <imagen>                  # Ejecutar en segundo plano (detached)
docker run -p 8080:80 <imagen>          # Mapear puerto host:contenedor
docker run -v ./datos:/app <imagen>     # Montar volumen (carpeta local:carpeta contenedor)
docker run --name mi-app <imagen>       # Asignar nombre al contenedor
docker run -e MI_VAR=valor <imagen>     # Definir variable de entorno
docker run --rm <imagen>                # Eliminar contenedor al detenerse
```

**Ejemplo combinado:**

```bash
docker run -d --name web -p 8080:80 -e ENV=prod --rm nginx
```

---

## Dockerfile (instrucciones principales)

```dockerfile
FROM node:20-alpine          # Imagen base
WORKDIR /app                 # Directorio de trabajo dentro del contenedor
COPY package*.json ./        # Copiar archivos al contenedor
RUN npm install              # Ejecutar comando durante la construccion
COPY . .                     # Copiar el resto del codigo
ENV PORT=3000                # Definir variable de entorno por defecto
EXPOSE 3000                  # Documentar puerto que usa la app
CMD ["node", "server.js"]    # Comando que se ejecuta al iniciar el contenedor
```

---

## Docker Compose

Archivo `docker-compose.yml` para definir multiples servicios.

```bash
docker compose up                       # Levantar todos los servicios
docker compose up -d                    # Levantar en segundo plano
docker compose down                     # Detener y eliminar contenedores
docker compose ps                       # Ver estado de los servicios
docker compose logs                     # Ver logs de todos los servicios
docker compose logs -f <servicio>       # Ver logs de un servicio en tiempo real
docker compose build                    # Reconstruir imagenes
docker compose exec <servicio> bash     # Terminal dentro de un servicio
```

---

## Limpieza

```bash
docker system prune                     # Eliminar todo lo no utilizado (contenedores, redes, imagenes)
docker system prune -a                  # Igual pero incluye TODAS las imagenes sin usar
docker image prune                      # Eliminar imagenes sin tag
docker volume prune                     # Eliminar volumenes sin usar
docker container prune                  # Eliminar contenedores detenidos
```

---

## Flujo tipico

```
1. Escribir Dockerfile               # Definir como se construye tu app
2. docker build -t mi-app .          # Construir la imagen
3. docker run -d -p 8080:80 mi-app   # Ejecutar contenedor
4. docker logs mi-app                 # Verificar que funciona
5. (escribir docker-compose.yml)      # Definir servicios si hay mas de uno
6. docker compose up -d               # Levantar todo el entorno
7. docker compose down                # Apagar cuando termines
```
