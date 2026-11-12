# Roadmap - Myclimbook Web

> **Propósito.** Este documento recoge las funcionalidades futuras planificadas para Myclimbook.
> No forman parte de la primera versión mínima (MVP) del TFG, pero son ampliaciones muy útiles
> que se pueden ir implementando progresivamente después del MVP o en fases avanzadas del TFG
> si el tiempo lo permite.
>
> **Prioridades:**
> - 🔴 Alta - Muy útil, impacta mucho en la experiencia del usuario
> - 🟡 Media - Útil, aporta valor pero no es crítico
> - 🟢 Baja - Nice-to-have, se hará si hay tiempo
>
> **Estados:**
> - 💭 Idea - Solo propuesta, por definir viabilidad y alcance
> - 📋 Planificada - Definida y lista para implementar cuando toque
> - 🚧 En progreso - Siendo implementada
> - ✅ Completada - En producción

---

## 1. Funcionalidad Offline / Exportación de Datos

**Estado:** 💭 Idea  
**Prioridad:** 🔴 Alta

### Descripción
Permitir a los usuarios exportar/descargar los datos de los sectores de escalada o spraywalls
a los que vayan a ir, para poder consultarlos sin conexión a internet.

### Problema que resuelve
Muchos sectores de escalada en roca están en zonas sin cobertura móvil. Los usuarios necesitan
poder consultar la información del sector (vías, grados, descripción, ubicación, parking) cuando
ya están en la pared, sin depender de conexión.

### Posible implementación
- Exportar a PDF con información del sector y mapas de las vías
- Exportar a JSON para reimportar en la app
- Caché inteligente en Service Worker (PWA)
- Descarga de imágenes en baja resolución para consulta offline
- Sincronización al volver a tener conexión

### Complejidad estimada
Media-Alta (especialmente la parte offline con Service Workers y sincronización)

---

## 2. Sistema de Suscripción Premium

**Estado:** 💭 Idea  
**Prioridad:** 🔴 Alta

### Descripción
Implementar un sistema de suscripción (freemium) con restricciones no abusivas para usuarios
gratuitos. El objetivo es monetizar el proyecto manteniendo una buena experiencia para todos.

### Posibles restricciones para usuarios gratuitos
- Límite en el número de muros/sectores privados
- Marca de agua sutil en las imágenes exportadas
- Límite en exportaciones offline por mes
- Acceso básico a estadísticas
- Anuncios no intrusivos (ver punto 3)

### Beneficios para usuarios premium
- Contenido sin anuncios
- Exportaciones ilimitadas
- Estadísticas avanzadas
- Sectores/muros privados ilimitados
- Acceso anticipado a nuevas funcionalidades

### Posible implementación
- Integración con Stripe para pagos
- Niveles de suscripción (gratuito, premium, pro)
- Gestión de cuotas y límites en backend
- Panel de administración para gestionar suscripciones

### Complejidad estimada
Alta (integración con pasarela de pagos, gestión de cuotas, facturación)

---

## 3. Sistema de Publicidad / Anuncios

**Estado:** 💭 Idea (pendiente de decidir)  
**Prioridad:** 🟡 Media

### Descripción
Implementar un sistema de anuncios para monetizar la aplicación. La decisión sobre si
implementarlo y cómo depende del modelo de negocio final y del feedback de los usuarios.

### Consideraciones importantes
- **No abusivos**: Los anuncios nunca deben romper la experiencia de usuario
- **Contexto**: Solo anuncios relacionados con escalada (material, viajes, competiciones)
- **Usuarios premium**: Sin anuncios
- **Ubicaciones posibles**: Sidebar, entre listados, en página de detalle
- **Proveedores**: Google AdSense, redes especializadas en deportes

### Posibles proveedores
- Google AdSense (fácil de integrar)
- Redes de anuncios de deportes de montaña
- Patrocinios directos con marcas del sector

### Complejidad estimada
Baja-Media (integración técnica sencilla, pero requiere diseño cuidadoso de UX)

---

## 4. Algoritmo de Verificación de Imágenes

**Estado:** 💭 Idea  
**Prioridad:** 🔴 Alta

### Descripción
Diseñar e implementar un algoritmo (posiblemente con IA/ML) que verifique automáticamente
si una imagen subida es realmente un sector de escalada, spraywall o vía, para evitar:
- Contenido basura o spam
- Imágenes inapropiadas
- Imágenes fuera de contexto (paisajes, memes, etc.)
- Contenido duplicado

### Problema que resuelve
La moderación manual de imágenes es costosa y lenta. Un algoritmo automático puede filtrar
el contenido obvio antes de que llegue a la base de datos, reduciendo la carga de moderación.

### Posible implementación
- **Opción A: APIs externas**
  - Google Cloud Vision API
  - AWS Rekognition
  - Azure Computer Vision
  - Clarifai (especializado en deporte)

