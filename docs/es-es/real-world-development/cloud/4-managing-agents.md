---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los ejercicios anteriores, pediste al agente de Copilot en la nube que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente trabaja de forma asíncrona y autónoma, también es importante poder supervisar sus tareas.

Hay varias herramientas para gestionar las tareas que asignas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas con solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas y orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo decide que también sería buen momento para añadir un modo claro. Como ya se están actualizando los estilos del sitio y añadiendo controles, parece lógico incluir esta funcionalidad. Orientarás el trabajo del agente para que implemente ambos modos.

## Revisar las tareas del agente de Copilot en la nube

Consulta el estado actual de las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el lateral. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos variarán según el estado del trabajo de Copilot y el enfoque que haya elegido.

4. Observa también el panel de solicitudes de incorporación de cambios, situado a la derecha. Desde allí puedes consultar la PR y los archivos modificados.

## Dirigir el agente en la nube

Ahora que has revisado las tareas activas, pide a Copilot que añada un modo claro mientras implementa el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes como los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, envía esta indicación:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes con el cuadro para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa que Copilot la confirma y la incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo de trabajo cuando termine el paso que estaba realizando al recibir el mensaje.

El proceso tardará unos minutos. Aprovecha el tiempo para repasar lo que has aprendido hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas del agente en la nube. Puedes hacer seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para ajustar su alcance o proporcionar directrices adicionales.

Has explorado estos conceptos:

- supervisar las tareas del agente en la nube desde Mission Control y la página de agentes.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
