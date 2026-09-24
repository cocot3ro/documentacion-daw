# Análisis de caja blanca

## 1. Introducción

El análisis de caja blanca es una técnica de pruebas que permite comprobar el funcionamiento interno de una aplicación mediante el análisis de su código fuente, estructuras de control y flujo de ejecución.

En este proyecto se utilizará para comprobar que las principales funciones de la aplicación de gestión de proyectos y tareas funcionan correctamente en las diferentes rutas de ejecución posibles.

El análisis se centrará principalmente en:

* Autenticación de usuarios.
* Creación de tareas.
* Asignación de tareas.
* Cambio de estado de las tareas.
* Control de permisos.

---

# 2. Objetivos

Los objetivos del análisis son:

* Detectar errores en la lógica interna.
* Comprobar todas las ramas de las estructuras condicionales.
* Identificar caminos de ejecución no probados.
* Comprobar las condiciones límite.
* Aumentar la cobertura del código mediante pruebas unitarias.
* Detectar posibles errores de autorización.

---

# 3. Función analizada: inicio de sesión

Se analizará la función responsable de autenticar a un usuario.

### Código de ejemplo

```java
public boolean iniciarSesion(String email, String password) {

    Usuario usuario = usuarioRepository.buscarPorEmail(email);

    if (usuario == null) {
        return false;
    }

    if (!usuario.isActivo()) {
        return false;
    }

    if (!passwordEncoder.matches(password, usuario.getPasswordHash())) {
        return false;
    }

    crearSesion(usuario);

    return true;
}
```

---

# 4. Grafo de flujo

El flujo de ejecución de la función puede representarse de la siguiente forma:

```text
              ┌─────────────┐
              │    Inicio   │
              └──────┬──────┘
                     │
                     v
          ┌─────────────────────┐
          │ Buscar usuario      │
          └──────────┬──────────┘
                     │
                     v
              ┌─────────────┐
              │ usuario ==  │
              │    null?    │
              └──────┬──────┘
                 Sí  │  No
                     │
              ┌──────v──────┐
              │    False    │
              └─────────────┘
                    

                     No
                     │
                     v
              ┌─────────────┐
              │ ¿Está       │
              │ activo?     │
              └──────┬──────┘
                 No  │  Sí
                     │
              ┌──────v──────┐
              │    False    │
              └─────────────┘

                     Sí
                     │
                     v
          ┌─────────────────────┐
          │ ¿Contraseña        │
          │ correcta?          │
          └──────────┬──────────┘
                 No  │  Sí
                     │
              ┌──────v──────┐
              │    False    │
              └─────────────┘

                     Sí
                     │
                     v
          ┌─────────────────────┐
          │ Crear sesión        │
          └──────────┬──────────┘
                     │
                     v
              ┌─────────────┐
              │    True     │
              └─────────────┘
```

---

# 5. Complejidad ciclomática

La complejidad ciclomática permite determinar el número de caminos independientes que deben comprobarse.

La fórmula utilizada es:

```text
M = E - N + 2P
```

Donde:

* `M` = complejidad ciclomática.
* `E` = número de aristas.
* `N` = número de nodos.
* `P` = número de componentes conexos.

En este caso existen **3 decisiones independientes**:

1. El usuario existe.
2. El usuario está activo.
3. La contraseña es correcta.

Por tanto:

```text
M = decisiones + 1

M = 3 + 1

M = 4
```

La función tiene una **complejidad ciclomática de 4**, por lo que como mínimo se necesitan cuatro caminos independientes para cubrir todas las ramas principales.

---

# 6. Caminos independientes

## Camino 1: Usuario inexistente

```text
Inicio
  ↓
Buscar usuario
  ↓
Usuario == null
  ↓
False
```

**Resultado esperado:** el inicio de sesión debe rechazarse.

---

## Camino 2: Usuario desactivado

```text
Inicio
  ↓
Buscar usuario
  ↓
Usuario encontrado
  ↓
Usuario no activo
  ↓
False
```

**Resultado esperado:** el inicio de sesión debe rechazarse.

---

## Camino 3: Contraseña incorrecta

```text
Inicio
  ↓
Buscar usuario
  ↓
Usuario encontrado
  ↓
Usuario activo
  ↓
Contraseña incorrecta
  ↓
False
```

**Resultado esperado:** el inicio de sesión debe rechazarse.

---

## Camino 4: Inicio de sesión correcto

```text
Inicio
  ↓
Buscar usuario
  ↓
Usuario encontrado
  ↓
Usuario activo
  ↓
Contraseña correcta
  ↓
Crear sesión
  ↓
True
```

**Resultado esperado:** el usuario inicia sesión correctamente.

---

# 7. Tabla de pruebas

| ID    | Usuario   | Estado      | Contraseña | Resultado esperado         |
| ----- | --------- | ----------- | ---------- | -------------------------- |
| CP-01 | No existe | -           | -          | Inicio de sesión rechazado |
| CP-02 | Existe    | Desactivado | Correcta   | Inicio de sesión rechazado |
| CP-03 | Existe    | Activo      | Incorrecta | Inicio de sesión rechazado |
| CP-04 | Existe    | Activo      | Correcta   | Inicio de sesión aceptado  |

