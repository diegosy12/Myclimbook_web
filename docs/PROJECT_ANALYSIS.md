# Myclimbook - Análisis del Proyecto Android y Plan de Migración Web

> **Nota 24-nov-2026:** este documento recoge el análisis inicial y una propuesta de
> migración a SQL. La decisión vigente para el TFG es **mantener Firebase** (Auth, Firestore
> y Storage) y añadir una API REST con Spring Boot como capa intermedia, para no trasladar
> datos ya publicados. La comparación de alternativas y la decisión adoptada están en
> [01-analisis.md](01-analisis.md), apartado 3.

## Resumen Ejecutivo

**Myclimbook (Climbook)** es una aplicación Android nativa desarrollada en Kotlin para la gestión de contenidos de escalada, tanto en rocódromos (spraywalls) como en sectores de escalada en roca. La aplicación utiliza Firebase como backend (Authentication, Firestore, Storage) y está construida con Jetpack Compose siguiendo arquitectura MVVM.

Este documento analiza la lógica de negocio, funcionalidades y arquitectura existente, y establece un plan para la migración a una aplicación web con dominio **myclimbook.com**.

---

## 1. Análisis del Proyecto Android

### 1.1 Arquitectura Actual

```
┌─────────────────────────────────────────────────────────────┐
│                      PRESENTACIÓN (UI)                       │
│  Jetpack Compose - Screens, Components, Navigation          │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                    LÓGICA DE NEGOCIO                          │
│  ViewModel - Gestión de estado, validaciones, coordinación  │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                      CAPA DE DATOS                            │
│  Repository Pattern - FirebaseClimbookRepository            │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                      FIREBASE                                 │
│  Authentication | Firestore | Storage                        │
└─────────────────────────────────────────────────────────────┘
```

**Tecnologías utilizadas:**
- **Lenguaje**: Kotlin
- **UI Framework**: Jetpack Compose
- **Arquitectura**: MVVM (Model-View-ViewModel)
- **Backend**: Firebase (Auth, Firestore, Storage)
- **Navegación**: Jetpack Navigation Compose
- **Corrutinas**: Para operaciones asíncronas

### 1.2 Modelos de Datos

#### Usuarios
```kotlin
SessionUser(
    id: String,
    username: String,
    email: String,
    isGuest: Boolean,
    isBanned: Boolean
)
```

#### Muros (Spraywalls)
```kotlin
Wall(
    id: String,
    ownerId: String,
    name: String,
    imageUrl: String,
    artwork: WallArtwork, // SUNSET, METAL, CANYON, NIGHT
    isPublic: Boolean,
    routeCount: Int,
    dateLabel: String,
    createdAtMillis: Long
)
```

#### Rutas en Muros
```kotlin
ClimbingRoute(
    id: String,
    wallId: String,
    ownerId: String,
    name: String,
    gradeV: String,        // Escala V (búlder americano)
    gradeFont: String,     // Escala Fontaine (búlder francés)
    averageRating: Double,
    ratingsCount: Int,
    markers: List<RouteMarker>,
    commentCount: Int,
    createdAtLabel: String,
    createdAtMillis: Long
)
```

#### Marcadores de Rutas
```kotlin
RouteMarker(
    x: Float,              // Coordenada X normalizada (0-1)
    y: Float,              // Coordenada Y normalizada (0-1)
    type: MarkerType,      // START, FOOT, HAND, TOP, BOLT, ANCHOR
    index: Int,
    sizeScale: Float
)
```

#### Sectores 3D (Sketchfab)
```kotlin
Sector3D(
    id: String,
    ownerId: String,
    name: String,
    description: String,
    sketchfabModelId: String,
    thumbnailUrl: String,
    authorName: String,
    dateLabel: String,
    isPublic: Boolean,
    createdAtMillis: Long
)
```

#### Sectores de Escalada en Roca
```kotlin
ClimbingSector(
    id: String,
    ownerId: String,
    name: String,
    description: String,
    imageUrl: String,
    locationName: String,
    mapsUrl: String,
    parkingName: String,
    parkingMapsUrl: String,
    routeCount: Int,
    boulderCount: Int,
    isPublic: Boolean,
    createdAtMillis: Long
)
```

#### Vías Deportivas
```kotlin
SportRoute(
    id: String,
    sectorId: String,
    ownerId: String,
    name: String,
    description: String,
    imageUrl: String,
    grade: String,
    heightMeters: Double,
    boltCount: Int,
    anchorType: AnchorType, // NONE, CHAIN, RINGS, CARABINER, OTHER
    markers: List<RouteMarker>,
    averageRating: Double,
    ratingsCount: Int,
    commentCount: Int,
    createdAtMillis: Long
)
```

