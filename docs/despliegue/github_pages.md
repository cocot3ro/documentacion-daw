# Despliegue en GitHub Pages

## 1. Introducción

GitHub Pages es un servicio de alojamiento de sitios web estáticos integrado en GitHub.

En este proyecto se utilizará GitHub Pages para publicar el frontend de la aplicación de gestión de tareas.

El backend y la base de datos no se desplegarán en GitHub Pages, ya que requieren un entorno de ejecución de servidor.

La arquitectura de despliegue será:

```text
┌──────────────────────┐
│       Usuario        │
└──────────┬───────────┘
           │
           │ HTTPS
           ▼
┌──────────────────────┐
│    GitHub Pages      │
│                      │
│      Frontend        │
└──────────┬───────────┘
           │
           │ HTTPS / REST API
           ▼
┌──────────────────────┐
│       Backend        │
│                      │
│      REST API        │
└──────────┬───────────┘
           │
           │ SQL
           ▼
┌──────────────────────┐
│      Base de datos   │
└──────────────────────┘
```

---

# 2. Requisitos previos

Para realizar el despliegue será necesario disponer de:

* Una cuenta de GitHub.
* Un repositorio que contenga el proyecto.
* Git instalado.
* Node.js y npm instalados.
* Un frontend que pueda generar una versión estática.
* Un backend accesible desde Internet mediante HTTPS.

---

# 3. Preparación del proyecto

El frontend deberá poder generar una versión de producción.

Por ejemplo, utilizando npm:

```bash
npm install
```

Posteriormente se generará la aplicación:

```bash
npm run build
```

Esto generará un directorio con los archivos estáticos necesarios para ejecutar la aplicación.

Por ejemplo:

```text
frontend/
├── src/
├── public/
├── package.json
└── dist/
    ├── index.html
    ├── assets/
    └── ...
```

El contenido del directorio `dist/` será el que se publique en GitHub Pages.

---

# 4. Configuración de GitHub Pages

Desde el repositorio de GitHub se accederá a:

```text
Settings
    ↓
Pages
```

En la configuración de GitHub Pages se seleccionará:

```text
Source:
GitHub Actions
```

De esta forma, el proceso de despliegue se realizará automáticamente mediante GitHub Actions.

---

# 5. GitHub Actions

Se creará el siguiente archivo:

```text
.github/
└── workflows/
    └── deploy-pages.yml
```

El workflow será responsable de:

1. Descargar el código del repositorio.
2. Instalar las dependencias.
3. Compilar el frontend.
4. Preparar los archivos generados.
5. Publicarlos en GitHub Pages.

Un workflow básico será:

```yaml
name: Deploy frontend

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Descargar código
        uses: actions/checkout@v4

      - name: Configurar Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm
          cache-dependency-path: frontend/package-lock.json

      - name: Instalar dependencias
        working-directory: frontend
        run: npm ci

      - name: Compilar aplicación
        working-directory: frontend
        run: npm run build

      - name: Configurar Pages
        uses: actions/configure-pages@v5

      - name: Subir artefacto
        uses: actions/upload-pages-artifact@v3
        with:
          path: frontend/dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    runs-on: ubuntu-latest

    needs: build

    steps:
      - name: Desplegar en GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

---

# 6. Proceso de despliegue

Una vez configurado el workflow, el despliegue se realizará automáticamente cada vez que se realice un `push` sobre la rama `main`.

El proceso será:

```text
Desarrollador
     │
     │ git push
     ▼
┌───────────────┐
│    GitHub     │
└───────┬───────┘
        │
        ▼
┌──────────────────────┐
│    GitHub Actions    │
│                      │
│  1. Checkout         │
│  2. npm ci           │
│  3. npm run build    │
│  4. Upload artifact  │
│  5. Deploy           │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│    GitHub Pages      │
│                      │
│      Frontend        │
└──────────────────────┘
```

---

# 7. Configuración de la API

El frontend necesitará conocer la dirección del backend para realizar las peticiones HTTP.

No se deberá utilizar una dirección `localhost` en la versión desplegada.

Por ejemplo, durante el desarrollo:

```text
http://localhost:8080/api
```

Mientras que en producción se utilizará una dirección accesible públicamente:

```text
https://api.example.com/api
```

La URL podrá configurarse mediante una variable de entorno.

Por ejemplo:

```text
VITE_API_URL=https://api.example.com
```

El código del frontend podrá utilizarla:

```javascript
const API_URL = import.meta.env.VITE_API_URL;
```

---

# 8. Configuración para diferentes entornos

Se podrán utilizar diferentes configuraciones para desarrollo y producción.

### Desarrollo

```text
VITE_API_URL=http://localhost:8080
```

### Producción

```text
VITE_API_URL=https://api.example.com
```

La configuración de producción deberá utilizarse únicamente durante el proceso de compilación.

---

# 9. Comunicación con el backend

GitHub Pages solamente alojará los archivos del frontend.

Cuando un usuario realice una operación que requiera datos del servidor, el frontend realizará una petición al backend.

Por ejemplo:

```text
Frontend
   │
   │ GET /api/projects
   ▼
Backend
   │
   │ Consulta
   ▼
Base de datos
   │
   │ Resultado
   ▼
Backend
   │
   │ JSON
   ▼
