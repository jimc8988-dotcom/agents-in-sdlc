---
title: "Lección 8 - Crea y combina la solicitud de cambios de la funcionalidad"
description: "Revisa los filtros, las instrucciones, la actualización de la skill, el perfil de QA y las pruebas; después, crea una solicitud de cambios y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación de los filtros, las actualizaciones de instrucciones y skills, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste tú mismo la solicitud de cambios (PR) de la valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, dividiríamos la funcionalidad, las actualizaciones de instrucciones y skills y el agente de QA en varias solicitudes de cambios. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtros y calidad en una sola conversación y rama, de modo que todo ese trabajo se incluirá en esta solicitud.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- revisarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la solicitud de cambios para los filtros.
- habilitarás Agent Merge solo después de la revisión y confirmarás que la solicitud se ha combinado.

## Escenario

Durante el flujo de trabajo de los filtros, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante de la solicitud de cambios, manteniendo el control de autorización de la combinación en manos del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios. Cuando lo habilitas, Copilot se ocupa de los obstáculos que bloquean la solicitud: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a basar la rama cuando es necesario. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora, has seleccionado **Merge pull request**. Agent Merge puede hacerse cargo de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Usa Agent Merge para gestionar la solicitud de cambios

Ya has creado todo el código. Ahora revisaremos los cambios en conjunto, crearemos la solicitud de cambios y dejaremos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una solicitud de cambios nueva con el comando `/pr create`.
5. Activa Agent Merge con el comando `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la solicitud. Cuando todo se complete correctamente, realizará la combinación.
7. Comprueba que Copilot muestre un mensaje similar a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el proceso de solicitud de cambios. Has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- revisado el cambio completo de la funcionalidad y las pruebas de verificación.
- creado la solicitud de cambios para los filtros.
- habilitado Agent Merge solo después de la revisión y confirmado que la solicitud se ha combinado.

A continuación, [explorarás otros comandos slash útiles de Copilot CLI][next-lesson] para trabajar con el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