#### Problemas de Búlder
```kotlin
BoulderProblem(
    id: String,
    sectorId: String,
    ownerId: String,
    name: String,
    description: String,
    imageUrl: String,
    grade: String,
    heightMeters: Double,
    fallType: FallType,    // SAFE, PADDED, DANGEROUS, UNKNOWN
    markers: List<RouteMarker>,
    averageRating: Double,
    ratingsCount: Int,
    commentCount: Int,
    createdAtMillis: Long
)
```

#### Comentarios
```kotlin
Comment(
    id: String,
    routeId: String,
    userId: String,
    username: String,
    text: String,
    dateLabel: String,
    createdAtMillis: Long,
    contentType: String,   // "spray_route", "sport_route", "boulder"
    contentId: String
)
```

#### Reportes de Contenido
```kotlin
ContentReport(
    id: String,
    contentType: String,
    contentId: String,
    contentTitle: String,
    reportedOwnerId: String,
    reporterUserId: String,
    reporterUsername: String,
    reporterEmail: String,
    reason: String,
    details: String,
    createdAtMillis: Long,
    status: String         // "pending"
)
```

### 1.3 Funcionalidades Principales

#### Autenticación y Usuarios
- ✅ Registro con verificación de email
- ✅ Login con email/password
- ✅ Modo invitado (acceso limitado)
- ✅ Recuperación de contraseña
- ✅ Validación de nombre de usuario único
- ✅ Cambio de nombre de usuario
- ✅ Sistema de banning

#### Gestión de Muros (Spraywalls)
- ✅ Crear muros con imagen y estilo artístico
- ✅ Listar muros públicos y propios
- ✅ Búsqueda de muros por nombre
- ✅ Paginación de resultados
- ✅ Detalle de muro con sus rutas
- ✅ Eliminar muros propios

#### Gestión de Rutas en Muros
- ✅ Crear rutas con marcadores interactivos
- ✅ Tipos de marcadores: inicio, pies, manos, top, chapas, reunión
- ✅ Sistema de grados (V-scale y Fontaine)
- ✅ Valoración con estrellas (1-5)
- ✅ Comentarios en rutas
- ✅ Marcar rutas como completadas
- ✅ Eliminar rutas propias

#### Sectores 3D (Sketchfab)
- ✅ Crear sectores con modelos 3D de Sketchfab
- ✅ Visualización de modelos embebidos
- ✅ Extracción automática de thumbnails
- ✅ Listar sectores públicos y propios

#### Sectores de Escalada en Roca
- ✅ Crear sectores con ubicación y fotos
- ✅ Información de parking y mapas
- ✅ Gestión de vías deportivas
- ✅ Gestión de problemas de búlder
- ✅ Contadores automáticos de rutas/búlders

#### Vías Deportivas
- ✅ Crear vías con imagen y detalles técnicos
- ✅ Grado, altura, número de chapas
- ✅ Tipo de reunión (cadenas, anillas, mosquetón, otro)
- ✅ Marcadores en imagen (inicio, chapas, reunión)
- ✅ Valoraciones y comentarios
- ✅ Marcar como completadas

#### Problemas de Búlder
- ✅ Crear problemas con imagen y detalles
- ✅ Grado, altura, tipo de caída
- ✅ Marcadores en imagen
- ✅ Valoraciones y comentarios
- ✅ Marcar como completados

#### Interacción Social
- ✅ Sistema de favoritos (muros y rutas)
- ✅ Comentarios en contenidos
- ✅ Valoraciones con promedio automático
- ✅ Reporte de contenido inapropiado

#### Perfil y Progreso
- ✅ Ver muros creados
- ✅ Ver rutas completadas
- ✅ Nivel máximo alcanzado (grado más difícil)
- ✅ Estadísticas de progreso

#### Configuración
- ✅ Cambio de idioma (ES/EN)
- ✅ Cerrar sesión
- ✅ Enlaces a términos y privacidad

### 1.4 Validaciones de Entrada

El sistema implementa validaciones robustas:

