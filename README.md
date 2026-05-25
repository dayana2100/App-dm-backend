# BACKEND ACCESORIOS DM MOVIL

# Inventory Service - Accesorios DM

Servicio de gestión de inventario, catálogo y productos para la plataforma de comercio electrónico Accesorios DM.

## Requisitos Previos

* Docker Desktop
* Docker Compose
* Java 17 (para ejecutar localmente)
* Maven (opcional, se incluye wrapper)
* Git

## Estructura del Proyecto

```text
accesorios-dm-inventory-service/
├── src/
│   ├── main/
│   │   ├── java/com/accesoriosdm/inventory/
│   │   │   ├── controller/        # Endpoints REST
│   │   │   ├── service/           # Lógica de negocio
│   │   │   ├── repository/        # Acceso a datos
│   │   │   ├── entity/            # Entidades JPA
│   │   │   ├── dto/               # Data Transfer Objects
│   │   │   └── exception/         # Manejo de excepciones
│   │   └── resources/
│   │       └── application.yml    # Configuración
│   └── test/                      # Pruebas unitarias
├── Dockerfile
├── docker-compose.yml
├── pom.xml
└── README.md
```

# IMPORTANTE

## Dependencia: Base de Datos

Este servicio requiere la base de datos `App-dm-database`. Debes tenerla clonada y corriendo.

### Clonar la base de datos

```bash
git clone https://github.com/dayana2100/App-dm-database.git
cd App-dm-database
```

### Levantar la base de datos (ambiente develop)

```bash
git checkout develop
docker-compose -f docker-compose.yml up -d
```

### Verificar que PostgreSQL está corriendo

```bash
docker ps | findstr "postgres"
```

Debes ver `accesorios-dm-postgres-dev` corriendo en el puerto `5434`.

# Configuración del Servicio

## Variables de entorno

| Variable                   | Valor                                                              | Descripción         |
| -------------------------- | ------------------------------------------------------------------ | ------------------- |
| SPRING_DATASOURCE_URL      | jdbc:postgresql://accesorios-dm-postgres-dev:5432/accesorios_dm_db | Conexión a BD       |
| SPRING_DATASOURCE_USERNAME | admin                                                              | Usuario de BD       |
| SPRING_DATASOURCE_PASSWORD | admin123                                                           | Contraseña de BD    |
| SERVER_PORT                | 8082                                                               | Puerto del servicio |

## Archivo de configuración local (opcional)

Si deseas ejecutar sin Docker, crea:

```text
src/main/resources/application-local.yml
```

Contenido:

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5434/accesorios_dm_db
    username: admin
    password: admin123
```

# Cómo Levantar el Microservicio

## Opción 1: Con Docker (Recomendado)

```bash
# 1. Asegurar que la base de datos está corriendo
cd ../App-dm-database
docker-compose -f docker-compose.yml up -d

# 2. Volver al microservicio
cd ../accesorios-dm-inventory-service

# 3. Construir la imagen
docker-compose build

# 4. Levantar el contenedor
docker-compose up -d

# 5. Ver logs
docker-compose logs -f
```

## Opción 2: Ejecución local (sin Docker)

```bash
# 1. Asegurar que la base de datos está corriendo
cd ../App-dm-database
docker-compose -f docker-compose.yml up -d

# 2. Volver al microservicio
cd ../accesorios-dm-inventory-service

# 3. Compilar
./mvnw clean package -DskipTests

