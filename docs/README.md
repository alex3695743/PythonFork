CI/CD con Terraform, Docker y Jenkins

Este documento describe los pasos seguidos para configurar un entorno de Integración y Despliegue Continuo (CI/CD) utilizando Terraform, Docker y Jenkins.

1. Preparación del entorno

 - Se creó un archivo main.tf, outputs.tf, variables.tf utilizando Terraform para definir la infraestructura Docker.
 - Se especificó el proveedor kreuzwerker/docker.
 - Se definió una red Docker llamada ci_network para conectar los contenedores de Jenkins y Docker-in-Docker (DinD).

2. Creación de contenedores

Docker-in-Docker (DinD) : 

 - Se utilizó la imagen oficial docker:dind.
 - Se desplegó un contenedor con modo privilegiado, conectado a la red ci_network y con el puerto 2375 expuesto.

Jenkins : 

 - Se creó una imagen personalizada de Jenkins mediante un Dockerfile.
 - El contenedor de Jenkins se creó a través de Terraform, usando la imagen personalizada creada previamente.
 - Se conectó a la red ci_network.
 - Se montó el volumen del socket Docker (/var/run/docker.sock) para que Jenkins pueda ejecutar contenedores.

3. Levantamiento de la infraestructura

Inicialización y aplicación de Terraform:

terraform init
terraform apply

Esto desplegó automáticamente los contenedores de Jenkins y DinD.

4. Configuración de Jenkins

Acceso a Jenkins desde http://localhost:8080.

Configuración inicial del servidor Jenkins (usuario, plugins).

Creación de un pipeline desde la interfaz de Jenkins y usando la creación de Pipeline por SCM, configurando:

 - Repositorio Git: https://github.com/alex3695743/PythonFork
 - Jenkinsfile incluido en el repositorio para definir las etapas de build y test.
 - Cambiamos a la rama /*main

5. Ejecución del pipeline

 - Jenkins descargó el código desde GitHub.
 - Se ejecutó la etapa de compilación de scripts Python.
 - Se ejecutaron los tests con pytest, y los resultados se mostraron como reporte JUnit en la interfaz de Jenkins.

6. Gestión de la infraestructura

Para detener la infraestructura:

terraform destroy

Para levantarla nuevamente:

terraform apply



