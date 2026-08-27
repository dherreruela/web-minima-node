# web-minima-node

Web mínima en Node puro (sin dependencias, sin framework, sin Docker escrito a mano).
Muestra un fondo azul con el texto centrado **LEVANTADO SIN DOCKER**.

- **Disco**: solo `server.js` + `package.json` (~2 KB). No hay `node_modules`.
- **RAM**: ~40–60 MB en reposo (el mínimo del runtime de Node).
- Rutas: `/` (la web), `/health` (devuelve `ok` para healthchecks).

## Probar en local

```bash
npm start
# o con otro puerto:
PORT=8080 npm start
```

Abre http://localhost:3000

## Desplegar en Dokploy (build Nixpacks, sin Dockerfile)

### 1. Subir el código a un repositorio Git

Dokploy despliega desde Git. Crea un repo (GitHub / GitLab / Gitea) y sube esto:

```bash
git add .
git commit -m "web minima: LEVANTADO SIN DOCKER"
git branch -M main
git remote add origin <URL_DE_TU_REPO>
git push -u origin main
```

### 2. Crear la aplicación en Dokploy

1. **Projects → Create Project** (p. ej. `web-minima`).
2. Dentro del proyecto: **Create Service → Application**.
3. Pestaña **General → Provider**: elige GitHub (o Git genérico con la URL),
   selecciona el repositorio y la rama `main`.
4. **Build Type**: selecciona **Nixpacks**.
   Nixpacks detecta Node por el `package.json`, hace `npm install`
   (no instala nada porque no hay dependencias) y arranca con `npm start`.
   No necesitas escribir ningún Dockerfile.

### 3. Puerto y dominio

1. Pestaña **Environment**, añade:
   ```
   PORT=3000
   ```
2. Pestaña **Domains → Add Domain**:
   - Host: tu dominio o el subdominio que te dé Dokploy.
   - **Container Port**: `3000`.
   - Activa HTTPS (Let's Encrypt) si quieres certificado.

### 4. Desplegar

Pulsa **Deploy**. Cuando termine, abre el dominio y verás la web.

### 5. (Opcional) Healthcheck

En **Advanced → Health Check** puedes usar la ruta `/health`.

## Cambiar el color o el texto

Edita `server.js`:
- Color de fondo: la propiedad `background: #0047AB;` dentro de `<style>`.
- Texto: el contenido de `<main>...</main>`.

Haz commit y push; Dokploy redesplegará (o pulsa **Deploy** de nuevo).
