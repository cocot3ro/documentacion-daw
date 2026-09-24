# Estructura del proyecto

## 1. Introducción

La aplicación se organizará siguiendo una arquitectura por capas, con el objetivo de separar las diferentes responsabilidades del sistema y facilitar su mantenimiento y evolución.

El proyecto estará dividido principalmente en:

* **Frontend:** interfaz de usuario.
* **Backend:** lógica de negocio y API.
* **Dominio:** entidades y reglas de negocio.
* **Persistencia:** acceso a la base de datos.
* **Seguridad:** autenticación y autorización.
* **Pruebas:** pruebas automatizadas del sistema.

---

# 2. Estructura de directorios

La estructura general del proyecto será la siguiente:

```text
gestion-tareas/
│
├── frontend/
│   ├── public/
│   │   └── favicon.ico
│   │
│   └── src/
│       ├── components/
│       ├── pages/
│       ├── layouts/
│       ├── services/
│       ├── hooks/
│       ├── models/
│       ├── utils/
│       ├── assets/
│       ├── App.jsx
│       └── main.jsx
│
├── backend/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/
│   │   │   │       └── gestiontareas/
│   │   │   │           ├── controller/
│   │   │   │           ├── service/
│   │   │   │           ├── repository/
│   │   │   │           ├── model/
│   │   │   │           ├── dto/
│   │   │   │           ├── security/
│   │   │   │           ├── exception/
│   │   │   │           └── config/
│   │   │   │
│   │   │   └── resources/
│   │   │       ├── application.yml
│   │   │       └── db/
│   │   │
│   │   └── test/
│   │       └── java/
│   │
│   └── build.gradle
│
├── database/
│   ├── migrations/
│   └── seed/
│
├── docs/
│   ├── requisitos.md
│   ├── diagrama-clases.md
│   ├── caja-blanca.md
│   └── estructura-proyecto.md
│
├── .gitignore
├── README.md
└── docker-compose.yml
```

---

# 3. Frontend

El frontend será responsable de proporcionar la interfaz gráfica con la que interactuarán los usuarios.

## 3.1. `public/`

Contendrá recursos estáticos que no necesitan ser procesados por el sistema de construcción.

```text
public/
└── favicon.ico
```

## 3.2. `components/`

Contendrá componentes reutilizables de la interfaz.

Ejemplos:

```text
components/
├── Button.jsx
├── TaskCard.jsx
├── ProjectCard.jsx
├── Navbar.jsx
├── Sidebar.jsx
└── NotificationList.jsx
```

Los componentes deberán intentar mantener una única responsabilidad.

## 3.3. `pages/`

Contendrá las diferentes páginas de la aplicación.

```text
pages/
├── Login.jsx
├── Register.jsx
├── Dashboard.jsx
├── Projects.jsx
├── ProjectDetails.jsx
├── TaskDetails.jsx
└── Users.jsx
```

## 3.4. `layouts/`

Contendrá las estructuras generales de las páginas.

```text
layouts/
├── MainLayout.jsx
└── AuthLayout.jsx
```

Por ejemplo, `MainLayout` podrá contener la barra de navegación, el menú lateral y el área principal.

## 3.5. `services/`

Contendrá el código encargado de comunicarse con el backend.

```text
services/
├── api.js
├── authService.js
├── projectService.js
├── taskService.js
└── userService.js
```

De esta forma, los componentes de la interfaz no necesitarán conocer directamente los detalles de las peticiones HTTP.

## 3.6. `hooks/`

Contendrá hooks reutilizables para encapsular lógica del frontend.

Ejemplos:

```text
hooks/
├── useAuth.js
├── useProjects.js
└── useTasks.js
```

## 3.7. `models/`

Contendrá las estructuras utilizadas para representar los datos recibidos del backend.

```text
models/
├── User.js
├── Project.js
├── Task.js
└── Notification.js
```

## 3.8. `utils/`

Contendrá funciones auxiliares.

Por ejemplo:

```text
utils/
├── dateUtils.js
├── validation.js
└── formatters.js
```

---

# 4. Backend

El backend será responsable de implementar la lógica de negocio y proporcionar una API para el frontend.

## 4.1. `controller/`

Los controladores recibirán las peticiones HTTP y devolverán las respuestas correspondientes.

```text
controller/
├── AuthController.java
├── UserController.java
├── ProjectController.java
├── TaskController.java
└── NotificationController.java
```