# 4. Ejecutar
java -jar target/inventory-service-0.0.1-SNAPSHOT.jar
```

# Verificar que Funciona

## Health check

```bash
curl http://localhost:8082/api/v1/health
```

Respuesta esperada:

```json
{"service":"inventory-service","version":"1.0.0","status":"UP"}
```

# Endpoints Disponibles

## Categorías

| Método | Endpoint                | Descripción          |
| ------ | ----------------------- | -------------------- |
| GET    | /api/v1/categorias      | Listar categorías    |
| POST   | /api/v1/categorias      | Crear categoría      |
| PUT    | /api/v1/categorias/{id} | Actualizar categoría |
| DELETE | /api/v1/categorias/{id} | Eliminar categoría   |

## Materiales

| Método | Endpoint                | Descripción         |
| ------ | ----------------------- | ------------------- |
| GET    | /api/v1/materiales      | Listar materiales   |
| POST   | /api/v1/materiales      | Crear material      |
| PUT    | /api/v1/materiales/{id} | Actualizar material |
| DELETE | /api/v1/materiales/{id} | Eliminar material   |

## Productos

| Método | Endpoint               | Descripción         |
| ------ | ---------------------- | ------------------- |
| GET    | /api/v1/productos      | Listar productos    |
| GET    | /api/v1/productos/{id} | Obtener producto    |
| POST   | /api/v1/productos      | Crear producto      |
| PUT    | /api/v1/productos/{id} | Actualizar producto |
| DELETE | /api/v1/productos/{id} | Eliminar producto   |

## Promociones

| Método | Endpoint                 | Descripción          |
| ------ | ------------------------ | -------------------- |
| GET    | /api/v1/promociones      | Listar promociones   |
| POST   | /api/v1/promociones      | Crear promoción      |
| PUT    | /api/v1/promociones/{id} | Actualizar promoción |
| DELETE | /api/v1/promociones/{id} | Eliminar promoción   |

# Comandos Útiles

```bash
# Ver logs
docker-compose logs -f

# Detener servicio
docker-compose down

# Reconstruir imagen
docker-compose build --no-cache
```

# Configuración de Puertos

| Ambiente | BD (localhost) | BD (red Docker) | Microservicio |
| -------- | -------------- | --------------- | ------------- |
| develop  | 5434           | 5432            | 8082          |
| qa       | 5433           | 5432            | 8081          |
| main     | 5432           | 5432            | 8080          |

# Flujo de Trabajo con Git

```bash
# Crear rama para nueva funcionalidad
git checkout develop
git pull origin develop
git checkout -b HU-XX-develop-inventory-nombre

