# 🔧 Documento de mantenimiento

## 1. Actualización de preguntas
1. Ir a la pestaña **"📋 Preguntas"**.
2. Usar el formulario para agregar, editar o eliminar preguntas.
3. Exportar a JSON como respaldo.
4. Para cargar un nuevo banco completo, usar **"⬆ Importar JSON"**.

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
- La partida se guarda automáticamente en `localStorage` cada 30 segundos y al realizar acciones clave.
- Para respaldo manual:
  - Abrir DevTools (F12) → Aplicación → Almacenamiento local → Copiar el valor de `dilo_como_es_save`.
  - También se puede usar el botón **💾 Guardar**.
- Los sonidos personalizados se guardan en IndexedDB (no es fácil exportarlos manualmente, pero persisten entre sesiones).

## 4. Restauración de una partida guardada
- Si se cerró el navegador, al abrir la página aparecerá la opción **"Restaurar"**.
- Si se perdió el `localStorage`, pegar el respaldo en la consola:

```js
localStorage.setItem('dilo_como_es_save', 'aquí_el_json');
```

Luego recargar la página.

---

## 5. Limpieza de datos

- Para reiniciar completamente el torneo: botón **"🔄 Nueva"**.

- Para borrar solo las preguntas personalizadas y volver a las predeterminadas: botón **"⟳ Restablecer predeterminadas"** en la pestaña preguntas.

- Para borrar todos los datos (partida + preguntas + sonidos personalizados):

  1. Abrir **DevTools → Aplicación → Almacenamiento local → Limpiar todo**.
  2. Abrir **DevTools → Aplicación → IndexedDB → Eliminar la base de datos `DiloSoundDB`**.

- Para borrar solo los sonidos personalizados pero mantener la partida y preguntas:

  - En la pestaña **"🎵 Sonidos"**, usar **"🔄 Restaurar predefinido"** para cada sonido/fase, o eliminar manualmente la base de datos IndexedDB.

---

## 6. Solución de problemas comunes

| Problema | Posible causa | Solución |
|---|---|---|
| El proyector no muestra nada | Ventana bloqueada por popup | Permitir ventanas emergentes en el navegador. |
| No se escucha sonido/música | `AudioContext` suspendido | Hacer clic en el botón 🎵 (esto reanuda el contexto). |
| Las preguntas personalizadas desaparecen | Se borró el `localStorage` o se cambió de navegador | Exportar periódicamente el banco de preguntas a JSON. |
| El torneo no avanza | Enfrentamiento no generado | Hacer clic en **"⚡ Generar enfrentamientos"** en la columna derecha. |
| El temporizador se detiene | Se pausó manualmente o el equipo usó PASO | Verificar estado del botón ⏸. |
| Al cargar un sonido aparece error `ArrayBuffer is detached` | (Este error ya está corregido en la versión actual) | Asegurar que se usa `arrayBuffer.slice(0)` antes de decodificar. |
| La música no cambia de fase automáticamente | El nombre de la ronda no coincide con las fases esperadas (`Ronda Inicial`, `Cuartos de Final`, `Semifinal`, `Final`) | Verificar en `TournamentMgr` que `roundName` tenga exactamente esos valores. |

---

## 7. Soporte y actualizaciones

- El código está completamente contenido en un solo archivo HTML. Para actualizar, reemplazar el archivo.

- Se recomienda mantener una copia de seguridad del archivo original antes de modificar.

- Para reportar errores o solicitar mejoras: contactar al desarrollador.