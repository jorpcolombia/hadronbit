# HadronBit — Sitio Estático

Reconstrucción estática de **hadronbit.com**, originalmente un sitio en WordPress. Desplegado en Cloudflare Pages.

## Descripción general

HTML/CSS/JS puro — sin frameworks ni pasos de compilación. Las páginas están organizadas en una estructura de carpetas con URLs limpias y se despliegan tal cual en Cloudflare Pages (principal) o GitHub Pages (respaldo).

## Estructura

```
/
├── index.html
├── about/index.html
├── services/index.html
├── contact/index.html
├── assets/
│   ├── css/
│   ├── js/
│   ├── fonts/
│   └── images/
├── _redirects          ← Reglas de redirección de Cloudflare Pages
├── _headers            ← Cabeceras de caché y seguridad
└── 404.html
```

## Páginas

| URL | Archivo |
|-----|---------|
| `/` | `index.html` |
| `/auditoria-de-ciberseguridad-diagnostico-empresarial/` | `auditoria-.../index.html` |
| `/serviciosciberseguridad/` | `serviciosciberseguridad/index.html` |
| `/servicios-it-para-empresas-en-colombia-outsourcing-soporte-y-microsoft-365/` | `servicios-it-.../index.html` |
| `/contacto-para-servicios-de-tecnologia-y-ciberseguridad-en-colombia/` | `contacto-.../index.html` |

## Convenciones

- Los enlaces internos siempre usan rutas absolutas desde la raíz: `/about/`, `/services/`
- Los recursos van en `/assets/{css,js,images,fonts}/`
- Sin markup de WordPress, query strings (`?ver=`) ni código de administración
- Sin herramientas de compilación ni `package.json` a menos que se necesiten explícitamente

## Despliegue

### Cloudflare Pages

1. Conectar el repositorio de GitHub en el panel de Cloudflare Pages
2. Comando de compilación: *(ninguno)*
3. Directorio de salida: `/`
4. Configurar dominio personalizado: `hadronbit.com`

### GitHub Pages

Activar en la rama `main` en la raíz `/`.

## Edición con Claude Cowork

Puedes editar este sitio de forma conversacional usando **Claude Cowork** — una app de escritorio con inteligencia artificial que lee y edita tus archivos de código directamente a partir de instrucciones en lenguaje natural.

### Descarga y creación de cuenta

1. **Descargar** — ingresa a [claude.ai/download](https://claude.ai/download) y descarga la app de escritorio de Claude para tu sistema operativo (Mac o Windows).

2. **Crear una cuenta** — abre la app y haz clic en **Registrarse**. Puedes registrarte con un correo electrónico o una cuenta de Google. Hay un plan gratuito disponible; el plan Pro desbloquea límites de uso más altos.

3. **Activar Claude Cowork** — una vez que hayas iniciado sesión, abre **Configuración → Claude Cowork** y actívalo. Esto le permite a la app leer y editar archivos en tu computador.

### Requisitos previos

- Claude Cowork activado (ver arriba)
- Git instalado y configurado con acceso a este repositorio
- El repositorio clonado localmente

### Flujo de trabajo

1. **Abrir el proyecto** — en Claude Cowork, abre una nueva conversación y agrega la carpeta del repositorio como contexto (arrastra la carpeta o usa el botón **Agregar archivos**).

2. **Describir el cambio** — Claude Cowork lee los archivos HTML y hace las ediciones directamente. Ejemplos:
   - *"Actualiza el titular principal de la página de inicio"*
   - *"Agrega una nueva tarjeta de servicio a la página de ciberseguridad"*
   - *"Cambia el correo de contacto en el pie de página de todas las páginas"*

3. **Revisar el diff** — Claude Cowork muestra los cambios antes de guardarlos. Aprueba o rechaza cada modificación.

4. **Sincronizar con GitHub** — cuando estés satisfecho con los cambios, haz commit y push:
   ```bash
   git add .
   git commit -m "tu mensaje"
   git push origin main
   ```
   O pídeselo a Claude Cowork: *"Haz commit de estos cambios con el mensaje 'actualizar texto del hero'"*

5. **Desplegar** — hacer push a `main` dispara un despliegue automático en Cloudflare Pages (no se necesita ningún paso adicional).

### Consejos

- Puedes editar varias páginas en una misma sesión — simplemente sigue describiendo los cambios.
- Para deshacer un cambio antes de hacer commit: `git checkout -- <archivo>` o pide *"Revierte ese último cambio"*.
- Para previsualizar localmente, abre cualquier `index.html` en el navegador