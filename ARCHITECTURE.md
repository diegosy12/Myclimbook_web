# Arquitectura del Proyecto

## Visión General

Este documento describe la arquitectura del proyecto TFG de DAW, las decisiones técnicas tomadas y el razonamiento detrás de ellas.

## Estado Actual

🚧 **En desarrollo** - La arquitectura se definirá completamente después de establecer los requisitos del proyecto.

## Principios Arquitectónicos

### 1. Separación de Responsabilidades

- **Backend**: Lógica de negocio, APIs, autenticación, autorización
- **Frontend**: Interfaz de usuario, experiencia de usuario, presentación
- **Base de datos**: Persistencia de datos, integridad, consultas
- **Shared**: Código reutilizable entre frontend y backend

### 2. Modularidad

- Módulos independientes y cohesivos
- Bajo acoplamiento entre componentes
- Interfaces bien definidas
- Fácil mantenimiento y testing

### 3. Escalabilidad

- Arquitectura que permite crecimiento
- Diseño que soporta aumento de carga
- Separación clara de preocupaciones
- Fácil adición de nuevas funcionalidades

### 4. Seguridad

- Autenticación y autorización robustas
- Validación de entradas en todos los niveles
- Protección contra vulnerabilidades comunes
- Gestión segura de secretos y configuración

### 5. Mantenibilidad

- Código limpio y bien documentado
- Convenciones claras y consistentes
- Testing exhaustivo
- Documentación actualizada

## Decisiones Técnicas Pendientes

### Stack Tecnológico

Las siguientes decisiones deben tomarse después de definir los requisitos:

#### Backend
- [ ] Framework (Express.js, NestJS, Fastify, etc.)
- [ ] Lenguaje (JavaScript, TypeScript)
- [ ] ORM/Query builder (Prisma, TypeORM, Knex, etc.)
- [ ] Autenticación (JWT, sesiones, OAuth, etc.)

#### Frontend
- [ ] Framework (React, Vue, Angular, Svelte, etc.)
- [ ] Estado global (Redux, Zustand, Vuex, etc.)
- [ ] Estilos (CSS Modules, Tailwind, Styled Components, etc.)
- [ ] Router (React Router, Vue Router, etc.)

#### Base de Datos
- [ ] Tipo (Relacional vs NoSQL)
- [ ] Motor (PostgreSQL, MySQL, MongoDB, etc.)
- [ ] Migraciones (Herramienta y estrategia)

#### Testing
- [ ] Framework de testing (Jest, Vitest, Mocha, etc.)
- [ ] Testing E2E (Playwright, Cypress, etc.)
- [ ] Cobertura objetivo

### Arquitectura de Aplicación

#### Patrón de Arquitectura
- [ ] MVC (Model-View-Controller)
- [ ] Clean Architecture
- [ ] Hexagonal Architecture
- [ ] Feature-based
- [ ] Otro

#### Comunicación
- [ ] REST API
- [ ] GraphQL
- [ ] WebSocket (si aplica)
- [ ] gRPC (si aplica)

#### Despliegue
- [ ] Plataforma (Vercel, Netlify, AWS, etc.)
- [ ] Contenedores (Docker)
- [ ] CI/CD (GitHub Actions, etc.)

## Diagramas de Arquitectura

### Arquitectura General (Pendiente)

```
┌─────────────────────────────────────────────────────────────┐
│                         FRONTEND                             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Views   │  │Components│  │  State   │  │  Router  │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                          API                                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Routes  │  │Controllers│ │ Services │  │Middleware│   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                      DATABASE                                │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Models  │  │Migrations│  │  Seeds   │  │  Queries │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### Flujo de Datos (Pendiente)

```
Usuario → Frontend → API → Servicios → Base de Datos
  ↑                                      ↓
  └──────────── Respuesta ───────────────┘
