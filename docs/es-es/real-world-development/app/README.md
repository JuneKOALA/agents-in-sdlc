---
slug: es-es/real-world-development/app
title: "Aplicación GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-09-17
---

La **[aplicación GitHub Copilot](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** es una aplicación de escritorio basada en Copilot CLI que reúne el desarrollo dirigido por agentes en un único espacio de trabajo específico. Añade sesiones de agente en paralelo, modos de sesión intercambiables, lienzos compartidos y gestión nativa de incidencias y solicitudes de incorporación de cambios de GitHub, incluido **Agent Merge**, que guía una solicitud durante los rebases, los comentarios de revisión, las correcciones de integración continua (CI) y la integración.

El taller sigue un flujo de trabajo continuo con Tailspin Toys:

1. Prepara el proyecto, instala la aplicación, conecta el repositorio y explora el espacio de trabajo y el backlog inicial.
2. Realiza un cambio específico para mostrar valoraciones con estrellas, revísalo en el explorador e integra manualmente tu primera solicitud de incorporación de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en modo **Plan**, desarróllalo en modo **Autopilot** y revísalo en modo **Interactive**.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la aptitud existente `quality-checks` y utilízala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Playwright Model Context Protocol (MCP) y utilízalo para explorar el filtrado en un explorador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa el cambio completo de filtrado y utiliza Agent Merge para la segunda PR.
9. Utiliza el lienzo Database Explorer existente y, después, crea y prueba un lienzo de clasificación respaldado por el repositorio.

Para que el taller sea conciso, crearás dos PR: una para las valoraciones con estrellas y otra para el filtrado con las actualizaciones de instrucciones y de la aptitud, el perfil de QA y las pruebas. Empieza cada una desde `main` actualizado. El flujo de filtrado y calidad comparte una sesión, un árbol de trabajo y una rama para que puedas basarte en el trabajo existente mientras exploras cada herramienta. El ejercicio final del lienzo permanece en su propia sesión para que puedas centrarte en crear y probar la superficie compartida en lugar de repetir el flujo de PR.

## Lecciones

| Lección | Tema | Descripción |
|--------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Instala Node.js y crea tu copia del proyecto Tailspin Toys |
| [1. Instalar la aplicación Copilot][ex1] | Configuración | Instala la aplicación, conecta el proyecto y familiarízate con el espacio de trabajo |
| [2. Añadir valoraciones con estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el estado sin valoración; después, integra la PR 1 |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la función a partir de su incidencia, créala con Autopilot y revísala en modo Interactive |
| [4. Orientar a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personalizar y utilizar una aptitud de comprobación de calidad][ex5] | Comprobaciones repetibles | Explora la aptitud existente, cambia el formato del informe y ejecútala |
| [6. Validar funcionalidad con Playwright MCP][ex6] | Observación con un explorador | Configura MCP mediante Customize e inspecciona el comportamiento del filtrado |
| [7. Crear y utilizar un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas finales de verificación |
| [8. Crear e integrar la PR de la función][ex8] | Revisión e integración | Revisa el filtrado, las instrucciones, la aptitud, el perfil de QA y las pruebas; después, utiliza Agent Merge para la segunda PR |
| [9. Explorar y crear lienzos][ex9] | Colaboración | Utiliza Database Explorer y, después, crea y prueba un lienzo de clasificación respaldado por el repositorio |
| [10. Repaso y pasos siguientes][ex10] | Resumen | Repasa el flujo de trabajo, los recursos creados y otros materiales |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Un ordenador con **macOS, Linux o Windows**
- [ ] [Git instalado][install-git] en el ordenador

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes cuya condición se haya verificado pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y las funciones de Copilot CLI que utiliza este taller, por lo que puedes completar todos los recorridos.

> [!NOTE]
> Como la aplicación Copilot se ejecuta en tu equipo y no en un codespace, el [ejercicio de requisitos previos][ex0] explica cómo instalar Node.js y crear una copia del proyecto antes de instalar la aplicación.

> [!NOTE]
> Si utilizas Copilot Business o Copilot Enterprise, el administrador debe habilitar la directiva **Copilot CLI** para que puedas utilizar la aplicación.

## Primeros pasos

**[Empieza por los requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
