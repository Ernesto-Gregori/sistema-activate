# Mantenimiento del sistema "Dilo Como Es"

Este documento explica cómo administrar, respaldar y solucionar problemas comunes del sistema.

## 📦 Respaldo y restauración de datos

### Datos guardados en IndexedDB

El sistema usa dos bases de datos:

- `DiloGameDB` – contiene el estado actual del torneo y las preguntas.
- `DiloSoundDB` – contiene los buffers de sonidos y música personalizados.

Para **respaldar** manualmente:

1. Abre las herramientas de desarrollador (F12).
2. Ve a la pestaña **Application** → **IndexedDB**.
3. Haz clic derecho en `DiloGameDB` → **Exportar** (guardar como JSON).
4. Repite con `DiloSoundDB`.

Para **restaurar**:

- Usa la misma herramienta para importar los JSON.

> 💡 El juego también guarda automáticamente el estado del torneo cada 30 segundos y al cambiar de pantalla.

### Exportación de resultados del torneo

Desde la pantalla principal, haz clic en **📊 Exportar resultados**. Se generará un archivo Excel con:

- Resumen de equipos (puntaje final, eliminado)
- Detalle de eventos (log)
- Ranking final
- Rondas jugadas

## 🛠️ Personalización avanzada

### Sonidos y música por fases

Usa la pestaña **🔊 Sonidos**. Puedes cargar archivos MP3, WAV o OGG para cada evento:

| Evento            | Clave interna         |
|-------------------|-----------------------|
| Acierto           | `correct`             |
| Error             | `wrong`               |
| Tiempo agotado    | `timeout`             |
| Tick (cuenta regresiva) | `tick`         |
| Fanfarria (desempate) | `fanfare`         |
| Comodín           | `joker`               |
| Inicio de cuenta regresiva | `countdown`   |
| Fin de ronda      | `roundEnd`            |
| Música de inicio  | `music_start`         |
| Música ronda inicial / cuartos | `music_initial` |
| Música semifinal  | `music_semifinal`     |
| Música final      | `music_final`         |
| Música desempate  | `music_tiebreaker`    |

Los archivos se guardan en `DiloSoundDB`. Para restaurar el sonido por defecto, haz clic en **Restaurar predefinido**.

### Pantalla de espera

En **Ajustes** → **Pantalla de espera personalizable**:

- **Mensaje** – texto que se muestra.
- **Imagen** – puedes subir una imagen (se convierte a base64 y se guarda en IndexedDB).

### Preguntas

Desde la pestaña **📋 Preguntas**:

- **Importar Excel**: el archivo debe tener columnas: `tipo`, `etapa`, `nivel`, `texto`, `opciones` (separadas por `|`), `respuesta`.
- **Importar JSON**: estructura de array de objetos.
- **Exportar**: tanto JSON como Excel.

## ⚠️ Solución de problemas comunes

| Problema | Posible causa | Solución |
|----------|---------------|----------|
| El proyector no se actualiza | Bloqueo de ventanas emergentes | Permite ventanas emergentes para el sitio. Cierra y vuelve a abrir el proyector. |
| No se escuchan los sonidos | AudioContext suspendido | Haz clic en cualquier botón (por ejemplo, "Probar") para activar el audio. Los navegadores requieren interacción del usuario. |
| El juego no guarda el estado | IndexedDB llena o corrupta | Abre Application → IndexedDB → elimina `DiloGameDB` y recarga. Se recreará automáticamente. |
| Error "loop infinito" en la consola | Mensajes excesivos por segundo | Revisa que no haya `useEffect` sin dependencias o con dependencias que cambien siempre. Usa `debugger.html` para detectar la causa. |
| El control remoto no responde | BroadcastChannel bloqueado | Asegúrate de que ambas ventanas estén en el mismo origen (mismo protocolo, puerto y dominio). Usa `localhost` o HTTPS. |
| La música no cambia de fase | Fase incorrecta en `AudioEngine.setMusicPhase` | Verifica que el nombre de la fase sea uno de: `'start'`, `'initial'`, `'semifinal'`, `'final'`, `'tiebreaker'`. |

## 🔄 Actualización del sistema

El sistema es un único archivo HTML. Para actualizar:

1. Descarga la nueva versión de `index.html`.
2. Reemplaza el archivo anterior.
3. **Importante**: Los datos de IndexedDB persisten. Si hay cambios estructurales en la base de datos, se disparará `onupgradeneeded` automáticamente al abrir la nueva versión. No se perderán datos.
4. Limpia la caché del Service Worker si notas comportamientos extraños: Application → Service Workers → Update / Unregister.

## 📞 Soporte

Para reportar errores o solicitar mejoras, abre un issue en el repositorio de GitHub:  
https://github.com/Ernesto-Gregori/sistema-activate