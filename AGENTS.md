# AGENTS.md (estructura del curso, semana 1)

Reglas de este proyecto para cualquier agente de OpenCode. Versión: 0.1.0 (adaptada el 24-nov-2026).

## Proyecto

**Myclimbook Web**: evolución a aplicación web de la app móvil Myclimbook para gestionar y compartir contenidos de escalada en rocódromos (spraywalls) y sectores de roca.

## Reglas del curso

- Responde **en español**, con frases cortas y sin jerga innecesaria.
- **Un paso cada vez.** No escribas apartados completos de `docs/` sin que la persona lo confirme.
- **No califiques.** La nota es del profesor.
- **No inventes** datos, cifras, empresas, normas ni enlaces. Si no puedes comprobar algo, dilo.
- Todo dato externo lleva **enlace, fecha de acceso (AAAA-MM-DD) y estado** (`leído` / `sin leer`).
- El repositorio es **público**: nada de claves, contraseñas ni datos personales.
- No ejecutes `git push`, `git reset` ni borrados: los ejecuta la persona.
- Diagramas en **Mermaid**, no imágenes.

## Dónde está cada cosa

- `docs/00-documentoInicio.md`, `docs/01-analisis.md`, `docs/01b-sota.md`: documentación del proyecto.
- `.opencode/`: agentes, skills y comandos, con versión (ver `.opencode/README.md`, `MANIFIESTO.md` y `CHANGELOG.md`).
- `opencode.jsonc`: permisos, modelo y agente por defecto.
- `asistenciaIA/`: bitácora del trabajo de los agentes, esquema de agentes de cada momento, documentación y diagramas. **Todo uso de IA se anota ahí.**
- `docs/registro-tutoria.md` y `docs/publicacion-y-privacidad.md` se añadieron en la semana 2.

---

# Apéndice: convenciones anteriores del proyecto (conservadas)

# TFG DAW - Instrucciones del Equipo

## Visión General

Este documento establece las convenciones, arquitectura y metodología de trabajo para el desarrollo del Trabajo de Final de Grado (TFG) de Desarrollo de Aplicaciones Web (DAW).

## Equipo Multiagente

El proyecto utiliza una arquitectura multiagente con los siguientes roles especializados:

### @build (Coordinador Principal)
- **Modelo**: Qwen3.7 Plus
- **Responsabilidades**: Coordinación general, análisis de requisitos, delegación de tareas, supervisión de calidad
- **Cuándo usarlo**: Tareas generales, coordinación, decisiones arquitectónicas

### @architect (Arquitecto de Software)
- **Modelo**: Qwen3.7 Plus
- **Responsabilidades**: Diseño de arquitectura, patrones de diseño, convenciones técnicas, evaluación tecnológica
- **Cuándo usarlo**: Decisiones arquitectónicas importantes, diseño de sistemas, evaluación de tecnologías

### @backend (Desarrollador Backend)
- **Modelo**: Kimi K2.7 Code (modelo potente para código complejo)
- **Responsabilidades**: Implementación de lógica de negocio, APIs, servicios, autenticación, bases de datos
- **Cuándo usarlo**: Desarrollo de funcionalidades backend complejas, implementación de APIs

### @frontend (Desarrollador Frontend)
- **Modelo**: Kimi K2.7 Code (modelo potente para código complejo)
- **Responsabilidades**: Implementación de interfaces, componentes reutilizables, gestión de estados, UX
- **Cuándo usarlo**: Desarrollo de funcionalidades frontend, implementación de UI/UX

### @database (Especialista en Bases de Datos)
- **Modelo**: Qwen3.7 Plus
- **Responsabilidades**: Diseño de modelos de datos, optimización de consultas, migraciones, integridad de datos
- **Cuándo usarlo**: Diseño de esquemas, optimización de consultas, problemas de datos

### @tester (QA y Testing)
- **Modelo**: GLM-5.3-Flash (modelo económico para tareas repetitivas)
- **Responsabilidades**: Diseño de pruebas unitarias e integración, detección de casos límite, verificación de requisitos
- **Cuándo usarlo**: Creación de tests, verificación de funcionalidades, QA

### @reviewer (Revisor de Código)
- **Modelo**: Qwen3.7 Plus
- **Responsabilidades**: Revisión de código, detección de errores y vulnerabilidades, verificación de buenas prácticas
- **Cuándo usarlo**: Revisiones de código, auditorías de seguridad, verificación de calidad
- **Nota**: NO modifica código, solo reporta problemas

## Metodología de Trabajo

### Para cada funcionalidad:

1. **Análisis**: Comprender requisitos y contexto
2. **Diseño**: Definir solución antes de implementar
3. **Delegación**: Asignar a agentes especializados si es apropiado
4. **Implementación**: Desarrollar código siguiendo convenciones
5. **Testing**: Ejecutar pruebas y verificar funcionalidad
6. **Revisión**: Comprobar calidad y cumplimiento de arquitectura
7. **Documentación**: Actualizar documentos relevantes

### Criterios de Delegación

**Delega cuando:**
- La tarea requiere especialización específica
- El trabajo es independiente y puede ejecutarse en paralelo
- Se necesita un modelo diferente al principal

**No delegues cuando:**
- La tarea es simple y directa
- Requiere contexto extenso que ya está en la conversación actual
- El overhead de delegación supera el beneficio

## Convenciones de Código

### Principios Obligatorios

