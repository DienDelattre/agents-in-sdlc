---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos en una aplicación suelen requerir editar varios archivos y realizar otras tareas, como ejecutar pruebas. El flujo habitual implica localizar los archivos, hacer cambios, ejecutar las pruebas, depurar, detectar qué archivo falta y volver a editar. El modo agente de Copilot puede ayudarte con este proceso.

El modo agente trabaja con más autonomía en el IDE. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que detecta. Veamos cómo utilizarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres sean parecidos, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite recibir comentarios e interactuar rápidamente. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- utilizar el modo agente de GitHub Copilot para implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que se filtren por categoría y editorial. En el ejercicio anterior añadiste un helper de editoriales; ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys y sus funcionalidades actuales.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite consultar juegos y sus detalles. Es una aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local en sus metadatos mediante Drizzle ORM; no hay una API de backend independiente ni un marco de interfaz del lado del cliente. Los helpers reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con un pequeño elemento Astro `<script>` de ámbito limitado que utiliza las API estándar del DOM.

### Iniciar el sitio web

La tarea `npm run dev` inicia el servidor de desarrollo Astro. Ejecútala en el Codespace:

1. Vuelve al Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, verás un mensaje parecido al siguiente:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te invita a abrir `http://localhost:4321` en un navegador, ciérralo con **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost del terminal, se te redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en marcha, explora sus funcionalidades principales:

- **Home Page**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Game Details Page**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar la lista de trabajo pendiente con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea importante.

La primera versión del sitio funciona, pero queremos añadir funcionalidades. Empieza revisando la lista de trabajo pendiente, que se creó automáticamente al generar el proyecto a partir de la plantilla. Pide a GitHub Copilot que te muestre las incidencias.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. Este agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Selector de agentes de la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> La inclusión de un modelo concreto en estas instrucciones no implica que sus autores lo recomienden frente a otros. Se utilizó Claude Sonnet 4.5 para preparar el taller y se incluye para mantener una experiencia relativamente coherente. Los modelos de lenguaje no son deterministas, por lo que las sugerencias pueden variar. Es normal.

> [!NOTE]
> Debido al comportamiento probabilístico de los modelos de lenguaje, Copilot puede utilizar un comando MCP distinto y aun así completar la tarea.

3. Envía esta indicación a Copilot para consultar la lista de incidencias:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según su posible utilidad para quienes utilizan el sitio.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Utilizarás el archivo de instrucciones de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar los cambios del sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones específicas para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto permite reutilizar el contenido, dividir conjuntos complejos en archivos manejables y proporcionar ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado hay que hacer al menos tres cambios:

- añadir o mejorar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para añadir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y completarse correctamente antes de combinar los cambios. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa la conversación actual o inicia otra con **New Chat**.
2. Selecciona **Add Context**, **Instructions** y el archivo `ui`.

   ![Ejemplo de selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en el selector de agentes de la vista Chat.

   ![Selector de agentes de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** siga seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada de la lista:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorará el proyecto, modificará archivos y ejecutará pruebas. Puede tardar unos minutos, según la complejidad de la tarea y del código. Durante el proceso, quizá aparezcan los controles **Keep** y **Undo** en el editor. Cuando Copilot termine, tendrás uno de esos controles para cada cambio; no hace falta seleccionarlos mientras el trabajo siga en curso.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que solicita permiso para ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta terminar y no detectar errores.

   ![Sesión completa de Copilot Agent Mode en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Es importante revisar siempre el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas comprobado que todo funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Has explorado cómo utilizar el modo agente de GitHub Copilot para añadir capacidades al sitio de Tailspin Toys. Has aprendido a:

- implementar funcionalidades en la interfaz y la capa de datos con el modo agente.
- explorar el proyecto, identificar archivos y coordinar cambios.
- revisar los cambios y las pruebas generados antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que aumenta la lista de juegos, puede ser necesario añadir paginación. Utiliza lo aprendido para pedir a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las buenas prácticas existentes y utiliza los archivos de instrucciones.
- decide cómo implementar la paginación y si quieres permitir que se elija el tamaño de página.
- proporciona en la indicación las directrices necesarias para obtener la implementación deseada.
- quizá tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es el proceso habitual al trabajar con Copilot.

## Recursos

- [Modos de Copilot: ask, edit y agent; qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
