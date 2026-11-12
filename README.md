# TFG DAW - Proyecto de Fin de Grado

## Descripción

Proyecto de Trabajo de Final de Grado para el ciclo formativo de Desarrollo de Aplicaciones Web (DAW).

## Estado Actual

🚧 **Proyecto en fase inicial** - Configuración del entorno de desarrollo completada.

### Completado

- [x] Configuración del entorno de desarrollo multiagente
- [x] Definición de arquitectura y convenciones
- [x] Establecimiento de metodología de trabajo
- [x] Documentación inicial del proyecto

### Pendiente

- [ ] Definición de requisitos del proyecto
- [ ] Selección de stack tecnológico
- [ ] Diseño de arquitectura detallada
- [ ] Implementación de funcionalidades

## Requisitos del Sistema

### Para Desarrollo

- **Node.js**: v18+ (recomendado v20 LTS)
- **npm**: v9+ (incluido con Node.js)
- **Git**: v2.30+
- **OpenCode**: Versión actual con suscripción Go

### Para Producción

- Servidor web (Node.js, Nginx, o similar)
- Base de datos (PostgreSQL/MySQL/MongoDB según stack)
- HTTPS configurado
- Variables de entorno para configuración

## Instalación

```bash
# Clonar el repositorio
git clone [URL_DEL_REPOSITORIO]
cd TFG

# Instalar dependencias
npm install

# Configurar variables de entorno
cp .env.example .env
# Editar .env con tus valores

# Iniciar servidor de desarrollo
npm run dev
```

## Estructura del Proyecto

```
TFG/
├── opencode.jsonc          # Configuración de OpenCode
├── AGENTS.md               # Convenciones del equipo
├── README.md               # Este archivo
├── ARCHITECTURE.md         # Arquitectura y decisiones técnicas
├── TASKS.md                # Tareas pendientes
├── CHANGELOG.md            # Registro de cambios
├── src/                    # Código fuente
│   ├── backend/           # Lógica backend
│   ├── frontend/          # Lógica frontend
│   ├── database/          # Esquemas y migraciones
│   └── shared/            # Código compartido
├── tests/                 # Pruebas
├── docs/                  # Documentación adicional
└── scripts/               # Scripts de utilidad
```

## Equipo de Desarrollo

El proyecto utiliza una arquitectura multiagente con los siguientes roles:

- **@build**: Coordinador principal (Qwen3.7 Plus)
- **@architect**: Arquitecto de software (Qwen3.7 Plus)
- **@backend**: Desarrollador backend (Kimi K2.7 Code)
- **@frontend**: Desarrollador frontend (Kimi K2.7 Code)
- **@database**: Especialista en bases de datos (Qwen3.7 Plus)
- **@tester**: QA y testing (GLM-5.3-Flash)
- **@reviewer**: Revisor de código (Qwen3.7 Plus)

Consulta `AGENTS.md` para detalles sobre responsabilidades y convenciones.

## Scripts Disponibles

```bash
# Desarrollo
npm run dev              # Inicia servidor de desarrollo
npm start               # Inicia servidor de producción

# Testing
npm test                # Ejecuta todas las pruebas
npm run test:watch      # Ejecuta pruebas en modo watch
npm run test:coverage   # Genera reporte de cobertura

# Calidad de código
npm run lint            # Ejecuta linter
npm run lint:fix        # Corrige problemas de lint
npm run format          # Formatea código con Prettier

# Base de datos
npm run db:migrate      # Ejecuta migraciones
npm run db:seed         # Carga datos de prueba
npm run db:reset        # Reinicia base de datos

# Build
npm run build           # Genera build de producción
```

## Tecnologías

### Stack Tecnológico (Pendiente de Definir)

- **Backend**: Por definir
- **Frontend**: Por definir
- **Base de datos**: Por definir
- **Testing**: Por definir

Consulta `ARCHITECTURE.md` para detalles sobre decisiones técnicas.

## Documentación

- [AGENTS.md](AGENTS.md) - Convenciones y metodología del equipo
- [ARCHITECTURE.md](ARCHITECTURE.md) - Arquitectura y decisiones técnicas
- [TASKS.md](TASKS.md) - Tareas pendientes y progreso
- [CHANGELOG.md](CHANGELOG.md) - Historial de cambios

## Desarrollo

### Flujo de Trabajo

1. Revisa las tareas pendientes en `TASKS.md`
2. Selecciona una tarea según prioridad
3. Implementa siguiendo las convenciones de `AGENTS.md`
4. Escribe pruebas para tu implementación
5. Ejecuta `npm test` y `npm run lint`
6. Actualiza la documentación si es necesario
7. Actualiza `TASKS.md` y `CHANGELOG.md`

### Convenciones

Consulta `AGENTS.md` para:
- Estilo de código
- Gestión de errores
- Seguridad
- Testing
- Documentación

## Contribuciones

Este es un proyecto de TFG desarrollado por un único estudiante, pero utiliza un equipo multiagente de IA para asistencia en el desarrollo.

Para reportar problemas o sugerir mejoras:
1. Revisa las tareas pendientes en `TASKS.md`
2. Documenta el problema o mejora
3. Propón una solución si es posible

## Licencia

Por definir según requisitos del centro educativo.

## Contacto

- **Autor**: Diego Salinas Yañez
- **Email**: diegosy12@educastur.es
- **Centro**: CIFP La Laboral
- **Tutor**: Tever

---

**Nota**: Este proyecto está en desarrollo activo como parte del TFG de DAW.
