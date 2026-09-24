# Casos de uso

## Introducción

Los casos de uso describen las principales interacciones entre los usuarios y la aplicación **TaskFlow**. Permiten definir de forma clara qué acciones puede realizar cada tipo de usuario y cómo responde el sistema.

## Actores

* **Usuario**: puede gestionar sus proyectos y tareas, consultar notificaciones y modificar su perfil.
* **Administrador**: además de las funciones de un usuario, puede gestionar usuarios y supervisar los proyectos de la aplicación.

## Casos de uso principales

### CU-01: Iniciar sesión

**Actor:** Usuario, Administrador

**Descripción:** Permite acceder a la aplicación mediante las credenciales registradas.

**Flujo principal:**

1. El usuario introduce su correo electrónico y contraseña.
2. El sistema valida las credenciales.
3. El sistema comprueba que la cuenta esté activa.
4. El sistema inicia la sesión.
5. El usuario accede a la aplicación.

**Resultado:** El usuario queda autenticado.

---

### CU-02: Gestionar proyectos

**Actor:** Usuario

**Descripción:** Permite crear, consultar, modificar y eliminar proyectos.

**Flujo principal:**

1. El usuario accede a la sección de proyectos.
2. El sistema muestra sus proyectos.
3. El usuario selecciona una acción.
4. El sistema realiza la operación solicitada.
5. El sistema actualiza la información mostrada.

**Resultado:** El proyecto queda actualizado según la operación realizada.

---

### CU-03: Gestionar tareas

**Actor:** Usuario

**Descripción:** Permite crear y gestionar las tareas asociadas a un proyecto.

**Flujo principal:**

1. El usuario selecciona un proyecto.
2. El usuario crea o selecciona una tarea.
3. Introduce o modifica sus datos.
4. El sistema valida la información.
5. El sistema guarda los cambios.

**Resultado:** La tarea queda registrada o actualizada.

---

### CU-04: Consultar notificaciones

**Actor:** Usuario

**Descripción:** Permite consultar las notificaciones generadas por la aplicación.

**Flujo principal:**

1. El usuario accede a la sección de notificaciones.
2. El sistema muestra las notificaciones recibidas.
3. El usuario puede consultar una notificación.
4. El sistema marca la notificación como leída.

**Resultado:** El usuario puede consultar el estado de sus notificaciones.

---

### CU-05: Administrar usuarios

**Actor:** Administrador

**Descripción:** Permite al administrador gestionar las cuentas de los usuarios.

**Flujo principal:**

1. El administrador accede al panel de administración.
2. El sistema muestra la lista de usuarios.
3. El administrador selecciona un usuario.
4. Puede modificar sus datos, cambiar su estado o eliminar la cuenta.
5. El sistema guarda los cambios.

**Resultado:** La información del usuario queda actualizada.

## Resumen

| Código | Caso de uso              | Actor principal         |
| ------ | ------------------------ | ----------------------- |
| CU-01  | Iniciar sesión           | Usuario / Administrador |
| CU-02  | Gestionar proyectos      | Usuario                 |
| CU-03  | Gestionar tareas         | Usuario                 |
| CU-04  | Consultar notificaciones | Usuario                 |
| CU-05  | Administrar usuarios     | Administrador           |
# Casos de uso

## Introducción

Los casos de uso describen las principales interacciones entre los usuarios y la aplicación **TaskFlow**. Permiten definir de forma clara qué acciones puede realizar cada tipo de usuario y cómo responde el sistema.

## Actores

* **Usuario**: puede gestionar sus proyectos y tareas, consultar notificaciones y modificar su perfil.
* **Administrador**: además de las funciones de un usuario, puede gestionar usuarios y supervisar los proyectos de la aplicación.

## Casos de uso principales

### CU-01: Iniciar sesión

**Actor:** Usuario, Administrador

**Descripción:** Permite acceder a la aplicación mediante las credenciales registradas.

**Flujo principal:**

1. El usuario introduce su correo electrónico y contraseña.
2. El sistema valida las credenciales.
3. El sistema comprueba que la cuenta esté activa.
4. El sistema inicia la sesión.
5. El usuario accede a la aplicación.

**Resultado:** El usuario queda autenticado.

---

### CU-02: Gestionar proyectos

**Actor:** Usuario

**Descripción:** Permite crear, consultar, modificar y eliminar proyectos.

**Flujo principal:**

1. El usuario accede a la sección de proyectos.
2. El sistema muestra sus proyectos.
3. El usuario selecciona una acción.
4. El sistema realiza la operación solicitada.
5. El sistema actualiza la información mostrada.

**Resultado:** El proyecto queda actualizado según la operación realizada.

---

### CU-03: Gestionar tareas

**Actor:** Usuario

**Descripción:** Permite crear y gestionar las tareas asociadas a un proyecto.

**Flujo principal:**

1. El usuario selecciona un proyecto.
2. El usuario crea o selecciona una tarea.
3. Introduce o modifica sus datos.
4. El sistema valida la información.
5. El sistema guarda los cambios.

**Resultado:** La tarea queda registrada o actualizada.

---

### CU-04: Consultar notificaciones

**Actor:** Usuario

**Descripción:** Permite consultar las notificaciones generadas por la aplicación.

**Flujo principal:**

1. El usuario accede a la sección de notificaciones.
2. El sistema muestra las notificaciones recibidas.
3. El usuario puede consultar una notificación.
4. El sistema marca la notificación como leída.

**Resultado:** El usuario puede consultar el estado de sus notificaciones.

---

### CU-05: Administrar usuarios

**Actor:** Administrador

**Descripción:** Permite al administrador gestionar las cuentas de los usuarios.

**Flujo principal:**

1. El administrador accede al panel de administración.
2. El sistema muestra la lista de usuarios.
3. El administrador selecciona un usuario.
4. Puede modificar sus datos, cambiar su estado o eliminar la cuenta.
5. El sistema guarda los cambios.

**Resultado:** La información del usuario queda actualizada.

## Resumen

| Código | Caso de uso              | Actor principal         |
| ------ | ------------------------ | ----------------------- |
| CU-01  | Iniciar sesión           | Usuario / Administrador |
| CU-02  | Gestionar proyectos      | Usuario                 |
| CU-03  | Gestionar tareas         | Usuario                 |
| CU-04  | Consultar notificaciones | Usuario                 |
| CU-05  | Administrar usuarios     | Administrador           |

