# 📦 Microservicio de Gestión de Ventas (Grupo Cordillera / InnovaTech Chile)

Este módulo forma parte del backend distribuido de **InnovaTech Chile**, encargado de procesar y administrar el ciclo de vida de las transacciones comerciales. 

Diseñado bajo una arquitectura de microservicios apátrida (*stateless*), el servicio ha sido evolucionado para operar de forma independiente y nativa en la nube, desplegándose dentro de contenedores orquestados en **Amazon EKS (Kubernetes)** y automatizado mediante integración y despliegue continuo (CI/CD).

---

## 🚀 Ficha Técnica y Tecnologías

* **Framework Principal:** Spring Boot 3.4.4 (Spring Web, Validation)
* **Capa de Datos:** Spring Data JPA + Hibernate ORM
* **Motor Base de Datos Cloud:** Amazon RDS (MySQL/MariaDB)
* **Lenguaje:** Java 17 (OpenJDK)
* **Gestor de Dependencias:** Maven 3+
* **Orquestación y Escalabilidad:** Amazon EKS & Horizontal Pod Autoscaler (HPA)
* **Registro de Contenedores:** Amazon ECR
* **CI/CD:** GitHub Actions
* **Documentación Viva:** OpenAPI 3 / Springdoc Swagger UI v2.7.0
* **Utilidades Extra:** Lombok (Reducción de código boilerplate)

---

## ☁️ Novedades Arquitectónicas (Evaluación 3)

Para esta fase del proyecto, el microservicio ha sido migrado de una instancia EC2 monolítica a una arquitectura Cloud-Native de nivel empresarial:

* **Orquestación con Amazon EKS:** El servicio se ejecuta como un *Deployment* replicable dentro de un clúster de Kubernetes, garantizando tolerancia a fallos.
* **Autoescalado Dinámico (HPA):** Se integró el *Horizontal Pod Autoscaler*, permitiendo que el sistema inicie nuevos pods automáticamente si el consumo de CPU aumenta debido a un pico de ventas.
* **Pipeline CI/CD Directo:** A través de GitHub Actions, cada *push* a la rama principal compila el proyecto, construye la imagen Docker, la almacena en **Amazon ECR** y actualiza el clúster sin intervención manual.
* **Seguridad y Secrets:** Las credenciales de conexión a RDS están protegidas mediante **Kubernetes Secrets** y se inyectan en tiempo de ejecución, eliminando vulnerabilidades en el código fuente.
* **Aislamiento de Red:** Este backend opera de forma privada en el **puerto 8092** dentro del clúster. Todo el tráfico público es recibido por el Load Balancer y enrutado internamente por el servidor Nginx del Frontend (Reverse Proxy).

---

## ⚙️ Variables de Entorno y Seguridad

La conexión a la base de datos se gestiona dinámicamente. En el entorno de producción (AWS), estas variables son aprovisionadas por el clúster. Para desarrollo local, debes configurarlas manualmente:

| Variable | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `DB_ENDPOINT` | Dirección IP o Host del motor de Base de Datos (URL de RDS) | `innovatech-db.cxxx.us-east-1.rds.amazonaws.com` |
| `DB_PORT` | Puerto de escucha del motor SQL | `3306` |
| `DB_NAME` | Nombre de la base de datos del negocio | `innovatech_db` |
| `DB_USERNAME` | Usuario con privilegios | `admin` |
| `DB_PASSWORD` | Contraseña segura de acceso | `********` |

---

## ⚡ Instrucciones de Ejecución (Local)

### Opción A: Despliegue Local Tradicional (Maven)
1. Navega a la raíz del directorio del microservicio de Ventas.
2. Inyecta las variables de entorno necesarias y compila el proyecto:
   ```bash
   mvn clean package -DskipTests
Ejecuta el archivo .jar generado:

Bash
java -jar target/Springboot-API-REST-0.0.1-SNAPSHOT.jar
Opción B: Pruebas con Docker
Para construir la imagen simulando el proceso del pipeline antes de subirla a ECR:

Bash
docker build -t innovatech-backend-ventas:latest .
docker run -p 8092:8092 --env-file .env innovatech-backend-ventas:latest
📑 Documentación Interactiva de la API (Swagger)
El microservicio expone automáticamente la interfaz de Swagger UI para facilitar las pruebas de los endpoints. Con el servicio activo en el puerto 8092 (local o mediante kubectl port-forward), accede a:

URL de Swagger UI: http://localhost:8092/swagger-ui.html

🛣️ Catálogo de Endpoints (API Reference)
Todas las rutas parten del endpoint base: /api/v1/ventas

1. Obtener todas las ventas
Método: GET

Ruta: /api/v1/ventas

Respuesta Exitosa: 200 OK (Retorna un arreglo JSON con el historial de ventas).

2. Obtener una venta por ID
Método: GET

Ruta: /api/v1/ventas/{idVenta}

Parámetro: idVenta (Long)

Respuestas: 200 OK / 404 Not Found

3. Crear una nueva venta (Payload validado)
Método: POST

Ruta: /api/v1/ventas

Cabecera Obligatoria: Content-Type: application/json

Cuerpo de la Petición (Payload):

JSON
{
  "direccionCompra": "Av. Providencia 1234, Oficina 501",
  "valorCompra": 250000,
  "fechaCompra": "2026-05-23",
  "despachoGenerado": false
}
Respuesta Exitosa: 201 Created (Retorna la entidad guardada con su idVenta autogenerado).

4. Actualizar una venta existente
Método: PUT

Ruta: /api/v1/ventas/{idVenta}

Cabecera Obligatoria: Content-Type: application/json

Respuesta Exitosa: 200 OK (Retorna la transacción actualizada).

5. Eliminar una venta por ID
Método: DELETE

Ruta: /api/v1/ventas/{idVenta}

Respuesta Exitosa: 204 No Content (Confirmación de eliminación sin cuerpo de respuesta).
