---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muchas organizaciones tengan dificultades con la deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo pendientes. El agente de GitHub Copilot en la nube está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios en borrador para que la revise una persona. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que el equipo se centre en prioridades más amplias.

Con el agente de Copilot en la nube explorarás:

- cómo personalizar el entorno en el que se genera código.
- cómo asegurarte de que las operaciones se realizan de forma segura.
- por qué es importante que las incidencias tengan un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Quienes desarrollaron la primera versión del sitio dejaron la documentación en mal estado; de hecho, no hay documentación. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar cómo se descubren los juegos. Quiere que cada página de detalles muestre otros títulos de la misma categoría, para que las personas puedan seguir explorando. Por ahora no necesita una implementación pulida: solo algo que pueda utilizar para probar la experiencia de usuario. Aunque es un impedimento para el equipo, hay otras incidencias de mayor prioridad.

Son tareas que pueden quedar relegadas con facilidad y que se pueden asignar al agente de Copilot en la nube. El agente trabajará en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, podrá revisar su trabajo.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al asignarla, Copilot crea una solicitud de incorporación de cambios en borrador para registrar el progreso, configura el entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando haya terminado. Cuando la solución esté lista para revisarla, Copilot te mencionará en la solicitud.

## La importancia de definir bien el alcance

GitHub Copilot no es magia: no elimina la necesidad de reflexionar sobre el trabajo. Incluso las tareas que parecen sencillas pueden tener aspectos complejos. Ten en cuenta las [buenas prácticas para definir el alcance de las tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has aprendido cómo orientan a Copilot las instrucciones personalizadas. Si ya completaste el ejercicio práctico, conoces `.github/copilot-instructions.md`; si no, este es un buen momento para consultarlo. Antes de asignar trabajo al agente, revisa en modo de solo lectura los archivos de instrucciones del repositorio para identificar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en particular las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa los metadatos `applyTo`, que limitan estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la sección siguiente, busca en la solicitud de incorporación de cambios los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Proceden de estos archivos de instrucciones. Volveremos a señalarlos al revisar las solicitudes en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube

El desarrollo de código suele requerir un entorno específico y scripts de configuración. Lo mismo se aplica al asignar tareas a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así puede instalar las herramientas y dependencias necesarias. El flujo ya está configurado para este laboratorio: proporciona Node.js, las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y cargada con datos de la aplicación Astro:

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
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la interfaz y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
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

      # Migra y carga datos en la base de datos SQLite local para que haya datos disponibles en las compilaciones y las pruebas.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo, `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud.
- El activador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Esto resulta útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Muchos proyectos tienen información obsoleta o carecen de documentación. Esa deuda técnica ralentiza el trabajo y dificulta el mantenimiento del código y la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta es una buena incidencia para asignar al agente en la nube. Más adelante revisarás el resultado.

1. En una nueva pestaña del navegador, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que acaba de crear el agente. Su título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está configurando el entorno con el flujo de trabajo descrito antes, analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a realizar.
14. Desplázate por la cronología de la PR y busca una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Desplázate por la sesión en directo para ver cómo resuelve Copilot la tarea: explora el código, analiza su estado, decide un plan y crea código.

El proceso puede tardar varios minutos. Una de las ventajas principales del agente en la nube es que puede trabajar de forma asíncrona, lo que te permite ocuparte de otras tareas. Aprovecharemos esa capacidad asignándole otra tarea y, a continuación, desarrollaremos una funcionalidad de la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una ventaja del agente en la nube es que permite repartir el trabajo: tú puedes encargarte de unas tareas mientras el agente se ocupa de otras. Asignemos al agente la solicitud del equipo de diseño para mostrar juegos relacionados.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot**.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás un conjunto de 👀 en el primer comentario de la incidencia, que indica que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube trabaja de forma parecida a un ingeniero de software: no tienes que supervisarlo constantemente, sino revisar el resultado cuando termine. Mientras tanto, vamos a crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona, abordar deuda técnica, crear funcionalidades o ayudar a migrar código de un marco de trabajo a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- definir con claridad el alcance de las incidencias.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes crear y utilizar agentes personalizados. El [agente de Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que ya has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

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
