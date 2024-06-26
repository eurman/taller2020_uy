# Obligatorio de ISC - Primer Semestre 2024

## Repositorio del obligatorio  

### Autores:
#####     - Lucio Lastra    - 121447
#####     - Etzequiel Urman - 168879
#
### Fecha de entrega: 26/06/24  
#
## Layout

Hay tres carpetas:

1. **codigo-terraform** - contiene distintas versiones del código Terraform y la *final*.
2. **diagrama-arquitectura** - contiene diagramas en distintas versiones y el *final*.
3. **documentacion** - contiene las diapositivas de la defensa.


## Ejecución del código:


```
// Clonar el repositorio (es privado):
git clone https://github.com/luciolastra/isc-obligatorio.git

// Ir a la carpeta del repositorio de la solucion final:
cd isc-obligatorio/codigo-terraform/final

// Si es la primera vez que ejecutamos Terraform:
terraform init

// Para validar el código:
terraform validate

// Para construir la solución:
terraform apply -var-file=11-Valores-Variables.tfvars

/* 
   Acto seguido va a solicitar el valor de la variable
   del password de la base de datos y se ve en claro.
   
   Debemos escribir una contraseña de al menos 8 caracteres
   con números, letras y símbolos.
*/


// Para destruir la solución
terraform destroy -var-file=11-Valores-Variables.tfvars

```

# Diagrama de solución final

![Imagen solucion final](https://github.com/luciolastra/isc-obligatorio/blob/main/diagrama-arquitectura/Final/arquitectura-diagrama-final.png)

Tenemos dos servidores web que se conectan al RDS y al EFS Principal, su función es alojar la pagina web y se conectan al EFS principal a modo ilustrativo.

Estos servidores están bajo un load balancer y bajo un autoscaling web que tiene una capacidad de mínimo 2 instancias, deseable 3 y máximo 5.

Luego, tenemos una instancia de backup cuya función es alojar datos dentro de un EFS backup que hace de espejo del EFS principal. Todo esto bajo un autoscaling que tiene una capacidad de mínimo 1, deseable 1 y máximo 2.

Tambien tenemos un RDS cuya función es alojar la base de datos (MySQL) de la pagina web (backend de los servidores web).

Contamos con un VPC principal donde se hace una división en 4 subredes, dos publicas y dos privadas, todas las subredes se alojan en dos availability zone. Para la salida a internet utilizamos un internet Gateway.
