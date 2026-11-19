---
title: "Lección 1 - Instala la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado a partir de la plantilla, explora el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para desarrollar con agentes. Está basada en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan directamente. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en lugar de hacerlo todo tú mismo, y automatizas tareas repetitivas. Ya tienes Node.js instalado y una copia del proyecto preparada. Ahora instalarás la aplicación y conectarás el repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión.
- añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- explorarás el espacio de trabajo y el backlog incluido en la plantilla.
- probarás un chat rápido para conocer la aplicación.

## Escenario

Tu equipo está empezando a utilizar agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot reúne en un solo lugar las herramientas para dirigir este trabajo: elegir incidencias, ejecutar agentes, revisar cambios y combinar solicitudes. En esta lección instalarás la aplicación, la conectarás y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Necesitas un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva de **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

Para utilizar la aplicación GitHub Copilot, primero debes instalarla. Hay versiones para Windows, macOS y Linux. Instalemos la aplicación, autentiquémonos y añadamos el repositorio Tailspin Toys.

1. En un navegador, abre la [página de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación una vez instalada.
4. Selecciona **Sign in to GitHub** y sigue las instrucciones para iniciar sesión con una cuenta de GitHub.com o GHE.com. Si utilizas GitHub Enterprise Cloud con residencia de datos, selecciona **GitHub Enterprise Cloud** y, cuando se te solicite, escribe el nombre de host `*.ghe.com`.

   Si utilizas GitHub Enterprise Server, primero completa la incorporación con tu cuenta en la nube. Después, abre **Settings**, selecciona **Accounts**, selecciona **Add account** y elige **GitHub Enterprise Server**. Sigue las instrucciones para autorizar la aplicación en el navegador, tal como se describe en la [guía de inicio de sesión de Enterprise Server][ghes-sign-in].

5. Si durante la configuración se te ofrece seleccionar un repositorio, selecciona el repositorio Tailspin Toys que has creado a partir de la plantilla. También puedes omitir este paso y añadir el proyecto más adelante.
6. Cuando se te solicite, elige un tema y completa la configuración.
7. Busca tu repositorio en **Sessions**, en la barra lateral. Si ya aparece en la lista, no hace falta volver a añadirlo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Escribe el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En la disposición que se muestra aquí, las carpetas de los repositorios, como `tailspin-toys`, están agrupadas en **Sessions** y las sesiones individuales aparecen debajo de cada repositorio.

## Explora el espacio de trabajo

Con el proyecto conectado, familiarízate con la aplicación. La barra lateral organiza el trabajo en varias áreas:

- **New**: inicia una sesión de chat nueva con Copilot.
- **Pull requests**: muestra tus solicitudes de cambios mediante la integración nativa con GitHub. Puedes explorarlas, filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: muestra las incidencias de la misma forma. Puedes explorarlas, filtrarlas e iniciar una sesión desde una incidencia.
- **Automations**: tareas guardadas de agentes que se ejecutan según una programación o cuando las inicias. Son útiles para gestionar listas de tareas, mantenimiento periódico del proyecto u otras tareas rutinarias. El resumen las propone como paso siguiente, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, complementos, skills y otros componentes. La utilizarás para configurar Playwright MCP.
- **Chats**: conversaciones breves para preguntas e ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de los repositorios y las sesiones de sus agentes. Selecciona el nombre de un repositorio, como `tailspin-toys`, para abrir la vista del repositorio, o una sesión debajo para volver a esa conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, así que puedes ejecutar varias sin que interfieran entre sí. Iniciarás tu primera sesión al añadir las valoraciones por estrellas.

Explorarás el espacio de trabajo a medida que avances en el taller.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si una acción es posible, puede orientarte.

### Encuentra el backlog incluido en la plantilla

Es probable que todos los proyectos tengan un backlog; Tailspin Toys no es una excepción. Exploremos las incidencias existentes, que se crearon al crear el repositorio a partir de la plantilla.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, no des por hecho que tienen un número concreto:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer los detalles. Cada incidencia también permite iniciar una sesión de agente. Empezarás por la incidencia de los filtros después de realizar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo el contenido de los repositorios que has añadido a la aplicación Copilot. Si quieres ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena manera de familiarizarte con la aplicación es utilizarla para conocer la propia aplicación. Para ello, un **chat rápido** es perfecto. Puedes hacer una pregunta o explorar una idea sin crear una rama ni un árbol de trabajo; es ideal para preguntas breves y desechables, sin necesidad de iniciar una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si **Chats** aún no está visible, selecciona **+** junto a **Sessions** para iniciar tu primer chat; después aparecerá **Chats**.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo Git aislado, lo que te permite ejecutar varios agentes en paralelo sin que interfieran entre sí. Puedes continuar la conversación o iniciar otro chat en cualquier momento.

## Resumen y pasos siguientes

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- familiarizarte con el espacio de trabajo y encontrar el backlog en **Issues**.
- utilizar un chat rápido para hacer una pregunta breve.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y mostrarás una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
