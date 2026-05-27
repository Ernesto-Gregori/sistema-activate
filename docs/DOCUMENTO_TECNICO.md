# 🛠️ Documento técnico – Arquitectura y componentes

## 1. Estructura del archivo
- HTML único con estilos CSS, código JavaScript (Babel) e inline Service Worker.
- Dependencias externas (cargadas desde CDN) pero cacheadas localmente.

## 2. Componentes React principales

| Componente | Función |
|------------|---------|
| `App` | Maneja pestañas (setup, questions, game) y persistencia global. |
| `TeamSetup` | Configuración de nombres de equipos. |
| `QManager` | CRUD de preguntas, import/export, reset a valores por defecto. |
| `GameView` | Control central del torneo: enfrentamientos, preguntas, puntajes. |
| `MatchBoard` | Panel de control de un enfrentamiento (puntos rápidos, comodines). |
| `ProjectorView` | Vista para pantalla pública (se abre en ventana nueva). |
| `TournamentMgr` | Muestra enfrentamientos, posiciones, historial y opciones de ronda. |
| `Timer` | Componente visual de cuenta regresiva con sonido de tick. |
| `TieBreaker` | Modal para resolver empates por recitación de un texto. |
| `WaitScreen` | Pantalla de transición entre rondas. |
| `RoundHistory` | Historial colapsable de rondas completadas. |
| `ScoreFloat` | Animación de puntos flotantes. |
| `AudioEngine` | Módulo singleton para manejo de Web Audio (sonidos y música). |

## 3. Estados persistentes (localStorage)

| Clave | Contenido |
|-------|------------|
| `dilo_como_es_save` | Estado completo del torneo: equipos, puntajes, rondas, enfrentamientos, pregunta activa, temporizador, selecciones, comodines usados, historial, etc. |
| `dilo_como_es_questions` | Banco de preguntas personalizado (array de objetos). |

## 4. Flujo de datos
- `GameView` envía actualizaciones al `ProjectorView` mediante `postMessage`.
- Los cambios en preguntas o torneo se guardan automáticamente con `useEffect`.
- El Service Worker intercepta las peticiones a CDNs y sirve respuestas cacheadas.

## 5. Seguridad y limitaciones
- No se envía información a ningún servidor.
- Máximo 8 equipos por diseño (ajustable en `TeamSetup`).
- Las preguntas de Etapa 1 deben tener exactamente 4 opciones.
- Las preguntas de Etapa 2 y 3 usan campo `answer` (texto).

## 6. Personalización avanzada
- Colores de equipos: modificar el array `TC` en el código.
- Tiempos y puntajes: modificar el objeto `SC`.
- Preguntas iniciales: modificar `DEMO_Q`.