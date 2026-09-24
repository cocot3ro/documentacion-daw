# Análisis de requisitos

## 1. Introducción

### 1.1. Propósito

El objetivo de este documento es definir los requisitos funcionales y no funcionales de una aplicación web de gestión de tareas y proyectos.

La aplicación permitirá a los usuarios crear y organizar proyectos, gestionar tareas, establecer prioridades y fechas límite, y consultar el estado de los trabajos pendientes.

Este documento servirá como referencia para el diseño, desarrollo y pruebas de la aplicación.

### 1.2. Alcance

La aplicación permitirá:

* Crear y gestionar proyectos.
* Crear, modificar y eliminar tareas.
* Asignar tareas a usuarios.
* Establecer prioridades y fechas límite.
* Cambiar el estado de las tareas.
* Consultar las tareas de un proyecto.
* Filtrar y ordenar tareas.
* Gestionar usuarios y sus permisos.
* Recibir notificaciones relacionadas con las tareas.

Quedan fuera del alcance de esta versión:

* Integración con herramientas externas de gestión de proyectos.
* Aplicaciones móviles nativas.
* Videollamadas y comunicación por voz.
* Gestión de facturación o pagos.

### 1.3. Definiciones

| Término       | Definición                                                     |
| ------------- | -------------------------------------------------------------- |
| Usuario       | Persona que utiliza la aplicación.                             |
| Administrador | Usuario con permisos para gestionar usuarios y proyectos.      |
| Proyecto      | Conjunto de tareas relacionadas con un mismo objetivo.         |
| Tarea         | Elemento de trabajo que debe realizarse dentro de un proyecto. |
| Prioridad     | Nivel de importancia asignado a una tarea.                     |
| Estado        | Situación actual de una tarea.                                 |

---

# 2. Actores del sistema

La aplicación tendrá los siguientes tipos de usuario:

## 2.1. Usuario

Puede:

* Iniciar y cerrar sesión.
* Consultar los proyectos a los que pertenece.
* Consultar las tareas de sus proyectos.
* Crear tareas.
* Modificar las tareas que tenga permiso para modificar.
* Cambiar el estado de las tareas asignadas.
* Recibir notificaciones.

## 2.2. Administrador

Además de las funciones de un usuario normal, puede:

* Crear y eliminar usuarios.
* Modificar los datos de los usuarios.
* Crear y eliminar proyectos.
* Añadir o eliminar usuarios de proyectos.
* Consultar todas las tareas del sistema.

---

# 3. Requisitos funcionales

## RF-01. Registro de usuarios

El sistema deberá permitir registrar nuevos usuarios mediante un formulario.

El formulario deberá solicitar como mínimo:

* Nombre.
* Dirección de correo electrónico.
* Contraseña.

El sistema deberá comprobar que el correo electrónico no esté asociado previamente a otro usuario.

## RF-02. Inicio de sesión

El sistema deberá permitir a los usuarios autenticarse mediante su correo electrónico y contraseña.

Si las credenciales son incorrectas, el sistema deberá mostrar un mensaje indicando que los datos introducidos no son válidos.

## RF-03. Cierre de sesión

El usuario deberá poder cerrar su sesión en cualquier momento.

Después de cerrar sesión, no podrá acceder a las páginas que requieran autenticación sin volver a iniciar sesión.

## RF-04. Gestión de proyectos

Los usuarios con permisos suficientes deberán poder:

* Crear proyectos.
* Modificar proyectos.
* Eliminar proyectos.
* Consultar proyectos.

Cada proyecto deberá tener como mínimo:

* Nombre.
* Descripción.
* Fecha de creación.
* Estado.

## RF-05. Gestión de miembros

El administrador o responsable del proyecto deberá poder añadir usuarios a un proyecto y eliminarlos posteriormente.

Un usuario solamente podrá consultar las tareas de los proyectos a los que tenga acceso.

## RF-06. Creación de tareas

El sistema deberá permitir crear tareas dentro de un proyecto.

Cada tarea deberá contener como mínimo:

* Título.
* Descripción.
* Estado.
* Prioridad.
* Fecha de creación.
* Fecha límite.
* Usuario asignado.

## RF-07. Modificación de tareas

Los usuarios con permisos suficientes podrán modificar los datos de una tarea.

El sistema deberá guardar los cambios realizados.

## RF-08. Eliminación de tareas

Los usuarios con permisos suficientes podrán eliminar tareas.

Antes de eliminar una tarea, el sistema deberá solicitar confirmación.

## RF-09. Estados de las tareas

Cada tarea deberá tener uno de los siguientes estados:

1. Pendiente.
2. En progreso.
3. Completada.
4. Cancelada.

El usuario podrá cambiar el estado de una tarea cuando disponga de permisos para hacerlo.

## RF-10. Prioridad de las tareas

Cada tarea deberá disponer de un nivel de prioridad.

Los niveles disponibles serán:

* Baja.
* Media.
* Alta.
* Crítica.

## RF-11. Asignación de tareas

Una tarea podrá ser asignada a un usuario que pertenezca al proyecto correspondiente.

El sistema no permitirá asignar una tarea a un usuario que no pertenezca al proyecto.

## RF-12. Consulta de tareas

El usuario podrá consultar las tareas de un proyecto.

La aplicación deberá mostrar, como mínimo:

* Título.
* Estado.
* Prioridad.
* Usuario asignado.
* Fecha límite.

## RF-13. Filtrado de tareas

El sistema deberá permitir filtrar las tareas utilizando diferentes criterios:

* Estado.
* Prioridad.
* Usuario asignado.
* Fecha límite.

Los filtros podrán combinarse.

## RF-14. Ordenación de tareas

El usuario podrá ordenar las tareas por:

* Fecha de creación.
* Fecha límite.
* Prioridad.
* Estado.
* Nombre.

## RF-15. Notificaciones

El sistema deberá generar notificaciones cuando:

* Se asigne una tarea a un usuario.
* Se modifique una tarea asignada al usuario.
* Se aproxime la fecha límite de una tarea.
* Se elimine una tarea asignada al usuario.

## RF-16. Gestión de usuarios

Los administradores podrán consultar los usuarios registrados.

También podrán:

* Modificar los datos de un usuario.
* Desactivar una cuenta.
* Reactivar una cuenta.
* Eliminar una cuenta.

## RF-17. Búsqueda

El sistema deberá permitir buscar proyectos y tareas mediante texto.

La búsqueda deberá realizarse al menos sobre:

* Nombre del proyecto.
* Título de la tarea.
* Descripción de la tarea.

## RF-18. Persistencia de datos

Todos los datos relevantes de la aplicación deberán almacenarse en una base de datos.

La información deberá mantenerse después de cerrar o reiniciar la aplicación.

---

# 4. Requisitos no funcionales

## RNF-01. Rendimiento

Las operaciones habituales de consulta deberán responder en menos de 2 segundos bajo condiciones normales de funcionamiento.

## RNF-02. Disponibilidad

La aplicación deberá estar disponible de forma continua, excepto durante tareas de mantenimiento previamente planificadas.

## RNF-03. Seguridad

Las contraseñas no deberán almacenarse en texto plano.

El sistema deberá utilizar mecanismos seguros de autenticación y autorización.

Las operaciones que requieran permisos deberán comprobar la identidad y los permisos del usuario antes de ejecutarse.

## RNF-04. Protección de datos

El sistema deberá evitar que un usuario pueda consultar información perteneciente a proyectos a los que no tenga acceso.

Los datos personales de los usuarios deberán almacenarse y tratarse de forma segura.

## RNF-05. Usabilidad

La interfaz deberá ser sencilla e intuitiva.

Las acciones principales deberán poder realizarse sin necesidad de conocimientos técnicos.

## RNF-06. Compatibilidad

La aplicación web deberá funcionar correctamente en las versiones recientes de los principales navegadores:

* Google Chrome.
* Mozilla Firefox.
* Microsoft Edge.
* Safari.

## RNF-07. Diseño responsive

La interfaz deberá adaptarse a diferentes tamaños de pantalla, incluyendo:

* Ordenadores.
* Tablets.
* Teléfonos móviles.

## RNF-08. Mantenibilidad

El código deberá estar organizado en módulos independientes para facilitar su mantenimiento y evolución.

Las funciones principales deberán estar documentadas cuando su funcionamiento no resulte evidente.

## RNF-09. Escalabilidad

La arquitectura deberá permitir aumentar el número de usuarios y proyectos sin requerir una modificación completa del sistema.

## RNF-10. Copias de seguridad

La información almacenada deberá disponer de copias de seguridad periódicas.

---

# 5. Reglas de negocio

## RN-01. Unicidad del correo

Cada dirección de correo electrónico podrá estar asociada únicamente a una cuenta.

## RN-02. Acceso a proyectos

Un usuario solamente podrá acceder a los proyectos de los que sea miembro, salvo que tenga permisos de administrador.

## RN-03. Asignación de tareas

Una tarea solamente podrá asignarse a usuarios que pertenezcan al proyecto.

## RN-04. Eliminación de proyectos

Al eliminar un proyecto, todas las tareas asociadas al mismo dejarán de estar disponibles.