- **Email**: Regex estándar, solo caracteres ASCII
- **Contraseña**: Mínimo 6 caracteres, solo ASCII imprimible
- **Nombre de usuario**: 3-30 caracteres, letras/números/guiones/puntos
- **Nombres**: Sin enlaces, sin HTML, longitud máxima
- **Comentarios**: Sin enlaces, longitud máxima
- **Textos generales**: Sin HTML, sin enlaces, caracteres permitidos
- **Sketchfab**: Validación de ID de 32 caracteres o URL válida

### 1.5 Estructura de Base de Datos (Firestore)

```
users/
  {userId}/
    username: String
    usernameLowercase: String
    email: String
    favoriteWallIds: List<String>
    favoriteRouteIds: List<String>
    completedRouteIds: List<String>
    completedClimbingIds: List<String>
    banned: Boolean

usernames/
  {usernameLowercase}/
    userId: String
    username: String

walls/
  {wallId}/
    ownerId, name, nameLowercase, imageUrl, artwork,
    isPublic, routeCount, dateLabel, createdAtMillis

sectors3d/
  {sectorId}/
    ownerId, name, nameLowercase, description,
    sketchfabModelId, thumbnailUrl, authorName,
    isPublic, dateLabel, createdAtMillis

routes/
  {routeId}/
    wallId, ownerId, name, gradeV, gradeFont,
    averageRating, ratingsCount, markers, commentCount,
    createdAtLabel, createdAtMillis
    ratings/
      {userId}/ stars: Int

climbingSectors/
  {sectorId}/
    ownerId, name, description, imageUrl, locationName,
    mapsUrl, parkingName, parkingMapsUrl, routeCount,
    boulderCount, isPublic, createdAtMillis

sportRoutes/
  {routeId}/
    sectorId, ownerId, name, description, imageUrl,
    grade, heightMeters, boltCount, anchorType, markers,
    averageRating, ratingsCount, commentCount, createdAtMillis
    ratings/
      {userId}/ stars: Int

boulders/
  {boulderId}/
    sectorId, ownerId, name, description, imageUrl,
    grade, heightMeters, fallType, markers,
    averageRating, ratingsCount, commentCount, createdAtMillis
    ratings/
      {userId}/ stars: Int

comments/
  {commentId}/
    routeId, contentType, contentId, userId, username,
    text, dateLabel, createdAtMillis

reports/
  {reportId}/
    contentType, contentId, contentTitle, reportedOwnerId,
    reporterUserId, reporterUsername, reporterEmail,
    reason, details, createdAtMillis, status
```

---

## 2. Plan de Migración a Web

### 2.1 Objetivos

1. **Mantener toda la funcionalidad** de la aplicación Android
2. **Mejorar la experiencia** con características web (responsive, accesible)
3. **Optimizar para SEO** y descubrimiento
4. **Facilitar el mantenimiento** con código más estructurado
5. **Preparar para escalabilidad** futura

### 2.2 Restricciones del TFG

- ✅ Backend: **Spring Boot con Java**
- ✅ Frontend: **JavaScript con AJAX** (a discutir con tutor)
- ✅ Dominio: **myclimbook.com** (ya registrado)

### 2.3 Stack Tecnológico Propuesto

#### Backend (Spring Boot)
- **Framework**: Spring Boot 3.x
- **Lenguaje**: Java 17+
- **Seguridad**: Spring Security + JWT
- **ORM**: Spring Data JPA con Hibernate
- **Base de datos**: MySQL 8 o PostgreSQL 14
- **Storage**: Local (sistema de archivos) para TFG
- **Validación**: Jakarta Validation
- **Documentación API**: SpringDoc OpenAPI (Swagger)

**Nota sobre almacenamiento de imágenes:**
- En Firebase actual: Firebase Storage (servicio separado)
- En Web propuesta: Sistema de archivos local del servidor
- Las imágenes NO van en la base de datos (ni ahora ni en la migración)
- Alternativas futuras: Cloudinary, AWS S3 (si se necesita escalar)

#### Frontend (JavaScript)
- **Lenguaje**: JavaScript ES6+ o TypeScript
- **Framework**: Por definir (React, Vue, o vanilla JS)
- **HTTP Client**: Fetch API o Axios
- **CSS**: CSS3 con Flexbox/Grid, o Tailwind CSS
- **Mapas**: Leaflet o Google Maps API
- **3D**: Sketchfab Viewer API

#### Infraestructura
- **Servidor**: Por definir (Heroku, Railway, VPS)
- **Base de datos**: MySQL/PostgreSQL managed
- **Storage**: Cloud storage para imágenes
- **CDN**: Para assets estáticos
- **SSL**: Let's Encrypt o similar

