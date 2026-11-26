# Registro de tutoría

> **Este fichero es público.** Tu repositorio lo puede ver cualquiera. Aquí escribe **solo lo técnico**: horas, avances, decisiones técnicas y bloqueos técnicos. **Nada personal** (circunstancias tuyas o de otras personas, salud, familia, datos de la empresa) ni lo que acuerdes en privado con el tutor: eso va por correo o por el aula virtual.

Una entrada **cada semana**. El tutor puede comentar aspectos técnicos en el Pull Request.

**Horas:** anota cuántas horas has dedicado esta semana al proyecto, redondeadas a 0,5 h, separadas en **gestión y documentación** (análisis, diseño, planificación, memoria) y **producto** (código, pruebas, despliegue). Las declaras tú, **no se evalúan** y sirven para comprobar si el plan del módulo y el tuyo son realistas. Si no lo sabes con exactitud, pon una estimación honesta: es mejor que dejar la casilla vacía.

| Fecha | Horas gestión/doc. | Horas producto | Decisiones técnicas | Avances | Bloqueos | Siguiente paso |
|---|---|---|---|---|---|---|
| 2026-11-06 | 2 | 0 | Elegir Myclimbook Web como proyecto del módulo | Repositorio y documentación inicial | Sin bloqueos técnicos | Investigar el sector |
| 2026-11-13 | 6 | 0 | Mantener el enfoque web sobre la app Android existente | Sector, 6 organizaciones, 6 soluciones y 5 necesidades | `webfetch` no abre PDFs; se usó `r.jina.ai` | Completar objetivos y requisitos |
| 2026-11-20 | 8 | 0 | Mantener Firebase y añadir API REST con Spring Boot | Objetivos, RF/RNF/REST, obligaciones, ayuda, guion y glosario | Pendiente la aprobación del tutor antes de implementar | Validar H1 y preparar la entrega |

## Formato de registro anterior (conservado)

Cada semana añade una entrada con este formato:

```markdown
### Semana [X] - [Fecha]

**Horas dedicadas**: [X]h

**Avances**:
- [Describir lo que has hecho]

**Bloqueos**:
- [Problemas encontrados, si los hay]

**Acuerdos con el tutor**:
- [Decisiones tomadas]

**Siguiente paso**:
- [Qué vas a hacer la próxima semana]
```

---

## Registro

### Semana 1 - 6-12 noviembre 2026

**Horas dedicadas**: 2h

**Avances**:
- Sesión de inicio del proyecto (2h)
- Creación del repositorio
- Estructura inicial del proyecto

**Bloqueos**:
- Ninguno

**Acuerdos con el tutor**:
- Definir la necesidad del sector para el anteproyecto
- Establecer tecnologías a usar

**Siguiente paso**:
- Investigar necesidades del sector
- Definir alcance del proyecto
- Empezar a redactar el anteproyecto

---

### Semana 2 - 13-19 noviembre 2026

**Horas dedicadas**: 6 h (estimación; no se ha llevado un cronómetro)

**Avances**:
- Definido el problema y 4 preguntas de búsqueda para el SOTA.
- Localizadas y abiertas 6 soluciones: theCrag, Mountain Project, The Topo, OpenBeta,
  Rockfax y la alternativa no digital de cuaderno de escalada.
- Redactados el sector, la tabla de 6 organizaciones y el organigrama Mermaid de la empresa tipo.
- Priorizadas 5 necesidades con criterio y fuente, y localizado el dato cuantitativo del CSD.

**Bloqueos**:
- No se pudo abrir directamente un PDF con `webfetch`; se solventó usando el mirror de texto
  `r.jina.ai` para las fuentes CSD, IDEPA e INE.
- Sin respuesta aún del tutor a las dudas técnicas (Firebase, despliegue y editor de marcadores).

**Acuerdos con el tutor**:
- Revisión del avance prevista en la sesión del jueves 19-nov; sin acuerdos nuevos registrados.

**Siguiente paso**:
- Completar objetivos, requisitos verificables, obligaciones, ayudas y guion de trabajo.

---

### Semana 3 - 20-26 noviembre 2026

**Horas dedicadas**: 8 h (estimación; no se ha llevado un cronómetro)

**Avances**:
- Redactados 3 objetivos `OBJ-xx`, 5 requisitos `RF`, 4 `RNF` y 4 `REST` con su verificación.
- Añadida la matriz de trazabilidad, los criterios de aceptación y el hueco del SOTA.
- Documentadas las obligaciones fiscales, laborales y de PRL con norma del BOE, y la ayuda
  CHEQUES 2026 del IDEPA/SEKUENS (convocatoria cerrada).
- Creado el guion con 7 fases, 3 hitos y responsable por tarea, y el glosario de 8 términos.
- Declarado el uso de IA en `docs/declaracion-uso-ia.md`.

**Bloqueos**:
- Falta la confirmación explícita antes de abrir el PR, integrarlo en `main`, poner la
  etiqueta `h1` y enviar el correo de entrega.
- La aprobación del tutor sobre Firebase y el despliegue sigue pendiente; no se ha empezado
  la implementación de la aplicación.

**Acuerdos con el tutor**:
- Sin acuerdos nuevos en esta semana; las dudas técnicas quedan para el H2.

**Siguiente paso**:
- Revisar los dos entregables y el estado de Git; esperar confirmación para publicar H1.

---

*Próxima actualización: Semana 4 (27-nov-2026 a 3-dic-2026)*
