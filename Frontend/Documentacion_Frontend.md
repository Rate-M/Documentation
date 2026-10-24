# RateM Dating · Documentación del Frontend

Documentación del frontend de **RateM Dating**, una aplicación web de citas con verificación de identidad y reseñas mutuas. Este archivo vive en el repositorio **Documentation** de la organización de GitHub **Rate-M** y se complementa con el archivo `.md` del backend.

**Asignatura:** Formulación de Proyectos de Tecnología · **Grado y sección:** 7 B · **Profesor:** Jesús Del Toro Robledo
**Metodología:** Scrum
**Equipo:** Nidia Elizabeth Ruiz Barraza · Ángel Gabriel Tovar Campirano · Natalia Cabral Veliz · Mario Antonio Portillo Romero
**Stack del frontend:** Vue 3 · TypeScript · Vue Router · Pinia · Socket.io-client · Vite
**Prototipo en Figma:** https://www.figma.com/design/uqQhPFBjJPSIJo5VGT2QFG/Ratem

## Organización y repositorios

El proyecto se aloja en la organización de GitHub [Rate-M](https://github.com/Rate-M), que agrupa tres repositorios:

| Repositorio | Contenido | Enlace |
|---|---|---|
| `ratem-backend` | Código del backend: API REST, WebSockets, modelos y servicios externos | https://github.com/Rate-M/ratem-backend.git |
| `Frontend` | Código del frontend: aplicación Vue 3 | https://github.com/Rate-M/Frontend.git |
| `Documentation` | Documentación del proyecto: un archivo `.md` para el backend y otro para el frontend (este) | https://github.com/Rate-M/Documentation.git |

## Contenido

1. [Visión del producto](#1-visión-del-producto)
2. [Metodología de desarrollo: Scrum](#2-metodología-de-desarrollo-scrum)
3. [Requisitos](#3-requisitos)
4. [Análisis de riesgos](#4-análisis-de-riesgos)
5. [Planificación](#5-planificación)
6. [Diseño de la solución](#6-diseño-de-la-solución)
7. [Control de versiones](#7-control-de-versiones)
8. [Maquetado](#8-maquetado)
9. [Diagramas](#9-diagramas)
10. [Prototipado](#10-prototipado)
11. [Guía del frontend](#11-guía-del-frontend)

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

**Fuera del alcance:** aplicaciones móviles nativas (el sistema es una aplicación web responsiva), pagos y funciones premium, videollamadas o audio en el chat, notificaciones push nativas, recomendación de perfiles con inteligencia artificial y soporte multilenguaje (la aplicación está solo en español).

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
| R-02 | Curva de aprendizaje de las nuevas tecnologías | 4 | 5 | 20 Extremo | Semana de capacitación y spike antes del Sprint 1; plantilla base del proyecto |
| R-12 | Proveedor de verificación inaccesible por costo | 4 | 5 | 20 Extremo | Sandbox del proveedor; servicio intercambiable; plan B de verificación manual |
| R-14 | Falla o imposibilidad de probar la geolocalización | 4 | 4 | 16 Extremo | Radio y ventana configurables; pruebas simuladas y con 2 teléfonos; confirmación manual |
| R-18 | Subestimación del esfuerzo de las historias | 4 | 4 | 16 Extremo | Medir la velocidad real desde el Sprint 1; proteger el MVP |
| R-16 | Incumplimiento de la LFPDPPP por falta de asesoría | 3 | 5 | 15 Alto | Aviso basado en el texto vigente y revisión docente; consentimientos separados |
| R-17 | Sin usuarios reales para validar reseñas y rachas | 5 | 3 | 15 Alto | Seeders y piloto cerrado en el Sprint 6 |
| R-05 | Retraso por otras responsabilidades | 4 | 3 | 12 Alto | Velocidad conservadora; el Sprint 6 funciona como margen |
| R-07 | Fallos al implementar APIs externas | 3 | 4 | 12 Alto | Servicio aislado por API con interfaz común y mocks |
| R-03 | Cambios de diseño en el frontend (nivel medio) | 4 | 2 | 8 Medio | Validar los wireframes con el Product Owner antes de programar cada pantalla |
| R-01 | Abandono de un integrante | 2 | 5 | 10 Alto | Revisión cruzada y programación en pares |
| R-08 | Fallos de seguridad y filtración de información | 2 | 5 | 10 Alto | Validación de entradas, HTTPS y credenciales fuera del repositorio |

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

### 6.1 Estructura del sitio

La aplicación es una SPA construida con Vue 3 y Vue Router que consume la API REST del backend y un canal de WebSockets para el chat. Las rutas privadas requieren sesión y verificación aprobada, y las de moderación requieren el rol de moderador.

| Zona | Rutas |
|---|---|
| Pública | `/`, `/registro`, `/login`, `/recuperar`, `/legal/privacidad`, `/legal/terminos` |
| Privada (usuario verificado) | `/verificacion`, `/descubrir`, `/perfil`, `/perfil/editar`, `/usuarios/:id`, `/matches`, `/chat/:matchId`, `/lugares`, `/citas`, `/citas/:id`, `/citas/:id/resena`, `/configuracion` |
| Moderación | `/moderacion/resenas`, `/moderacion/lugares` |

**Flujo de navegación principal:** Inicio → Registro → Verificación → Mi perfil → Descubrir → Match → Chat (racha) → Invitar a cita (mapa) → Cita confirmada → Reseña → Perfil público actualizado.

### 6.2 Tecnologías del frontend

| Tecnología | Uso | Justificación |
|---|---|---|
| Vue 3 + TypeScript + Pinia | Frontend | Experiencia previa del equipo con Vue (reduce el riesgo R-02); componentes reutilizables y tipado. Pinia gestiona la sesión y el estado del chat |
| Vue Router | Rutas y guardas | Protege las vistas según sesión, verificación y rol |
| Socket.io-client | Chat | Reconexión automática y salas por match; cumple la entrega en menos de 1 s |
| Google Maps JavaScript API | Mapa de lugares | Visualización de lugares por categoría |
| Geolocation API del navegador | Confirmación de cita | Ubicación solicitada solo dentro de la ventana de la cita |
| Vite | Compilación | Servidor de desarrollo rápido y compilación optimizada |
| ESLint | Calidad | Linter sin errores exigido por la Definition of Done |
| GitHub + Vercel | Repositorio y despliegue | Plan gratuito suficiente; despliegue automático desde `main` |

---

## 7. Control de versiones

### 7.1 Justificación de la herramienta y la plataforma

**Git** se eligió porque es distribuido: cada integrante trabaja con una copia completa del historial y crea ramas en segundos, lo que permite una rama por tarea de Jira que se combina a `main` solo cuando cumple la Definition of Done. Se descartó Subversion por ser centralizado y no ofrecer pull requests para la revisión de código que exige la Definition of Done.

**GitHub** se eligió frente a GitLab y Bitbucket porque reúne en un solo lugar la revisión de código, la integración continua y la evidencia de participación, mientras que el backlog y los sprints se gestionan en Jira. Además es la plataforma que el equipo ya adoptó, lo que reduce el riesgo R-02. GitLab se descartó por la curva de aprendizaje adicional y Bitbucket porque el equipo ya trabajaba con GitHub y de la suite de Atlassian solo necesita Jira.

### 7.2 Estructura de repositorios

El proyecto se aloja en la organización de GitHub **Rate-M**, con tres repositorios (ver la tabla del inicio). El backend y el frontend están separados porque se despliegan en servicios distintos (Render y Vercel), tienen dependencias y pruebas independientes y reflejan la división de roles (Dev Backend y Dev Frontend). La documentación se mantiene en un repositorio propio. El punto de integración entre el código es el contrato de la API (ruta, parámetros y respuesta), que se define antes de implementar cada historia (mitigación de R-04).

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
2. La descripción incluye la tarea de Jira, los cambios realizados, cómo probarlos, **capturas de pantalla** y la lista de verificación de la Definition of Done.
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
| Verificaciones automáticas obligatorias | Sí (ESLint, revisión de tipos y compilación) |
| Requerir rama actualizada antes de combinar | Sí |
| Push directo | No |
| Force push y eliminación de la rama | No |
| Aplicar las reglas también a administradores | Sí |

**Archivos de exclusión** (`.gitignore` del repositorio `Frontend`):

```gitignore
# Dependencias
node_modules/

# Variables de entorno
.env
.env.*
!.env.example
*.local

# Compilados, caché y cobertura
dist/
dist-ssr/
.vite/
*.tsbuildinfo
coverage/

# Registros
logs/
*.log

# Sistema operativo y editor
.DS_Store
Thumbs.db
.idea/
.vscode/*
!.vscode/extensions.json
```

**Credenciales y llaves.** Ninguna credencial se sube al repositorio. El repositorio incluye un archivo `.env.example` con los nombres de las variables y sin valores; los valores reales se guardan en el `.env` local de cada integrante (ignorado por Git), en los secretos de GitHub Actions y en las variables de entorno de Vercel.

| Variable | Descripción |
|---|---|
| `VITE_API_URL` | URL base de la API REST |
| `VITE_SOCKET_URL` | URL del servidor de WebSockets del chat |
| `VITE_GOOGLE_MAPS_API_KEY` | Llave del mapa. Las variables `VITE_` quedan visibles en el navegador, por lo que esta llave se restringe por dominio en la consola de Google |

Si una credencial se publica por error, se revoca y se rota de inmediato, y se limpia el historial cuando es necesario. Dependabot está activo para las alertas de dependencias.

**Integración continua**

| Evento | Pasos del flujo |
|---|---|
| Pull request hacia `main` | Instalar dependencias (`npm ci`), ejecutar ESLint, revisar tipos con `vue-tsc --noEmit` y compilar con `npm run build` |
| Push a `main` | Despliegue automático al ambiente de pruebas en Vercel |

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
| Verificaciones automáticas | ESLint, revisión de tipos y compilación en cada pull request | Pestaña Actions |

---

## 8. Maquetado

### 8.1 Descripción y enfoque

El maquetado de la interfaz ya está elaborado como dibujos digitales de las pantallas, reunidos en un archivo PDF adjunto en el Drive del proyecto. Es un entregable de diseño: todavía no se ha escrito código de la interfaz, y la implementación con Vue 3 y TypeScript se realizará en los sprints a partir de estas maquetas. Los dibujos parten de un diseño móvil primero (desde 360 px de ancho, RNF-09), se amplían a tableta y escritorio y siguen buenas prácticas de accesibilidad (WCAG 2.1 AA, RNF-10), de modo que la interfaz se pueda implementar sin rediseñarla. Se entregaron 14 pantallas (sección 8.5) y su prototipo navegable está en Figma (sección 10).

| Elemento de la interfaz | Criterio de diseño |
|---|---|
| Encabezado y navegación | Logotipo y navegación principal visibles en cada vista, con una opción clara para saltar al contenido |
| Contenido principal | Cada vista tiene un contenido principal con título de página y bloques temáticos |
| Tarjetas | Perfil, reseña, lugar y cita se presentan como tarjetas autocontenidas |
| Formularios | Todos los campos tienen etiqueta visible; los grupos (consentimientos, preferencias) se agrupan bajo un título |
| Listas | Matches, conversaciones, lugares y reseñas se presentan como listas |
| Fechas y horas | Los mensajes y las citas muestran la fecha y la hora en un formato legible |
| Botones y enlaces | Los botones realizan acciones (dar “me gusta”, enviar); los enlaces permiten navegar entre pantallas |
| Imágenes | Fotos con texto alternativo previsto y formato WebP |
| Mensajes de estado | Los errores de validación, los nuevos mensajes del chat y los cambios de estado de la verificación se muestran de forma visible y comprensible |

### 8.2 Sistema de diseño

La identidad visual parte de la paleta y la tipografía definidas en la sección 7.4 de la Unidad 1 y se aplica de la misma forma en todas las pantallas dibujadas. Al implementarse, estos valores se definirán una sola vez como variables compartidas, de modo que un cambio de color o de tamaño se refleje en toda la aplicación.

| Color | Valor | Uso | Contraste |
|---|---|---|---|
| Primario | #E8505B | Acentos, íconos, bordes y títulos grandes | 3.7:1 sobre blanco (solo elementos grandes) |
| Primario intenso | #C93E49 | Fondo de botones con texto blanco y enlaces | 4.9:1 con texto blanco |
| Secundario | #1F3864 | Insignia de verificación, encabezados y navegación | 11.6:1 sobre blanco |
| Éxito | #2E9D6A | Cita confirmada, verificación aprobada y racha cumplida | 5.1:1 con texto #1A1A1A |
| Error | #B3261E | Mensajes de error de validación | 6.5:1 sobre blanco |
| Fondo / superficie | #FFFFFF / #F5F5F5 | Fondo de la página y de las tarjetas | — |
| Texto | #1A1A1A | Texto principal | 17:1 sobre blanco |
| Texto auxiliar | #6B6B6B | Texto auxiliar y marcadores de posición | 5.3:1 sobre blanco |

El coral #E8505B no alcanza el contraste mínimo de 4.5:1 con texto blanco pequeño; por eso los botones con texto blanco usan la variante más oscura, y el coral original se reserva para elementos grandes y gráficos (RNF-10).

| Elemento | Fuente | Tamaño | Peso |
|---|---|---|---|
| Título de página (`h1`) | Poppins | 1.75 rem | SemiBold 600 |
| Subtítulo de sección (`h2`) | Poppins | 1.375 rem | SemiBold 600 |
| Título de tarjeta (`h3`) | Poppins | 1.125 rem | SemiBold 600 |
| Texto de cuerpo | Inter | 1 rem (interlineado 1.5) | Regular 400 |
| Etiquetas de campos y botones | Inter | 1 rem | Medium 500 |
| Texto auxiliar y mensajes de ayuda | Inter | 0.875 rem | Regular 400 |

Los campos de formulario tienen forma de píldora, las tarjetas usan esquinas redondeadas y sombra suave, y el foco del teclado se indica siempre con un contorno visible de 2 px en el color secundario. Estas reglas se aplican igual en todas las pantallas dibujadas para dar una experiencia consistente.

### 8.3 Componentes de la interfaz

Las maquetas se construyen con un conjunto de elementos reutilizables, que se identifican desde el diseño para implementarlos después como componentes de Vue y mantener la consistencia entre pantallas.

| Componente | Tipo | Descripción | Dónde se usa |
|---|---|---|---|
| Botón | Base | Primario, secundario y de texto, con estados de carga y deshabilitado | Todas las pantallas |
| Campo de texto | Base | Campo con etiqueta, ayuda y mensaje de error asociados, en forma de píldora | Registro, login, perfil |
| Tarjeta | Base | Contenedor con esquinas redondeadas y sombra | Todas las pantallas |
| Alerta | Base | Mensajes de éxito, advertencia y error | Formularios y estados |
| Carga de fotos | Dominio | Carga de 1 a 6 fotos con vista previa y reordenamiento | Perfil |
| Tarjeta de perfil | Dominio | Fotos, nombre, edad, biografía, insignia y calificación promedio | Descubrir, perfil público |
| Burbuja de mensaje | Dominio | Mensaje con hora e indicador de leído | Chat |
| Indicador de racha | Dominio | “Racha: X de Y días” y estado del botón “Invitar a cita” | Chat, conversaciones |
| Tarjeta de lugar y mapa | Dominio | Tarjeta de lugar y mapa con filtros por categoría | Lugares, invitación |
| Formulario de invitación | Dominio | Selección de lugar, fecha y hora de la cita | Citas |
| Calificación con estrellas | Dominio | Escala de 1 a 5 con descripción de cada estrella, operable con teclado | Reseñas, perfil público |
| Cola de moderación | Dominio | Lista, detalle y acciones de la cola de moderación | Moderación |

### 8.4 Distribución responsiva

| Punto de quiebre | Ancho | Distribución |
|---|---|---|
| Móvil (base) | 360 a 767 px | Una columna; navegación inferior fija; tarjetas y formularios a todo el ancho con márgenes de 1 rem |
| Tableta | 768 a 1023 px | Lista de conversaciones y chat en dos columnas; lugares en cuadrícula de dos columnas |
| Escritorio | 1024 px en adelante | Barra lateral de navegación y contenido con ancho máximo de 1200 px; el mapa se muestra junto a la lista de lugares |

Se usan tres plantillas de página: la pública (tarjeta centrada, para registro, login y legales), la privada (navegación y contenido, para usuarios verificados) y la variante de moderación, que agrega el panel de acciones del moderador.

### 8.5 Pantallas maquetadas

Se entregaron las 14 pantallas de la tabla, cada una asociada a la ruta de la estructura del sitio. Las vistas que no aparecen aquí (recuperar contraseña, aviso de privacidad y términos, configuración y moderación) no forman parte de esta entrega. La implementación en código de cada vista se hará en el sprint indicado, a partir de la maqueta.

| Pantalla | Ruta | Historias | Sprint |
|---|---|---|---|
| Landing page | `/` | — | 1 |
| Iniciar sesión | `/login` | US-02 | 1 |
| Crear cuenta | `/registro` | US-01, US-33, US-34 | 1 |
| Verificación de cuenta | `/verificacion` | US-04 a US-06 | 2 |
| Mi perfil | `/perfil` y `/perfil/editar` | US-07, US-08 | 1 |
| Perfil de usuario | `/usuarios/:id` | US-09 | 2 |
| Descubrir | `/descubrir` | US-10, US-11 | 3 |
| Match | `/matches` | US-12 | 3 |
| Mensajes | `/matches` y `/chat/:matchId` | US-13 a US-15 | 3 y 4 |
| Lugares | `/lugares` | US-19 | 4 y 6 |
| Filtros | `/lugares` | US-20 | 4 y 6 |
| Proponer lugar | `/lugares` | US-21 | 4 y 6 |
| Invitar cita | `/citas` | US-22 | 4 y 5 |
| Calificar cita | `/citas/:id/resena` | US-26, US-27 | 5 |

Prototipo navegable de estas pantallas en Figma: https://www.figma.com/design/uqQhPFBjJPSIJo5VGT2QFG/Ratem

### 8.6 Criterios de verificación del maquetado

Estos criterios guían la revisión de las maquetas y se comprueban sobre el producto cuando cada vista se implemente en código.

| Criterio | Cómo se verifica | Requisito |
|---|---|---|
| Accesibilidad con puntaje de 90 o más | Auditoría con Lighthouse en cada vista implementada | RNF-10 |
| Diseño responsivo desde 360 px | Herramientas del navegador y dispositivos reales | RNF-09 |
| Compatibilidad con las dos últimas versiones de Chrome, Edge, Firefox y Safari | Pruebas manuales por navegador al cierre de cada sprint | RNF-09 |
| Navegación completa con teclado y foco visible | Recorrido de cada formulario sin usar el mouse | RNF-10 |
| HTML válido y semántico | Validación del marcado y revisión en el pull request | Definition of Done |
| Imágenes optimizadas (WebP, menos de 200 KB en la pantalla de inicio) | Pestaña de red del navegador | Unidad 1, sección 7.4 |

---

## 9. Diagramas

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

El modelo de datos se documenta en el diccionario de datos del archivo `.md` del backend.

---

## 10. Prototipado

### 10.1 Objetivo y alcance

El prototipo es una representación navegable de media fidelidad de las pantallas principales y está elaborado en **Figma** a partir de las maquetas de la sección 8: https://www.figma.com/design/uqQhPFBjJPSIJo5VGT2QFG/Ratem. Define la distribución de cada pantalla, el flujo de navegación y los estados de la experiencia de usuario antes de programar. Su propósito es validar con el Product Owner cada pantalla antes de implementarla, lo que mitiga el riesgo de cambios de diseño en el frontend (R-03).

El prototipo incluye las pantallas: Landing page, iniciar sesión, crear cuenta, descubrir, mensajes, lugares, perfil de usuario, mi perfil, calificar cita, verificación de cuenta, invitar cita, proponer lugar, filtros y match.

### 10.2 Flujo de navegación

El flujo principal que recorre un usuario nuevo coincide con el flujo que valida el MVP:

1. **Inicio:** conoce la propuesta de valor y elige Registrarme o Iniciar sesión.
2. **Registro:** crea su cuenta y acepta el aviso de privacidad y los consentimientos.
3. **Verificación:** carga su identificación y una selfie, y ve el estado de su verificación.
4. **Mi perfil:** completa sus fotos, biografía y preferencias.
5. **Descubrir:** revisa perfiles uno a uno y da “me gusta” o “pasar”.
6. **Match:** recibe el aviso cuando el interés es mutuo y accede al chat.
7. **Chat con racha:** conversa y avanza la racha hasta el umbral.
8. **Invitar a cita:** elige un lugar en el mapa, la fecha y la hora.
9. **Cita confirmada:** la cita se confirma por GPS o de forma manual.
10. **Reseña:** ambos califican y, al publicarse, el perfil público se actualiza.

| Camino alterno | Disparador | Pantalla de destino |
|---|---|---|
| Verificación rechazada | El proveedor rechaza el documento o detecta un menor de edad | Verificación, con el motivo y las opciones para reintentar o contactar al moderador |
| Racha no cumplida | No se alcanzó el umbral de 3 a 5 días | Chat, con el botón “Invitar a cita” deshabilitado y los días que faltan |
| Racha perdida | Transcurre un día sin mensajes de ambos | Chat, con el aviso de racha reiniciada |
| Falla del GPS | La confirmación automática no se completa | Detalle de la cita, con la opción de confirmación manual por ambos |
| Invitación rechazada o reprogramada | El invitado rechaza o propone otra hora | Citas, con el nuevo estado y las acciones disponibles |
| Reseña fuera de plazo | Pasaron 7 días de la cita | Detalle de la cita, con el aviso de que la reseña expiró |
| Match deshecho | Un usuario deshace el match | Lista de matches, sin la conversación |

### 10.3 Distribución de pantallas

| Pantalla | Distribución | Elementos clave | Estados considerados |
|---|---|---|---|
| Inicio | Encabezado, bloque principal, tres beneficios y pie de página | Propuesta de valor en una frase, botones Registrarme e Iniciar sesión | Visitante sin sesión |
| Registro y login | Tarjeta centrada con un solo formulario | Campos con etiqueta, reglas de contraseña, casilla de aceptación legal | Error de validación, cargando, éxito |
| Verificación | Pasos numerados en una columna y estado al final | Instrucciones para fotografiar la identificación y la selfie, aviso de que no se guardan las imágenes | Pendiente, aprobada, rechazada |
| Mi perfil y edición | Galería de fotos arriba y secciones de datos debajo | Carga de fotos, biografía con contador de 500 caracteres, preferencias de búsqueda | Vacío, edición, guardado |
| Descubrir | Una tarjeta de perfil a pantalla completa con acciones fijas abajo | Fotos, datos, insignia de verificación, calificación, botones “me gusta” y “pasar” | Sin más perfiles, cargando |
| Matches y chat | Lista de conversaciones; en tableta y escritorio, lista y chat lado a lado | Racha por conversación, mensajes, indicador de leído y botón “Invitar a cita” | Sin matches, racha cumplida o no, desconectado |
| Mapa de lugares | Mapa con filtros arriba y lista de tarjetas | Filtro por categoría, nombre, dirección y foto de cada lugar | Cargando, sin resultados, error de la API |
| Citas | Lista de citas y detalle con línea de estado | Lugar, fecha y hora, acciones de aceptar, rechazar o proponer otra hora, confirmación | Pendiente, aceptada, confirmada, no confirmada |
| Reseña | Formulario corto con reglas visibles antes de escribir | Escala de 1 a 5 con descripción de cada estrella, comentario de máximo 300 caracteres | Esperando a la otra persona, publicada, rechazada |
| Perfil público | Encabezado con fotos, calificación promedio y lista de reseñas | Reseñas publicadas, botón de reporte | Sin reseñas todavía |
| Moderación | Lista de la cola y panel de detalle con acciones | Motivo del reporte, contenido, acciones aprobar, editar y ocultar | Cola vacía |

### 10.4 Experiencia de usuario

| Principio | Cómo se aplica en el prototipo |
|---|---|
| Claridad en los errores | Cada mensaje explica qué pasó y cómo corregirlo, sin términos técnicos |
| Transparencia en el uso de datos | Las pantallas de verificación y de citas explican por qué se pide la información, que no se guardan las imágenes y que la ubicación solo se solicita durante la cita |
| Motivar sin presionar | La racha se presenta como progreso (“Racha: X de Y días”), sin lenguaje que apure al usuario a tener una cita |
| Retroalimentación inmediata | Todas las acciones muestran un estado de carga, éxito o error |
| Estados vacíos útiles | Cuando no hay contenido, la pantalla explica el siguiente paso en lugar de quedar en blanco |
| Lenguaje incluyente | Se usa “persona” y “tu match” en lugar de términos con género, en español de México y con trato de tú |
| Accesibilidad desde el diseño | Contraste, tamaños táctiles de al menos 44 px y navegación por teclado se definen en el prototipo, no al final |

### 10.5 Validación del prototipo

- **Con el Product Owner:** cada pantalla se revisa antes de programarla; los cambios posteriores se registran como una historia nueva en el Product Backlog y no entran al sprint en curso (contingencia del riesgo R-03).
- **Con usuarios:** se realiza una prueba de usabilidad con 5 personas en la que un usuario nuevo debe completar el registro y el perfil en menos de 5 minutos sin ayuda (RNF-08).
- **Con el equipo:** los componentes se identifican en el prototipo de Figma para reutilizarlos en Vue y mantener la consistencia (sección 8.3).

---

## 11. Guía del frontend

Todavía no se ha escrito código de la interfaz; esta guía aplica una vez creado el proyecto base en el repositorio `Frontend` y se confirma conforme se implementan las vistas en cada sprint.

### 11.1 Requisitos

Node.js 20 o superior, npm y el backend en ejecución (ver el archivo `.md` del backend).

### 11.2 Instalación y ejecución

```bash
git clone https://github.com/Rate-M/Frontend.git
cd Frontend
npm ci
cp .env.example .env     # completar los valores
npm run dev
```

| Script | Descripción |
|---|---|
| `npm run dev` | Servidor de desarrollo con recarga en caliente |
| `npm run build` | Compilación de producción (incluye la revisión de tipos) |
| `npm run preview` | Vista previa de la compilación |
| `npm run lint` | Análisis con ESLint |
| `npm run type-check` | Revisión de tipos con `vue-tsc --noEmit` |

### 11.3 Despliegue

El ambiente de pruebas se despliega automáticamente en Vercel al hacer push a `main`. Las variables `VITE_API_URL`, `VITE_SOCKET_URL` y `VITE_GOOGLE_MAPS_API_KEY` se configuran en el panel del proyecto.

---

## Referencias

- The Competitive Intelligence Unit (2026). Uso de aplicaciones de citas en México.
- Norton (2025). Ciberdelincuencia en aplicaciones de citas en México.
- Sumsub, citado en El CEO (2026). Fraudes en aplicaciones de citas en México.
- GBG (s. f.). Estudio sobre verificación de identidad en aplicaciones de citas (Reino Unido).
