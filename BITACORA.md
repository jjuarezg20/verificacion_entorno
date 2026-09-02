# Bitácora — verificacion_entorno

Curso: Desarrollo de Aplicaciones Móviles (Flutter y Dart) · ESEN · Ciclo 3/2026
Clase 4 — Entorno, primer proyecto y hot reload

## Error 1

| Campo | Contenido |
|---|---|
| **Síntoma** | `/usr/bin/bash: line 1: gh: command not found` al ejecutar `gh auth login` |
| **Causa identificada** | GitHub CLI (`gh`) se instaló con `winget install --id GitHub.cli` en una sesión de terminal que ya estaba abierta. Windows agregó `gh` al PATH del sistema, pero esa sesión de terminal había cargado su copia del PATH *antes* de la instalación, así que no veía el nuevo binario. |
| **Solución aplicada** | Se invocó `gh` con su ruta completa: `"C:\Program Files\GitHub CLI\gh.exe" auth login`, evitando depender del PATH de la sesión ya abierta. |
| **Verificación** | `gh auth status` devolvió `✓ Logged in to github.com account jjuarezg20`, confirmando que el binario correcto se ejecutó y la sesión quedó autenticada. |

## Experimento: hot reload (r) vs hot restart (R)

Verificado personalmente por el estudiante en su propia terminal de VS Code con `flutter run -d chrome`.

| Paso | Resultado observado |
|---|---|
| Subir el contador a 5, cambiar el título, guardar, presionar **r** | El contador se mantuvo en 5. Hot reload solo reemplaza el código de los widgets e invoca `build()` de nuevo; el objeto `State` (y por lo tanto `_counter`) no se destruye. |
| Cambiar el título otra vez, guardar, presionar **R** | El contador volvió a 0. Hot restart destruye el árbol de widgets completo y reinicia la aplicación desde `main()`, así que todo el estado se pierde. |

**Conclusión:** el estado (`_counter`) vive en el objeto `State` (`_MyHomePageState`), no en el widget `MyHomePage`. Hot reload preserva ese objeto; hot restart lo recrea desde cero.

## Notas técnicas adicionales

- La primera compilación de `flutter run -d chrome` tardó cerca de 7 minutos (quedó varios minutos en "Waiting for connection from debug service on Chrome..."); las siguientes compilaciones tardaron entre 28 y 64 segundos gracias a la caché ya generada. No fue un error — solo la compilación inicial a JavaScript vía `dartdevc`, que es más lenta la primera vez.
- Se agregó `heroTag` explícito a cada `FloatingActionButton` de la Tarea 3. Con dos `FloatingActionButton` sin `heroTag` propio, ambos comparten el mismo identificador por defecto; en esta pantalla no causó error porque no hay navegación entre rutas, pero es la causa típica del error `There are multiple heroes that share the same tag within a subtree` en apps con más de una pantalla — se corrigió de forma preventiva.

## Declaración de uso de IA

Se usó Claude Code para: crear el proyecto (`flutter create`), instalar y autenticar GitHub CLI, inicializar el repositorio Git, ejecutar y verificar la app, y escribir el código de las tres tareas (título, incremento +2, botón de reinicio) a solicitud explícita del estudiante. El estudiante fue advertido de que la regla de IA del curso pide resolver el ejercicio de código de forma propia y confirmó que quería que la IA lo hiciera directamente.