- **Opción B: Modelo propio**
  - Red neuronal entrenada con dataset propio de imágenes de escalada
  - Clasificación: escalada / no escalada / dudoso
  - Detección de objetos: presas, cuerdas, arneses, roca

- **Opción C: Híbrida**
  - API externa para detección inicial
  - Modelo propio para casos dudosos
  - Cola de moderación manual para casos límite

### Flujo propuesto
```
Usuario sube imagen
      ↓
Validación básica (formato, tamaño, dimensiones)
      ↓
Algoritmo de verificación (IA/ML)
      ↓
   ┌──────┴──────┐
   │             │
OK (score > 0.8)   Dudo (0.3-0.8)   Rechazo (< 0.3)
   │             │              │
   ↓             ↓              ↓
Publicar      Cola moderación   Rechazar + notificar
              manual
```

### Complejidad estimada
Alta (requiere entrenamiento de modelo o integración con APIs de pago)

---

## 5. Automatización y Revisión Periódica de Contenido

**Estado:** 💭 Idea  
**Prioridad:** 🟡 Media

### Descripción
Sistema automatizado que revise periódicamente el contenido de la aplicación para detectar:
- Contenido obsoleto o desactualizado
- Contenido duplicado
- Patrones de spam o abuso
- Contenido reportado múltiples veces
- Cuentas inactivas durante mucho tiempo

### Posibles automatizaciones

#### Revisión de contenido reportado
- Si un contenido recibe más de X reportes en Y días → ocultar automáticamente
- Notificar al moderador para revisión manual
- Sistema de "strikes" para usuarios reincidentes

#### Detección de patrones sospechosos
- Usuario que crea muchos contenidos en poco tiempo
- Múltiples cuentas desde la misma IP
- Comentarios con patrones de spam (enlaces, texto repetitivo)

#### Limpieza automática
- Eliminar imágenes huérfanas (sin contenido asociado)
- Archivar contenido antiguo sin interacción
- Notificar a usuarios de contenido que podría estar desactualizado

#### Combinación con sistema de reportes actual
- Dashboard de moderación con métricas
- Sistema de prioridades basado en gravedad de reportes
- Historial de acciones de moderación
- Estadísticas de contenido reportado/resuelto

### Posible implementación
- **Cron jobs** en Spring Boot para tareas periódicas
- **Cola de mensajes** para procesamiento asíncrono (Redis/RabbitMQ)
- **Panel de administración** para moderadores
- **Reglas configurables** (no hardcodeadas)

### Complejidad estimada
Media-Alta (depende del alcance, pero es muy escalable incrementalmente)

---

## 6. Otras Ideas (Por Definir)

Estas son ideas sueltas que pueden convertirse en funcionalidades completas si tienen sentido:

- 📍 **Mapa interactivo global**: Ver todos los sectores de escalada en un mapa mundial
- 👥 **Sistema de seguidores**: Seguir a otros escaladores y ver su actividad
- 📰 **Feed de actividad**: Timeline con actividad reciente de seguidos
- 🏆 **Sistema de logros**: Badges y logros por actividad (primer muro, 10 rutas, etc.)
- 💬 **Chat entre usuarios**: Mensajería interna para coordinar salidas
- 📅 **Eventos de escalada**: Calendario de competiciones, quedadas, etc.
- 📊 **Estadísticas avanzadas**: Gráficos de progreso, comparativas, predicciones
- 🎨 **Editor de rutas avanzado**: Más tipos de marcadores, colores personalizados
- 🌐 **Multi-idioma**: Soporte completo para inglés, francés, italiano, etc.
- 📱 **App móvil nativa**: Mantener la app Android conectada con la web

---

## Priorización para el TFG

### MVP (antes 31-mar-2027)
1. ✅ Autenticación
2. ✅ Gestión de muros con rutas
3. ✅ Sectores de escalada con vías y búlders
4. ✅ Comentarios y valoraciones
5. ✅ Seguimiento de progreso

### Post-MVP (si da tiempo durante el TFG)
1. 🔴 Sistema de verificación de imágenes (muy útil, buen contenido técnico para el TFG)
2. 🟡 Automatización y revisión de contenido (complementa la verificación)

### Post-TFG (después de la defensa)
1. 🔴 Funcionalidad offline / exportación
2. 🔴 Sistema de suscripción premium
3. 🟡 Sistema de anuncios
4. 🟢 Otras ideas

---

## Notas

- Este documento es vivo, se actualizará conforme se definan mejor las funcionalidades
- Las prioridades pueden cambiar según feedback de usuarios y del tutor
- Algunas ideas pueden combinarse (ej: verificación + automatización)
- El orden de implementación post-MVP depende del tiempo disponible y del interés técnico

---

**Última actualización:** 2026-11-06
