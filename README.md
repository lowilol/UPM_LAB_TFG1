## Tabla de Contenidos
- [Sistema de Gestión de Laboratorios ETSISI - UPM](#sistema-de-Gestion-de-laboratorios-ETSISI-UPM)
- [Descripción del Proyecto](#descripcion-del-proyecto)
- [Desplegar Servidor Frontend](#desplegar-servidor-frontend)
- [Desplegar Servidor Backend](#desplegar-servidor-backend)
- [Ejecutar las Pruebas](#ejecutar-las-pruebas)
  - [Pruebas del Frontend (E2E)](#pruebas-del-frontend-e2e)
  - [Pruebas del Backend (API / caja negra)](#pruebas-del-backend-api--caja-negra)
## Sistema de Gestión de Laboratorios ETSISI - UPM
Este repositorio contiene el código fuente completo del Trabajo de Fin de Grado (TFG) basado en la digitalización de la gestión de laboratorios y turnos en la Escuela Técnica Superior de Ingenieros de Sistemas Informáticos (ETSISI) de la Universidad Politécnica de Madrid (UPM).
## Descripción del Proyecto
La aplicación permite a alumnos, profesores y personal PAS gestionar digitalmente la reserva de laboratorios, la creación y edición de turnos, el reporte de incidencias y la visualización de estados, todo desde una interfaz web intuitiva.
## Desplegar Servidor Frontend
Sigue estos pasos para desplegar el servidor frontend en tu entorno local:
1. **Accede al directorio:**
    ```bash
    cd ./TFG_GESTIONGRUPOSLAB/front
    ```
2. **Instala las dependencias:**
    ```bash
    npm install
    ```
3. **Ejecuta en modo desarrollo:**
    ```bash
    npm run dev
    ```
## Desplegar Servidor Backend
Sigue estos pasos para desplegar el servidor backend en tu entorno local:
1. **Accede al directorio:**
    ```bash
    cd ./TFG_GESTIONGRUPOSLAB/back
    ```
2. **Instala las dependencias:**
    ```bash
    npm install
    ```
3. **Ejecuta en modo desarrollo:**
    ```bash
    nodemon app.js
    ```
## Ejecutar las Pruebas
El proyecto incluye pruebas automatizadas con **Playwright**: pruebas E2E en el frontend y pruebas de API (caja negra) en el backend.

### Pruebas del Frontend (E2E)
Las pruebas E2E simulan (mockean) las llamadas de red, por lo que **no necesitan** el backend ni la base de datos. Arrancan automáticamente el servidor de Vite en el puerto 3000.

1. **Accede al directorio e instala las dependencias:**
    ```bash
    cd ./TFG_GESTIONGRUPOSLAB/front
    npm install
    ```
2. **Instala el navegador de Playwright** (solo la primera vez):
    ```bash
    npx playwright install chromium
    ```
3. **Ejecuta las pruebas:**
    ```bash
    npm run test:e2e            # ejecuta todas las pruebas
    npm run test:e2e:ui         # modo interactivo (UI)
    npm run test:e2e:report     # abre el último informe HTML
    ```

### Pruebas del Backend (API / caja negra)
Estas pruebas atacan al servidor **real** contra la **base de datos real**, por lo que requieren:
- El backend levantado en `http://127.0.0.1:4000` (en una terminal aparte).
- **MySQL** y **Redis** accesibles (credenciales en `back/.env`).
- **SMTP** operativo (`EMAIL_USER` / `EMAIL_PASS` en `back/.env`) — solo para las pruebas de notificación por correo.

1. **Accede al directorio e instala las dependencias:**
    ```bash
    cd ./TFG_GESTIONGRUPOSLAB/back
    npm install
    npx playwright install        # binarios de Playwright (solo la primera vez)
    ```
2. **Arranca el backend** en una terminal aparte:
    ```bash
    npm run dev                   # o npm start
    ```
3. **En otra terminal, ejecuta las pruebas:**
    ```bash
    npm run test:api              # ejecuta CP-01 .. CP-27
    npm run test:email            # solo las pruebas de envío de correo
    npm run test:api:report       # abre el informe HTML
    ```

Para filtrar por módulo o caso concreto:
```bash
npx playwright test --config=playwright.api.config.ts cp-auth.spec.ts
npx playwright test --config=playwright.api.config.ts -g "CP-13"
```

> Los tests del backend siembran usuarios de prueba deterministas y los eliminan al terminar. Encontrarás más detalles (mapa de casos CP-01…CP-27, notas de alcance) en `back/tests/README.md`.
