# Changelog

Todos los cambios importantes en este proyecto serán documentados en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/),
y este proyecto sigue [Semantic Versioning](https://semver.org/lang/es/).

## [Unreleased]

### Próximos cambios

- Revisión del plan con tutor del TFG
- Inicialización del proyecto Spring Boot
- Configuración de base de datos Firebase Admin SDK
- Implementación de autenticación

### Añadido

- **ROADMAP.md** - Planificación de funcionalidades futuras:
  - Exportación offline de sectores y spraywalls (para zonas sin cobertura)
  - Sistema de suscripción premium con restricciones no abusivas
  - Sistema de anuncios (por decidir)
  - Algoritmo de verificación de imágenes con IA/ML
  - Automatización y revisión periódica de contenido
  - Otras ideas: mapa interactivo, sistema de seguidores, logros, etc.

---

## [0.1.0] - 2026-03-14

### Añadido

- **Configuración inicial del entorno de desarrollo multiagente**
  - Configuración de OpenCode con 7 agentes especializados
  - Modelos asignados: Qwen3.7 Plus (principal), Kimi K2.7 Code (complejo), GLM-5.3-Flash (económico)
  - Agentes creados: @build, @architect, @backend, @frontend, @database, @tester, @reviewer
  - Archivo: `opencode.jsonc`

- **Documentación base del proyecto**
  - `AGENTS.md`: Convenciones, metodología y guía del equipo multiagente
  - `README.md`: Documentación inicial del proyecto
  - `ARCHITECTURE.md`: Plantilla de arquitectura y decisiones técnicas
  - `TASKS.md`: Sistema de seguimiento de tareas
  - `CHANGELOG.md`: Este archivo

- **Definición de metodología de trabajo**
  - Metodología incremental y profesional
  - Criterios de delegación de tareas
  - Convenciones de código
  - Estrategias de testing y calidad
  - Políticas de seguridad

- **Definición de convenciones de código**
  - Principios de código limpio
  - Estructura de directorios
  - Estilo de código (indentación, longitud de línea, nombres)
  - Gestión de errores
  - Seguridad desde el diseño

- **Análisis completo del proyecto Android Climbook**
  - Análisis de arquitectura MVVM con Kotlin/Jetpack Compose
  - Documentación de 14 modelos de datos principales
  - Identificación de 14 funcionalidades core
  - Análisis de estructura de base de datos Firebase
  - Mapeo completo de validaciones de entrada
  - Archivo: `docs/PROJECT_ANALYSIS.md`

- **Plan de migración a aplicación web**
  - Definición de arquitectura backend (Spring Boot)
  - Definición de arquitectura frontend (JavaScript + AJAX)
  - Diseño de esquema de base de datos relacional (MySQL/PostgreSQL)
  - Estructura de paquetes Java detallada
  - Estructura de frontend con Thymeleaf
  - Plan de implementación en 6 fases (14 semanas)
  - Estimación de esfuerzo: 200-300 horas
  - Identificación de riesgos y mitigaciones
  - Archivo: `docs/PROJECT_ANALYSIS.md`

### Cambiado

- N/A (versión inicial)

### Corregido

- N/A (versión inicial)

### Eliminado

- N/A (versión inicial)

---

## Tipos de Cambios

- **Añadido**: Nuevas funcionalidades
- **Cambiado**: Cambios en funcionalidades existentes
- **Deprecado**: Funcionalidades que serán eliminadas próximamente
- **Eliminado**: Funcionalidades eliminadas
- **Corregido**: Correcciones de errores
- **Seguridad**: Cambios relacionados con seguridad

---

## Historial de Versiones

### Formato de Versión

- **MAJOR**: Cambios incompatibles con versiones anteriores
- **MINOR**: Nuevas funcionalidades compatibles
- **PATCH**: Correcciones de errores compatibles

### Ejemplo

```
[1.0.0] - 2026-05-15
### Añadido
- Funcionalidad X
- Funcionalidad Y

### Cambiado
- Comportamiento de Z

### Corregido
- Error en W
```

---

## Notas

- Este archivo se actualiza con cada cambio importante
- Los cambios se agrupan por versión y fecha
- Consulta TASKS.md para ver el progreso actual
- Consulta AGENTS.md para convenciones del equipo

---

**Última actualización**: 2026-03-14 (Análisis y plan de migración completados)
