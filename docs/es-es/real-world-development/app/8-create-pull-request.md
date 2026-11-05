---
title: "Lección 8 - Crear y combinar la solicitud de incorporación de cambios de funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas; después, crea una solicitud de incorporación de cambios y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios (PR). Ya combinaste personalmente la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la función, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y lo incluirás en esta PR.

En esta lección:

- conocerás Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás la PR completa de la función y las pruebas de verificación.
- autorizarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión de la aplicación lee la PR y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y hace rebase de los cambios cuando es necesario. Después, la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando aunque se reinicie la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad, pero su capacidad para editar código y combinarlo requiere tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de concederle permiso para combinar.

## Usa Agent Merge para gestionar la PR

Con todo el código creado y revisado, deja que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

El proceso de Agent Merge comenzará y:

- Creará la solicitud de incorporación de cambios con un título y una descripción.
- Si iniciaste la sesión desde una incidencia, la mencionará en el cuerpo de la descripción.
- Reorganizará la base o resolverá posibles conflictos con la rama de destino.
- Supervisará el proceso de integración continua para comprobar que se superen todas las verificaciones.
- Supervisará la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y aplicará cambios para resolverlos.
- De forma opcional, combinará automáticamente la PR cuando todo se haya completado correctamente.

Permite que Agent Merge también combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Combina la PR cuando se hayan superado todas las comprobaciones y aprobaciones obligatorias. No eludas las protecciones del repositorio.
    ```

9. Supervisa el progreso y resuelve los bloqueos que indique. Antes de continuar, confirma que el estado de la PR sea **Merged**. Tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos ausentes. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas y validación, y ahora también el proceso de la solicitud de incorporación de cambios. En concreto:

- has conocido Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has inspeccionado la PR completa de la función y las pruebas de verificación.
- has autorizado Agent Merge solo después de revisar los cambios y has confirmado la combinación de la PR.

A continuación, [utilizarás un Canvas existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
