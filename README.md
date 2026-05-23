# 📦 Microservicio de Gestión de Ventas (InnovaTech Chile)

Este módulo forma parte del backend distribuido de **InnovaTech Chile**, encargado de procesar y administrar el ciclo de vida de las transacciones comerciales. Diseñado bajo una arquitectura de microservicios apátrida (*stateless*), opera de forma independiente y se integra con la base de datos centralizada mediante contenedores Docker para su despliegue en AWS EC2.

## 🚀 Ficha Técnica y Tecnologías
* **Framework Principal:** Spring Boot 3.4.4 (Spring Web, Validation)
* **Capa de Datos:** Spring Data JPA + Hibernate ORM
* **Motor Base de Datos:** MariaDB / MySQL 8.0+
* **Lenguaje:** Java 17 (OpenJDK)
* **Gestor de Dependencias:** Maven 3+
* **Documentación Viva:** OpenAPI 3 / Springdoc Swagger UI v2.7.0
* **Utilidades Extra:** Lombok (Reducción de código boilerplate), H2 Database (Para entornos de testing)

---

## 🛠️ Requisitos Previos (Local)
Para ejecutar este microservicio en un entorno de desarrollo local, requieres:
1.  **Java Development Kit (JDK) 17** configurado en tu sistema (`JAVA_HOME`).
2.  **Apache Maven** instalado.
3.  Una instancia activa de **MySQL o MariaDB** (Local o en contenedor).

---

## ⚙️ Variables de Entorno Requeridas
La conexión a la base de datos se gestiona dinámicamente mediante variables de entorno para garantizar la seguridad en el despliegue. Debes configurar las siguientes variables:

| Variable | Descripción | Ejemplo Local |
| :--- | :--- | :--- |
| `DB_ENDPOINT` | Dirección IP o Host del motor de Base de Datos | `localhost` |
| `DB_PORT` | Puerto de escucha del motor SQL | `3306` |
| `DB_NAME` | Nombre de la base de datos del negocio | `innovatech_db` |
| `DB_USERNAME` | Usuario con privilegios | `root` |
| `DB_PASSWORD` | Contraseña segura de acceso | `tu_password_seguro` |

---

## ⚡ Instrucciones de Ejecución

### Opción A: Despliegue Local Tradicional (Maven)
1. Navega a la raíz del directorio del microservicio de Ventas.
2. Inyecta las variables de entorno necesarias y compila el proyecto:
   ```bash
   mvn clean package -DskipTests
Ejecuta el archivo .jar generado:

Bash
java -jar target/Springboot-API-REST-0.0.1-SNAPSHOT.jar
Opción B: Despliegue Contenerizado (Docker)
Para pruebas de paridad con producción o integración en docker-compose:

Bash
docker build -t innovatech-backend-ventas:latest .
📑 Documentación Interactiva de la API (Swagger)
El microservicio expone automáticamente la interfaz de Swagger UI para facilitar las pruebas de los endpoints y visualizar el contrato de la API. Con el servicio activo en el puerto 8092, accede a:

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

Cuerpo de la Petición: Mismo formato JSON que en el método POST.

Respuesta Exitosa: 200 OK (Retorna la transacción actualizada).

5. Eliminar una venta por ID
   Método: DELETE

Ruta: /api/v1/ventas/{idVenta}

Respuesta Exitosa: 204 No Content (Confirmación de eliminación sin cuerpo de respuesta).