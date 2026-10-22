---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas GitHub Copilot en modo agente, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar las diferencias, aceptar o rechazar cambios individuales y dirigir la conversación con indicaciones de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- dirigirás el agente durante la sesión para añadir un modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes consideran demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación, dirigirás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada cambio aparece en el chat con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerá la diferencia:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y dejar que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso, por ejemplo para ejecutar un comando de terminal, el agente se detendrá y mostrará la solicitud **Allow**. Aprueba solo los comandos que te parezcan seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Pedir al agente que añada el modo claro

Una vez implementado el modo de alto contraste, amplía la misma conversación para añadir un control del modo claro. Como continúas en la misma sesión, el agente conserva el contexto de su trabajo: los componentes modificados, los estilos y el patrón de almacenamiento local.

1. Envía esta indicación de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo, explorará los cambios que ya ha realizado y propondrá los cambios para el modo claro.
3. Revisa cada nueva edición y utiliza **Keep** o **Undo** para decidir si la aceptas.
4. Si el agente toma un rumbo que no te convence —por ejemplo, intenta reorganizar los controles de una forma que no quieres—, detenlo con el botón **Stop** junto a la entrada del chat y envíale una indicación para aclarar lo que necesitas.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Utiliza indicaciones de seguimiento para pedir ajustes («simplifica el CSS; utiliza variables CSS en vez de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que siguen las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando hayas terminado, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión activa: has revisado cada edición, aceptado o rechazado cambios y enviado indicaciones de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- has supervisado el trabajo del agente en tiempo real.
- has aceptado y rechazado ediciones individuales.
- has dirigido una sesión en curso mediante indicaciones de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha producido el agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
