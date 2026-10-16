---
title: "Lección 8: Crear e integrar la PR de la función"
description: "Revisa conjuntamente el filtrado, las instrucciones, la aptitud actualizada, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la aptitud, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos conjuntamente y abrir una solicitud de incorporación de cambios. Tú integraste la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente dividiríamos la función, las actualizaciones de instrucciones y de la aptitud y el agente de QA en varias PR. Para simplificar el taller, hemos mantenido todo el flujo de trabajo de filtrado y calidad en una sola conversación y rama, y lo incluiremos en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de integración;
- inspeccionarás el cambio completo de la función y las pruebas de verificación;
- crearás la PR del filtrado;
- habilitarás Agent Merge únicamente después de revisar el trabajo y confirmarás que la PR se ha integrado.

## Escenario

Durante el flujo de trabajo del filtrado has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el resto del trabajo de la PR, pero mantener bajo control del desarrollador la autorización de integración.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo que falta para integrar una solicitud de incorporación de cambios. Al habilitarlo, Copilot se ocupa de los impedimentos de la PR, como corregir las comprobaciones de integración continua (CI) que fallen, responder a comentarios de revisión y actualizar la rama mediante rebase cuando sea necesario. Después habilita la integración automática de GitHub si el repositorio lo permite.

Hasta ahora, seleccionabas tú **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que esté listo antes de habilitarlo.

## Usar Agent Merge para gestionar la PR

Ya has creado el código. Ahora lo revisaremos conjuntamente, crearemos la PR y dejaremos que Agent Merge gestione el resto del proceso.

1. Vuelve al codespace.
2. Abre el cuadro de agentes escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la integrará.
7. Confirma que Copilot muestre un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no elude las aprobaciones obligatorias, la protección de ramas, las colas de integración, la configuración del repositorio ni la falta de permisos. Si se bloquea, lee el motivo indicado y completa manualmente la integración revisada cuando la configuración del repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo y, ahora, gestionar las solicitudes de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de integración;
- has inspeccionado el cambio completo de la función y las pruebas de verificación;
- has creado la PR del filtrado;
- has habilitado Agent Merge solo después de revisar el trabajo y has confirmado que se integró la PR.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
