# 🔧 Documento de mantenimiento

## 1. Actualización de preguntas
### Desde la interfaz
1. Ir a la pestaña **"📋 Preguntas"**.
2. Usar el formulario para agregar, editar o eliminar preguntas.
3. Exportar a JSON o Excel como respaldo.

### Desde Excel
1. Crear un archivo Excel con las columnas: `etapa`, `nivel`, `texto`, `opciones` (separadas por `|`), `respuesta`.
2. En la pestaña **"📋 Preguntas"**, usar **"📊 Importar Excel"** y seleccionar el archivo.
3. Las preguntas importadas reemplazarán el banco actual.

## 2. Personalización de sonidos y música
### Sonidos de eventos
- Ir a la pestaña **"🎵 Sonidos"**.
- Para cada efecto, usar **"📁 Cargar"** y seleccionar un archivo de audio (MP3, WAV, OGG).
- El sonido se guarda automáticamente en IndexedDB y se reemplaza el predefinido.
- Usar **"🔊 Probar"** para escuchar el sonido actual.
- Usar **"🔄 Restaurar predefinido"** para volver al sonido sintético original.

### Música por fases
- Misma pestaña, sección "Música por fases".
- Cada fase (inicio, ronda inicial, semifinal, final, desempate) puede tener su propia canción.
- Al cargar un archivo para una fase, la música cambiará automáticamente si esa fase está activa.
- Si no se carga música para una fase, se usará la versión sintética por defecto (con tensión creciente).

## 3. Respaldo de partidas
- La partida se guarda automáticamente en IndexedDB cada 30 segundos y tras cada acción relevante.
- Para respaldo manual:
  - Abrir DevTools (F12) → **Aplicación** → **IndexedDB** → `DiloGameDB` → `gameState` → copiar el valor.
  - También se puede usar el botón **💾 Guardar** (que fuerza una escritura en IndexedDB).
- Los sonidos personalizados se guardan en `DiloSoundDB` (persisten entre sesiones).

## 4. Restauración de una partida guardada
- Si se cerró el navegador, al abrir la página aparecerá la opción **"Restaurar"** (si existe una partida en IndexedDB).
- Si se perdió IndexedDB (por ejemplo, al limpiar datos del navegador), no hay forma de recuperar la partida a menos que se haya exportado previamente el estado.

## 5. Limpieza de datos
- Para reiniciar completamente el torneo: botón **"🔄 Nueva"**.
- Para borrar solo las preguntas personalizadas y volver a las predeterminadas: botón **"⟳ Restablecer predeterminadas"** en la pestaña preguntas.
- Para borrar todos los datos (partida + preguntas + sonidos personalizados):
  - Abrir DevTools → **Aplicación** → **IndexedDB** → Eliminar las bases de datos `DiloGameDB` y `DiloSoundDB`.
- Para borrar solo los sonidos personalizados pero mantener la partida y preguntas:
  - En la pestaña "🎵 Sonidos", usar **"🔄 Restaurar predefinido"** para cada sonido/fase, o eliminar manualmente la base de datos `DiloSoundDB`.

## 6. Solución de problemas comunes

| Problema | Posible causa | Solución |
|----------|---------------|----------|
| El proyector no muestra nada | Ventana bloqueada por popup | Permitir ventanas emergentes en el navegador. |
| No se escucha sonido/música | AudioContext suspendido | Hacer clic en el botón 🎵 (esto reanuda el contexto). |
| Las preguntas personalizadas desaparecen | Se borró IndexedDB o se cambió de navegador | Exportar periódicamente el banco de preguntas a Excel/JSON. |
| El torneo no avanza | Enfrentamiento no generado | Hacer clic en **"⚡ Generar enfrentamientos"** en la columna derecha. |
| El temporizador se detiene | Se pausó manualmente o el equipo usó PASO | Verificar estado del botón ⏸. |
| Al cargar un sonido aparece error `ArrayBuffer is detached` | (Este error ya está corregido) | Asegurar que se usa `arrayBuffer.slice(0)` antes de decodificar. |
| La música no cambia de fase automáticamente | El nombre de la ronda no coincide con las fases esperadas (`Ronda Inicial`, `Cuartos de Final`, `Semifinal`, `Final`) | Verificar en `TournamentMgr` que `roundName` tenga exactamente esos valores. |
| El temporizador de desempate no avanza | El estado `running` no se está actualizando correctamente | Asegurar que el callback `onStateChange` se llame cada vez que cambia el estado dentro de `TieBreaker`. |

## 7. Soporte y actualizaciones
- El código está completamente contenido en un solo archivo HTML. Para actualizar, reemplazar el archivo.
- Se recomienda mantener una copia de seguridad del archivo original antes de modificar.
- Para reportar errores o solicitar mejoras: contactar al desarrollador.