## RN-05. Fecha límite

La fecha límite de una tarea no podrá ser anterior a su fecha de creación.

## RN-06. Estados

Una tarea completada podrá volver a pasar a estado "En progreso" si es necesario realizar modificaciones sobre ella.

## RN-07. Usuarios desactivados

Un usuario desactivado no podrá iniciar sesión ni recibir nuevas tareas.

## RN-08. Permisos

Los usuarios solamente podrán ejecutar las operaciones para las que dispongan de permisos.

---

# 6. Casos de uso

## CU-01. Iniciar sesión

**Actor:** Usuario

**Precondiciones:**

* El usuario debe estar registrado.
* La cuenta debe estar activa.

**Flujo principal:**

1. El usuario accede a la página de inicio de sesión.
2. Introduce su correo electrónico.
3. Introduce su contraseña.
4. El sistema valida las credenciales.
5. El sistema inicia la sesión.
6. El usuario accede a la aplicación.

**Flujo alternativo:**

1. El sistema detecta que las credenciales no son correctas.
2. Se muestra un mensaje de error.
3. El usuario puede volver a introducir sus credenciales.

---

## CU-02. Crear proyecto

**Actor:** Administrador o usuario con permisos de gestión.

**Precondiciones:**

* El usuario debe haber iniciado sesión.

**Flujo principal:**

1. El usuario accede a la sección de proyectos.
2. Selecciona la opción "Crear proyecto".
3. Introduce el nombre y descripción.
4. Confirma la creación.
5. El sistema valida los datos.
6. El sistema crea el proyecto.
7. El proyecto aparece en la lista de proyectos.

---

## CU-03. Crear tarea

**Actor:** Usuario con permisos de edición.

**Precondiciones:**

* El usuario debe pertenecer al proyecto.
* El proyecto debe existir.

**Flujo principal:**

1. El usuario accede al proyecto.
2. Selecciona "Nueva tarea".
3. Introduce los datos de la tarea.
4. Selecciona un usuario responsable.
5. Confirma la creación.
6. El sistema valida los datos.
7. La tarea se almacena en la base de datos.
8. El usuario asignado recibe una notificación.

---

## CU-04. Modificar tarea

**Actor:** Usuario con permisos de edición.

**Precondiciones:**

* La tarea debe existir.
* El usuario debe tener permisos para modificarla.

**Flujo principal:**

1. El usuario selecciona una tarea.
2. Selecciona la opción "Editar".
3. Modifica los datos necesarios.
4. Guarda los cambios.
5. El sistema valida los datos.
6. El sistema actualiza la tarea.

---

## CU-05. Completar tarea

**Actor:** Usuario asignado.

**Precondiciones:**

* La tarea debe estar asignada al usuario.
* La tarea no debe estar cancelada.

**Flujo principal:**

1. El usuario abre la tarea.
2. Selecciona "Marcar como completada".
3. El sistema cambia el estado de la tarea a "Completada".
4. El sistema guarda el cambio.

---

# 7. Requisitos de interfaz

## 7.1. Pantalla de inicio de sesión

Deberá contener:

* Campo de correo electrónico.
* Campo de contraseña.
* Botón de inicio de sesión.
* Enlace para registrarse.
* Enlace para recuperar la contraseña.

## 7.2. Panel principal

El panel principal deberá mostrar:

* Proyectos del usuario.
* Tareas pendientes.
* Tareas próximas a vencer.
* Notificaciones recientes.

## 7.3. Vista de proyecto

La vista de un proyecto deberá mostrar:

* Nombre del proyecto.
* Descripción.
* Miembros.
* Lista de tareas.
* Filtros.
* Opción para crear una tarea.

## 7.4. Vista de tarea

La vista de una tarea deberá mostrar:

* Título.
* Descripción.
* Estado.
* Prioridad.
* Usuario asignado.
* Fecha de creación.
* Fecha límite.
* Historial de modificaciones.

---

# 8. Requisitos de seguridad

El sistema deberá cumplir las siguientes condiciones:

* Las contraseñas deberán almacenarse mediante un algoritmo de hash seguro.
* Las comunicaciones entre cliente y servidor deberán utilizar HTTPS.
* Las sesiones deberán estar protegidas contra accesos no autorizados.
* El servidor deberá validar los datos recibidos del cliente.
* El sistema deberá aplicar control de acceso basado en roles y permisos.
* Un usuario no podrá modificar directamente identificadores o permisos para obtener acceso a recursos no autorizados.
* Las operaciones administrativas deberán quedar registradas en un sistema de auditoría.

---

# 9. Restricciones