Estas cuatro pruebas permiten recorrer los cuatro caminos independientes identificados.

---

# 8. Función analizada: creación de tareas

También se analizará la función encargada de crear una tarea.

### Código de ejemplo

```java
public boolean crearTarea(
        Usuario usuario,
        Proyecto proyecto,
        String titulo,
        Date fechaLimite) {

    if (usuario == null || proyecto == null) {
        return false;
    }

    if (!proyecto.getMiembros().contains(usuario)) {
        return false;
    }

    if (titulo == null || titulo.isBlank()) {
        return false;
    }

    if (fechaLimite.before(new Date())) {
        return false;
    }

    Tarea tarea = new Tarea();
    tarea.setTitulo(titulo);
    tarea.setFechaLimite(fechaLimite);
    tarea.setProyecto(proyecto);

    tareaRepository.guardar(tarea);

    return true;
}
```

---

# 9. Decisiones de la función

La función contiene las siguientes condiciones:

1. El usuario y el proyecto deben existir.
2. El usuario debe pertenecer al proyecto.
3. El título debe ser válido.
4. La fecha límite debe ser válida.

La complejidad ciclomática aproximada es:

```text
M = 4 + 1

M = 5
```

Por tanto, existen al menos **5 caminos independientes** que deberían comprobarse.

---

# 10. Casos de prueba de creación de tareas

| ID    | Usuario   | Proyecto | Miembro | Título | Fecha  | Resultado |
| ----- | --------- | -------- | ------- | ------ | ------ | --------- |
| CT-01 | No existe | Existe   | -       | Válido | Futura | Rechazado |
| CT-02 | Existe    | Existe   | No      | Válido | Futura | Rechazado |
| CT-03 | Existe    | Existe   | Sí      | Vacío  | Futura | Rechazado |
| CT-04 | Existe    | Existe   | Sí      | Válido | Pasada | Rechazado |
| CT-05 | Existe    | Existe   | Sí      | Válido | Futura | Creada    |

---

# 11. Análisis de control de permisos

El control de permisos es especialmente importante porque determina qué operaciones puede realizar cada usuario.

### Código de ejemplo

```java
public boolean puedeEliminarProyecto(
        Usuario usuario,
        Proyecto proyecto) {

    if (usuario == null || proyecto == null) {
        return false;
    }

    if (usuario.getRol() == Rol.ADMINISTRADOR) {
        return true;
    }

    return false;
}
```

La función contiene dos decisiones:

* El usuario y el proyecto existen.
* El usuario tiene rol de administrador.

La complejidad ciclomática es:

```text
M = 2 + 1

M = 3
```

---

# 12. Casos de prueba de permisos

| ID    | Usuario   | Proyecto | Rol           | Resultado        |
| ----- | --------- | -------- | ------------- | ---------------- |
| CP-01 | No existe | Existe   | -             | Acceso rechazado |
| CP-02 | Existe    | Existe   | USUARIO       | Acceso rechazado |
| CP-03 | Existe    | Existe   | ADMINISTRADOR | Acceso permitido |

---

# 13. Cobertura de código

La cobertura permite determinar qué porcentaje del código ha sido ejecutado durante las pruebas.

Se tendrán en cuenta principalmente:

### Cobertura de sentencias

Comprueba que cada instrucción del código haya sido ejecutada al menos una vez.

### Cobertura de decisiones

Comprueba que cada decisión haya sido evaluada tanto como verdadera como falsa.

### Cobertura de caminos

Comprueba que los diferentes caminos independientes identificados mediante la complejidad ciclomática hayan sido ejecutados.

Para las funciones analizadas se pretende conseguir una cobertura de decisiones del 100 %.

---

# 14. Resumen de cobertura

| Función           | Complejidad ciclomática | Caminos independientes | Pruebas |
| ----------------- | ----------------------: | ---------------------: | ------: |
| Iniciar sesión    |                       4 |                      4 |       4 |
| Crear tarea       |                       5 |                      5 |       5 |
| Eliminar proyecto |                       3 |                      3 |       3 |

Total:

```text
Complejidad ciclomática = 12
Caminos independientes = 12
Casos de prueba = 12
```

---

# 15. Conclusiones

El análisis de caja blanca permite comprobar la lógica interna de las funciones críticas de la aplicación.

Las funciones analizadas presentan diferentes caminos de ejecución relacionados principalmente con:

* Validación de datos.
* Autenticación.
* Autorización.
* Estado de los usuarios.
* Pertenencia a proyectos.
* Validación de fechas.

La utilización de la complejidad ciclomática permite identificar los caminos independientes que deben cubrirse mediante pruebas.

Para las funciones analizadas se han definido **12 caminos independientes**, que constituyen la base para las pruebas estructurales de la aplicación.