## 2.5 Opción A: Frontend Tradicional (Recomendada para TFG)

**JavaScript + AJAX + JSP/Thymeleaf**

Ventajas:
- ✅ Cumple requisitos del TFG
- ✅ Más sencillo de implementar
- ✅ Mejor integración con Spring Boot
- ✅ Menos complejidad en el deployment

Desventajas:
- ❌ Menos moderno
- ❌ Más código repetitivo
- ❌ Experiencia de usuario menos fluida

### 2.6 Opción B: Frontend Moderno (SPA)

**React/Vue + API REST**

Ventajas:
- ✅ Experiencia de usuario superior
- ✅ Código más mantenible
- ✅ Mejor rendimiento
- ✅ Más moderno y profesional

Desventajas:
- ❌ Más complejo
- ❌ Requiere discutir con tutor
- ❌ Deployment separado

### 2.7 Recomendación

**Opción A con mejoras**: Usar Thymeleaf + JavaScript + AJAX para mantener la simplicidad y cumplir requisitos, pero con un diseño responsive moderno y componentes reutilizables.

---

## 2.8 Migración de Firebase a SQL

### Situación actual (Firebase)

El proyecto Android usa **3 servicios de Firebase**:

1. **Firebase Authentication**
   - Gestión de usuarios (login/registro)
   - Verificación de email
   - Sesiones

2. **Firebase Firestore** (NoSQL)
   - Base de datos de documentos JSON
   - Sin esquema fijo
   - Consultas limitadas

3. **Firebase Storage**
   - Almacenamiento de imágenes
   - URLs de descarga
   - Control de acceso

### Situación propuesta (Web)

1. **Spring Security + JWT**
   - Autenticación basada en tokens
   - Gestión de sesiones
   - Roles y permisos

2. **MySQL/PostgreSQL** (SQL)
   - Base de datos relacional
   - Esquema definido con tablas
   - Relaciones entre entidades
   - Consultas complejas (JOINs)

3. **Almacenamiento de imágenes**
   - **Opción 1**: Sistema de archivos local (recomendado para TFG)
   - **Opción 2**: Cloudinary (servicio cloud, gratis hasta 25GB)
   - **Opción 3**: AWS S3 (profesional, coste variable)

### ¿Por qué cambiar a SQL?

1. **Spring Boot está diseñado para SQL**
   - JPA/Hibernate son ORM para bases de datos relacionales
   - Estándar en desarrollo web Java
   - Más fácil de testear y mantener

2. **Requisito académico**
   - Los TFG de DAW suelen usar bases de datos relacionales
   - Demuestra conocimiento de SQL (esencial en el mercado laboral)

3. **Mejor para los datos del proyecto**
   - Los datos tienen relaciones claras (usuario → muros → rutas)
   - Necesitas consultas complejas (ej: "muros públicos con más de 5 rutas")
   - Integridad referencial (eliminar usuario elimina sus muros)

### Diferencias clave: NoSQL vs SQL

```
FIRESTORE (NoSQL):
{
  "id": "abc123",
  "name": "Muro Escuela",
  "routes": ["ruta1", "ruta2"]  ← Lista embebida
}

MySQL/PostgreSQL (SQL):
users        │  walls         │  routes
─────────────│────────────────│────────────
id (PK)      │  id (PK)       │  id (PK)
username     │  owner_id (FK) │  wall_id (FK)
email        │  name          │  name
password     │  image_url     │  grade
```

### Ventajas de SQL para este proyecto

- ✅ Relaciones fuertes entre entidades
- ✅ Integridad referencial (CASCADE)
- ✅ Consultas complejas (JOINs, agregaciones)
- ✅ Transacciones ACID
- ✅ Índices para optimización
- ✅ Estándar de la industria

---

## 3. Arquitectura Propuesta

### 3.1 Backend (Spring Boot)

