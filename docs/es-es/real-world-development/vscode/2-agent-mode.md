---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo de desarrollo puede implicar localizar todos los archivos necesarios, hacer los cambios, ejecutar pruebas, depurar, averiguar qué archivo falta, volver a modificarlo… y así sucesivamente.

El modo agente de Copilot ayuda a gestionar este trabajo.

El modo agente está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo utilizarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite iterar y colaborar rápidamente. El agente en la nube funciona de manera asíncrona, como un compañero de equipo que interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo el modo agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina los cambios.
- cómo puede implementar funcionalidades en la interfaz y la capa de datos.
- cómo revisar los cambios y las pruebas generados por Copilot antes de integrarlos en el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que las personas filtren por categoría y editor. En el ejercicio anterior creaste una función auxiliar para obtener editores; ahora completarás con el modo agente el trabajo restante en la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender su funcionalidad actual.

El sitio de Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo. Permite mostrar juegos y sus detalles. Se trata de una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un marco de interfaz ejecutado en el cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con scripts `<script>` pequeños y específicos de Astro mediante las API estándar del DOM.

### Inicia el sitio

Para facilitar la ejecución del sitio, el comando `npm run dev` inicia el servidor de desarrollo de Astro.

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador con `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar un enlace localhost desde el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al servidor que se ejecuta en tu codespace.

### Explora el sitio

Cuando el sitio esté en marcha, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, verás información adicional, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará cuándo cambiarlos.

El sitio funciona, pero queremos mejorar sus capacidades. Primero, revisemos el backlog. Al crear el proyecto a partir de la plantilla se generaron incidencias automáticamente. Pide a GitHub Copilot que las muestre.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos.

> [!CAUTION]
> Los autores no recomiendan un modelo concreto. Durante la elaboración del taller se utilizó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes, pero los modelos de lenguaje son probabilísticos y los resultados pueden variar. Es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje, Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía el siguiente prompt para preguntar por el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según las que considera más útiles para las personas usuarias.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [instrucciones de interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las pautas generales para añadir funcionalidad:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, pruebas y accesibilidad.
   - enlaces a archivos de instrucciones para tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo que contiene el enlace. Así se pueden reutilizar ejemplos, dividir instrucciones complejas y proporcionar plantillas.

## Implementa el filtrado

Para completar el filtrado tendrás que realizar al menos tres cambios:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

También debes ejecutar las pruebas y confirmar que se superan antes de integrar los cambios. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa en la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura donde se muestra cómo seleccionar el archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad a partir de la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relevantes de la funcionalidad: las funciones auxiliares de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura donde se muestra a Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, habrá una opción **Keep** o **Undo** para cada cambio; no hace falta que selecciones ninguna mientras el trabajo siga en curso.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que el proceso se interrumpa y que algunas pruebas fallen. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! Has utilizado el modo agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido:

- cómo implementar funcionalidades en la interfaz y la capa de datos.
- cómo explorar el proyecto, identificar archivos relevantes y coordinar cambios.
- cómo revisar los cambios y las pruebas generados antes de integrarlos en el código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, puede que necesites añadir paginación. Utiliza lo que has aprendido para pedir a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación; por ejemplo, si permitirás elegir el tamaño de página o lo establecerás de forma fija.
- proporciona a Copilot las indicaciones necesarias para crear la implementación que necesitas.
- puede que tengas que iterar con Copilot y proporcionar más contexto. Es lo habitual.

## Recursos

- [Modos ask, edit y agent de Copilot: diferencias y usos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
