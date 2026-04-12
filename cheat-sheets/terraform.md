# TERRAFORM CHEAT SHEET

**Bootcamp DevOps 2026** | Autor: Carlos Palanca
Referencia rapida para estudiantes STEM

---

## Workflow basico

```bash
terraform init                        # Inicializar proyecto y descargar providers
terraform plan                        # Previsualizar cambios sin aplicarlos
terraform apply                       # Aplicar cambios (crea/modifica infraestructura)
terraform destroy                     # Destruir toda la infraestructura gestionada
```

---

## Estructura de archivos

```
mi-proyecto/
  main.tf              # Recursos principales (provider, resources)
  variables.tf         # Declaracion de variables de entrada
  outputs.tf           # Valores de salida tras el apply
  terraform.tfvars     # Valores concretos para las variables
  .terraform/          # Directorio interno (no se sube a Git)
  terraform.tfstate    # Estado actual de la infraestructura (no editar manualmente)
```

---

## HCL basico

```hcl
# Provider: define que plataforma usamos
provider "docker" {}

# Resource: crea un recurso de infraestructura
resource "docker_container" "web" {
  name  = "mi-web"
  image = "nginx:latest"
}

# Variable: parametro de entrada
variable "nombre" {
  type        = string
  default     = "mi-app"
  description = "Nombre de la aplicacion"
}

# Output: valor de salida
output "ip" {
  value = docker_container.web.network_data[0].ip_address
}

# Data: leer un recurso existente (sin crearlo)
data "docker_image" "nginx" {
  name = "nginx:latest"
}

# Locals: variables internas calculadas
locals {
  entorno = "desarrollo"
  prefijo = "dev-${var.nombre}"
}
```

---

## Variables

Tipos basicos:

```hcl
variable "nombre"   { type = string }    # Texto
variable "puerto"   { type = number }    # Numero
variable "activo"   { type = bool }      # true o false
variable "puertos"  { type = list(number) }   # Lista: [80, 443]
variable "tags"     { type = map(string) }    # Mapa: { env = "dev" }
```

Uso en recursos:

```hcl
name = var.nombre
port = var.puerto
```

---

## State

```bash
terraform state list                  # Listar todos los recursos en el state
terraform state show <recurso>        # Ver detalles de un recurso especifico
terraform state rm <recurso>          # Quitar un recurso del state (sin destruirlo)
terraform show                        # Mostrar el state completo
```

---

## Providers utiles

| Provider   | Para que sirve                        | Ejemplo de recurso           |
|------------|---------------------------------------|------------------------------|
| `local`    | Crear archivos locales                | `local_file`                 |
| `docker`   | Gestionar contenedores Docker         | `docker_container`           |
| `aws`      | Amazon Web Services                   | `aws_instance`               |
| `azurerm`  | Microsoft Azure                       | `azurerm_resource_group`     |
| `google`   | Google Cloud Platform                 | `google_compute_instance`    |

---

## Flags utiles

```bash
terraform apply -auto-approve         # Aplicar sin pedir confirmacion
terraform plan -var="nombre=prod"     # Pasar variable por linea de comandos
terraform apply -var-file=prod.tfvars # Usar archivo de variables especifico
terraform apply -target=docker_container.web   # Aplicar solo un recurso concreto
terraform fmt                         # Formatear archivos .tf automaticamente
terraform validate                    # Validar sintaxis sin conectar a providers
```

---

## Flujo tipico

1. Crear `main.tf` con provider y recursos
2. Crear `variables.tf` y `terraform.tfvars` si se necesitan variables
3. `terraform init` -- descargar providers y preparar el proyecto
4. `terraform plan` -- revisar que cambios se van a hacer
5. Leer el plan con atencion (recursos a crear, modificar o destruir)
6. `terraform apply` -- aplicar los cambios
7. Verificar que la infraestructura se creo correctamente
8. `terraform destroy` -- destruir cuando ya no se necesite
