# Gantt: web app

Web app de diagramas de Gantt para ordenador, iPad y móvil. Se instala con su propio icono, funciona sin conexión y guarda tus proyectos en tu Google Drive, en la carpeta **"Gantt (web app)"**.

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | La app |
| `config.js` | Aquí va tu Client ID de Google (paso 2) |
| `manifest.webmanifest` | Nombre, icono y colores para instalarla |
| `sw.js` | Hace que funcione sin conexión |
| `icons/` | Iconos de la app |

## 1. Subirla a GitHub Pages

1. Crea un repositorio nuevo en GitHub, por ejemplo `gantt`.
2. Sube **todos** los archivos manteniendo la carpeta `icons/` (*Add file → Upload files*).
3. Ve a *Settings → Pages*. En *Source* elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`. Guarda.
4. En un minuto tendrás la app en `https://TU-USUARIO.github.io/gantt/`.

## 2. Conectar con Google Drive (una sola vez)

> Si ya tienes un Client ID de tu otra app, puedes reutilizarlo: haz solo los pasos d y e.

1. **Crear proyecto:** entra en [console.cloud.google.com](https://console.cloud.google.com) y crea un proyecto (o usa el de tu otra app).
2. **Activar la API:** ve a *APIs y servicios → Biblioteca*, busca **Google Drive API** y pulsa **Habilitar**.
3. **Pantalla de consentimiento:** en *APIs y servicios → Pantalla de consentimiento de OAuth* (o *Google Auth Platform*), elige tipo **Externo**, ponle un nombre (p. ej. "Gantt") y tu correo. En *Público / Usuarios de prueba*, añade tu correo de Google.
4. **Credenciales:** ve a *Credenciales → Crear credenciales → ID de cliente de OAuth*, tipo **Aplicación web**. En **Orígenes autorizados de JavaScript**, añade:
   `https://TU-USUARIO.github.io`
   (solo el dominio: sin `/gantt` y sin barra final). Crea y copia el **ID de cliente**, que termina en `.apps.googleusercontent.com`.
5. **Pegarlo en la app:** abre `config.js` en GitHub (icono del lápiz), pega el ID entre las comillas y guarda (*Commit*):
   ```js
   googleClientId: '1234567890-abc.apps.googleusercontent.com'
   ```

Abre la app y toca **"Conectar Google Drive"** arriba. La primera vez Google te avisará de que la app "no está verificada": es normal porque es tuya. Pulsa *Continuar*.

La app solo puede ver los archivos que crea ella misma (permiso `drive.file`), no el resto de tu Drive.

## 3. Instalarla

- **iPhone / iPad (Safari):** Compartir → *Añadir a pantalla de inicio*.
- **Android (Chrome):** ⋮ → *Instalar app* (o *Añadir a pantalla de inicio*).
- **PC/Mac (Chrome o Edge):** icono de instalar en la barra de direcciones.

## Cómo funciona el guardado

- Cada proyecto es un archivo `Nombre.gantt.json` en la carpeta **Gantt (web app)** de tu Drive. Se guarda unos segundos después de cada cambio.
- Al abrir la app, al volver a ella y cada 45 segundos, se trae los cambios hechos en tus otros dispositivos.
- Sin conexión sigue funcionando y sube los cambios cuando vuelve la conexión.
- Google pide volver a entrar cada cierto tiempo (la sesión dura 1 hora). Si arriba aparece **"Conectar Google Drive"**, tócalo; normalmente basta un toque.
- Los proyectos de la versión anterior dentro de Claude están en otra carpeta ("Gantt"). Para traerlos, descarga el `.json` y usa **⋯ → Importar archivo…**.

## Actualizar la app

Si cambias algo, sube los archivos nuevos y cambia en `sw.js` la línea `const VERSION = 'gantt-v1'` (por ejemplo, a `gantt-v2`) para que los dispositivos cojan la versión nueva.
