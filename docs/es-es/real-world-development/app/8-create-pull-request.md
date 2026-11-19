---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa en conjunto los filtros, las instrucciones, la actualización de la skill, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación de los filtros, las actualizaciones de instrucciones y de la skill, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos en conjunto y abrir una solicitud de cambios. Combinaste tú mismo la solicitud de cambios (PR) de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, dividiríamos la funcionalidad, las actualizaciones de instrucciones y de la skill y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtros y calidad en una sola sesión y rama, de modo que todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de cambios.
- revisarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de los filtros, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante de la PR, manteniendo el control de autorización de la combinación en manos del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación revisa la solicitud y resuelve los obstáculos que la bloquean: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a basar la rama cuando es necesario. Después, la combina en cuanto GitHub lo permite. El proceso se ejecuta en segundo plano, continúa aunque reinicies la aplicación y se desactiva cuando la solicitud ya está combinada.

Hasta ahora, has seleccionado **Merge pull request** tú mismo. Agent Merge puede hacerse cargo de esa responsabilidad, pero sigue necesitando tu autorización explícita para editar código y combinarlo. Antes de conceder permiso para combinar, revisa las acciones que tiene permitidas y el trabajo realizado.

## Usa Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Ahora dejaremos que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para elegir **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge. El agente:

- creará la solicitud de cambios con un título y una descripción.
- si has iniciado la sesión desde una incidencia, la mencionará en el cuerpo de la descripción.
- volverá a basar la rama o resolverá los posibles conflictos con la rama de destino.
- supervisará el proceso de integración continua para comprobar que se superan todas las comprobaciones.
- supervisará la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y actualizará el código para resolverlos.
- opcionalmente, combinará la PR automáticamente cuando todo se haya completado correctamente.

Permitamos también que Agent Merge combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que la opción **Merge pull request** está seleccionada.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Supervisa el progreso y resuelve los obstáculos que se indiquen. Antes de continuar, confirma que el estado de la PR sea **Merged**. Tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos obstáculos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el proceso de solicitud de cambios. Has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de cambios.
- revisado la PR completa de la funcionalidad y las pruebas de verificación.
- autorizado Agent Merge solo después de la revisión y confirmado que la PR se ha combinado.

A continuación, [utilizarás un canvas existente y crearás uno de triaje][next-lesson] para explorar una forma más completa de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