Por ejemplo:

```text
POST   /api/auth/login
POST   /api/auth/register

GET    /api/projects
POST   /api/projects
GET    /api/projects/{id}
PUT    /api/projects/{id}
DELETE /api/projects/{id}

GET    /api/tasks
POST   /api/tasks
PUT    /api/tasks/{id}
DELETE /api/tasks/{id}
```

Los controladores no deberán contener lógica de negocio compleja. Su función principal será recibir los datos, validarlos inicialmente y delegar el procesamiento en los servicios.

---

# 5. Capa de servicios

El directorio `service/` contendrá la lógica de negocio de la aplicación.

```text
service/
├── AuthService.java
├── UserService.java
├── ProjectService.java
├── TaskService.java
└── NotificationService.java
```

Por ejemplo, `TaskService` será responsable de:

* Crear tareas.
* Validar los datos.
* Comprobar los permisos.
* Comprobar que el usuario pertenece al proyecto.
* Asignar tareas.
* Modificar tareas.
* Eliminar tareas.

Esta separación evita que la lógica de negocio quede mezclada con el código encargado de gestionar las peticiones HTTP.

---

# 6. Modelo de dominio

El directorio `model/` contendrá las entidades principales de la aplicación.

```text
model/
├── Usuario.java
├── Proyecto.java
├── Tarea.java
├── Notificacion.java
├── Rol.java
├── EstadoProyecto.java
├── EstadoTarea.java
├── Prioridad.java
└── TipoNotificacion.java
```

Estas clases representan los objetos principales definidos en el diagrama de clases.

---

# 7. DTO

El directorio `dto/` contendrá los objetos utilizados para intercambiar información entre el frontend y el backend.

```text
dto/
├── LoginRequest.java
├── RegisterRequest.java
├── UserResponse.java
├── ProjectRequest.java
├── ProjectResponse.java
├── TaskRequest.java
└── TaskResponse.java
```

Los DTO permiten evitar que las entidades internas del sistema se expongan directamente mediante la API.

Por ejemplo, la entidad `Usuario` contiene información sensible como el hash de la contraseña, mientras que `UserResponse` solamente incluirá los datos que puedan enviarse al cliente.

---

# 8. Repositorios

El directorio `repository/` será responsable del acceso a la base de datos.

```text
repository/
├── UsuarioRepository.java
├── ProyectoRepository.java
├── TareaRepository.java
└── NotificacionRepository.java
```

Los repositorios proporcionarán operaciones como:

* Buscar registros.
* Insertar registros.
* Actualizar registros.
* Eliminar registros.
* Realizar consultas específicas.

La capa de servicios utilizará los repositorios sin tener que conocer los detalles de implementación de la base de datos.

---

# 9. Seguridad

El directorio `security/` contendrá los componentes relacionados con la autenticación y autorización.

```text
security/
├── AuthenticationService.java
├── JwtService.java
├── SecurityConfig.java
└── AuthenticationFilter.java
```

Sus responsabilidades serán:

* Autenticar usuarios.
* Generar tokens de sesión.
* Validar tokens.
* Controlar el acceso a los endpoints.
* Comprobar roles y permisos.

---

# 10. Gestión de excepciones

El directorio `exception/` centralizará las excepciones de la aplicación.

```text
exception/
├── UserNotFoundException.java
├── ProjectNotFoundException.java
├── TaskNotFoundException.java
├── UnauthorizedException.java
└── GlobalExceptionHandler.java
```

Esto permitirá devolver respuestas HTTP coherentes cuando se produzca un error.

Por ejemplo:

```text
GET /api/tasks/123

404 Not Found
```

cuando la tarea solicitada no existe.

---

# 11. Configuración

El directorio `config/` contendrá la configuración general del backend.

```text
config/
├── DatabaseConfig.java
├── CorsConfig.java
└── WebConfig.java
```

La configuración específica del entorno se almacenará en:

```text
resources/
└── application.yml
```

Los datos sensibles, como contraseñas de bases de datos o claves secretas, no deberán almacenarse directamente en el repositorio.

---

# 12. Base de datos

El directorio `database/` contendrá los scripts relacionados con la base de datos.

```text
database/
├── migrations/
│   ├── V001__create_users.sql
│   ├── V002__create_projects.sql
│   ├── V003__create_tasks.sql
│   └── V004__create_notifications.sql
│
└── seed/
    └── initial_data.sql
```

