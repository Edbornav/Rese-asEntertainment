# Entertainment Reviews

**Alumno:** [TU NOMBRE] — **Matrícula:** [TU MATRÍCULA] — **Grupo:** [TU GRUPO]  
**Tema:** Plataforma de reseñas de entretenimiento con persistencia en MongoDB 7

Plataforma donde los usuarios pueden explorar un catálogo de videojuegos, películas, series, animes y música, escribir reseñas con calificaciones, y solicitar nuevos ítems. Los datos persisten en MongoDB Atlas y el backend está desarrollado en C# (.NET 8).

## Entidades y relación

| Entidad | Colección | Descripción |
|---|---|---|
| CatalogItem | `catalog_items` | Ítems del catálogo con título, categoría, portada, creador y descripción |
| Review | `reviews` | Reseñas de usuarios vinculadas a un ítem, con comentario y calificación (1-10) |

**Relación:** Referencia por ObjectId.  
**¿Por qué?** Las reseñas tienen ciclo de vida independiente (se crean y eliminan sin modificar el ítem), necesito consultarlas tanto por ítem como por usuario, y un ítem popular puede tener cientos de reseñas — embebidas harían el documento del ítem demasiado grande.

## Versión de MongoDB

MongoDB **7.0.[X]** — confirmado en Atlas (ver captura en `docs/`).

## Cómo correr el seed

El seed.js registra usuarios vía la API REST y luego inserta ítems y reseñas directamente en MongoDB.

```bash
# Instalar dependencias
npm install mongodb

# Configurar variables
$env:API_URL="http://localhost:5000/api"
$env:MONGO_URI="mongodb+srv://admin:Demo1234@cluster.mongodb.net/entertainment_reviews"

# Ejecutar
node seed.js
```

Los usuarios creados:
- **demo@demo.com** / **Demo1234** — rol: admin
- **user@test.com** / **User1234** — rol: user

## Stack tecnológico

| Capa | Tecnología |
|---|---|
| Base de datos | MongoDB 7 (Atlas M0) |
| Backend | C# (.NET 8) |
| Autenticación | JWT + BCrypt |
| ODM | MongoDB.Driver |
| Frontend | HTML, CSS y JavaScript vanilla (SPA) |
| Hosting | Render (API) + Vercel (frontend) |

## Estructura

```
ENTERTAINMENT_REVIEWS_FRONT/    → Frontend SPA (HTML/CSS/JS)
EntertainmentReviews.API/       → Backend .NET 8
seed.js                         → Script de seed
docs/                           → Capturas y script MongoDB
```

## Endpoints principales

| Método | Ruta | Auth | Descripción |
|---|---|---|---|
| POST | `/api/auth/register` | No | Registrar usuario |
| POST | `/api/auth/login` | No | Iniciar sesión |
| GET | `/api/catalog` | No | Listar catálogo |
| GET | `/api/catalog/{id}` | No | Detalle de ítem |
| POST | `/api/catalog` | Admin | Crear ítem |
| PUT | `/api/catalog/{id}` | Admin | Editar ítem |
| DELETE | `/api/catalog/{id}` | Admin | Eliminar ítem |
| GET | `/api/catalog/{id}/reviews` | No | Ver reseñas de un ítem |
| POST | `/api/catalog/{id}/reviews` | Sí | Crear reseña |
| PUT | `/api/reviews/{id}` | Sí | Editar reseña propia |
| DELETE | `/api/reviews/{id}` | Sí | Eliminar reseña (dueño o admin) |
| GET | `/api/reviews/user/{userId}` | Sí | Reseñas de un usuario |