```
┌─────────────────────────────────────────────────────────────┐
│                    PRESENTACIÓN (Web)                         │
│  Thymeleaf + JavaScript + AJAX                              │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                    CONTROLADORES                               │
│  @RestController - Endpoints REST API                       │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                    SERVICIOS                                   │
│  @Service - Lógica de negocio, validaciones                 │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                    REPOSITORIOS                                │
│  @Repository - Acceso a datos (Spring Data JPA)             │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                    BASE DE DATOS                               │
│  MySQL / PostgreSQL                                          │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 Estructura de Paquete (Backend)

```
com.salinas.myclimbook/
├── config/
│   ├── SecurityConfig.java
│   ├── JwtConfig.java
│   ├── WebConfig.java
│   └── StorageConfig.java
├── controller/
│   ├── AuthController.java
│   ├── WallController.java
│   ├── RouteController.java
│   ├── ClimbingSectorController.java
│   ├── SportRouteController.java
│   ├── BoulderController.java
│   ├── UserController.java
│   └── ReportController.java
├── service/
│   ├── AuthService.java
│   ├── UserService.java
│   ├── WallService.java
│   ├── RouteService.java
│   ├── ClimbingSectorService.java
│   ├── SportRouteService.java
│   ├── BoulderService.java
│   ├── CommentService.java
│   ├── RatingService.java
│   ├── ReportService.java
│   └── StorageService.java
├── repository/
│   ├── UserRepository.java
│   ├── WallRepository.java
│   ├── RouteRepository.java
│   ├── ClimbingSectorRepository.java
│   ├── SportRouteRepository.java
│   ├── BoulderRepository.java
│   ├── CommentRepository.java
│   ├── RatingRepository.java
│   └── ReportRepository.java
├── model/
│   ├── entity/
│   │   ├── User.java
│   │   ├── Wall.java
│   │   ├── ClimbingRoute.java
│   │   ├── RouteMarker.java
│   │   ├── Sector3D.java
│   │   ├── ClimbingSector.java
│   │   ├── SportRoute.java
│   │   ├── BoulderProblem.java
│   │   ├── Comment.java
│   │   ├── Rating.java
│   │   └── ContentReport.java
│   ├── dto/
│   │   ├── UserDTO.java
│   │   ├── WallDTO.java
│   │   ├── RouteDTO.java
│   │   └── ...
│   └── enums/
│       ├── MarkerType.java
│       ├── WallArtwork.java
│       ├── AnchorType.java
│       └── FallType.java
├── security/
│   ├── JwtTokenProvider.java
│   ├── JwtAuthenticationFilter.java
│   ├── UserDetailsServiceImpl.java
│   └── SecurityUtils.java
├── exception/
│   ├── GlobalExceptionHandler.java
│   ├── ResourceNotFoundException.java
│   ├── UnauthorizedException.java
│   └── ValidationException.java
├── util/
│   ├── InputValidation.java
│   └── DateUtils.java
└── MyclimbookApplication.java
```

### 3.3 Frontend (JavaScript + Thymeleaf)

```
src/main/resources/
├── templates/
│   ├── layout/
│   │   └── main-layout.html
│   ├── auth/
│   │   ├── login.html
│   │   ├── register.html
│   │   └── forgot-password.html
│   ├── walls/
│   │   ├── walls-list.html
│   │   ├── wall-detail.html
│   │   └── wall-form.html
│   ├── routes/
│   │   ├── route-detail.html
│   │   └── route-form.html
│   ├── sectors/
│   │   ├── climbing-sectors.html
│   │   ├── sector-detail.html
│   │   └── sector-form.html
│   ├── sport-routes/
│   │   ├── sport-route-detail.html
│   │   └── sport-route-form.html
│   ├── boulders/
│   │   ├── boulder-detail.html
│   │   └── boulder-form.html
│   ├── profile/
│   │   ├── profile.html
│   │   └── settings.html
│   └── legal/
│       ├── terms.html
│       └── privacy.html
├── static/
│   ├── css/
│   │   ├── main.css
│   │   ├── components.css
│   │   └── responsive.css
│   ├── js/
│   │   ├── api.js              # Cliente API
│   │   ├── auth.js             # Autenticación
│   │   ├── walls.js            # Gestión de muros
│   │   ├── routes.js           # Gestión de rutas
│   │   ├── sectors.js          # Gestión de sectores
│   │   ├── markers.js          # Editor de marcadores
│   │   ├── comments.js         # Sistema de comentarios
│   │   ├── ratings.js          # Sistema de valoraciones
│   │   ├── favorites.js        # Sistema de favoritos
│   │   └── utils.js            # Utilidades
│   └── images/
│       └── ...
```

---

## 4. Esquema de Base de Datos Relacional

### 4.1 Tablas Principales

```sql
-- Usuarios
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    uuid VARCHAR(36) UNIQUE NOT NULL,
    username VARCHAR(30) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    is_banned BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Muros (Spraywalls)
