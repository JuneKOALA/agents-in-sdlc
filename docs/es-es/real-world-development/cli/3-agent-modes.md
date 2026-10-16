---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Al principio añadimos una función pequeña al proyecto. Los cambios de mayor alcance requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con los flujos existentes de una organización y ayudar a crear las funciones adecuadas de la manera adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo con agentes: partirás de una incidencia para crear una función, comprobarás el código y el comportamiento y, finalmente, integrarás el cambio en el proyecto.

> [!NOTE]
> Seguirás con la misma conversación y la misma rama mientras continúas con el flujo de trabajo de la función. Normalmente se crearían ramas o PR distintas para los diferentes cambios, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la función en el modo Autopilot;
- revisarás el código;
- validarás manualmente la función en un explorador reenviado.

Al continuar con esta función, actualizarás las instrucciones del repositorio, personalizarás la aptitud existente `quality-checks`, añadirás la validación MCP, crearás un agente de QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar juegos por categoría y editorial. La incidencia del backlog describe la función, pero hay que acordar algunos detalles, como la combinación de categorías, antes de programarla. Utilizarás el modo Plan para tomar esas decisiones y, después, autorizarás una implementación con un alcance definido mediante Autopilot.

## Contexto

Incorporar agentes de programación de IA al flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Planificar lo que se va a crear.
3. Crear y revisar el código.
4. Ejecutar pruebas para validarlo.
5. Validar manualmente la nueva función.
6. Crear una solicitud de incorporación de cambios (PR).
7. Integrar el código cuando se haya revisado y el proceso de integración continua se haya completado correctamente.

> [!NOTE]
> Los detalles variarán según el equipo y la organización, pero normalmente el proceso será una variante de los pasos anteriores.

Seguir este enfoque estándar ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el nivel de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza con Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Empezar por la incidencia

Antes de comenzar el trabajo de filtrado, vuelve al codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve al codespace. Si está detenido, reinícialo.
2. Confirma que se ha integrado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Utiliza las flechas para resaltar **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt empieza por `#7` (o un número similar). El prefijo `#` permite incluir en el contexto una incidencia o solicitud de incorporación de cambios de GitHub.

## Planificar la función de filtrado

Planificar permite definir el enfoque para implementar una función o realizar una tarea antes de asignársela a Copilot. Para cualquier tarea compleja, conviene dedicar tiempo a preparar el plan. Cambia al modo Plan y pídele a Copilot que lo elabore.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Comprueba que el indicador situado debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Puede que Copilot te pregunte cómo deben funcionar las opciones de filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que modifique cualquier parte que no coincida con tus decisiones.

## Aprobar Autopilot

Una vez redactado y revisado el plan, puedes implementarlo con Autopilot. Este modo permite a Copilot iterar sobre el problema hasta que considere que la tarea está completa.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Comprueba que el indicador bajo el prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo antes. Si Copilot informa de dependencias que faltan o de un conflicto de puertos, resuelve el problema de configuración antes de considerar completadas las comprobaciones.

## Revisar y verificar la implementación

Al igual que cualquier otro código, el código generado debe revisarse antes de integrarlo. Revisaremos el código e iniciaremos el sitio para comprobar que funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los errores.

## Explorar la nueva función

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría y editorial, así como las combinaciones que acordaste en el plan.
4. Confirma que el reinicio y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y pasos siguientes

Has utilizado distintos modos de conversación para crear y revisar una función. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub;
- has definido requisitos en el modo Plan;
- has implementado una función con Autopilot;
- has revisado el código;
- has validado la función manualmente en un explorador reenviado.

A continuación, profundizarás en la generación de código y en cómo conseguir que siga las prácticas documentadas mediante las [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
