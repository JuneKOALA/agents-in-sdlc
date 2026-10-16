---
title: "Lección 5: Personalizar y utilizar una aptitud de comprobación de calidad"
description: "Explora la aptitud existente quality-checks, personaliza el formato del informe y utilízala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código es solo una parte del trabajo. Ya hemos comprobado manualmente que el código funciona y utilizado archivos de instrucciones para que siga nuestros estándares. ¿Y las pruebas, el lint y las demás partes de la integración continua (CI)?

Para estas tareas, las **aptitudes de agente** son una buena opción. Ayudan a Copilot a entender cómo realizar operaciones de este tipo.

En esta lección:

- explorarás la aptitud existente `quality-checks`;
- personalizarás el formato de sus resultados;
- volverás a cargar y ejecutar la aptitud.

## Escenario

Tailspin Toys utiliza la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para facilitar la lectura de los resultados.

## Instrucciones, scripts y recursos

Las aptitudes de agente reúnen instrucciones reutilizables para tareas, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. En esencia, una aptitud es una carpeta con su nombre que contiene un archivo Markdown llamado `SKILL.md`. El Markdown incluye frontmatter con un nombre y una descripción que definen la aptitud, una descripción general de lo que hace y directrices sobre cuándo debe utilizarse. La carpeta también puede contener subcarpetas con scripts y otros recursos.

> [!NOTE]
> No es necesario que la aptitud incluya otros archivos ni carpetas. La aptitud `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque utiliza los comandos existentes del proyecto.

Las aptitudes pueden estar en la carpeta `.github/skills` del proyecto, donde se convierten en recursos del repositorio que el equipo puede compartir y reutilizar, o en la carpeta de aptitudes del usuario `~/.copilot/skills`.

## Explorar la aptitud

Vamos a explorar la aptitud que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve al codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo utilizar la aptitud.
3. Lee las instrucciones y observa cómo guían a Copilot por el proceso de pruebas y lint.
4. Fíjate en que todavía no hay ninguna sección **Results output formatting**.

## Ejecutar la aptitud antes de cambiarla

Las aptitudes se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones.

1. Vuelve a la conversación de filtrado en el modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personalizar el informe

Mejoremos el informe para que indique qué se ejecutó, si se completó correctamente y qué resultados proporcionaron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guardará automáticamente.

## Ejecutar la aptitud actualizada

Copilot CLI puede volver a cargar las aptitudes editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y pasos siguientes

Has personalizado y utilizado una aptitud de agente existente. En esta lección:

- has explorado la aptitud `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos;
- has personalizado el formato de los resultados;
- has vuelto a cargar y ejecutado la aptitud.

El cambio de la aptitud formará parte de la PR de la función de filtrado. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de aptitudes

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del explorador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
