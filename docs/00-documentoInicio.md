# 00 · Documento de inicio del proyecto

> **Nota.** Documento completado el 6-nov-2026. Afinar hasta el 12-nov para aprobación del tutor.
>
> **No escribir datos personales sensibles** (teléfono, dirección, DNI) ni correo personal.

## 1. Datos del alumno/a

| | |
|---|---|
| **Nombre y apellidos** | Diego Salinas |
| **Usuario de GitHub** | *pendiente - completar* |
| **Repositorio del proyecto** | *pendiente - crear repositorio* |
| **Ciclo y curso** | DAW, 2.º · curso 2026-27 |
| **Fecha** | 2026-11-06 |

## 2. Nombre del proyecto

**Myclimbook Web** - Evolución a aplicación web de la app móvil Myclimbook para gestión de contenidos de escalada en rocódromos (spraywalls) y sectores de escalada en roca.

## 3. Qué quiero hacer

Myclimbook es una plataforma comunitaria para escaladores que permite gestionar y compartir contenidos de escalada. Los usuarios pueden crear muros de rocódromo con rutas marcadas sobre fotos, sectores de escalada en roca con vías deportivas y problemas de búlder, y sectores 3D integrados con Sketchfab.

La evolución consiste en llevar la aplicación Android existente a una aplicación web accesible desde myclimbook.com, manteniendo toda la funcionalidad actual (autenticación, gestión de muros, rutas, sectores, comentarios, valoraciones, favoritos) y mejorando la experiencia con diseño responsive, mejor SEO y accesibilidad.

La aplicación usa Firebase como backend (Authentication, Firestore, Storage) y se desarrollará una API REST con Spring Boot como capa intermedia entre el frontend web y Firebase.

## 4. Qué problema o necesidad resuelve

Actualmente no existe una plataforma web específica para gestionar spraywalls y sectores de escalada de forma visual e interactiva. Los escaladores necesitan herramientas para:

- Documentar rutas creadas en rocódromos con marcadores visuales sobre fotos
- Compartir información de sectores de escalada en roca (ubicación, parking, vías)
- Llevar un registro personal de rutas completadas y progreso
- Descubrir nuevos muros y sectores creados por la comunidad

Lo sé porque Myclimbook ya existe como app Android con usuarios reales activos que utilizan estas funcionalidades diariamente. La evolución a web permitirá alcanzar a más usuarios y mejorar la accesibilidad.

## 5. Para quién

- **Escaladores de rocódromo**: Para documentar y compartir rutas en spraywalls
- **Escaladores de roca**: Para gestionar información de sectores, vías y búlders
- **Aperturistas**: Para documentar vías nuevas creadas en rocódromos o roca
- **Comunidad de escalada**: Para descubrir nuevos contenidos y seguir el progreso

Contexto: Aplicación web accesible desde cualquier dispositivo con navegador, dominio myclimbook.com (ya registrado).

## 6. Funcionalidades principales

| # | Funcionalidad | ¿Imprescindible? (sí / no) |
|---|---|---|
| 1 | Autenticación de usuarios (login, registro, recuperación contraseña) | sí |
| 2 | Gestión de muros con rutas marcadas sobre fotos (spraywalls) | sí |
| 3 | Gestión de sectores de escalada en roca con vías deportivas y búlders | sí |
| 4 | Sistema de comentarios y valoraciones en contenidos | sí |
| 5 | Seguimiento de progreso (rutas completadas, estadísticas) | sí |
| 6 | Sectores 3D integrados con Sketchfab | no |

**Primera versión mínima (antes 31-mar):** Funcionalidades 1-5 implementadas y funcionando con usuarios reales.

## 7. Tecnologías previstas

| Tecnología | Para qué | Nivel actual (nada / algo / bien) |
|---|---|---|
| Java 17 | Backend - API REST con Spring Boot | bien |
| Spring Boot 3.x | Framework backend | algo |
| Spring Security + JWT | Autenticación y autorización | algo |
| Firebase Admin SDK | Conexión con Firestore, Auth y Storage | bien |
| JavaScript ES6+ | Frontend - lógica de cliente | bien |
| AJAX (Fetch API) | Comunicación frontend-backend | bien |
| HTML5 + CSS3 | Estructura y estilos web | bien |
| Thymeleaf | Templates HTML del lado servidor | algo |
| Git + GitHub | Control de versiones | bien |
| Maven | Gestión de dependencias Java | algo |
| Nginx | Servidor web / reverse proxy | algo |
| VPS (Hetzner) | Despliegue en producción | algo |

## 8. Dónde se integra con los módulos de 2.º

- [x] Desarrollo web en entorno cliente (0612) - JavaScript, AJAX, DOM, responsive design
- [x] Desarrollo web en entorno servidor (0613) - Spring Boot, API REST, seguridad, MVC
- [x] Despliegue de aplicaciones web (0614) - VPS, Nginx, SSL, dominio, configuración servidor
- [x] Diseño de interfaces web (0615) - UX/UI, accesibilidad, diseño responsive, componentes reutilizables

## 9. Datos y empresas

- ¿El proyecto parte de una **empresa** (por ejemplo, de tus prácticas)? **No**. Es un proyecto personal en producción con usuarios reales.
- ¿Tienes su **autorización escrita** para usar información suya? **No aplica** - proyecto propio.
- ¿Usará **datos personales reales**? **Sí**, pero son datos de usuarios existentes que ya usan la app Android. Los usuarios se registraron voluntariamente aceptando la política de privacidad. No se migrarán datos, sino que la web accederá a los mismos datos de Firebase que usa la app actual.

## 10. Riesgos y dudas iniciales

**Riesgos:**
- **Integración Spring Boot + Firebase Admin SDK**: No tengo experiencia previa combinando estas tecnologías. Necesito verificar que Firebase Admin SDK permite todas las operaciones necesarias desde Java.
- **Editor de marcadores interactivo en web**: La funcionalidad más compleja es el editor de marcadores sobre imágenes (crear rutas en muros). En Android funciona bien con Canvas, en web necesito evaluar si usar Canvas HTML5 o SVG.
- **Tiempo disponible**: El proyecto es amplio (14 funcionalidades principales). Priorizaré las 5 imprescindibles para la primera versión mínima antes del 31-mar.

**Dudas:**
- ¿Es aceptable usar Firebase como backend (en lugar de MySQL) dado que ya tengo datos en producción?
- ¿Qué opción de despliegue es más adecuada: VPS o servicio PaaS (Railway, Heroku)?
- ¿El editor de marcadores interactivo es suficiente con JavaScript o necesito algún framework específico?

## 11. Qué espero aprender

Espero profundizar en el desarrollo de aplicaciones web completas (fullstack) con Spring Boot, mejorar mis habilidades en JavaScript moderno y AJAX, y aprender sobre despliegue y configuración de servidores en producción. También quiero consolidar buenas prácticas de seguridad, testing y arquitectura de software.

---

## Aprobación del tutor

*Esta parte la rellena el tutor.*

| Criterio mínimo | Cumple |
|---|---|
| Necesidad real y documentada | ☐ |
| Alcance abordable en el tiempo disponible | ☐ |
| Integra contenidos de al menos tres módulos de 2.º | ☐ |
| No usa datos personales reales | ☐ |

**Estado:** ☐ Aprobada · ☐ Aprobada con cambios · ☐ Necesita rehacerse

**Fecha:** 

**Comentarios del tutor:**
