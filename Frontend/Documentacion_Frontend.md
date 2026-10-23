# RateM Dating · Frontend

Aplicación web (SPA) de **RateM Dating**, una aplicación de citas con verificación de identidad y reseñas mutuas.

**Asignatura:** Formulación de Proyectos de Tecnología · Desarrollo Web Integral · **Grado y sección:** 7 B
**Profesor:** Jesús Del Toro Robledo · **Metodología:** Scrum
**Equipo:** Nidia Elizabeth Ruiz Barraza · Angel Gabriel Tovar Campirano · Natalia Cabral Véliz · Mario Antonio Portillo Romero
**Stack del frontend:** Vue 3 · TypeScript · Vue Router · Pinia · Socket.io-client · Vite

## Contenido

1. [Visión del producto](#1-visión-del-producto)
2. [Metodología de desarrollo: Scrum](#2-metodología-de-desarrollo-scrum)
3. [Requisitos](#3-requisitos)
4. [Análisis de riesgos](#4-análisis-de-riesgos)
5. [Planificación](#5-planificación)
6. [Diseño de la solución](#6-diseño-de-la-solución)
7. [Control de versiones](#7-control-de-versiones)
8. [Maquetado](#8-maquetado)
9. [Prototipado](#9-prototipado)
10. [Guía del frontend](#10-guía-del-frontend)

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

Scrum organiza el desarrollo en sprints de 2 semanas que entregan un incremento funcional, priorizado según el valor para el usuario. La documentación es incremental: el Product Backlog se refina de forma continua y la documentación técnica se genera conforme existen los módulos.

**Justificación.** Se eligió Scrum frente a Cascada y Kanban porque combina iteraciones de duración fija (necesarias para el plazo académico), priorización continua del backlog (necesaria por la dependencia de APIs externas cuyo costo y disponibilidad no son seguros) y eventos de inspección que generan evidencia en cada sprint.

### 2.1 Equipo y roles

| Rol | Integrante | Suplente |
|---|---|---|
| Product Owner | Nidia Elizabeth Ruiz Barraza | Angel Gabriel Tovar Campirano |
| Scrum Master | Nidia Elizabeth Ruiz Barraza | Product Owner |
| Dev Backend | Nidia Elizabeth Ruiz Barraza | Mario Antonio Portillo Romero |
| Dev Frontend | Angel Gabriel Tovar Campirano | Natalia Cabral Véliz |
| QA / Documentación | Angel Gabriel Tovar Campirano | Nidia Elizabeth Ruiz Barraza |

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

El backlog tiene **130 puntos de historia en 35 historias**, organizadas en 10 épicas. Las historias siguen el formato «Como [rol], quiero [acción], para [beneficio]» y sus criterios de aceptación están en el documento de proyecto de la Unidad 1.

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

1. El código está en el repositorio (rama integrada a `develop` mediante pull request revisado por otro miembro del equipo).
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
| GitHub + Vercel | Repositorio y despliegue | Plan gratuito suficiente; despliegue automático desde `develop` |

---

## 7. Control de versiones

### 7.1 Justificación de la herramienta y la plataforma

**Git** se eligió porque es distribuido: cada integrante trabaja con una copia completa del historial y crea ramas en segundos, lo que permite una rama por historia de usuario. Se descartó Subversion por ser centralizado y no ofrecer pull requests para la revisión de código que exige la Definition of Done.

**GitHub** se eligió frente a GitLab y Bitbucket porque reúne en un solo lugar el backlog (Issues y Projects), la revisión de código, la integración continua (Actions) y la evidencia de participación, sin sumar herramientas. Además es la plataforma que el equipo ya adoptó, lo que reduce el riesgo R-02. GitLab se descartó por la curva de aprendizaje adicional y Bitbucket por su dependencia de Jira para el backlog.

### 7.2 Estructura de repositorios

| Repositorio | Contenido | Despliegue |
|---|---|---|
| `ratem-dating-backend` | API REST, WebSockets, modelos y servicios externos | Render |
| `ratem-dating-frontend` (este) | Aplicación Vue 3 | Vercel |

El punto de integración es el contrato de la API, que se define antes de implementar cada historia (mitigación de R-04).

### 7.3 Flujo de trabajo

Se usa **Git Flow simplificado**: `main`, `develop` y una rama por historia. Se descartó GitHub Flow porque no tiene rama de integración y el ambiente de pruebas se despliega desde `develop`; se descartó Git Flow completo por ser excesivo para 6 sprints y 4 integrantes.

| Rama | Propósito | Se integra a | Ejemplo |
|---|---|---|---|
| `main` | Código estable; recibe el incremento de cada sprint y lleva etiqueta de versión | — | `main` |
| `develop` | Integración del sprint y origen del despliegue de pruebas | `main` (cierre de sprint) | `develop` |
| `feature/*` | Una historia de usuario | `develop` | `feature/US-07-formulario-perfil` |
| `fix/*` | Corrección de un defecto en `develop` | `develop` | `fix/US-02-validacion-login` |
| `docs/*` | Documentación | `develop` | `docs/US-09-componentes-perfil` |
| `chore/*` | Configuración y dependencias | `develop` | `chore/configuracion-eslint` |
| `hotfix/*` | Error crítico en `main` | `main` y `develop` | `hotfix/ruta-protegida` |

**Convención de commits** (Conventional Commits en español): `tipo(US-XX): descripción`, en presente, con máximo 72 caracteres y un solo cambio lógico por commit.

| Tipo | Uso | Ejemplo |
|---|---|---|
| `feat` | Nueva funcionalidad | `feat(US-07): agrega carga de fotos al perfil` |
| `fix` | Corrección | `fix(US-02): corrige el mensaje de credenciales incorrectas` |
| `docs` | Documentación | `docs(US-07): documenta el componente PhotoUploader` |
| `style` | Formato o estilos sin cambio de lógica | `style(US-08): aplica campos en forma de píldora` |
| `refactor` | Reestructura sin cambiar el comportamiento | `refactor(US-13): separa el servicio del socket` |
| `chore` / `ci` | Configuración y flujos de integración continua | `ci: agrega el flujo de revisión de tipos` |

**Políticas de combinación**

| Combinación | Método | Condiciones |
|---|---|---|
| Ramas de trabajo → `develop` | Merge commit | Pull request aprobado por un integrante distinto al autor, verificaciones en verde, rama actualizada y criterios de aceptación validados |
| `develop` → `main` | Merge commit | Solo al cierre del sprint, tras el Sprint Review aceptado por el Product Owner y con regresión aprobada; se crea la etiqueta |
| `hotfix/*` → `main` y `develop` | Merge commit | Revisión de un integrante; etiqueta de parche |

**Gestión de pull requests:** título `US-XX: descripción`, plantilla con historia, cambios, forma de probarlos, **capturas de pantalla** y lista de la Definition of Done; etiquetas `frontend`, tipo y `sprint-N`; vínculo al issue con `Closes #n`; revisión en máximo 24 horas hábiles; pull requests pequeños (menos de 400 líneas); y actualización previa con `git pull --rebase origin develop`. No se reescribe el historial de `main` ni de `develop`.

```bash
git switch develop
git pull origin develop
git switch -c feature/US-07-formulario-perfil
git add .
git commit -m "feat(US-07): agrega carga de fotos al perfil"
git pull --rebase origin develop
git push -u origin feature/US-07-formulario-perfil
# abrir el pull request hacia develop
```

**Versionado:** semántico, con etiquetas anotadas en `main` al cierre de cada sprint.

| Etiqueta | Sprint | Cierre | Contenido |
|---|---|---|---|
| `v0.1.0` | 1 | 09/10/2026 | Registro, inicio de sesión, consentimientos y perfil |
| `v0.2.0` | 2 | 23/10/2026 | Verificación de identidad y perfil público |
| `v0.3.0` | 3 | 06/11/2026 | Match y chat en tiempo real |
| `v0.4.0` | 4 | 20/11/2026 | Racha, mapa e invitación |
| `v0.5.0` | 5 | 04/12/2026 | MVP completo |
| `v1.0.0` | 6 | 18/12/2026 | Moderación completa y documentación final |

**Tablero:** GitHub Projects con las columnas Product Backlog, Sprint Backlog, En progreso, En revisión y Terminada; un milestone por sprint y un issue por historia con sus tareas (por ejemplo, T07.1 a T07.3).

### 7.4 Configuración del entorno de versiones

**Parámetros iniciales de Git** (cada integrante):

```bash
git config --global user.name "Nombre Apellido"
git config --global user.email "correo@institucional.edu.mx"
git config --global init.defaultBranch main
git config --global pull.rebase true
```

El archivo `.gitattributes` contiene `* text=auto eol=lf` para normalizar los saltos de línea.

**Control de accesos**

| Rol | Quién | Permisos |
|---|---|---|
| Administrador | Scrum Master | Configuración, protección de ramas y secretos |
| Escritura | Demás integrantes | Ramas de trabajo y pull requests (nunca aprobar el propio) |
| Lectura | Docente | Consulta del código, historial y tablero |

El repositorio es privado durante el desarrollo. Todos los integrantes usan verificación en dos pasos y autenticación con llave SSH (ed25519) o token personal con permisos mínimos.

**Protección de ramas (`main` y `develop`):** pull request obligatorio; 1 aprobación (en `main`, además la aceptación del Product Owner); aprobaciones descartadas al subir nuevos commits; conversaciones resueltas; verificaciones automáticas obligatorias (ESLint, revisión de tipos y compilación); rama actualizada antes de combinar; sin push directo, sin force push y sin eliminación; reglas aplicadas también a administradores.

**Archivos de exclusión** (`.gitignore`):

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

**Credenciales y llaves.** Ninguna credencial se sube al repositorio. Los valores reales viven en el `.env` local de cada integrante, en los secretos de GitHub Actions y en las variables de entorno de Vercel. El archivo `.env.example` documenta los nombres sin valores:

| Variable | Descripción |
|---|---|
| `VITE_API_URL` | URL base de la API REST |
| `VITE_SOCKET_URL` | URL del servidor de WebSockets del chat |
| `VITE_GOOGLE_MAPS_API_KEY` | Llave del mapa. Las variables `VITE_` quedan visibles en el navegador, por lo que esta llave se restringe por dominio en la consola de Google |

Si una credencial se publica por error, se revoca y se rota de inmediato. Dependabot está activo para las alertas de dependencias.

**Integración continua.** En cada pull request hacia `develop` o `main` se ejecuta `npm ci`, ESLint, `vue-tsc --noEmit` y `npm run build`. Al hacer push a `develop` se despliega automáticamente el ambiente de pruebas en Vercel.

---

## 8. Maquetado

### 8.1 Estándares y enfoque

El maquetado usa componentes de un solo archivo de Vue 3 (`<script setup lang="ts">`), estilos con alcance (`scoped`) y un sistema de variables CSS compartido. Se parte de un diseño móvil primero (desde 360 px) y se amplía con cuadrículas y flexbox. Se aplican HTML5 semántico, accesibilidad WCAG 2.1 AA (RNF-10) y unidades relativas (`rem`).

| Elemento | Uso en la aplicación |
|---|---|
| `<header>` y `<nav aria-label>` | Encabezado con logotipo y navegación principal etiquetada |
| `<main id="contenido">` | Contenido único de cada vista, con enlace “Saltar al contenido” |
| `<section>` y `<article>` | Bloques temáticos y tarjetas autocontenidas (perfil, reseña, lugar, cita) |
| `<form>`, `<label for>`, `<fieldset>` | Todos los campos tienen etiqueta asociada; los grupos usan `fieldset` |
| `<time datetime>` | Fechas y horas de mensajes y citas |
| `<img alt loading="lazy">` | Imágenes con texto alternativo, carga diferida y formato WebP |
| `aria-live` y `role="alert"` | Anuncian errores, mensajes nuevos y cambios de estado |

### 8.2 Sistema de diseño

La paleta y la tipografía provienen de la definición de contenido del documento de la Unidad 1. Se implementan como variables CSS con el prefijo `--rm-` en `src/styles/tokens.css`.

| Variable | Valor | Uso | Contraste |
|---|---|---|---|
| `--rm-color-primary` | #E8505B | Acentos, íconos, bordes y títulos grandes | 3.7:1 sobre blanco (solo elementos grandes) |
| `--rm-color-primary-strong` | #C93E49 | Fondo de botones con texto blanco | 4.9:1 con texto blanco |
| `--rm-color-secondary` | #1F3864 | Insignia de verificación y navegación | 11.6:1 sobre blanco |
| `--rm-color-success` | #2E9D6A | Cita confirmada, verificación aprobada y racha cumplida | 5.1:1 con texto #1A1A1A |
| `--rm-color-error` | #B3261E | Errores de validación | 6.5:1 sobre blanco |
| `--rm-color-bg` / `--rm-color-surface` | #FFFFFF / #F5F5F5 | Fondo de página y de tarjetas | — |
| `--rm-color-text` / `--rm-color-text-muted` | #1A1A1A / #6B6B6B | Texto principal y auxiliar | 17:1 / 5.3:1 sobre blanco |

El coral #E8505B no alcanza 4.5:1 con texto blanco pequeño; por eso los botones usan la variante más oscura y el coral original se reserva para elementos grandes y gráficos.

```css
:root {
  --rm-color-primary: #E8505B;
  --rm-color-primary-strong: #C93E49;
  --rm-color-secondary: #1F3864;
  --rm-color-success: #2E9D6A;
  --rm-color-error: #B3261E;
  --rm-color-bg: #FFFFFF;
  --rm-color-surface: #F5F5F5;
  --rm-color-text: #1A1A1A;
  --rm-color-text-muted: #6B6B6B;

  --rm-font-title: "Poppins", system-ui, sans-serif;
  --rm-font-text: "Inter", system-ui, sans-serif;

  --rm-space-1: 0.25rem;  --rm-space-2: 0.5rem;  --rm-space-3: 0.75rem;
  --rm-space-4: 1rem;     --rm-space-6: 1.5rem;  --rm-space-8: 2rem;

  --rm-radius-card: 1.25rem;
  --rm-radius-pill: 999px;
  --rm-shadow-card: 0 4px 16px rgb(26 26 26 / 8%);
}
```

| Elemento | Fuente | Tamaño | Peso |
|---|---|---|---|
| `h1` | Poppins | 1.75 rem | SemiBold 600 |
| `h2` | Poppins | 1.375 rem | SemiBold 600 |
| `h3` | Poppins | 1.125 rem | SemiBold 600 |
| Cuerpo | Inter | 1 rem (interlineado 1.5) | Regular 400 |
| Etiquetas y botones | Inter | 1 rem | Medium 500 |
| Texto auxiliar | Inter | 0.875 rem | Regular 400 |

Los campos de formulario tienen forma de píldora, las tarjetas usan esquinas redondeadas con sombra suave y el foco del teclado se muestra siempre con un contorno visible de 2 px.

### 8.3 Estructura y componentes

```text
src/
├── assets/              imágenes, logotipo e íconos
├── components/
│   ├── base/            RmButton, RmInput, RmCheckbox, RmCard,
│   │                    RmAlert, RmBadge, RmModal
│   ├── layout/          AppHeader, AppNav, AppFooter,
│   │                    PublicLayout, PrivateLayout
│   └── domain/          ProfileCard, PhotoUploader, ChatBubble,
│                        StreakIndicator, PlaceCard, DateInviteForm,
│                        RatingStars, ModerationQueue
├── views/               una vista por ruta
├── router/              rutas y guardas (sesión, verificación y rol)
├── stores/              estado global con Pinia (sesión, chat)
├── services/            cliente de la API REST y del socket
├── composables/         lógica reutilizable (geolocalización)
├── styles/              tokens.css, base.css y utilidades
└── types/               tipos de TypeScript del modelo de datos
```

| Componente | Descripción | Dónde se usa |
|---|---|---|
| `RmButton` | Botón primario, secundario y de texto, con estados de carga y deshabilitado | Todas las pantallas |
| `RmInput` | Campo con etiqueta, ayuda y error asociados, en forma de píldora | Registro, login, perfil |
| `PhotoUploader` | Carga de 1 a 6 fotos con vista previa y reordenamiento | Perfil |
| `ProfileCard` | Fotos, nombre, edad, biografía, insignia y calificación promedio | Descubrir, perfil público |
| `ChatBubble` | Mensaje con hora e indicador de leído | Chat |
| `StreakIndicator` | “Racha: X de Y días” y estado del botón “Invitar a cita” | Chat, conversaciones |
| `PlaceCard` y `PlacesMap` | Tarjeta de lugar y mapa con filtros por categoría | Lugares, invitación |
| `DateInviteForm` | Selección de lugar, fecha y hora | Citas |
| `RatingStars` | Escala de 1 a 5 operable con teclado | Reseñas, perfil público |
| `ModerationQueue` | Lista, detalle y acciones de moderación | Moderación |

### 8.4 Distribución responsiva

| Punto de quiebre | Ancho | Distribución |
|---|---|---|
| Móvil (base) | 360 a 767 px | Una columna; navegación inferior fija; tarjetas y formularios a todo el ancho |
| Tableta | 768 a 1023 px | Lista de conversaciones y chat en dos columnas; lugares en dos columnas |
| Escritorio | 1024 px en adelante | Barra lateral y contenido con ancho máximo de 1200 px; mapa junto a la lista |

Se usan tres plantillas: `PublicLayout` (tarjeta centrada), `PrivateLayout` (navegación y contenido) y la variante de moderación con panel de acciones.

### 8.5 Pantallas maquetadas

| Ruta | Vista | Componentes principales | Historias | Sprint |
|---|---|---|---|---|
| `/` | Inicio | AppHeader, propuesta de valor, 3 beneficios | — | 1 |
| `/registro` | Registro | RmInput, aceptación legal, consentimientos | US-01, US-33, US-34 | 1 |
| `/login` | Inicio de sesión | RmInput, RmButton, RmAlert | US-02 | 1 |
| `/recuperar` | Recuperar contraseña | RmInput, RmAlert | US-03 | 1 |
| `/legal/*` | Aviso y términos | Texto estructurado | US-33 | 1 |
| `/perfil`, `/perfil/editar` | Mi perfil y edición | PhotoUploader, formulario, preferencias | US-07, US-08 | 1 |
| `/configuracion` | Configuración | Consentimientos revocables, eliminar cuenta | US-34, US-35 | 1 y 2 |
| `/verificacion` | Verificación | Captura de identificación y selfie, insignia | US-04 a US-06 | 2 |
| `/usuarios/:id` | Perfil público | ProfileCard, RatingStars, reseñas | US-09 | 2 |
| `/descubrir` | Descubrir | ProfileCard, “me gusta” y “pasar” | US-10, US-11 | 3 |
| `/matches` | Matches | Conversaciones, aviso de match | US-12, US-14, US-15 | 3 |
| `/chat/:matchId` | Chat | ChatBubble, StreakIndicator | US-13, US-16 a US-18 | 3 y 4 |
| `/lugares` | Mapa de lugares | PlacesMap, PlaceCard, filtro | US-19, US-20 | 4 y 6 |
| `/citas`, `/citas/:id` | Citas | DateInviteForm, confirmación | US-22 a US-25 | 4 y 5 |
| `/citas/:id/resena` | Reseña | RatingStars, comentario | US-26, US-27 | 5 |
| `/moderacion/*` | Moderación | ModerationQueue | US-21, US-31 | 6 |

### 8.6 Criterios de verificación

| Criterio | Cómo se verifica | Requisito |
|---|---|---|
| Accesibilidad de 90 puntos o más | Auditoría con Lighthouse en cada vista | RNF-10 |
| Diseño responsivo desde 360 px | Herramientas del navegador y dispositivos reales | RNF-09 |
| Compatibilidad con Chrome, Edge, Firefox y Safari | Pruebas manuales al cierre de cada sprint | RNF-09 |
| Navegación completa con teclado y foco visible | Recorrido de cada formulario sin mouse | RNF-10 |
| Imágenes optimizadas (WebP, menos de 200 KB en la pantalla de inicio) | Pestaña de red del navegador | Documento de la Unidad 1 |

---

## 9. Prototipado

El prototipo es una representación navegable de media fidelidad de las pantallas principales. Define la distribución, el flujo de navegación y los estados de la experiencia de usuario antes de programar, y se valida con el Product Owner pantalla por pantalla (mitigación del riesgo R-03). Los wireframes se anexan al documento de la Unidad 1.

### 9.1 Flujo de navegación

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
| Verificación rechazada | El proveedor rechaza el documento o detecta un menor de edad | Verificación, con el motivo y opciones para reintentar |
| Racha no cumplida | No se alcanzó el umbral de 3 a 5 días | Chat, con el botón “Invitar a cita” deshabilitado y los días restantes |
| Racha perdida | Pasa un día sin mensajes de ambos | Chat, con el aviso de racha reiniciada |
| Falla del GPS | La confirmación automática no se completa | Detalle de la cita, con confirmación manual |
| Invitación rechazada o reprogramada | El invitado rechaza o propone otra hora | Citas, con el nuevo estado |
| Reseña fuera de plazo | Pasaron 7 días de la cita | Detalle de la cita, con el aviso de expiración |
| Match deshecho | Un usuario deshace el match | Lista de matches, sin la conversación |

### 9.2 Distribución de pantallas

| Pantalla | Distribución | Estados considerados |
|---|---|---|
| Inicio | Encabezado, bloque principal, tres beneficios y pie de página | Visitante sin sesión |
| Registro y login | Tarjeta centrada con un solo formulario | Error de validación, cargando, éxito |
| Verificación | Pasos numerados en una columna y estado al final | Pendiente, aprobada, rechazada |
| Mi perfil y edición | Galería de fotos arriba y secciones de datos debajo | Vacío, edición, guardado |
| Descubrir | Tarjeta de perfil a pantalla completa con acciones fijas abajo | Sin más perfiles, cargando |
| Matches y chat | Lista de conversaciones; en pantallas grandes, lista y chat lado a lado | Sin matches, racha cumplida o no, desconectado |
| Mapa de lugares | Mapa con filtros arriba y lista de tarjetas | Cargando, sin resultados, error de la API |
| Citas | Lista de citas y detalle con línea de estado | Pendiente, aceptada, confirmada, no confirmada |
| Reseña | Formulario corto con reglas visibles antes de escribir | Esperando a la otra persona, publicada, rechazada |
| Moderación | Lista de la cola y panel de detalle con acciones | Cola vacía |

### 9.3 Experiencia de usuario

- **Errores claros:** cada mensaje explica qué pasó y cómo corregirlo.
- **Transparencia:** se explica por qué se pide cada dato, que no se guardan las imágenes y que la ubicación solo se solicita durante la cita.
- **Motivar sin presionar:** la racha se presenta como progreso, sin apurar al usuario a tener una cita.
- **Retroalimentación inmediata:** toda acción muestra un estado de carga, éxito o error.
- **Estados vacíos útiles:** explican el siguiente paso en lugar de quedar en blanco.
- **Lenguaje incluyente:** se usa “persona” y “tu match”, en español de México y con trato de tú.
- **Validación:** prueba de usabilidad con 5 personas; un usuario nuevo debe completar el registro y el perfil en menos de 5 minutos sin ayuda (RNF-08).

---

## 10. Guía del frontend

### 10.1 Requisitos

Node.js 20 o superior, npm y el backend en ejecución (ver el README de `ratem-dating-backend`).

### 10.2 Instalación y ejecución

```bash
git clone https://github.com/<organización>/ratem-dating-frontend.git
cd ratem-dating-frontend
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

### 10.3 Despliegue

El ambiente de pruebas se despliega automáticamente en Vercel al hacer push a `develop`. Las variables `VITE_API_URL`, `VITE_SOCKET_URL` y `VITE_GOOGLE_MAPS_API_KEY` se configuran en el panel del proyecto.

---

## Referencias

- The Competitive Intelligence Unit (2026). Uso de aplicaciones de citas en México.
- Norton (2025). Ciberdelincuencia en aplicaciones de citas en México.
- Sumsub, citado en El CEO (2026). Fraudes en aplicaciones de citas en México.
- GBG (s. f.). Estudio sobre verificación de identidad en aplicaciones de citas (Reino Unido).