Las migraciones permitirán controlar los cambios realizados sobre el esquema de la base de datos.

---

# 13. Pruebas

Las pruebas del backend estarán ubicadas dentro de:

```text
backend/src/test/
```

Se organizarán de forma similar al código principal:

```text
test/
└── java/
    └── com/
        └── gestiontareas/
            ├── controller/
            ├── service/
            ├── repository/
            └── security/
```

Ejemplos:

```text
TaskServiceTest.java
ProjectServiceTest.java
AuthenticationServiceTest.java
TaskControllerTest.java
```

Las pruebas deberán cubrir tanto los casos correctos como los casos de error.

---

# 14. Documentación

La documentación del proyecto estará almacenada en `docs/`.

```text
docs/
├── requisitos.md
├── diagrama-clases.md
├── caja-blanca.md
└── estructura-proyecto.md
```

Cada documento tendrá una finalidad concreta:

| Documento                | Contenido                                     |
| ------------------------ | --------------------------------------------- |
| `requisitos.md`          | Requisitos funcionales y no funcionales.      |
| `diagrama-clases.md`     | Clases, atributos, métodos y relaciones.      |
| `caja-blanca.md`         | Análisis estructural y pruebas del código.    |
| `estructura-proyecto.md` | Organización de los componentes del proyecto. |

---

# 15. Ficheros de configuración

## `.gitignore`

Indicará los archivos que no deberán incluirse en el repositorio Git.

Por ejemplo:

```text
node_modules/
build/
dist/
.env
.idea/
*.log
```

## `README.md`

Contendrá la información básica necesaria para instalar, configurar y ejecutar el proyecto.

Deberá incluir:

* Descripción del proyecto.
* Requisitos.
* Instalación.
* Configuración.
* Ejecución.
* Ejecución de pruebas.

## `docker-compose.yml`

Permitirá ejecutar los diferentes servicios necesarios para el funcionamiento de la aplicación mediante Docker.

Una posible configuración sería:

```text
docker-compose.yml
        │
        ├── frontend
        ├── backend
        └── database
```

---

# 16. Arquitectura general

La comunicación entre los diferentes componentes seguirá el siguiente flujo:

```text
┌──────────────────────┐
│       Usuario        │
└──────────┬───────────┘
           │
           │ HTTP/HTTPS
           ▼
┌──────────────────────┐
│       Frontend       │
│                      │
│  Componentes / UI    │
│  Pages / Services    │
└──────────┬───────────┘
           │
           │ REST API
           ▼
┌──────────────────────┐
│       Backend        │
│                      │
│    Controllers       │
│         │            │
│         ▼            │
│     Services         │
│         │            │
│         ▼            │
│    Repositories      │
└──────────┬───────────┘
           │
           │ SQL
           ▼
┌──────────────────────┐
│      Base de datos   │
└──────────────────────┘
```

---

# 17. Principios de organización

La estructura del proyecto seguirá los siguientes principios:

### Separación de responsabilidades

Cada componente tendrá una responsabilidad concreta.

Por ejemplo, un controlador no deberá encargarse directamente de ejecutar consultas SQL.

### Bajo acoplamiento

Los diferentes módulos deberán depender lo menos posible de implementaciones concretas.

### Alta cohesión

Las clases que pertenezcan a una misma funcionalidad deberán mantenerse agrupadas.

### Reutilización

Los componentes y servicios comunes deberán poder reutilizarse en diferentes partes de la aplicación.

### Testabilidad

La separación por capas permitirá probar cada componente de manera independiente.

---

# 18. Flujo de una petición

Por ejemplo, para modificar una tarea:

```text
Usuario
   │
   │ PUT /api/tasks/15
   ▼
TaskController
   │
   │ modificarTarea(...)
   ▼
TaskService
   │
   ├── Comprobar usuario
   ├── Comprobar permisos
   ├── Validar datos
   └── Comprobar tarea
   │
   ▼
TaskRepository
   │
   │ UPDATE
   ▼
Base de datos
   │
   ▼
TaskRepository
   │
   ▼
TaskService
   │
   ▼
TaskController
   │
   │ HTTP 200 OK
   ▼
Frontend
   │
   ▼
Usuario
```

Esta estructura permite mantener separadas la interfaz, la lógica de negocio y el acceso a datos, facilitando el mantenimiento y las pruebas de la aplicación.

