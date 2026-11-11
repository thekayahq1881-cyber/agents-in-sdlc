---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que documente el código, cree una función de juegos relacionados para que el equipo de diseño pueda probarla e implemente funciones de accesibilidad, como los modos de alto contraste y claro. Vamos a revisar los cambios y, si hace falta, pedir a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre debemos revisar el código generado y seguir el proceso habitual de DevOps. Revisaremos las propuestas de Copilot para documentar el código, crear la función de juegos relacionados e implementar las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, cuenta con restricciones de seguridad para mantener un entorno seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para trabajar.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno efímero independiente para realizar su trabajo.
- Los flujos de trabajo de GitHub Actions necesitan la aprobación de una persona antes de ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empezaremos con la primera solicitud de incorporación de cambios (PR) creada por el agente: añadir documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, puede que veas un aviso de que un firewall ha bloqueado a GitHub Copilot. Es **normal**, porque Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**. El título también podría ser más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los encabezados de archivo que proceden de las instrucciones personalizadas del Ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente. Esto puede tardar varios minutos.

## Pide cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de la PR o en comentarios en línea del código, como harías con cualquier otra persona del equipo. Copilot utilizará esos comentarios para iniciar otra sesión y resolverlos. Como los resultados no son deterministas, no hay un texto de prompt único; puedes pedirle, por ejemplo:

- que añada encabezados breves al principio de cada archivo de código.
- que añada comentarios TSDoc a archivos TypeScript y Astro.
- que cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario en la PR y menciona a **@copilot** para pedir un cambio en la documentación. Puedes usar una de las ideas anteriores.
2. Selecciona **View Session** para observar cómo Copilot inicia una nueva sesión y aplica el cambio.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una nueva confirmación en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando termines, puedes marcar la PR como lista para revisión y combinarla.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Ahora revisaremos la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas terminado, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Comprueba que se superen las comprobaciones del proyecto para la aplicación Astro.
9. **Opcional:** puedes cambiar a la rama del agente en el codespace para probar manualmente la función de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama que creó Copilot, como `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Puedes seguir iterando y pedirle que modifique, por ejemplo, el número de juegos que se muestran o que añada encabezados de comentarios y comentarios TSDoc. Como la incidencia se asignó **antes** de actualizar las instrucciones personalizadas, quizá no las aplique. Añade una solicitud como comentario en **Conversation** para que Copilot la vea e inicie otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad creadas con el agente personalizado. La PR debería incluir el modo de alto contraste que solicitaste en el Ejercicio 3 y el modo claro que pediste en mission control durante el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Selecciona **Files changed** para revisar el código.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre modos.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios CSS o de estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript o TypeScript que controla el cambio de modo.
6. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que se superen las comprobaciones del proyecto para la aplicación Astro.
10. **Opcional:** puedes cambiar a la rama del agente desde tu codespace y probar manualmente las funciones de accesibilidad:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que los cambios se mantienen al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a implementar las funciones según las prácticas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que los solucione.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para asignarle tareas reales del backlog mientras te ocupas de otras cosas. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés para patrocinadores en la página de detalles del juego.
- implementar la paginación en la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas** del repositorio para ver más adelante cómo influyen en el trabajo del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como mission control** para supervisar y dirigir la sesión de accesibilidad.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios** de Copilot mediante comentarios a **@copilot** y la aprobación de flujos de trabajo.

## Revisión y próximos pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP directamente en el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional de `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
