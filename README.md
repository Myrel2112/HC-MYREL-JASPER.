# Dashboard de Headcount — Operaciones Farmatodo

Panel web para calcular ideal, ocupadas, vacantes y sobregiro de headcount, cruzando tus archivos de Excel. Todo el procesamiento ocurre en el navegador (no hay servidor, no hay base de datos) — es un único archivo `index.html`.

## Formato esperado de los archivos

El sistema intenta reconocer tus columnas automáticamente por nombre (no importa mayúsculas ni acentos). Si no reconoce alguna, te la pedirá manualmente al procesar — así que puedes usar tus archivos tal cual los tienes hoy, y ajustar la primera vez.

### 1. Estructura ideal (obligatorio)
Una fila por combinación de tienda + cargo.

| Columna | Alias reconocidos | Obligatorio |
|---|---|---|
| Región | region, región | Sí |
| Área | area, área | Sí |
| Tienda | tienda, nombre tienda, cod tienda, codigo tienda | Sí |
| Cargo | cargo, posicion, posición, puesto | Sí |
| Cantidad ideal | cantidad ideal, ideal, hc ideal, headcount ideal, cantidad | Sí |
| Tipo de tienda | tipo tienda, tipo de tienda, clasificacion tienda | No (Expansión / Crecimiento / Mismas tiendas) |
| Es zafra | es zafra, zafra, temporal, es temporal | No (Sí/No — marca posiciones temporales) |
| Fecha inicio zafra | fecha inicio zafra, inicio zafra, fecha inicio | No |
| Fecha fin zafra | fecha fin zafra, fin zafra, fecha fin | No |

Si una posición está marcada como zafra, solo cuenta como "ideal" cuando la fecha de hoy cae entre fecha inicio y fecha fin. Fuera de ese rango, no aparece en los totales.

### 2. Nómina real (obligatorio)
Una fila por empleado.

| Columna | Alias reconocidos | Obligatorio |
|---|---|---|
| Región | region, región | Sí |
| Área | area, área | Sí |
| Tienda | tienda, nombre tienda, cod tienda, codigo tienda | Sí |
| Cargo | cargo, posicion, posición, puesto | Sí |
| Nombre | nombre, empleado, nombre empleado | No |

### 3. Farmacéuticos por tienda (opcional)
Si lo subes, sus conteos **reemplazan** el conteo de "Farmacéutico" que saldría de la nómina real (útil cuando llevas ese archivo aparte y más al día). Los cargos `Farmacéutico Adjunto`, `Farmacéutico Adjunto Temp` y `Regente de Farmacia` siempre se agrupan y muestran juntos bajo el cargo "Farmacéutico", tanto si vienen de este archivo como de la nómina real.

| Columna | Alias reconocidos |
|---|---|
| Tienda | tienda, nombre tienda, cod tienda, codigo tienda |
| Farmacéutico Adjunto | farmaceutico adjunto |
| Farmacéutico Adjunto Temp | farmaceutico adjunto temp |
| Regente de Farmacia | regente de farmacia, regente |

## Privacidad

Los archivos nunca salen de tu navegador — no hay backend ni base de datos. Aun así, como manejas nombres de empleados, este repositorio debe mantenerse **privado** en GitHub, y el sitio en Vercel debe configurarse con protección de acceso (ver más abajo) para que no quede público en internet.

---

## Cómo publicarlo (GitHub + Vercel)

### Paso 1: Crear el repositorio en GitHub (privado)

1. Entra a [github.com](https://github.com) y crea un repositorio nuevo (botón "New repository").
2. Ponle un nombre, por ejemplo `dashboard-headcount-operaciones`.
3. **Marca la opción "Private"** — esto es importante porque el archivo maneja nombres de empleados.
4. No agregues README ni .gitignore desde GitHub (ya los tienes aquí).
5. Crea el repositorio.

### Paso 2: Subir el código

Desde la carpeta donde tienes estos archivos (`index.html`, `README.md`), abre una terminal y ejecuta:

```bash
git init
git add .
git commit -m "Dashboard de headcount de operaciones"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/dashboard-headcount-operaciones.git
git push -u origin main
```

Reemplaza `TU-USUARIO` por tu usuario de GitHub. Si te pide iniciar sesión, sigue las instrucciones que te muestre GitHub (puede pedirte un token de acceso en vez de tu contraseña).

### Paso 3: Publicarlo en Vercel

1. Entra a [vercel.com](https://vercel.com) e inicia sesión (puedes hacerlo con tu cuenta de GitHub).
2. Haz clic en "Add New..." → "Project".
3. Autoriza a Vercel a ver tus repositorios privados de GitHub si te lo pide.
4. Busca y selecciona el repositorio `dashboard-headcount-operaciones`.
5. Vercel detecta que es un sitio estático (no necesitas configurar "Build Command" ni "Output Directory" — déjalo vacío o por defecto).
6. Haz clic en "Deploy".
7. En 1-2 minutos tendrás un link como `https://dashboard-headcount-operaciones.vercel.app`.

### Paso 4: Proteger el acceso (recomendado)

Como el repositorio es privado pero el link de Vercel, una vez publicado, es accesible por cualquiera que lo tenga, te recomiendo activar una contraseña de acceso:

1. En el proyecto dentro de Vercel, ve a **Settings → Deployment Protection**.
2. Activa **"Password Protection"** (disponible en planes Pro; si estás en el plan gratuito, esta opción no está disponible y el link, aunque no se comparta, queda accesible para quien lo adivine — en ese caso, evita compartir el link y considera esto para una fase futura si vas a manejar datos más sensibles).

### Cada vez que actualices tus Excel

No necesitas volver a desplegar nada: el archivo `index.html` no cambia. Solo entra al sitio y sube los 3 Excel actualizados de nuevo — el cálculo se rehace en tu navegador al instante. El dashboard recuerda los últimos datos procesados en tu propio computador (usando el almacenamiento del navegador), así que no tienes que resubir los archivos cada vez que abres la página, solo cuando quieras actualizar los números.
