# RateM Dating · Documentación del Backend

Documentación del backend de **RateM Dating**, una aplicación web de citas con verificación de identidad y reseñas mutuas. Este archivo vive en el repositorio **Documentation** de la organización de GitHub **Rate-M** y se complementa con el archivo `.md` del frontend.

**Asignatura:** Formulación de Proyectos de Tecnología · **Grado y sección:** 7 B · **Profesor:** Jesús Del Toro Robledo
**Metodología:** Scrum
**Equipo:** Nidia Elizabeth Ruiz Barraza · Ángel Gabriel Tovar Campirano · Natalia Cabral Veliz · Mario Antonio Portillo Romero
**Stack del backend:** Node.js · Express · MongoDB (Mongoose) · Socket.io · JWT · bcrypt · Jest

## Organización y repositorios

El proyecto se aloja en la organización de GitHub [Rate-M](https://github.com/Rate-M), que agrupa tres repositorios:

| Repositorio | Contenido | Enlace |
|---|---|---|
| `ratem-backend` | Código del backend: API REST, WebSockets, modelos y servicios externos | https://github.com/Rate-M/ratem-backend.git |
| `Frontend` | Código del frontend: aplicación Vue 3 | https://github.com/Rate-M/Frontend.git |
| `Documentation` | Documentación del proyecto: un archivo `.md` para el backend (este) y otro para el frontend | https://github.com/Rate-M/Documentation.git |

## Contenido

1. [Visión del producto](#1-visión-del-producto)
2. [Metodología de desarrollo: Scrum](#2-metodología-de-desarrollo-scrum)
3. [Requisitos](#3-requisitos)
4. [Análisis de riesgos](#4-análisis-de-riesgos)
5. [Planificación](#5-planificación)
6. [Diseño de la solución](#6-diseño-de-la-solución)
7. [Control de versiones](#7-control-de-versiones)
8. [Diagramas](#8-diagramas)
9. [Diccionario de datos](#9-diccionario-de-datos)
10. [Endpoints principales](#10-endpoints-principales)
11. [Guía del backend](#11-guía-del-backend)

---

## 1. Visión del producto

### 1.1 Problemática

Las aplicaciones de citas actuales generan matches con base en fotos e información autodeclarada, sin un mecanismo que valide esa información ni el comportamiento real de la persona en un encuentro presencial. Esto produce cuatro problemas:

1. **Asimetría de información:** el usuario no sabe cómo se ha comportado la otra persona en citas anteriores.
2. **Perfiles falsos y catfishing:** la falta de verificación robusta permite la suplantación de identidad, el fraude y el acoso.
3. **Ausencia de consecuencias por mal comportamiento:** las conductas problemáticas no afectan la reputación del usuario.
4. **Falta de confianza:** la inseguridad en el primer encuentro es una barrera para el uso de estas plataformas.

La problemática se respalda con datos: 8.6 millones de mexicanos buscan pareja mediante aplicaciones de citas, el 41.9 % de quienes no las usan señala la inseguridad como razón principal (The Competitive Intelligence Unit, 2026), y los fraudes en estas aplicaciones crecieron 27 % en México durante 2025 (Sumsub, citado en El CEO, 2026).

### 1.2 Justificación

Un sistema con verificación de identidad obligatoria, interacción sostenida previa a la cita y reseñas mutuas posteriores reduce la asimetría de información, incentiva comportamientos responsables mediante reputación pública y aumenta la confianza del usuario. El proyecto traslada a las citas presenciales el modelo de calificaciones mutuas que ya usan Uber, Airbnb y Mercado Libre, con la condición de que solo se pueda reseñar un encuentro confirmado.

### 1.3 Objetivo general

Desarrollar, en un plazo de 12 semanas, una aplicación web de citas que incorpore verificación de identidad obligatoria, confirmación de encuentros por geolocalización y un sistema de reseñas mutuas post-cita, con el fin de reducir la asimetría de información entre usuarios y aumentar la confianza y seguridad al concretar encuentros presenciales.

### 1.4 Objetivos específicos

Cada objetivo corresponde a una épica del Product Backlog y se cumple cuando todas sus historias cumplen la Definition of Done.

| # | Objetivo específico (épica) |
|---|---|
| 1 | Autenticación y verificación de identidad (documento oficial y reconocimiento facial) |
| 2 | Perfiles de usuario con historial de reseñas y calificación promedio |
| 3 | Match solo cuando existe interés mutuo |
| 4 | Mensajería en tiempo real (WebSockets) entre usuarios con match |
| 5 | Racha de días consecutivos con mensajes, que desbloquea la invitación a cita |
| 6 | Mapa de lugares para citas con Google Places y lugares de la comunidad moderados |
| 7 | Invitación y confirmación de cita por geolocalización, con alternativa manual |
| 8 | Reseñas mutuas, publicadas solo cuando ambas partes califican |
| 9 | Moderación de contenido (diccionario, IA y cola de revisión) |
| 10 | Documentación legal conforme a la LFPDPPP |

### 1.5 Alcances

| Tipo de usuario | Descripción | Funciones principales |
|---|---|---|
| Visitante | Persona sin cuenta o sin verificar | Consultar aviso de privacidad y términos, registrarse e iniciar sesión |
| Usuario verificado | Persona mayor de edad con identidad aprobada | Perfil, match, chat, racha, mapa, invitaciones, confirmación de cita, reseñas y reportes |
| Moderador | Integrante del equipo con permisos de revisión | Aprobar lugares, revisar reseñas reportadas y ocultar contenido |

**Plazo:** 12 semanas (6 sprints de 2 semanas), del 28 de septiembre al 18 de diciembre de 2026, precedidas por un Sprint 0 de planeación (14 al 25 de septiembre). El MVP se entrega al cierre del Sprint 5 (4 de diciembre de 2026).

**Fuera del alcance:** aplicaciones móviles nativas, pagos y funciones premium, videollamadas o audio en el chat, notificaciones push nativas, recomendación de perfiles con inteligencia artificial y soporte multilenguaje.

### 1.6 Limitaciones

Tiempo de desarrollo definido; presupuesto nulo (APIs externas en planes gratuitos o de prueba); equipo reducido con roles compartidos; primera experiencia del equipo con Express y MongoDB en un proyecto completo; falta de asesoría legal especializada; acceso restringido a proveedores de verificación; pruebas de geolocalización limitadas a simulaciones; ausencia de una base de usuarios reales; y dependencia de documentación externa de las APIs de terceros.

### 1.7 Producto mínimo viable (MVP)

El MVP es la versión más pequeña que permite validar la propuesta de valor: que dos personas verificadas puedan conocerse, tener una cita confirmada y dejarse una reseña confiable. Suma **108 de 130 puntos (29 historias)** y se priorizó con el método MoSCoW.

| Prioridad | Puntos | Entrega |
|---|---|---|
| Must have (22 historias) | 91 | MVP, Sprints 1 a 5 |
| Should have (7 historias) | 17 | MVP, Sprints 1 a 5 |
| Could have (6 historias) | 22 | Post-MVP, Sprint 6 |
| Won't have | — | Versiones futuras (sección 1.5) |

Flujo que valida el MVP: registro y aceptación del aviso de privacidad → verificación de identidad → perfil → descubrimiento y match → chat en tiempo real → racha cumplida → invitación a un lugar del mapa → confirmación de la cita por GPS o manual → reseñas mutuas publicadas.

---

## 2. Metodología de desarrollo: Scrum

Scrum organiza el desarrollo en sprints de 2 semanas que entregan un incremento funcional, priorizado según el valor para el usuario. La documentación es incremental: el Product Backlog se refina de forma continua y la documentación técnica se genera conforme existen los módulos. El backlog y los sprints se gestionan en Jira.

**Justificación.** Se eligió Scrum frente a Cascada y Kanban porque combina iteraciones de duración fija (necesarias para el plazo académico), priorización continua del backlog (necesaria por la dependencia de APIs externas cuyo costo y disponibilidad no son seguros) y eventos de inspección que generan evidencia en cada sprint.

### 2.1 Equipo y roles

| Rol | Integrante | Suplente |
|---|---|---|
| Product Owner | Nidia Elizabeth Ruiz Barraza | Ángel Gabriel Tovar Campirano |
| Scrum Master | Nidia Elizabeth Ruiz Barraza | Product Owner |
| Dev Backend | Nidia Elizabeth Ruiz Barraza | Mario Antonio Portillo Romero |
| Dev Frontend | Ángel Gabriel Tovar Campirano | Natalia Cabral Veliz |
| QA / Documentación | Ángel Gabriel Tovar Campirano | Nidia Elizabeth Ruiz Barraza |

### 2.2 Eventos y artefactos

| Evento | Descripción |
|---|---|
| Sprint | Ciclo de 2 semanas; 6 sprints en total |
| Sprint Planning | Se selecciona el Sprint Backlog y se define el objetivo del sprint |
| Daily Scrum | Reunión diaria de máximo 15 minutos |
| Sprint Review | Presentación del incremento al Product Owner, con capturas de pantalla |
| Sprint Retrospective | Qué salió bien, qué mejorar y acciones para el siguiente sprint |
| Backlog Refinement | Detalle y estimación de las historias de los siguientes sprints |

**Artefactos:** Product Backlog, Sprint Backlog, Incremento (que cumple la Definition of Done) y Burndown chart.

**Estimación:** puntos de historia con escala de Fibonacci (1, 2, 3, 5, 8). Velocidad inicial estimada de 20 a 25 puntos por sprint; 1 punto ≈ 4 horas de trabajo.

---

## 3. Requisitos

### 3.1 Requisitos funcionales

| ID | Requisito | Historias |
|---|---|---|
| RF-01 | Registro, inicio de sesión y recuperación de contraseña | US-01 a US-03 |
| RF-02 | Verificación de identidad y mayoría de edad antes de activar la cuenta | US-04 a US-06 |
| RF-03 | Crear, editar y consultar perfiles con reputación | US-07 a US-09 |
| RF-04 | Generar un match solo con interés mutuo | US-10 a US-12 |
| RF-05 | Mensajería en tiempo real entre usuarios con match | US-13 a US-15 |
| RF-06 | Contar la racha diaria y desbloquear la invitación al alcanzar el umbral | US-16 a US-18 |
| RF-07 | Mostrar lugares para citas y permitir proponer nuevos | US-19 a US-21 |
| RF-08 | Gestionar invitaciones y confirmar la cita por geolocalización o de forma manual | US-22 a US-25 |
| RF-09 | Reseñas mutuas publicadas solo cuando ambas partes califiquen | US-26 a US-28 |
| RF-10 | Moderar reseñas y lugares de forma automática y manual | US-29 a US-32 |
| RF-11 | Registrar consentimientos y atender solicitudes ARCO | US-33 a US-35 |

### 3.2 Requisitos no funcionales

| ID | Categoría | Requisito |
|---|---|---|
| RNF-01 | Seguridad | Contraseñas con bcrypt (factor 10 o mayor) y sesión JWT con expiración de 24 h |
| RNF-02 | Seguridad | HTTPS y límite de 5 intentos por minuto por IP en autenticación (el sexto responde 429) |
| RNF-03 | Privacidad | No se almacenan imágenes del documento ni de la selfie, solo el veredicto del proveedor |
| RNF-04 | Privacidad | La ubicación solo se solicita dentro de la ventana de la cita y no se guarda historial |
| RNF-05 | Legal | Cumplimiento de la LFPDPPP: aviso, consentimientos separados y revocables, derechos ARCO |
| RNF-06 | Rendimiento | API con respuesta menor a 500 ms (percentil 95) con 50 usuarios concurrentes |
| RNF-07 | Rendimiento | Mensajes del chat entregados en menos de 1 segundo |
| RNF-08 | Usabilidad | Un usuario nuevo completa registro y perfil en menos de 5 minutos |
| RNF-09 | Compatibilidad | Responsiva desde 360 px; últimas dos versiones de Chrome, Edge, Firefox y Safari |
| RNF-10 | Accesibilidad | Contraste WCAG 2.1 AA y navegación con teclado (Lighthouse ≥ 90) |
| RNF-11 | Disponibilidad | 99 % en el ambiente de pruebas durante los Sprint Reviews |
| RNF-12 | Mantenibilidad | Código sin errores de linter y cobertura de pruebas del backend de al menos 70 % |

### 3.3 Product Backlog

El backlog tiene **130 puntos de historia en 35 historias**, organizadas en 10 épicas, y se gestiona en Jira. Las historias siguen el formato «Como [rol], quiero [acción], para [beneficio]» y sus criterios de aceptación están en el documento de proyecto de la Unidad 1.

| Épica | Historias | Puntos |
|---|---|---|
| 1. Autenticación y verificación de identidad | US-01 a US-06 | 20 |
| 2. Perfiles de usuario | US-07 a US-09 | 10 |
| 3. Match | US-10 a US-12 | 10 |
| 4. Mensajería | US-13 a US-15 | 13 |
| 5. Racha (streak) | US-16 a US-18 | 9 |
| 6. Mapa de lugares para citas | US-19 a US-21 | 16 |
| 7. Invitación y confirmación de cita | US-22 a US-25 | 19 |
| 8. Reseñas mutuas | US-26 a US-28 | 10 |
| 9. Moderación de contenido | US-29 a US-32 | 15 |
| 10. Documentación legal | US-33 a US-35 | 8 |

### 3.4 Definition of Done

Una historia se considera terminada únicamente cuando cumple todos los criterios siguientes:

1. El código está en el repositorio: rama integrada a `main` mediante pull request validado por el owner del repositorio (la Unidad 1 preveía una rama `develop`; el flujo vigente se describe en la sección 7.3).
2. Cumple los criterios de aceptación y fue validada por el Product Owner.
3. Cuenta con pruebas automatizadas que pasan correctamente.
4. No introduce errores en funcionalidades existentes (pruebas de regresión).
5. Los endpoints nuevos están documentados (ruta, método, parámetros, respuestas).
6. Los cambios al modelo de datos están reflejados en la documentación técnica.
7. El código sigue las convenciones del proyecto (linter sin errores).
8. La funcionalidad está desplegada en el ambiente de pruebas.
9. Si maneja datos personales, sensibles o de ubicación, se verificó el cumplimiento del aviso de privacidad y los consentimientos.

---

## 4. Análisis de riesgos

Los riesgos se evaluaron con una escala de 1 a 5 para probabilidad (P) e impacto (I); el nivel es P × I. La matriz completa de 19 riesgos se entrega en el archivo *Pronóstico de riesgos.xlsx*. Los de nivel alto o extremo son:

| ID | Riesgo | P | I | Nivel | Mitigación principal |
|---|---|---|---|---|---|
| R-02 | Curva de aprendizaje (Express, MongoDB, Socket.io) | 4 | 5 | 20 Extremo | Semana de capacitación y spike antes del Sprint 1; plantilla base del proyecto |
| R-12 | Proveedor de verificación inaccesible por costo | 4 | 5 | 20 Extremo | Sandbox del proveedor; servicio intercambiable; plan B de verificación manual |
| R-14 | Falla o imposibilidad de probar la geolocalización | 4 | 4 | 16 Extremo | Radio y ventana configurables; pruebas simuladas y con 2 teléfonos; confirmación manual |
| R-18 | Subestimación del esfuerzo de las historias | 4 | 4 | 16 Extremo | Medir la velocidad real desde el Sprint 1; proteger el MVP |
| R-16 | Incumplimiento de la LFPDPPP por falta de asesoría | 3 | 5 | 15 Alto | Aviso basado en el texto vigente y revisión docente; consentimientos separados |
| R-17 | Sin usuarios reales para validar reseñas y rachas | 5 | 3 | 15 Alto | Seeders y piloto cerrado en el Sprint 6 |
| R-05 | Retraso por otras responsabilidades | 4 | 3 | 12 Alto | Velocidad conservadora; el Sprint 6 funciona como margen |
| R-07 | Fallos al implementar APIs externas | 3 | 4 | 12 Alto | Servicio aislado por API con interfaz común y mocks |
| R-01 | Abandono de un integrante | 2 | 5 | 10 Alto | Revisión cruzada y programación en pares |
| R-08 | Fallos de seguridad y filtración de información | 2 | 5 | 10 Alto | bcrypt, JWT, HTTPS, validación de entradas, rate limiting y credenciales fuera del repositorio |

---

## 5. Planificación

### 5.1 Planificación de sprints

| Sprint | Objetivo | Historias | Puntos | Fechas |
|---|---|---|---|---|
| Sprint 0 | Planeación: documento de proyecto, backlog, riesgos, wireframes y repositorio | — | — | 14/09 al 25/09/2026 |
| Sprint 1 | Cimientos: autenticación, perfil y base legal | US-33, 01, 02, 03, 34, 07, 08 | 19 | 28/09 al 09/10/2026 |
| Sprint 2 | Verificación de identidad y perfil público | US-04, 05, 06, 09, 35 | 19 | 12/10 al 23/10/2026 |
| Sprint 3 | Match y mensajería | US-10 a US-15 | 23 | 26/10 al 06/11/2026 |
| Sprint 4 | Racha, mapa de lugares e invitación | US-16, 17, 18, 19, 22 | 22 | 09/11 al 20/11/2026 |
| Sprint 5 | Confirmación de cita y reseñas (cierre del MVP) | US-23, 24, 25, 26, 27, 29 | 25 | 23/11 al 04/12/2026 |
| Sprint 6 | Comunidad y moderación avanzada (post-MVP) | US-20, 21, 28, 30, 31, 32 | 22 | 07/12 al 18/12/2026 |

El viernes 20 de noviembre de 2026 es feriado (Revolución Mexicana), por lo que la capacidad del Sprint 4 se ajustó a 22 puntos.

### 5.2 Hitos verificables

| Hito | Fecha | Criterio de verificación |
|---|---|---|
| H0 – Planeación aprobada | 25/09/2026 | Documento de proyecto entregado y evaluado |
| H1 – Acceso seguro | 09/10/2026 | Un usuario se registra, acepta el aviso, inicia sesión y crea su perfil |
| H2 – Identidad verificada | 23/10/2026 | Un menor de edad es rechazado y uno válido queda aprobado |
| H3 – Comunicación en tiempo real | 06/11/2026 | Dos usuarios hacen match y chatean desde dos navegadores |
| H4 – Invitación a cita | 20/11/2026 | Tras cumplir la racha se habilita el botón y se envía una invitación |
| H5 – MVP completo | 04/12/2026 | Recorrido completo del flujo del MVP en el Sprint Review |
| H6 – Versión 1.0 | 18/12/2026 | Moderación completa, piloto con usuarios de prueba y documentación final |

---

## 6. Diseño de la solución

### 6.1 Arquitectura

Arquitectura cliente-servidor en tres capas (presentación, lógica de negocio y datos). El backend se organiza por módulos (`auth`, `users`, `matches`, `messages`, `streaks`, `places`, `dates`, `reviews`, `moderation`), cada uno con rutas, controladores, servicios y modelos. Las APIs externas se encapsulan en servicios intercambiables para cambiar de proveedor sin modificar el resto del sistema (mitigación de R-07, R-12 y R-19).

### 6.2 Tecnologías del backend

| Tecnología | Uso | Justificación |
|---|---|---|
| Node.js + Express | API REST | Mismo lenguaje que el frontend y modelo no bloqueante, necesario para el chat. Se descartó Laravel porque su soporte de WebSockets requiere servicios adicionales |
| MongoDB + Mongoose | Base de datos | Índices geoespaciales 2dsphere nativos y esquema flexible. Se descartó MySQL por sus consultas geoespaciales más limitadas |
| Socket.io | Chat en tiempo real | Comunicación bidireccional con reconexión automática y salas por match. Se descartó el polling HTTP por latencia |
| JWT + bcrypt | Autenticación | Sesiones sin estado compatibles con la API y con Socket.io |
| Proveedor KYC (Veriff, Metamap o Sumsub) | Verificación de identidad | Validan documento y rostro sin que el sistema almacene imágenes. Plan B: verificación manual |
| Google Places API | Mapa de lugares | Catálogo amplio con caché en MongoDB para no exceder el plan gratuito |
| OpenAI Moderation / Perspective API | Moderación con IA | Complementa el filtro por diccionario |
| Jest + Supertest, ESLint | Pruebas y calidad | Exigidas por la Definition of Done |
| GitHub + Render + MongoDB Atlas | Repositorio y despliegue | Planes gratuitos suficientes; despliegue automático desde `main` |

---

## 7. Control de versiones

### 7.1 Justificación de la herramienta y la plataforma

**Git** se eligió porque es distribuido: cada integrante trabaja con una copia completa del historial y crea ramas en segundos, lo que permite una rama por tarea de Jira que se combina a `main` solo cuando cumple la Definition of Done. Se descartó Subversion por ser centralizado y no ofrecer pull requests para la revisión de código que exige la Definition of Done.

**GitHub** se eligió frente a GitLab y Bitbucket porque reúne en un solo lugar la revisión de código, la integración continua y la evidencia de participación, mientras que el backlog y los sprints se gestionan en Jira. Además es la plataforma que el equipo ya adoptó, lo que reduce el riesgo R-02. GitLab se descartó por la curva de aprendizaje adicional y Bitbucket porque el equipo ya trabajaba con GitHub y de la suite de Atlassian solo necesita Jira.

### 7.2 Estructura de repositorios

El proyecto se aloja en la organización de GitHub **Rate-M**, con tres repositorios (ver la tabla del inicio). El backend y el frontend están separados porque se despliegan en servicios distintos (Render y Vercel), tienen dependencias y pruebas independientes y reflejan la división de roles (Dev Backend y Dev Frontend). La documentación se mantiene en un repositorio propio. El punto de integración entre el código es el contrato de la API (sección 10), que se define antes de implementar cada historia (mitigación de R-04).

### 7.3 Flujo de trabajo

Se adopta **GitHub Flow**: una sola rama estable, `main`, y una rama de trabajo corta por cada tarea de Jira. A diferencia de lo previsto en la Unidad 1, el equipo no usa una rama `develop`. Se descartó Git Flow, que agrega ramas de integración, de release y de soporte, porque es excesivo para 6 sprints y 4 integrantes.

| Rama | Propósito | Se integra a | Ejemplo |
|---|---|---|---|
| `main` | Código estable. Única rama de larga duración; recibe el trabajo de cada tarea mediante pull request validado por el owner | — | `main` |
| `feature/*` | Una tarea de Jira (código `RM-#`) | `main` | `feature/RM-7-Formulario-de-registro-con-validación-en-Vue` |

**Nomenclatura.** La rama inicia con `feature/`, seguida del código que Jira asigna a la tarea (`RM-#`, donde `RM` es la clave del proyecto) y del título de la tarea con guiones en lugar de espacios.

**Convención de commits.** Cada commit lleva el código de la tarea de Jira seguido de su título, con el formato `RM-# Título de la tarea`, el mismo nombre que se usa en la rama. Cada vez que se avanza en la tarea se registra un commit con este formato, de modo que el historial se lea por tarea.

| Elemento | Formato | Ejemplo |
|---|---|---|
| Rama | `feature/RM-#-Título-de-la-tarea` | `feature/RM-7-Formulario-de-registro-con-validación-en-Vue` |
| Commit | `RM-# Título de la tarea` | `RM-7 Formulario de registro con validación en Vue` |

**Política de combinación.** `feature/*` → `main` con **merge**: pull request validado por el owner del repositorio, que realiza el merge (Definition of Done, punto 1); criterios de aceptación de la tarea validados y verificaciones automáticas en verde. Se conservan los commits de cada tarea y la gráfica de ramas como evidencia.

**Gestión de pull requests**

1. El autor abre el pull request hacia `main` desde su rama de trabajo, con el código y el título de la tarea (por ejemplo, `RM-7 Formulario de registro con validación en Vue`), y llena la plantilla del repositorio.
2. La descripción incluye la tarea de Jira, los cambios realizados, cómo probarlos y la lista de verificación de la Definition of Done.
3. El owner del repositorio revisa y valida el pull request; las conversaciones deben quedar resueltas antes de combinar.
4. Con su validación y las verificaciones en verde, el owner realiza el merge a `main`.
5. Cada pull request se mantiene pequeño (idealmente menos de 400 líneas modificadas), en línea con el límite de 8 puntos por historia.

Para evitar conflictos, antes de crear la rama se guardan los cambios pendientes, se actualiza `main` y se recuperan los cambios. No se reescribe el historial de `main` y no se permite el force push.

```bash
git stash
git pull origin main
git stash pop
git checkout -b feature/RM-7-Formulario-de-registro-con-validación-en-Vue
git add .
git commit -m "RM-7 Formulario de registro con validación en Vue"
git push origin feature/RM-7-Formulario-de-registro-con-validación-en-Vue
# abrir el pull request hacia main
```

**Versionado.** Semántico, con etiquetas anotadas en `main`: cada sprint incrementa la versión menor y la versión final es `v1.0.0`. Las etiquetas se crean en los repositorios de backend y frontend al cierre de cada sprint y coinciden con los hitos del cronograma.

| Etiqueta | Sprint | Cierre | Contenido |
|---|---|---|---|
| `v0.1.0` | 1 | 09/10/2026 | Registro, inicio de sesión, consentimientos y perfil (H1) |
| `v0.2.0` | 2 | 23/10/2026 | Verificación de identidad y perfil público (H2) |
| `v0.3.0` | 3 | 06/11/2026 | Match y chat en tiempo real (H3) |
| `v0.4.0` | 4 | 20/11/2026 | Racha, mapa e invitación (H4) |
| `v0.5.0` | 5 | 04/12/2026 | MVP completo (H5) |
| `v1.0.0` | 6 | 18/12/2026 | Moderación completa y documentación final (H6) |

**Gestión del trabajo en Jira.** Cada historia del Product Backlog y sus tareas técnicas se registran en Jira, que asigna a cada tarea un código `RM-#` (por ejemplo, `RM-7`). Ese código identifica la tarea en la rama, el commit y el pull request. Cada sprint se planifica en Jira y su tablero refleja los estados de Scrum:

| Columna del tablero | Significado |
|---|---|
| Product Backlog | Historias priorizadas y estimadas que aún no entran a un sprint |
| Sprint Backlog | Historias comprometidas en el Sprint Planning |
| En progreso | Tarea con rama de trabajo activa |
| En revisión | Pull request abierto y pendiente de validación del owner |
| Terminada | Historia que cumple los criterios de aceptación y la Definition of Done |

### 7.4 Configuración del entorno de versiones

**Parámetros iniciales de Git** (cada integrante):

```bash
git config --global user.name "Nombre Apellido"
git config --global user.email "correo@institucional.edu.mx"
git config --global init.defaultBranch main
git config --global pull.rebase true
git config --global core.editor "code --wait"
```

Cada repositorio incluye un archivo `.gitattributes` con la línea `* text=auto eol=lf` para normalizar los saltos de línea entre sistemas operativos.

**Control de accesos**

| Rol en el repositorio | Quién | Permisos |
|---|---|---|
| Administrador (owner) | Scrum Master | Validar los pull requests y realizar el merge a `main`; configurar el repositorio, la protección de ramas, los secretos y las integraciones de despliegue |
| Escritura | Demás integrantes | Crear ramas de trabajo, subir commits a ellas y abrir pull requests hacia `main` |
| Lectura | Docente | Consultar el código, el historial, los pull requests y el tablero |

Los tres repositorios de la organización Rate-M son privados durante el desarrollo. Todos los integrantes activan la verificación en dos pasos y se autentican con una llave SSH propia (ed25519) o con un token personal con permisos mínimos, nunca con su contraseña.

**Protección de la rama `main`**

| Regla | `main` |
|---|---|
| Requerir pull request antes de combinar | Sí |
| Aprobaciones requeridas | 1, la validación del owner del repositorio |
| Descartar aprobaciones al subir nuevos commits | Sí |
| Requerir resolución de conversaciones | Sí |
| Verificaciones automáticas obligatorias | Sí (ESLint, pruebas y compilación) |
| Requerir rama actualizada antes de combinar | Sí |
| Push directo | No |
| Force push y eliminación de la rama | No |
| Aplicar las reglas también a administradores | Sí |

**Archivos de exclusión** (`.gitignore` del repositorio `ratem-backend`):

```gitignore
# Dependencias
node_modules/

# Variables de entorno y credenciales
.env
.env.*
!.env.example

# Registros y cobertura de pruebas
logs/
*.log
coverage/

# Archivos generados y temporales
dist/
build/
.cache/
uploads/

# Sistema operativo y editor
.DS_Store
Thumbs.db
.idea/
.vscode/*
!.vscode/extensions.json
```

**Credenciales y llaves.** Ninguna credencial se sube al repositorio (mitigación de R-08). El repositorio incluye un archivo `.env.example` con los nombres de las variables y sin valores; los valores reales se guardan en el `.env` local de cada integrante (ignorado por Git), en los secretos de GitHub Actions y en las variables de entorno de Render.

| Variable | Descripción |
|---|---|
| `PORT`, `NODE_ENV` | Puerto y ambiente de ejecución |
| `MONGODB_URI` | Cadena de conexión a MongoDB Atlas |
| `JWT_SECRET`, `JWT_EXPIRES_IN` | Firma y expiración del token de sesión (24 h) |
| `BCRYPT_ROUNDS` | Factor de costo del hash de contraseñas (10 o mayor) |
| `CLIENT_URL` | Origen permitido por CORS (URL del frontend) |
| `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASS` | Correo de confirmación y recuperación de contraseña |
| `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET` | Almacenamiento de las fotos de perfil |
| `GOOGLE_PLACES_API_KEY` | Consulta de lugares para citas |
| `KYC_API_KEY`, `KYC_WEBHOOK_SECRET` | Verificación de identidad y validación de su webhook |
| `MODERATION_API_KEY` | API de moderación con IA |

Si una credencial se publica por error, se revoca y se rota de inmediato, y se limpia el historial cuando es necesario. Dependabot está activo para las alertas de dependencias.

**Integración continua**

| Evento | Pasos del flujo |
|---|---|
| Pull request hacia `main` | Instalar dependencias (`npm ci`), ejecutar ESLint y Jest con cobertura, y exigir al menos 70 % de cobertura (RNF-12) |
| Push a `main` | Despliegue automático al ambiente de pruebas en Render |

### 7.5 Evidencias del repositorio

| Evidencia | Qué demuestra | Dónde consultarla |
|---|---|---|
| Historial de commits | Commits pequeños, con el código de la tarea (`RM-#`) y de los cuatro integrantes | Pestaña Commits o `git log --oneline --graph` |
| Ramas por tarea | Una rama `feature/RM-#-*` por tarea de Jira, creada desde `main` | Pestaña Branches y gráfica de red (Insights → Network) |
| Pull requests cerrados | Validación del owner antes de combinar a `main` | Pestaña Pull requests (filtro: cerrados) |
| Protección de ramas | Reglas activas en `main` | Settings → Branches |
| Etiquetas de versión | Una versión por sprint (`v0.1.0` a `v1.0.0`) | Releases y Tags |
| Tablero de Jira | Seguimiento del Sprint Backlog por tarea y por sprint | Tablero del proyecto en Jira |
| Contribuciones | Participación de los cuatro integrantes | Insights → Contributors |
| Verificaciones automáticas | ESLint, pruebas y compilación en cada pull request | Pestaña Actions |

---

## 8. Diagramas

El paquete reúne **29 diagramas UML** del diseño del sistema, elaborados a partir de los requisitos funcionales (RF-01 a RF-11) y las historias de usuario (US-01 a US-35). Representan el diseño propuesto: sus mensajes y componentes son conceptuales y no corresponden a rutas ni a código implementados. El paquete completo en PDF, con los diagramas en su tamaño original, está adjunto en el Drive del proyecto.

| Tipo | Cantidad | Diagramas incluidos |
|---|---|---|
| Casos de uso | 11 | Acceso y autenticación (RF-01); verificación de identidad (RF-02); gestión de perfiles (RF-03); descubrimiento y match (RF-04); mensajería (RF-05); rachas de conversación (RF-06); mapa y lugares (RF-07); invitaciones y citas (RF-08); reseñas mutuas (RF-09); moderación de contenido (RF-10); privacidad y derechos (RF-11) |
| Actividades | 5 | Registro y verificación; match, chat y racha; invitación y confirmación; reseñas y publicación conjunta; moderación y reportes |
| Clases | 3 | Relaciones entre las 12 colecciones; usuarios, interacciones y mensajería (1/2); citas, reseñas y seguridad (2/2) |
| Componentes | 1 | Arquitectura lógica: aplicación web Vue 3, API Express, Socket.io, reglas de negocio, MongoDB y servicios externos |
| Despliegue | 1 | Navegador, hosting del frontend y del backend, MongoDB y servicios externos; concretado en Render y Vercel |
| Estados | 3 | Verificación de identidad; invitación y cita; ciclo de vida de la reseña |
| Secuencia | 5 | Registro y verificación; descubrimiento, match y chat; racha diaria sin doble cómputo; invitación y confirmación de cita; reseñas mutuas y correcciones |

**Modelado de la base de datos.** Se optó por una base de datos NoSQL en MongoDB porque la aplicación está orientada a una red social, que requiere datos semiestructurados para manejarlos de forma óptima. Los tres diagramas de clases documentan sus 12 colecciones, y el detalle de campos, restricciones e índices está en la sección 9.

---

## 9. Diccionario de datos

Modelo de documentos en MongoDB (Mongoose) con 12 colecciones, consistente con el diagrama del modelo de datos. **PK:** `_id` (ObjectId automático) en todas. **FK:** ObjectId con referencia a otra colección. **Req** = obligatorio; **Único** = índice único; **Def** = valor por defecto. Privacidad: no se guardan imágenes del documento ni de la selfie (RNF-03) y la ubicación solo se registra dentro de la ventana de la cita (RNF-04).

### User

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `email` | String | Req, Único, minúsculas. Correo de acceso |
| `passwordHash` | String | Req. Hash bcrypt (factor ≥ 10) |
| `emailVerified` | Boolean | Def: false. Si confirmó su correo |
| `emailVerificationTokenHash` | String | Opcional. Hash del token de confirmación |
| `emailVerificationExpires` | Date | Opcional. Expiración del token de confirmación |
| `verificationStatus` | String | Enum: pending, approved, rejected; Def: pending. Veredicto de la verificación de identidad |
| `role` | String | Enum: user, moderator; Def: user |
| `passwordResetTokenHash` | String | Opcional. Hash del token de recuperación |
| `passwordResetExpires` | Date | Opcional. Expiración del token de recuperación |
| `createdAt`, `updatedAt` | Date | Automáticos |

### Profile (1 a 1 con User)

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `user` | ObjectId | FK → User. Req, Único. Dueño del perfil |
| `name` | String | Req, máx. 50. Nombre visible |
| `birthDate` | String | Req, YYYY-MM-DD, edad ≥ 18. Fecha de nacimiento |
| `gender` | String | Req. Género del usuario |
| `interestedIn` | String[] | Req, mín. 1. Géneros que le interesan |
| `lookingFor` | String | Opcional. Lo que busca en la plataforma |
| `interests` | String[] | Opcional. Intereses y pasatiempos |
| `zodiacSign` | String | Opcional. Uno de los 12 signos |
| `bio` | String | Opcional, máx. 500. Biografía |
| `photos` | String[] | Req, mín. 1 y máx. 6. URLs de las fotos |
| `createdAt`, `updatedAt` | Date | Automáticos |

### Consent

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `user` | ObjectId | FK → User. Req |
| `type` | String | Req. Enum: privacy_terms, biometric, location |
| `version` | String | Req. Versión del aviso aceptado |
| `acceptedAt` | Date | Req. Fecha de aceptación |
| `revokedAt` | Date | Opcional. Si es nulo, el consentimiento está vigente |

### Swipe

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `from` | ObjectId | FK → User. Req. Quien evalúa |
| `to` | ObjectId | FK → User. Req, distinto de `from`. Evaluado |
| `action` | String | Req. Enum: like, pass (no reversible) |
| `createdAt` | Date | Automático |

Índice único compuesto `(from, to)`.

### Match

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK. También es la sala del chat |
| `users` | ObjectId[2] | FK → User. Req, exactamente 2. Usuarios del match |
| `pairKey` | String | Req, Único. Ids ordenados y concatenados; evita matches duplicados |
| `status` | String | Enum: active, unmatched; Def: active |
| `streak` | Object | Req. Datos de la racha (ver subdocumentos) |
| `lastMessageAt` | Date | Opcional. Ordena la lista de conversaciones |

### Message

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `match` | ObjectId | FK → Match. Req |
| `sender` | ObjectId | FK → User. Req, debe estar en `Match.users` |
| `type` | String | Enum: text, system; Def: text |
| `body` | String | Req, máx. 1000. Contenido |
| `moderation` | Object | Opcional. Resultado del filtro (ver subdocumentos) |
| `readAt` | Date | Opcional. Fecha de lectura |

Índice `(match, _id)` para cargar el historial en orden.

### Place

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `name` | String | Req, máx. 120 |
| `category` | String | Req. Enum: restaurant, cafe, park |
| `location` | Object | Req, índice 2dsphere. GeoJSON Point |
| `source` | String | Req. Enum: google, community |
| `googlePlaceId` | String | Único (sparse); obligatorio si `source` = google |
| `createdBy` | ObjectId | FK → User. Opcional; solo si `source` = community |
| `status` | String | Enum: approved, pending, rejected. Def: approved (google) o pending (community) |

### DateEvent

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `match` | ObjectId | FK → Match. Req; la racha debe estar cumplida |
| `participants` | ObjectId[2] | FK → User. Req, exactamente 2 |
| `invitedBy` | ObjectId | FK → User. Req, debe estar en `participants` |
| `place` | Object | Req. Lugar elegido con `placeId` (FK → Place) y copia de nombre, dirección y coordenadas |
| `scheduledAt` | Date | Req, fecha futura al crear |
| `status` | String | Req. Enum: pending, accepted, rejected, rescheduled, confirmed, not_confirmed, cancelled; Def: pending |
| `confirmation` | Object | Opcional. Confirmación por GPS o manual |
| `reviewDeadline` | Date | Opcional. 7 días después de la cita |

### Checkin

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `dateEvent` | ObjectId | FK → DateEvent. Req |
| `user` | ObjectId | FK → User. Req, debe estar en `DateEvent.participants` |
| `location` | Object | Req. GeoJSON Point reportado por el navegador |
| `accuracy` | Number | Req, ≥ 0. Precisión en metros |
| `at` | Date | Req. Momento del check-in |

Índice único compuesto `(dateEvent, user)`. Solo se crea dentro de la ventana de la cita (RNF-04).

### Review

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `dateEvent` | ObjectId | FK → DateEvent. Req; la cita debe estar confirmed |
| `match` | ObjectId | FK → Match. Req |
| `author` | ObjectId | FK → User. Req. Quien reseña |
| `subject` | ObjectId | FK → User. Req, distinto de `author`. Persona reseñada |
| `rating` | Number | Req, entero de 1 a 5 |
| `criteria` | Object | Opcional. Calificación por criterio |
| `comment` | String | Opcional, máx. 300 |
| `status` | String | Enum: pending, published, rejected, under_review, hidden; Def: pending |

Índice único compuesto `(dateEvent, author)`: una reseña por persona por cita.

### Report

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `targetType` | String | Req. Enum: review, place |
| `targetId` | ObjectId | Req. Referencia dinámica según `targetType` |
| `reportedBy` | ObjectId | FK → User. Req |
| `reason` | String | Req. Enum: harassment, personal_data, false_content, offensive_language, other |
| `status` | String | Enum: open, resolved; Def: open |

Índice único compuesto `(targetType, targetId, reportedBy)`.

### Block

| Campo | Tipo | Restricciones y descripción |
|---|---|---|
| `_id` | ObjectId | PK |
| `blocker` | ObjectId | FK → User. Req |
| `blocked` | ObjectId | FK → User. Req, distinto de `blocker` |
| `reason` | String | Opcional, máx. 200 |

Índice único compuesto `(blocker, blocked)`.

### Subdocumentos (campos tipo Object)

| Subdocumento | Campos |
|---|---|
| `Match.streak` | `current` (Number, Def: 0), `lastCountedDay` (String, YYYY-MM-DD), `todaySenders` (ObjectId[], máx. 2), `inviteUnlocked` (Boolean, Def: false) |
| `Message.moderation` | `status` (Enum: clean, flagged, blocked), `bannedWordHit` (Boolean), `aiScore` (Number de 0 a 1) |
| `Place.location`, `Checkin.location` | `type` (String, fijo: Point), `coordinates` (Number[2], [longitud, latitud]) |
| `DateEvent.place` | `placeId` (ObjectId, FK → Place), `name` (String), `address` (String), `location` (GeoJSON Point) |
| `DateEvent.confirmation` | `method` (Enum: gps, manual), `manualBy` (ObjectId[], máx. 2), `confirmedAt` (Date) |
| `Review.criteria` | `punctuality`, `respect`, `honesty`, `safety` (Number, entero de 1 a 5, opcionales) |

### Relaciones

| Origen | Cardinalidad | Destino | Campo |
|---|---|---|---|
| User | 1 a 1 | Profile | `Profile.user` |
| User | 1 a N | Consent, Swipe, Message, Place, Checkin, Report, Block | `Consent.user`, `Swipe.from`/`to`, `Message.sender`, `Place.createdBy`, `Checkin.user`, `Report.reportedBy`, `Block.blocker`/`blocked` |
| User | N a N | User | `Match.users` |
| Match | 1 a N | Message, DateEvent | `Message.match`, `DateEvent.match` |
| User | N a N | DateEvent | `DateEvent.participants` |
| Place | 1 a N | DateEvent | `DateEvent.place.placeId` |
| DateEvent | 1 a N | Checkin, Review | `Checkin.dateEvent`, `Review.dateEvent` |
| User | 1 a N | Review | `Review.author`, `Review.subject` |

### Índices principales

| Colección | Índice | Tipo | Propósito |
|---|---|---|---|
| User | `email` | Único | Evitar correos duplicados |
| Profile | `user` | Único | Un perfil por usuario |
| Swipe | `(from, to)` | Único compuesto | No repetir perfiles evaluados |
| Match | `pairKey` | Único | Evitar matches duplicados |
| Message | `(match, _id)` | Compuesto | Cargar el historial del chat en orden |
| Place | `location` | 2dsphere | Búsqueda de lugares cercanos |
| Place | `googlePlaceId` | Único (sparse) | Evitar duplicar lugares de Google |
| Checkin | `(dateEvent, user)` | Único compuesto | Un check-in por usuario por cita |
| Review | `(dateEvent, author)` | Único compuesto | Una reseña por persona por cita |
| Report | `(targetType, targetId, reportedBy)` | Único compuesto | Un reporte por usuario por contenido |
| Block | `(blocker, blocked)` | Único compuesto | Evitar bloqueos duplicados |

---

## 10. Endpoints principales

Contrato propuesto de la API. Cada endpoint se confirma y documenta con su ruta, método, parámetros y respuestas al cerrar su historia (Definition of Done, punto 5).

| Método | Ruta | Descripción | Historia |
|---|---|---|---|
| POST | `/auth/register` | Registro con correo y contraseña | US-01 |
| GET | `/auth/confirm/:token` | Confirmación de correo | US-01 |
| POST | `/auth/login` | Inicio de sesión (JWT) | US-02 |
| POST | `/auth/forgot-password` · `/auth/reset-password` | Recuperación de contraseña | US-03 |
| POST | `/consents` · DELETE `/consents/:type` | Registrar y revocar consentimientos | US-33, US-34 |
| POST | `/verification` · GET `/verification/status` | Iniciar y consultar la verificación | US-04, US-05 |
| POST | `/verification/webhook` | Veredicto del proveedor KYC | US-04, US-06 |
| POST · GET · PATCH | `/profiles` · `/profiles/me` | Crear, consultar y editar mi perfil | US-07, US-08 |
| GET | `/profiles/:id` | Perfil público con promedio y reseñas | US-09 |
| GET | `/discover` | Perfiles para evaluar, con filtros | US-10 |
| POST | `/swipes` | “Me gusta” o “pasar” | US-11, US-12 |
| GET · DELETE | `/matches` · `/matches/:id` | Listar y deshacer matches | US-12, US-14, US-15 |
| GET | `/matches/:id/messages` | Historial del chat | US-13 |
| WS | `message:send` · `message:new` · `message:read` | Eventos del chat en tiempo real | US-13 |
| GET · POST | `/places` | Lugares por categoría y proponer uno nuevo | US-19, US-20 |
| POST | `/dates` | Invitar a cita | US-22 |
| PATCH | `/dates/:id/respond` | Aceptar, rechazar o proponer otra hora | US-23 |
| POST | `/dates/:id/checkin` | Verificación por geolocalización | US-24 |
| POST | `/dates/:id/confirm-manual` | Confirmación manual | US-25 |
| POST | `/dates/:id/reviews` | Calificar y comentar | US-26, US-27 |
| POST | `/reports` · POST `/blocks` | Reportar contenido y bloquear usuarios | US-28 |
| GET · PATCH | `/moderation/queue` | Cola y decisiones del moderador | US-21, US-31 |
| DELETE | `/account` | Solicitud de eliminación (ARCO) | US-35 |

---

## 11. Guía del backend

La estructura y los scripts siguientes son la propuesta de diseño del repositorio `ratem-backend` y se confirman conforme se implementan los módulos en cada sprint.

### 11.1 Requisitos

Node.js 20 o superior, npm y una cuenta gratuita de MongoDB Atlas (o MongoDB local).

### 11.2 Instalación y ejecución

```bash
git clone https://github.com/Rate-M/ratem-backend.git
cd ratem-backend
npm ci
cp .env.example .env     # completar los valores
npm run dev
```

| Script | Descripción |
|---|---|
| `npm run dev` | Servidor en modo desarrollo con recarga automática |
| `npm start` | Servidor en modo producción |
| `npm run lint` | Análisis con ESLint |
| `npm test` | Pruebas con Jest y Supertest |
| `npm run test:coverage` | Pruebas con reporte de cobertura (mínimo 70 %) |
| `npm run seed` | Datos de prueba ficticios (perfiles generados con Faker) |

### 11.3 Estructura del proyecto

```text
src/
├── config/          conexión a la base de datos y variables de entorno
├── modules/
│   ├── auth/        registro, login, recuperación y confirmación de correo
│   ├── users/       perfiles, consentimientos y bloqueos
│   ├── matches/     swipes, matches y racha
│   ├── messages/    historial del chat y eventos de Socket.io
│   ├── places/      Google Places con caché y lugares de la comunidad
│   ├── dates/       invitaciones, check-ins y confirmación
│   ├── reviews/     reseñas mutuas y reportes
│   └── moderation/  diccionario, API de moderación y cola de revisión
├── services/        KYC, Google Places, moderación, correo y Cloudinary
├── middlewares/     autenticación JWT, roles, validación y límite de intentos
├── jobs/            tareas programadas (cálculo diario de la racha)
└── app.js           aplicación Express
tests/               pruebas unitarias y de integración
```

---

## Referencias

- The Competitive Intelligence Unit (2026). Uso de aplicaciones de citas en México.
- Norton (2025). Ciberdelincuencia en aplicaciones de citas en México.
- Sumsub, citado en El CEO (2026). Fraudes en aplicaciones de citas en México.
- GBG (s. f.). Estudio sobre verificación de identidad en aplicaciones de citas (Reino Unido).
