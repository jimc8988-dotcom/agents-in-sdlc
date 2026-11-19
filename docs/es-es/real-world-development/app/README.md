---
slug: es-es/real-world-development/app
title: "Aplicación GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** es una aplicación de escritorio basada en Copilot CLI que reúne el desarrollo dirigido por agentes en un único espacio de trabajo específico. Añade sesiones de agente en paralelo, modos de sesión intercambiables, lienzos compartidos y gestión nativa de incidencias y solicitudes de incorporación de cambios de GitHub, incluido **Agent Merge**, que guía una solicitud durante las actualizaciones de base, los comentarios de revisión, las correcciones de integración continua (CI) y la combinación.

El taller sigue un único flujo continuo de Tailspin Toys:

1. Prepara el proyecto, instala la aplicación, conecta el repositorio y explora el espacio de trabajo y la lista de trabajo pendiente inicial.
2. Realiza un cambio específico de valoraciones por estrellas, revísalo en el navegador y combina manualmente tu primera solicitud de incorporación de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en modo **Plan**, desarróllalo en modo **Autopilot** y revísalo en modo **Interactive**.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la habilidad `quality-checks` existente y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa el cambio completo de filtrado y utiliza Agent Merge para la segunda PR.
9. Comprueba si Canvas está disponible. Después, utiliza Database Explorer y crea y prueba un lienzo de triaje respaldado por el repositorio. Si las capacidades necesarias no están disponibles, anota el impedimento y utiliza la alternativa de solo lectura o continúa con el resumen.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones por estrellas y otra para el filtrado con las actualizaciones de instrucciones y de la habilidad, el perfil QA y las pruebas. Empieza cada una desde `main` actualizado. El flujo de filtrado y calidad comparte una sesión, un árbol de trabajo y una rama para que puedas aprovechar el trabajo realizado mientras exploras cada herramienta. El ejercicio final del lienzo permanece en su propia sesión para que puedas centrarte en crear y probar la superficie compartida en lugar de repetir el flujo de PR.

## Lecciones

| Lección | Tema | Descripción |
|--------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Instala Node.js y crea una copia del proyecto Tailspin Toys |
| [1. Instala la aplicación Copilot][ex1] | Configuración | Instala la aplicación, conecta el repositorio y explora el espacio de trabajo |
| [2. Añade valoraciones por estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y la alternativa para los valores nulos; después, combina la primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad a partir de una incidencia, desarróllala con Autopilot y revísala con Interactive |
| [4. Guía a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al trabajo de filtrado |
| [5. Personaliza y utiliza la skill quality-checks][ex5] | Comprobaciones repetibles | Explora la skill existente, cambia el formato del informe y ejecútala |
| [6. Valida la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP mediante Customize e inspecciona el comportamiento de los filtros |
| [7. Crea y utiliza un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas de verificación finales |
| [8. Crea y combina la PR de la funcionalidad][ex8] | Revisión y combinación | Revisa los filtros, las instrucciones, la skill, el perfil de QA y las pruebas; después, utiliza Agent Merge para la segunda PR |
| [9. Explora y crea lienzos][ex9] | Colaboración | Utiliza Database Explorer y crea y prueba un lienzo de triaje respaldado por el repositorio |
| [10. Resumen y pasos siguientes][ex10] | Resumen | Repasa el flujo de trabajo, los recursos creados y otros materiales |

## Requisitos previos

Antes de asistir a este taller, asegúrate de disponer de:

- [ ] Una cuenta de GitHub con un plan **Copilot Student, Pro, Pro+, Business o Enterprise** activo
- [ ] Un ordenador con **macOS, Linux o Windows**
- [ ] [Git instalado][install-git] en el ordenador

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes cuya condición se haya verificado pueden obtener GitHub Copilot gratis mediante [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y Copilot CLI que se utilizan en este taller, por lo que podrás completar todos los itinerarios.

> [!NOTE]
> Como la aplicación Copilot se ejecuta en tu propio equipo y no en un codespace, la [Lección 0][ex0] explica cómo instalar Node.js y crear tu copia del proyecto antes de instalar la aplicación.

> [!NOTE]
> Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva de **GitHub Copilot app**. Esta directiva es independiente de la directiva de Copilot CLI.

## Comenzar

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