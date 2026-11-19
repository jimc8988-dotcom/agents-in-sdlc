---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. Por suerte, la aplicación GitHub Copilot está diseñada para trabajar con el flujo de trabajo existente de una organización y ayudarnos a crear lo adecuado de la manera correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas, y finalmente la integrarás en el proyecto.

> [!NOTE]
> A medida que avances en el flujo de trabajo de esta funcionalidad, seguirás utilizando la misma sesión. Normalmente se usarían distintas sesiones o solicitudes de cambios para los diferentes tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente a partir de una incidencia de GitHub.
- definirás los requisitos en el modo **Plan**.
- implementarás la funcionalidad nueva con el modo **Autopilot**.
- revisarás el código.
- validarás manualmente la funcionalidad en un canvas del navegador.

Mientras sigues trabajando en esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la skill `quality-checks` existente, añadirás validación con MCP, crearás un agente de QA y abrirás la solicitud de cambios de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles, como la combinación de categorías, antes de programar. Utilizarás el modo Plan para tomar esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Información general

Introducir agentes de programación con IA en el flujo de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Abrir una incidencia registrada con los detalles del trabajo que hay que realizar.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de cambios (PR).
7. Cuando se haya revisado el código y se haya completado correctamente el proceso de integración continua, combinar el código.

> [!NOTE]
> Los detalles exactos varían según el equipo y la organización, pero la mayoría de los procesos son variantes del que se indica arriba.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el grado de autonomía del agente. Puedes elegirlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive**: tú y el agente trabajáis en conjunto. El agente sugiere cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en el modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que la PR de las valoraciones por estrellas se ha combinado y que tu `main` local está actualizado.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior del repositorio. Abre la incidencia existente **Allow users to filter games by category and publisher**. No hace falta crear otra incidencia.
2. Selecciona **New session** y elige un **nuevo árbol de trabajo** basado en el `main` actualizado.

    ![Vista de la incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modos.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en tu terminal, pero no en la sesión, revisa la configuración de PATH o del administrador de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones que hayan fallado. No reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtros

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene en contexto la solicitud de la funcionalidad. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye filtros por categoría y editorial, controles accesibles, cambios en el acceso a los datos y pruebas. Habla con Copilot sobre cualquier comportamiento que no esté claro, como la combinación de varias categorías o qué sucede cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobaciones de tipos mediante las herramientas existentes del proyecto. Limítalo a implementar y probar los filtros: crearás la PR después de completar el flujo de calidad. Pide los cambios necesarios antes de aprobar el plan y conserva el vínculo de la incidencia y las aclaraciones acordadas para validarlos más adelante.

## Autoriza explícitamente Autopilot

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de suponer que la aprobación lo cambia. Confirma que el indicador muestra **Autopilot**.

Copilot empezará a implementar la funcionalidad. Iterará siguiendo el plan acordado, generará código y ejecutará pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, soluciona el problema de configuración antes de dar por completadas las comprobaciones. Detén solo los servidores que hayas iniciado tú.

## Revisa y verifica la implementación

Debes revisar el código generado antes de combinarlo, como harías con cualquier otro código. Revisémoslo y ejecutemos el sitio para comprobar que todo funciona correctamente.

1. Abre **Changes** e inspecciona la implementación de los filtros y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Inspecciona los resultados de lint, las pruebas unitarias, las pruebas E2E y las comprobaciones de tipos. Una comprobación omitida no equivale a una comprobación superada.
4. Corrige los errores y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local. Asegúrate de que el servidor probado pertenece a este árbol de trabajo, no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un canvas del navegador.

1. Usa este prompt para pedir a Copilot que inicie la aplicación y abra la página en el canvas:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y aparecerá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y confirma que los resultados coinciden con ella.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo iniciado para esta sesión y cierre el canvas:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR de los filtros. Conserva esta sesión, su árbol de trabajo y su rama para las lecciones 4 a 8. A continuación, aplicarás instrucciones personalizadas al código de los filtros y añadirás mejoras de la skill y de QA antes de crear y combinar la PR conjunta en la lección 8. Puedes dejar la sesión inactiva después de detener su servidor de desarrollo y retomarla en la lección siguiente.

## Resumen y pasos siguientes

Has utilizado distintos modos de agente para crear y revisar una funcionalidad. En esta lección:

- has iniciado una sesión de agente a partir de una incidencia de GitHub.
- has definido los requisitos en el modo **Plan**.
- has implementado la funcionalidad nueva con el modo **Autopilot**.
- has revisado el código.
- has validado manualmente la funcionalidad en un canvas del navegador.

A continuación, profundizaremos en la generación de código y en cómo garantizar que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