```

## Estructura de Directorios (Propuesta)

```
src/
├── backend/
│   ├── controllers/      # Controladores de rutas
│   ├── services/         # Lógica de negocio
│   ├── models/           # Modelos de datos
│   ├── middleware/        # Middleware (auth, validación, etc.)
│   ├── utils/            # Utilidades
│   └── types/            # Tipos TypeScript
├── frontend/
│   ├── components/       # Componentes reutilizables
│   ├── pages/           # Páginas/vistas
│   ├── hooks/           # Hooks personalizados
│   ├── context/         # Contexto global
│   ├── utils/           # Utilidades
│   └── types/           # Tipos TypeScript
├── database/
│   ├── migrations/      # Migraciones de BD
│   ├── seeds/           # Datos de prueba
│   └── schemas/         # Esquemas de BD
└── shared/
    ├── types/           # Tipos compartidos
    ├── utils/           # Utilidades compartidas
    └── constants/       # Constantes compartidas
```

## Patrones de Diseño

### Patrones a Utilizar (Pendiente)

- **Repository Pattern**: Para acceso a datos
- **Service Layer**: Para lógica de negocio
- **DTO Pattern**: Para transferencia de datos
- **Factory Pattern**: Para creación de objetos
- **Observer Pattern**: Para eventos y notificaciones
- **Strategy Pattern**: Para algoritmos intercambiables

## Seguridad

### Estrategias de Seguridad (Pendiente)

- **Autenticación**: JWT/Sesiones con refresh tokens
- **Autorización**: RBAC (Role-Based Access Control)
- **Validación**: Validación en backend y frontend
- **Protección CSRF**: Tokens CSRF para formularios
- **Protección XSS**: Sanitización de inputs
- **Protección SQL Injection**: Uso de ORM/queries parametrizadas
- **Rate Limiting**: Limitación de requests
- **HTTPS**: Obligatorio en producción

## Rendimiento

### Estrategias de Optimización (Pendiente)

- **Caching**: Redis o similar para datos frecuentes
- **Lazy Loading**: Carga diferida de componentes
- **Code Splitting**: División de bundles
- **Image Optimization**: Compresión y formatos modernos
- **Database Indexing**: Índices en consultas frecuentes
- **Pagination**: Paginación de listas grandes

## Testing

### Estrategia de Testing (Pendiente)

- **Unit Tests**: 70%+ de cobertura
- **Integration Tests**: Flujos críticos
- **E2E Tests**: Flujos principales de usuario
- **Security Tests**: Auditoría de vulnerabilidades
- **Performance Tests**: Pruebas de carga

## Despliegue

### Estrategia de Despliegue (Pendiente)

- **Entornos**: Development, Staging, Production
- **CI/CD**: Pipeline automatizado
- **Rollback**: Estrategia de reversión
- **Monitoring**: Logs y métricas
- **Backups**: Estrategia de copias de seguridad

## Documentación Adicional

### Documentación a Crear

- [ ] Diagramas de secuencia para flujos principales
- [ ] Diagrama ER de base de datos
- [ ] Documentación de API (Swagger/OpenAPI)
- [ ] Guía de despliegue
- [ ] Manual de usuario

## Decisiones Registradas

### ADR (Architecture Decision Records)

#### ADR-001: Configuración Multiagente
- **Fecha**: 2026-03-14
- **Estado**: Aceptada
- **Contexto**: Necesidad de organizar el desarrollo con equipo multiagente
- **Decisión**: Usar 7 agentes especializados con modelos diferenciados
- **Consecuencias**: Mejor especialización, uso eficiente de recursos

#### ADR-002: Estructura de Documentación
- **Fecha**: 2026-03-14
- **Estado**: Aceptada
- **Contexto**: Necesidad de documentar proyecto de forma clara
- **Decisión**: Usar archivos markdown estándar (AGENTS.md, README.md, ARCHITECTURE.md, etc.)
- **Consecuencias**: Documentación consistente y mantenible

---

## Funcionalidades Futuras

Consulta el archivo `ROADMAP.md` para ver las funcionalidades planificadas a futuro:
- Exportación offline de sectores y spraywalls
- Sistema de suscripción premium
- Sistema de anuncios (por decidir)
- Algoritmo de verificación de imágenes con IA/ML
- Automatización y revisión periódica de contenido
- Y otras ideas por definir

---

**Nota**: Este documento evoluciona con el proyecto. Actualízalo conforme se tomen decisiones arquitectónicas.
