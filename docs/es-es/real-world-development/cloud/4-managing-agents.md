---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste al agente en la nube de Copilot que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube puede trabajar de forma autónoma y asíncrona, es importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas de agentes con solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones que han realizado e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de pedir al agente que creara un modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo una opción de alternancia, parecía lógico incluir esta función. Guiarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Vamos a consultar el estado de las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una de las tareas en curso. Revisa las acciones que ha realizado Copilot, por ejemplo:
   - obtener el código del repositorio.
   - preparar el entorno para que trabaje Copilot.
   - configurar servidores MCP.
   - realizar los distintos pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado de la tarea de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitud de incorporación de cambios que aparece a la derecha. Desde ahí puedes consultar la PR y los archivos modificados para seguir supervisando el trabajo.

## Dirige el agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que incluya el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado de la tarea.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que ha terminado de configurarse y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa que Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot comenzará a trabajar en la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando al recibir el mensaje.

La tarea puede tardar varios minutos, así que puedes aprovechar el tiempo para reflexionar sobre lo que has aprendido hasta ahora.

## Resumen y próximos pasos

En este ejercicio has explorado la página de agentes de Copilot, el espacio central para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde ahí puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o proporcionar nuevas indicaciones.

Has aprendido a usar mission control y la página de agentes para supervisar tareas y redirigir una sesión en curso para pedir una función adicional.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
