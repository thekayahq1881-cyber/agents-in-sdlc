---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo. Puedes revisar las diferencias, aceptar o rechazar cambios y orientar la conversación con prompts adicionales sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- orientarás la sesión para añadir un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un modo claro para las personas a quienes les resulte demasiado oscuro el tema predeterminado. En vez de iniciar otra conversación desde cero, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a Copilot Chat desde el Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta, lectura y edición de archivos, además de los comandos de terminal que solicita ejecutar.
3. Cuando proponga una edición:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Cuando el modo de alto contraste esté listo, amplía la misma conversación para añadir una opción de modo claro. Como continuarás en la misma sesión, el agente conservará el contexto del trabajo, los componentes que modificó, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía el siguiente prompt de seguimiento desde Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente retomará el trabajo, explorará los cambios realizados y propondrá los necesarios para el modo claro.
3. Revisa cada nueva edición y selecciona **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres, por ejemplo, si refactoriza la interfaz de alternancia de una forma que no te convence, detenlo con el botón **Stop** junto al cuadro de entrada y envía un prompt aclaratorio.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Pide ajustes con prompts de seguimiento, como «simplifica el CSS y usa variables CSS en lugar de duplicar selectores», corrige el rumbo («no añadas una tercera opción; integra el modo claro en el panel existente») o amplía el alcance («añade también métodos abreviados de teclado para ambas opciones»).

## Revisa los archivos modificados

Antes de confirmar los cambios, revisa todos los archivos que haya modificado el agente.

1. Abre **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías encontrar componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se utilizan los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio has aprendido a:

- supervisar el trabajo de un agente en tiempo real.
- aceptar y rechazar ediciones individuales.
- orientar una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios del agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