El desarrollo de la aplicación estará sujeto a las siguientes restricciones:

* La aplicación deberá funcionar mediante un navegador web.
* Será necesario disponer de una conexión a Internet para utilizar el sistema.
* Los datos deberán almacenarse en una base de datos relacional.
* La aplicación deberá utilizar una arquitectura cliente-servidor.
* El sistema deberá poder desplegarse en un servidor Linux.
* El software deberá utilizar tecnologías con soporte activo.

---

# 10. Supuestos y dependencias

Se consideran los siguientes supuestos:

* Los usuarios dispondrán de una dirección de correo electrónico válida.
* El servidor tendrá acceso a una base de datos.
* El servidor dispondrá de conexión a Internet.
* Los usuarios utilizarán navegadores compatibles.
* El sistema de envío de notificaciones estará disponible cuando sea necesario enviar correos electrónicos.

---

# 11. Criterios de aceptación

| ID    | Criterio                                                                      |
| ----- | ----------------------------------------------------------------------------- |
| CA-01 | Un usuario registrado puede iniciar sesión con sus credenciales.              |
| CA-02 | Un usuario no registrado no puede acceder a las zonas privadas.               |
| CA-03 | Un usuario puede consultar sus proyectos.                                     |
| CA-04 | Un usuario autorizado puede crear un proyecto.                                |
| CA-05 | Un usuario autorizado puede crear una tarea dentro de un proyecto.            |
| CA-06 | Una tarea puede asignarse únicamente a miembros del proyecto.                 |
| CA-07 | Un usuario autorizado puede modificar una tarea.                              |
| CA-08 | Un usuario autorizado puede eliminar una tarea.                               |
| CA-09 | Una tarea puede cambiar de estado.                                            |
| CA-10 | Las tareas pueden filtrarse por estado y prioridad.                           |
| CA-11 | Los usuarios reciben una notificación cuando se les asigna una tarea.         |
| CA-12 | Un usuario no puede acceder a proyectos de los que no sea miembro.            |
| CA-13 | Un administrador puede gestionar los usuarios.                                |
| CA-14 | Los datos permanecen almacenados después de reiniciar el servidor.            |
| CA-15 | La aplicación funciona correctamente en dispositivos móviles y de escritorio. |

---

# 12. Matriz de trazabilidad

La siguiente matriz relaciona los requisitos funcionales con algunos de sus criterios de aceptación.

| Requisito | Descripción            | Criterios de aceptación |
| --------- | ---------------------- | ----------------------- |
| RF-01     | Registro de usuarios   | CA-01, CA-02            |
| RF-02     | Inicio de sesión       | CA-01, CA-02            |
| RF-04     | Gestión de proyectos   | CA-03, CA-04            |
| RF-05     | Gestión de miembros    | CA-12                   |
| RF-06     | Creación de tareas     | CA-05, CA-06            |
| RF-07     | Modificación de tareas | CA-07                   |
| RF-08     | Eliminación de tareas  | CA-08                   |
| RF-09     | Estados de tareas      | CA-09                   |
| RF-10     | Prioridad de tareas    | CA-10                   |
| RF-11     | Asignación de tareas   | CA-06, CA-11            |
| RF-13     | Filtrado de tareas     | CA-10                   |
| RF-16     | Gestión de usuarios    | CA-13                   |
| RF-18     | Persistencia de datos  | CA-14                   |

---

# 13. Prioridad de los requisitos

Los requisitos se clasifican según su importancia para la primera versión del sistema.

| Prioridad | Requisitos                                             |
| --------- | ------------------------------------------------------ |
| Alta      | RF-02, RF-04, RF-06, RF-07, RF-09, RF-11, RF-12, RF-18 |
| Media     | RF-05, RF-08, RF-10, RF-13, RF-14, RF-16               |
| Baja      | RF-15, RF-17                                           |

Los requisitos de prioridad alta serán necesarios para disponer de una primera versión funcional de la aplicación. Los requisitos de prioridad media y baja podrán incorporarse progresivamente en versiones posteriores.

---

# 14. Resumen

La aplicación será un sistema web destinado a la gestión de proyectos y tareas. Permitirá a los usuarios organizar trabajos, asignar responsabilidades y realizar un seguimiento de su progreso.

El sistema contará con diferentes niveles de permisos para garantizar que cada usuario solamente pueda acceder y modificar la información correspondiente.

Los requisitos definidos en este documento servirán como base para las siguientes fases del proyecto:

1. Diseño de la arquitectura.
2. Diseño de la base de datos.
3. Diseño de la interfaz.
4. Implementación.
5. Pruebas.
6. Despliegue.
7. Mantenimiento.