CREATE TABLE walls (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    uuid VARCHAR(36) UNIQUE NOT NULL,
    owner_id BIGINT NOT NULL,
    name VARCHAR(100) NOT NULL,
    image_url VARCHAR(500),
    artwork ENUM('SUNSET', 'METAL', 'CANYON', 'NIGHT') NOT NULL,
    is_public BOOLEAN DEFAULT TRUE,
    route_count INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Rutas en muros
CREATE TABLE climbing_routes (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    uuid VARCHAR(36) UNIQUE NOT NULL,
    wall_id BIGINT NOT NULL,
    owner_id BIGINT NOT NULL,
    name VARCHAR(100) NOT NULL,
    grade_v VARCHAR(10),
    grade_font VARCHAR(10),
    average_rating DECIMAL(2,1) DEFAULT 0.0,
    ratings_count INT DEFAULT 0,
    comment_count INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (wall_id) REFERENCES walls(id) ON DELETE CASCADE,
    FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Marcadores de rutas
CREATE TABLE route_markers (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    route_id BIGINT NOT NULL,
    route_type ENUM('WALL', 'SPORT', 'BOULDER') NOT NULL,
    x FLOAT NOT NULL,
    y FLOAT NOT NULL,
    marker_type ENUM('START', 'FOOT', 'HAND', 'TOP', 'BOLT', 'ANCHOR') NOT NULL,
    marker_index INT NOT NULL,
    size_scale FLOAT DEFAULT 1.0,
    FOREIGN KEY (route_id) REFERENCES climbing_routes(id) ON DELETE CASCADE
);

-- Sectores 3D
CREATE TABLE sectors_3d (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    uuid VARCHAR(36) UNIQUE NOT NULL,
    owner_id BIGINT NOT NULL,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    sketchfab_model_id VARCHAR(32) NOT NULL,
    thumbnail_url VARCHAR(500),
    author_name VARCHAR(100),
    is_public BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Sectores de escalada en roca
CREATE TABLE climbing_sectors (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    uuid VARCHAR(36) UNIQUE NOT NULL,
    owner_id BIGINT NOT NULL,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    image_url VARCHAR(500),
    location_name VARCHAR(200) NOT NULL,
    maps_url VARCHAR(500),
    parking_name VARCHAR(200) NOT NULL,
    parking_maps_url VARCHAR(500),
    route_count INT DEFAULT 0,
    boulder_count INT DEFAULT 0,
    is_public BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Vías deportivas
CREATE TABLE sport_routes (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    uuid VARCHAR(36) UNIQUE NOT NULL,
    sector_id BIGINT NOT NULL,
    owner_id BIGINT NOT NULL,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    image_url VARCHAR(500),
    grade VARCHAR(10) NOT NULL,
    height_meters DECIMAL(5,1) NOT NULL,
    bolt_count INT NOT NULL,
    anchor_type ENUM('NONE', 'CHAIN', 'RINGS', 'CARABINER', 'OTHER') NOT NULL,
    average_rating DECIMAL(2,1) DEFAULT 0.0,
    ratings_count INT DEFAULT 0,
    comment_count INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (sector_id) REFERENCES climbing_sectors(id) ON DELETE CASCADE,
    FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Problemas de búlder
CREATE TABLE boulder_problems (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    uuid VARCHAR(36) UNIQUE NOT NULL,
    sector_id BIGINT NOT NULL,
    owner_id BIGINT NOT NULL,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    image_url VARCHAR(500),
    grade VARCHAR(10) NOT NULL,
    height_meters DECIMAL(5,1) NOT NULL,
    fall_type ENUM('SAFE', 'PADDED', 'DANGEROUS', 'UNKNOWN') NOT NULL,
    average_rating DECIMAL(2,1) DEFAULT 0.0,
    ratings_count INT DEFAULT 0,
    comment_count INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (sector_id) REFERENCES climbing_sectors(id) ON DELETE CASCADE,
    FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Comentarios (polimórfico)
CREATE TABLE comments (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    content_type ENUM('WALL_ROUTE', 'SPORT_ROUTE', 'BOULDER') NOT NULL,
    content_id BIGINT NOT NULL,
    text TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Valoraciones (polimórfico)
CREATE TABLE ratings (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    content_type ENUM('WALL_ROUTE', 'SPORT_ROUTE', 'BOULDER') NOT NULL,
    content_id BIGINT NOT NULL,
    stars INT NOT NULL CHECK (stars BETWEEN 1 AND 5),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE KEY (user_id, content_type, content_id),
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Favoritos de usuario
CREATE TABLE user_favorites (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    content_type ENUM('WALL', 'WALL_ROUTE', 'SPORT_ROUTE', 'BOULDER') NOT NULL,
    content_id BIGINT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE KEY (user_id, content_type, content_id),
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Progreso (contenidos completados)
CREATE TABLE user_progress (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    content_type ENUM('WALL_ROUTE', 'SPORT_ROUTE', 'BOULDER') NOT NULL,
    content_id BIGINT NOT NULL,
    completed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE KEY (user_id, content_type, content_id),
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Reportes de contenido
CREATE TABLE content_reports (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    content_type ENUM('WALL', 'SECTOR_3D', 'CLIMBING_SECTOR', 'SPORT_ROUTE', 'BOULDER') NOT NULL,
    content_id BIGINT NOT NULL,
    content_title VARCHAR(200),
    reported_owner_id BIGINT NOT NULL,
    reporter_user_id BIGINT NOT NULL,
    reason VARCHAR(200) NOT NULL,
    details TEXT,
    status ENUM('PENDING', 'REVIEWED', 'RESOLVED') DEFAULT 'PENDING',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (reported_owner_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (reporter_user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

---

## 5. Plan de Implementación

### Fase 1: Configuración Inicial (Semana 1-2)

#### Tarea 1.1: Proyecto Spring Boot
- [ ] Crear proyecto con Spring Initializr
- [ ] Configurar dependencias (Spring Web, Security, JPA, MySQL, Validation)
- [ ] Configurar application.properties
- [ ] Crear estructura de paquetes

#### Tarea 1.2: Base de Datos
- [ ] Instalar MySQL/PostgreSQL
- [ ] Crear esquema de base de datos
- [ ] Configurar JPA/Hibernate
- [ ] Crear migraciones (opcional: Flyway/Liquibase)

#### Tarea 1.3: Seguridad Básica
- [ ] Configurar Spring Security
- [ ] Implementar JWT token provider
- [ ] Crear filtro de autenticación
- [ ] Configurar endpoints públicos/privados

### Fase 2: Autenticación y Usuarios (Semana 3-4)

#### Tarea 2.1: Backend
- [ ] Entity User
- [ ] Repository UserRepository
- [ ] Service AuthService
- [ ] Controller AuthController
- [ ] Endpoints: register, login, logout, forgot-password

#### Tarea 2.2: Frontend
- [ ] Página de login
- [ ] Página de registro
- [ ] Página de recuperación de contraseña
- [ ] JavaScript para autenticación
- [ ] Gestión de tokens JWT

### Fase 3: Muros y Rutas (Semana 5-7)

#### Tarea 3.1: Backend
- [ ] Entities: Wall, ClimbingRoute, RouteMarker
- [ ] Repositories
- [ ] Services: WallService, RouteService
- [ ] Controllers
- [ ] Endpoints CRUD completos

#### Tarea 3.2: Frontend
- [ ] Lista de muros con búsqueda
- [ ] Detalle de muro
- [ ] Formulario de creación de muro
- [ ] Editor de marcadores (interactivo)
- [ ] Detalle de ruta
- [ ] Sistema de comentarios y valoraciones

### Fase 4: Sectores de Escalada (Semana 8-10)

#### Tarea 4.1: Backend
- [ ] Entities: ClimbingSector, SportRoute, BoulderProblem
- [ ] Repositories
- [ ] Services
- [ ] Controllers
- [ ] Endpoints CRUD

#### Tarea 4.2: Frontend
- [ ] Lista de sectores
- [ ] Detalle de sector con mapa
- [ ] Formulario de creación
- [ ] Gestión de vías deportivas
- [ ] Gestión de problemas de búlder

### Fase 5: Funcionalidades Adicionales (Semana 11-12)

#### Tarea 5.1: Perfil y Progreso
- [ ] Página de perfil
- [ ] Estadísticas de progreso
- [ ] Sistema de favoritos
- [ ] Configuración de usuario

#### Tarea 5.2: Sectores 3D
- [ ] Integración con Sketchfab
- [ ] Visualización de modelos 3D
- [ ] Gestión de sectores 3D

#### Tarea 5.3: Reportes
- [ ] Sistema de reportes
- [ ] Panel de administración (opcional)

### Fase 6: Testing y Despliegue (Semana 13-14)

#### Tarea 6.1: Testing
- [ ] Tests unitarios (JUnit)
- [ ] Tests de integración
- [ ] Tests de seguridad
- [ ] Testing manual completo

#### Tarea 6.2: Despliegue
- [ ] Configurar servidor
- [ ] Desplegar aplicación
- [ ] Configurar dominio myclimbook.com
- [ ] Configurar SSL
- [ ] Testing en producción

---

## 6. Mejoras Respecto a la Versión Android

### 6.1 Mejoras Planificadas

1. **Responsive Design**: Adaptación a todos los dispositivos
2. **SEO**: Optimización para buscadores
3. **Accesibilidad**: WCAG 2.1 AA
4. **Performance**: Lazy loading, caching, optimización de imágenes
5. **Offline**: Service Workers para funcionalidad offline
6. **Notificaciones**: Sistema de notificaciones web
7. **Compartir**: Integración con redes sociales
8. **Estadísticas**: Dashboard con métricas avanzadas

### 6.2 Nuevas Funcionalidades (Opcionales)

1. **Mapa interactivo**: Mapa global con todos los sectores
2. **Sistema de logros**: Badges y logros por actividad
3. **Seguir usuarios**: Sistema de followers
4. **Feed de actividad**: Timeline con actividad reciente
5. **Chat**: Mensajería entre usuarios
6. **Eventos**: Calendario de eventos de escalada

---

## 7. Consideraciones de Seguridad

### 7.1 Autenticación y Autorización

- ✅ JWT tokens con expiración
- ✅ Refresh tokens para sesiones largas
- ✅ Validación de permisos en cada endpoint
- ✅ Protección contra CSRF
- ✅ Rate limiting

### 7.2 Validación de Datos

- ✅ Validación en backend (Jakarta Validation)
- ✅ Validación en frontend (JavaScript)
- ✅ Sanitización de entradas
- ✅ Protección contra XSS
- ✅ Protección contra SQL Injection (JPA)

### 7.3 Almacenamiento Seguro

- ✅ Password hashing (BCrypt)
- ✅ Almacenamiento seguro de imágenes
- ✅ Variables de entorno para secretos
- ✅ HTTPS obligatorio en producción

---

## 8. Estimación de Esfuerzo

### Total: ~14 semanas (3-4 meses)

- **Fase 1**: 2 semanas
- **Fase 2**: 2 semanas
- **Fase 3**: 3 semanas
- **Fase 4**: 3 semanas
- **Fase 5**: 2 semanas
- **Fase 6**: 2 semanas

### Horas estimadas: 200-300 horas

---

## 9. Riesgos y Mitigación

### Riesgo 1: Complejidad del editor de marcadores
**Mitigación**: Usar librerías JavaScript existentes (Canvas/SVG)

### Riesgo 2: Rendimiento con muchas imágenes
**Mitigación**: CDN, lazy loading, optimización de imágenes

### Riesgo 3: Migración de datos desde Firebase
**Mitigación**: Script de migración, testing exhaustivo

### Riesgo 4: Tiempo limitado
**Mitigación**: Priorizar funcionalidades core, dejar opcionales para después

---

## 10. Conclusión

La migración de Myclimbook de Android a web es totalmente viable y aportará múltiples beneficios:

- ✅ Mayor accesibilidad (no requiere instalación)
- ✅ Mejor SEO y descubrimiento
- ✅ Más fácil de mantener y actualizar
- ✅ Preparado para escalabilidad
- ✅ Cumple requisitos del TFG

### Migración de Firebase a SQL

**Firebase (actual):**
- Firebase Auth → Usuarios
- Firestore (NoSQL) → Datos en documentos JSON
- Firebase Storage → Imágenes

**Web propuesta:**
- Spring Security + JWT → Autenticación
- MySQL/PostgreSQL (SQL) → Datos en tablas relacionales
- Sistema de archivos local → Imágenes

**¿Por qué SQL?**
- Spring Boot está diseñado para SQL (JPA/Hibernate)
- Estándar en TFG de DAW y mercado laboral
- Relaciones claras entre datos (usuario → muros → rutas)
- Consultas complejas (JOINs, agregaciones)

**Importante:** Las imágenes NO van en la base de datos, ni en Firebase ni en SQL. Se guardan en almacenamiento separado (Firebase Storage ahora, sistema de archivos en web).

El plan propuesto es realista y alcanzable en el tiempo disponible, priorizando las funcionalidades core y dejando mejoras opcionales para fases posteriores.

---

**Próximo paso**: Revisar este plan con el tutor del TFG y ajustar según sus indicaciones.
