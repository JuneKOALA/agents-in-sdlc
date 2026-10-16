---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que pocas organizaciones —si es que hay alguna— estén libres de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que debe actualizarse o solicitudes de funcionalidades que llevan mucho tiempo en el backlog por falta de tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidad de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios en borrador para que un desarrollador la revise. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que los desarrolladores se centren en asuntos de mayor alcance.

Con el agente de GitHub Copilot en la nube explorarás:

- cómo personalizar el entorno para generar código;
- cómo garantizar que las operaciones se realicen de forma segura;
- la importancia de definir incidencias con un alcance claro;
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que le gustaría resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado; de hecho, comprobarás que no hay ninguna. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. Le gustaría que la página de detalles de cada juego mostrara otros títulos de su misma categoría, para que los usuarios puedan seguir explorando. Todavía no hace falta una implementación pulida: quieren algo con lo que puedan hacer pruebas de aceptación de la experiencia de usuario. Ahora mismo es un impedimento, aunque hay incidencias de mayor prioridad.

Estos son ejemplos de tareas que pueden quedar relegadas y que conviene asignar al agente de GitHub Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisar el trabajo de Copilot para asegurarse de que cumple las expectativas.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma similar a un desarrollador. Al igual que con el trabajo de otra persona, hay varias maneras de hacerlo, entre ellas [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al recibir una asignación, Copilot crea una solicitud de incorporación de cambios en borrador para hacer seguimiento del progreso, configura un entorno y empieza a trabajar en la tarea. Puedes explorar la sesión de Copilot mientras sigue activa o después de que termine. Cuando la solución propuesta esté lista para revisarse, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de definir bien las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar en la tarea. Incluso las operaciones que parecen sencillas se vuelven complejas al examinarlas con detalle, así que [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un programador de IA con quien trabajas en pareja: avanza por etapas, aprende, experimenta y adapta el enfoque. La incorporación de la IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente de GitHub Copilot en la nube, revisa los archivos de instrucciones que incluye este repositorio, sin modificarlos, para que luego puedas apreciar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace, si ya tienes uno activo:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la siguiente sección, observa si la solicitud de incorporación de cambios incluye comentarios TSDoc, convenciones TypeScript y encabezados de comentarios. Estos elementos proceden de los archivos de instrucciones personalizadas que acabas de revisar al principio del ejercicio 2. Volveremos a señalarlos al revisar las solicitudes de incorporación de cambios en una lección posterior.

## Configurar el entorno de desarrollo del agente de GitHub Copilot en la nube

Desarrollar código, independientemente de quién lo haga, suele requerir un entorno concreto y ejecutar algunos scripts de configuración para que todo esté en condiciones. Lo mismo se aplica al asignar tareas a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. El flujo de trabajo ya está configurado para facilitar el laboratorio y esta oportunidad de aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y cargada con datos para la aplicación Astro única:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en los pasos de configuración, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y de la aplicación: Node.js (ahora toda la aplicación es Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos en la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Este flujo de trabajo se parece a cualquier otro archivo de GitHub Actions, pero incluye algunos aspectos importantes:

- Contiene un único trabajo, `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Esto resulta útil para comprobar que funciona correctamente sin tener que esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, la mayoría de los proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica suele quedar sin resolver, lo que reduce la productividad y dificulta el mantenimiento del código y la incorporación de nuevos desarrolladores. Copilot destaca en la creación de documentación, por lo que esta es una incidencia ideal para asignar al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás para revisar el resultado.

1. Abre el repositorio en github.com en una nueva pestaña del explorador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios (PR), cuyo título será similar a `[WIP]: Code lacks documentation`. Si no aparece en la lista, espera un momento y actualiza la ventana del explorador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> Es posible que la lista de tareas de Copilot tarde varios minutos en aparecer en la solicitud de incorporación de cambios. Copilot está creando su entorno (ejecuta el flujo de trabajo descrito antes), analizando el proyecto y determinando el mejor enfoque para resolver el problema.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa la sesión activa y cómo Copilot resuelve el problema. Verás cómo explora el código y analiza el estado, se detiene para reflexionar y elegir un plan adecuado, y crea código.

Es probable que tarde varios minutos. Una de las principales ventajas del agente de GitHub Copilot en la nube es que puede trabajar de forma asíncrona y dejarte ocuparte de otras tareas. Aprovecharemos esta función asignándole otra tarea y, mientras tanto, escribiremos código para añadir funcionalidad a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Como ya se ha explicado, una de las grandes ventajas del agente de GitHub Copilot en la nube es que permite repartir el trabajo: tú puedes ocuparte de unas tareas mientras el agente se encarga de otras. Añadir una sección de juegos relacionados para el equipo de diseño quizá no tarde mucho, pero aun así es tiempo que puedes dedicar a otras tareas. Vamos a asignársela al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para utilizar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás varios emojis 👀 en el primer comentario de la incidencia, que indican que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente de GitHub Copilot en la nube trabaja de forma parecida a un ingeniero de software: no hace falta supervisarlo continuamente, basta con revisar el resultado cuando termine. Ahora puedes centrarte en crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignar incidencias a Copilot para que las resuelva de forma asíncrona. Copilot puede ayudar a resolver deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- cómo personalizar el entorno para generar código;
- cómo garantizar que las operaciones se realicen de forma segura;
- la importancia de definir incidencias con un alcance claro;
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes centrarte en crear y utilizar agentes personalizados. [El agente en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que ya hemos explorado en módulos anteriores.

## Recursos

- [Información sobre el agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
