# 🛠️ Documento técnico – Arquitectura y componentes

## 1. Estructura del archivo
- HTML único con estilos CSS, código JavaScript (Babel) e inline Service Worker.
- Dependencias externas (cargadas desde CDN) pero cacheadas localmente.

## 2. Componentes React principales

| Componente | Función |
|------------|---------|
| `App` | Maneja pestañas (setup, questions, game, sound) y persistencia global (preguntas). |
| `TeamSetup` | Configuración de nombres de equipos. |
| `QManager` | CRUD de preguntas, import/export, reset a valores por defecto. |
| `SoundManager` | Configuración de sonidos personalizados y música por fases. Usa IndexedDB. |
| `GameView` | Control central del torneo: enfrentamientos, preguntas, puntajes, música de fondo. |
| `MatchBoard` | Panel de control de un enfrentamiento (puntos rápidos, comodines). |
| `ProjectorView` | Vista para pantalla pública (se abre en ventana nueva). |
| `TournamentMgr` | Muestra enfrentamientos, posiciones, historial y opciones de ronda. Detecta cambios de fase musical. |
| `TieBreaker` | Modal para resolver empates por recitación de un texto. Cambia la música a fase "tiebreaker". |
| `Timer` | Componente visual de cuenta regresiva con sonido de tick. |
| `WaitScreen` | Pantalla de transición entre rondas. |
| `RoundHistory` | Historial colapsable de rondas completadas. |
| `ScoreFloat` | Animación de puntos flotantes. |
| `AudioEngine` | Módulo singleton para manejo de Web Audio. Gestiona buffers de sonidos y música, IndexedDB, y reproducción por fases. |

## 3. Estados persistentes

| Clave | Contenido | Almacenamiento |
|-------|------------|----------------|
| `dilo_como_es_save` | Estado completo del torneo (equipos, puntajes, rondas, enfrentamientos, pregunta activa, selecciones, etc.) | localStorage |
| `dilo_como_es_questions` | Banco de preguntas personalizado | localStorage |
| `DiloSoundDB` (IndexedDB) | Almacena los buffers de sonidos personalizados y músicas por fase. Claves: `correct`, `wrong`, `timeout`, `tick`, `fanfare`, `joker`, `countdown`, `music_start`, `music_initial`, `music_semifinal`, `music_final`, `music_tiebreaker` | IndexedDB |

## 4. Música por fases
- **Fases**: `start`, `initial`, `semifinal`, `final`, `tiebreaker`.
- `AudioEngine.musicBuffers` guarda los buffers para cada fase.
- Si no hay buffer personalizado, se usa una síntesis de acordes con tempo y notas adaptadas a la fase (ver `playDefaultMusicForPhase`).
- `TournamentMgr` observa `roundName` y llama a `AudioEngine.setMusicPhase()` con la fase correspondiente.
- `TieBreaker` cambia a `tiebreaker` al abrirse, y el cierre del modal no restaura la música automáticamente (se deja que `TournamentMgr` lo haga al actualizar la ronda).

## 5. Flujo de datos
- `GameView` envía actualizaciones al `ProjectorView` mediante `postMessage`.
- Los cambios en preguntas o torneo se guardan automáticamente con `useEffect`.
- El Service Worker intercepta las peticiones a CDNs y sirve respuestas cacheadas.
- Los archivos de audio se cargan, se clonan (`arrayBuffer.slice(0)`) para evitar el error "detached ArrayBuffer", se decodifican y se guardan en IndexedDB. El buffer decodificado se usa para reproducción.

## 6. Seguridad y limitaciones
- No se envía información a ningún servidor.
- Máximo 8 equipos por diseño (ajustable en `TeamSetup`).
- Las preguntas de Etapa 1 deben tener exactamente 4 opciones.
- Las preguntas de Etapa 2 y 3 usan campo `answer` (texto).
- Los archivos de audio deben ser compatibles con `AudioContext.decodeAudioData` (MP3, WAV, OGG son soportados en la mayoría de navegadores).

## 7. Personalización avanzada
- Colores de equipos: modificar el array `TC` en el código.
- Tiempos y puntajes: modificar el objeto `SC`.
- Preguntas iniciales: modificar `DEMO_Q`.
- Frecuencias y tempo de la música sintética por defecto: modificar `playDefaultMusicForPhase`.