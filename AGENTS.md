# Reglas del Proyecto - Arquitectura de Agentes

## Arquitectura de Orquestación Local (Argon -> Flash)
En este repositorio, el agente principal opera con **Gemini 4 Argon** (`gemini-4-argon` con esfuerzo `high`) y delega la implementación táctica al subagente local **`flash-executor`** (`gemini-3.8-flash-high`).

1. **Rol de Gemini 4 Argon (Orquestador)**:
   - Analizar requerimientos, diseñar la arquitectura, definir interfaces/tipos y dividir las tareas en unidades de trabajo independientes.
   - Evitar escribir implementaciones extensas o código repetitivo de forma directa cuando pueda delegarse.
2. **Despacho Paralelo con `invoke_subagent`**:
   - Para implementar funcionalidades, refactorizar módulos o escribir tests, invoca una o varias instancias en paralelo del subagente `flash-executor`.
   - Dado que cada subagente tiene contexto aislado, cada llamada a `invoke_subagent` debe incluir un prompt autocontenido con:
     a) Rutas exactas de los archivos objetivo.
     b) Firmas de funciones, interfaces o contratos a respetar.
     c) El comando específico de test/lint que el subagente debe ejecutar para validar su cambio.
     d) Modo de aislamiento (`inherit` para módulos en carpetas separadas o `branch` si hay riesgo de colisión de archivos).
3. **Revisión e Integración**:
   - Una vez que los subagentes `flash-executor` finalicen, revisa los diffs, ejecuta la suite de pruebas general del proyecto y resuelve cualquier conflicto de integración.