- **Código limpio**: Legible, nombres descriptivos, comentarios solo cuando aportan valor
- **Responsabilidad única**: Cada función/clase tiene una sola responsabilidad
- **Bajo acoplamiento**: Minimiza dependencias entre módulos
- **Alta cohesión**: Elementos relacionados están juntos
- **Sin duplicación**: Evita código duplicado (DRY - Don't Repeat Yourself)
- **Modularidad**: Divide el código en módulos coherentes
- **Gestión de errores**: Maneja todos los errores adecuadamente
- **Validación**: Valida todas las entradas
- **Seguridad**: Prioriza la seguridad desde el diseño

### Estructura de Archivos

```
TFG/
├── opencode.jsonc          # Configuración de OpenCode
├── AGENTS.md               # Este archivo
├── README.md               # Documentación del proyecto
├── ARCHITECTURE.md         # Arquitectura y decisiones técnicas
├── TASKS.md                # Lista de tareas pendientes
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

### Estilo de Código

- **Indentación**: 2 espacios (configurar en editor)
- **Longitud de línea**: Máximo 100 caracteres
- **Nombres**: Descriptivos y en inglés para código, español para documentación
- **Comentarios**: Solo cuando aportan valor, explican el "por qué" no el "qué"
- **Funciones**: Máximo 50 líneas (recomendado)
- **Archivos**: Responsabilidad única, máximo 300 líneas (recomendado)

## Gestión de Errores

### Jerarquía de Manejo

1. **Prevención**: Validar entradas, usar tipos adecuados
2. **Detección**: Try-catch en operaciones críticas
3. **Recuperación**: Intentar recuperar cuando sea posible
4. **Logging**: Registrar errores con contexto suficiente
5. **Usuario**: Mostrar mensajes claros y útiles

### Niveles de Logging

- **ERROR**: Fallos críticos que impiden continuar
- **WARN**: Problemas que no impiden continuar pero son importantes
- **INFO**: Eventos relevantes del sistema
- **DEBUG**: Información detallada para depuración

## Seguridad

### Reglas Obligatorias

- **NUNCA** incluir credenciales en el código
- Usar variables de entorno para secretos
- Validar y sanitizar todas las entradas
- Implementar autenticación y autorización adecuadas
- Proteger contra inyecciones (SQL, XSS, etc.)
- Usar HTTPS en producción
- Actualizar dependencias regularmente

### Autenticación y Autorización

- Implementar JWT o sesiones seguras
- Validar tokens en cada request protegido
- Implementar roles y permisos granulares
- Proteger rutas sensibles
- Implementar rate limiting

## Testing

### Tipos de Pruebas

1. **Unitarias**: Funciones individuales, aisladas
2. **Integración**: Interacción entre componentes
3. **E2E**: Flujos completos de usuario
4. **Seguridad**: Pruebas de vulnerabilidades comunes

### Cobertura Objetivo

- **Mínimo**: 70% de cobertura
- **Objetivo**: 80% de cobertura
- **Crítico**: 90%+ en lógica de negocio y seguridad

## Documentación

### Archivos Principales

- **README.md**: Visión general, instalación, uso
- **ARCHITECTURE.md**: Arquitectura, decisiones técnicas, diagramas
- **AGENTS.md**: Este archivo, convenciones del equipo
- **TASKS.md**: Tareas pendientes y progreso
- **CHANGELOG.md**: Historial de cambios importantes

### Cuándo Documentar

- Decisiones arquitectónicas importantes
- APIs públicas y interfaces
- Funciones complejas o no obvias
- Decisiones de diseño
- Cambios importantes en el proyecto

## Control de Calidad

### Checklist antes de Considerar Terminada una Funcionalidad

- [ ] Código implementado siguiendo convenciones
- [ ] Pruebas unitarias escritas y pasando
- [ ] Pruebas de integración (si aplica) pasando
- [ ] Código revisado (manualmente o con @reviewer)
- [ ] Documentación actualizada
- [ ] Sin errores de linting
- [ ] Funcionalidad verificada manualmente
- [ ] Casos límite considerados
- [ ] Seguridad revisada
- [ ] Rendimiento aceptable

## Gestión de Tareas

### Prioridades

- **P0 (Crítica)**: Bloquea el proyecto, resolver inmediatamente
- **P1 (Alta)**: Importante, resolver pronto
- **P2 (Media)**: Necesaria, pero no urgente
- **P3 (Baja)**: Deseable, cuando haya tiempo

### Flujo de Tareas

1. **Backlog**: Tareas identificadas
2. **To Do**: Tareas listas para empezar
3. **In Progress**: Tareas en desarrollo
4. **Review**: Tareas listas para revisar
5. **Done**: Tareas completadas y verificadas

## Comunicación

### Reportes de Tarea

Al completar una tarea, reporta:

1. **Qué se implementó**: Resumen claro
2. **Archivos modificados**: Lista de archivos
3. **Decisiones técnicas**: Explicación breve de decisiones importantes
4. **Pruebas ejecutadas**: Qué se probó y resultados
5. **Problemas pendientes**: Si queda algo por resolver

### Cuándo Consultar

Consulta antes de:
- Cambios arquitectónicos importantes
- Decisiones que afectan el alcance del proyecto
- Cambios de tecnología o dependencias
- Decisiones con implicaciones de seguridad
- Cualquier decisión con consecuencias significativas

## Recursos

### Documentación de Referencia

- OpenCode: https://opencode.ai/v2/docs/
- Node.js: https://nodejs.org/docs/
- React (si aplica): https://react.dev/
- TypeScript: https://www.typescriptlang.org/docs/

### Herramientas

- **Editor**: VSCode recomendado
- **Terminal**: PowerShell (Windows)
- **Control de versiones**: Git
- **Gestor de paquetes**: npm
- **Testing**: Jest/Vitest (configurar según stack)

---

**Nota final**: Este documento evoluciona con el proyecto. Actualízalo cuando se identifiquen mejores prácticas o cambios necesarios.
