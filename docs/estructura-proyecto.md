# Estructura del proyecto

## Estructura de carpetas

```
proyecto-daw/
│
├── README.md                    # Descripción general del proyecto
├── .gitignore                   # Archivos ignorados por Git
├── LICENSE                      # Licencia del proyecto
│
├── docs/                        # Documentación
│   ├── README.md               # Índice de documentación
│   ├── registro-tutoria.md     # Registro semanal de tutoría
│   ├── declaracion-ia.md       # Declaración de uso de IA
│   ├── metodologia.md          # Metodología de trabajo
│   ├── analisis/               # Documentos de análisis
│   ├── diseno/                 # Documentos de diseño
│   ├── planificacion/          # Planificación del proyecto
│   ├── pruebas/                # Plan y resultados de pruebas
│   ├── despliegue/             # Manual de despliegue
│   └── memoria/                # Memoria final
│
├── src/                         # Código fuente
│   ├── backend/                # Lado del servidor
│   │   ├── controllers/        # Controladores
│   │   ├── models/             # Modelos de datos
│   │   ├── services/           # Lógica de negocio
│   │   ├── routes/             # Rutas de la API
│   │   └── middleware/         # Middleware
│   │
│   ├── frontend/               # Lado del cliente
│   │   ├── components/         # Componentes reutilizables
│   │   ├── pages/              # Páginas/vistas
│   │   ├── assets/             # Imágenes, fuentes, etc.
│   │   └── styles/             # Estilos CSS/SCSS
│   │
│   └── shared/                 # Código compartido (tipos, utilidades)
│
├── tests/                       # Pruebas automáticas
│   ├── unit/                   # Pruebas unitarias
│   ├── integration/            # Pruebas de integración
│   └── e2e/                    # Pruebas end-to-end
│
├── database/                    # Scripts de base de datos
│   ├── migrations/             # Migraciones
│   ├── seeds/                  # Datos de prueba
│   └── schema.sql             # Esquema de la BD
│
├── deploy/                      # Configuración de despliegue
│   ├── docker/                 # Dockerfiles
│   ├── scripts/                # Scripts de despliegue
│   └── config/                 # Configuración por entorno
│
└── .github/                     # Configuración de GitHub
    ├── workflows/              # GitHub Actions (CI/CD)
    ├── ISSUE_TEMPLATE/         # Plantillas de issues
    └── PULL_REQUEST_TEMPLATE.md # Plantilla de PRs
```

## Convenciones de nombres

- **Carpetas**: minúsculas, separadas por guiones
- **Archivos**: minúsculas, separadas por guiones
- **Variables y funciones**: camelCase
- **Clases y componentes**: PascalCase
- **Constantes**: UPPER_SNAKE_CASE
- **Archivos de base de datos**: snake_case