Frontend
```

---

# 10. CORS

Al estar alojados el frontend y el backend en dominios diferentes, el backend deberá permitir las peticiones procedentes del dominio utilizado por GitHub Pages.

Por ejemplo:

```text
https://usuario.github.io
```

El backend deberá configurar una política CORS que permita este origen.

Deberá evitarse permitir todos los orígenes indiscriminadamente en producción:

```text
Access-Control-Allow-Origin: *
```

En su lugar, deberá configurarse explícitamente el dominio del frontend.

---

# 11. Aplicaciones SPA

Si el frontend utiliza una arquitectura SPA (Single Page Application), por ejemplo React con React Router, existe una consideración adicional.

Una aplicación SPA puede tener rutas como:

```text
/
 /login
 /projects
 /projects/15
 /tasks/42
```

GitHub Pages sirve inicialmente archivos estáticos. Si el usuario accede directamente a:

```text
/projects/15
```

el servidor puede no encontrar un archivo correspondiente a esa ruta.

Para evitar este problema, la aplicación deberá configurarse para gestionar correctamente las rutas al desplegarse en GitHub Pages.

Una alternativa es utilizar `HashRouter`, produciendo URLs como:

```text
https://usuario.github.io/proyecto/#/projects/15
```

Otra posibilidad es configurar una estrategia de redirección para que las rutas de la SPA sean gestionadas por `index.html`.

---

# 12. Dominio

GitHub Pages proporciona una dirección basada en el repositorio.

Por ejemplo:

```text
https://usuario.github.io/gestion-tareas/
```

También es posible configurar un dominio personalizado.

Por ejemplo:

```text
https://gestion-tareas.example.com
```

Para utilizar un dominio personalizado será necesario configurar los registros DNS correspondientes y añadir el dominio en la configuración de GitHub Pages.

---

# 13. HTTPS

GitHub Pages proporciona HTTPS para los sitios publicados.

La aplicación deberá utilizar HTTPS también para comunicarse con el backend.

Por tanto, la configuración de producción deberá utilizar:

```text
https://usuario.github.io/gestion-tareas/
```

y:

```text
https://api.example.com
```

No se deberán utilizar conexiones HTTP sin cifrar para transmitir credenciales o información sensible.

---

# 14. Actualización de la aplicación

El proceso de actualización será automático.

Cuando se realicen cambios en el código:

```bash
git add .
git commit -m "Actualizar aplicación"
git push origin main
```

GitHub Actions ejecutará nuevamente el proceso de compilación y despliegue.

```text
git push
    │
    ▼
GitHub Actions
    │
    ├── Instalar dependencias
    ├── Compilar
    ├── Generar dist/
    └── Publicar
             │
             ▼
       GitHub Pages
```

---

# 15. Comprobación del despliegue

Después de realizar el despliegue se deberán comprobar:

* Que la página principal carga correctamente.
* Que los recursos JavaScript se cargan correctamente.
* Que los archivos CSS se cargan correctamente.
* Que las rutas funcionan.
* Que el frontend puede comunicarse con el backend.
* Que las peticiones HTTPS funcionan.
* Que la autenticación funciona correctamente.
* Que no aparecen errores en la consola del navegador.

También se deberá comprobar el resultado del workflow desde:

```text
GitHub
    ↓
Actions
    ↓
Deploy frontend
```

---

# 16. Estructura final del repositorio

La estructura del repositorio quedará aproximadamente así:

```text
gestion-tareas/
│
├── .github/
│   └── workflows/
│       └── deploy-pages.yml
│
├── frontend/
│   ├── public/
│   ├── src/
│   ├── package.json
│   └── package-lock.json
│
├── backend/
│   └── ...
│
├── database/
│   └── ...
│
├── docs/
│   ├── requisitos.md
│   ├── diagrama-clases.md
│   ├── caja-blanca.md
│   ├── estructura-proyecto.md
│   └── despliegue-github-pages.md
│
├── .gitignore
├── README.md
└── docker-compose.yml
```

---

# 17. Consideraciones

GitHub Pages solamente será responsable del alojamiento del frontend.

La arquitectura final será:

| Componente                    | Ubicación        |
| ----------------------------- | ---------------- |
| Frontend                      | GitHub Pages     |
| Backend                       | Servidor externo |
| Base de datos                 | Servidor externo |
| Código fuente                 | GitHub           |
| Automatización del despliegue | GitHub Actions   |

De esta forma, GitHub Pages se utilizará como servicio de alojamiento estático mientras que el backend y la base de datos se ejecutarán en una infraestructura capaz de proporcionar servicios de servidor.

# 18. Resumen del proceso

El proceso completo de despliegue será:

```text
┌──────────────┐
│ Desarrollador│
└──────┬───────┘
       │
       │ git push
       ▼
┌──────────────┐
│    GitHub    │
└──────┬───────┘
       │
       ▼
┌─────────────────────┐
│   GitHub Actions    │
│                     │
│ npm ci              │
│ npm run build       │
│ upload artifact     │
│ deploy               │
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│    GitHub Pages     │
│      Frontend       │
└─────────┬───────────┘
          │
          │ HTTPS / REST
          ▼
┌─────────────────────┐
│       Backend       │
└─────────┬───────────┘
          │
          │ SQL
          ▼
┌─────────────────────┐
│     Base de datos   │
└─────────────────────┘
```