# Hacer cambios...
git add .
git commit -m "feat: descripcion"
git push origin HU-XX-develop-inventory-nombre
```

# Historial de HUs Implementadas

| HU                                                  | Descripción                                             |
| --------------------------------------------------- | ------------------------------------------------------- |
| HU-01-develop-inventory-inicializar-microservicio   | Inicialización base del microservicio Inventory Service |
| HU-02-develop-inventory-configuracion-global        | Configuración global y recursos del microservicio       |
| HU-03-develop-inventory-entidades-repositorios      | Implementación de entidades y repositorios              |
| HU-04-develop-inventory-dtos-inventario             | Implementación de DTOs del sistema                      |
| HU-05-develop-inventory-servicios-negocio           | Desarrollo de servicios de negocio                      |
| HU-06-develop-inventory-controladores-rest          | Implementación de controladores REST                    |
| HU-07-develop-inventory-manejo-excepciones          | Manejo global de excepciones                            |
| HU-08-develop-inventory-almacenamiento-archivos     | Servicio de almacenamiento de archivos                  |
| HU-09-develop-inventory-configuracion-ambientes     | Configuración de propiedades y ambientes                |
| HU-10-develop-inventory-configuracion-docker        | Configuración Docker y despliegue                       |
| HU-11-develop-inventory-documentacion-configuracion | Documentación y configuración general                   |
| HU-12-develop-inventory-test                        | Implementación de pruebas iniciales                     |

# Tecnologías Utilizadas

| Tecnología      | Versión | Propósito                       |
| --------------- | ------- | ------------------------------- |
| Java            | 17      | Lenguaje principal              |
| Spring Boot     | 3.5.11  | Framework                       |
| Spring Data JPA | -       | Acceso a datos                  |
| PostgreSQL      | 16      | Base de datos                   |
| Maven           | -       | Gestión de dependencias         |
| Docker          | -       | Contenerización                 |
| Lombok          | -       | Reducción de código boilerplate |


# Payment Service - Accesorios DM

Servicio de gestión de carritos, pedidos y pagos para la plataforma de comercio electrónico Accesorios DM.

## Requisitos Previos

* Docker Desktop
* Docker Compose
* Node.js 18+
* PostgreSQL

## Estructura del Proyecto

```text
accesorios-dm-payment-service/
├── src/
│   ├── controllers/
│   ├── routes/
│   ├── prisma/
│   └── index.js
├── prisma/
│   └── schema.prisma
├── Dockerfile
├── docker-compose.yml
├── package.json
├── .env.example
└── README.md
```

# Dependencia: Base de Datos

Este servicio requiere la base de datos `App-dm-database`.

## Clonar la base de datos

```bash
git clone https://github.com/dayana2100/App-dm-database.git
```

# Ambientes y Puertos

| Ambiente | Puerto | BD (localhost) | BD (red Docker) |
| -------- | ------ | -------------- | --------------- |
| develop  | 9002   | 5434           | 5432            |
| qa       | 9001   | 5433           | 5432            |
| main     | 9000   | 5432           | 5432            |

# Configuración del Microservicio

## Variables de entorno

| Variable     | Valor                                    | Descripción         |
| ------------ | ---------------------------------------- | ------------------- |
| DATABASE_URL | postgresql://admin:admin123@host:port/db | Conexión a BD       |
| PORT         | 9002/9001/9000                           | Puerto del servicio |

## Archivo .env

```bash
cp .env.example .env
```

# Cómo Levantar el Servicio

## Docker

```bash
git checkout develop
docker-compose up -d
```

## Ejecución local

```bash
npm install
npm run dev
```

# Endpoints Disponibles

## Carrito

| Método | Endpoint                  | Descripción     |
| ------ | ------------------------- | --------------- |
| POST   | /api/v1/carrito           | Crear carrito   |
| GET    | /api/v1/carrito/:id       | Obtener carrito |
| POST   | /api/v1/carrito/:id/items | Agregar item    |
| DELETE | /api/v1/carrito/:id       | Vaciar carrito  |

## Pedidos

| Método | Endpoint                               | Descripción          |
| ------ | -------------------------------------- | -------------------- |
| POST   | /api/v1/pedidos/crear                  | Crear pedido         |
| GET    | /api/v1/pedidos/:id                    | Obtener pedido       |
| GET    | /api/v1/pedidos/cliente/correo/:correo | Historial por correo |

## Administración

| Método | Endpoint                         | Descripción    |
| ------ | -------------------------------- | -------------- |
| GET    | /api/v1/admin/pedidos            | Listar pedidos |
| PUT    | /api/v1/admin/pedidos/:id/estado | Cambiar estado |
| GET    | /api/v1/admin/stats              | Estadísticas   |

# Comandos Útiles

```bash
# Logs
docker-compose logs -f

# Reconstruir imagen
docker-compose build --no-cache

# Regenerar Prisma
npx prisma generate
```

# Flujo de Trabajo con Git

```bash
# Crear rama
git checkout develop
git pull origin develop
git checkout -b HU-XX-develop-payment-nombre

# Hacer cambios...
git add .
git commit -m "feat: descripcion"
git push origin HU-XX-develop-payment-nombre
```

# Historial de HUs Implementadas

| HU                                               | Descripción                                        |
| ------------------------------------------------ | -------------------------------------------------- |
| HU-13-develop-payment-inicializar-microservicio  | Inicialización del microservicio Payment Service   |
| HU-14-develop-payment-configurar-prisma          | Configuración de Prisma y conexión a base de datos |
| HU-15-develop-payment-controladores-pagos        | Desarrollo de controladores del microservicio      |
| HU-16-develop-payment-rutas-rest                 | Implementación de rutas REST                       |
| HU-17-develop-payment-configuracion-docker       | Configuración Docker y despliegue                  |
| HU-18-develop-payment-configuracion-general      | Configuración general y variables de entorno       |
| HU-19-develop-payment-documentacion-estructura   | Documentación y estructura del microservicio       |
| HU-20-develop-payment-configuracion-dockerignore | Configuración de exclusión Docker                  |

# Tecnologías Utilizadas

| Tecnología | Versión | Propósito       |
| ---------- | ------- | --------------- |
| Node.js    | 18      | Runtime         |
| Express    | 4.18.2  | Framework web   |
| Prisma     | 5.22.0  | ORM             |
| PostgreSQL | 16      | Base de datos   |
| Docker     | -       | Contenerización |

## Licencia

Proyecto interno de Accesorios DM.

