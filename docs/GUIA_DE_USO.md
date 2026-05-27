# 📘 Guía de uso – Operador del concurso

## 1. Acceso al sistema
- Abre el archivo `DiloComoEs_ACTIVATE2026_v4.html` en **Chrome**, **Edge** o **Firefox** (recomendado).
- No se necesita servidor web; funciona completamente offline después de la primera carga con internet.

## 2. Pantalla de inicio
- Configura los equipos (mínimo 2, máximo 8). Puedes cambiar sus nombres.
- Si existe una partida guardada, aparecerá la opción **"Cargar última"**.
- Haz clic en **"COMENZAR TORNEO"** para iniciar.

## 3. Interfaz principal del juego (tres columnas)

### Columna izquierda – Banco de preguntas
- **Filtros**: por etapa (E1, E2, E3) y nivel (Nv1, Nv2).
- **Banco**: lista de preguntas disponibles. Haz clic en una para seleccionarla como pregunta activa.
- **Reiniciar usadas**: vuelve a mostrar preguntas ya utilizadas.

### Columna central – Control de la pregunta y enfrentamiento
- **Pregunta activa**: se muestra con su etiqueta (Etapa, Nivel).
- **Botones del temporizador**:
  - ▶ Iniciar / ⏸ Pausar / ↺ Reset
  - ✓ Usada (marca la pregunta como utilizada)
- **Modos de respuesta (solo Etapa 1)**:
  - **👥 Todos responden**: cada equipo selecciona su opción.
  - **🎯 Un equipo responde**: el operador elige qué equipo tiene el turno.
- **Selección de opciones**: cada equipo (o el elegido) pulsa la letra A/B/C/D.
- **Confirmar respuesta(s)**: evalúa las selecciones y asigna puntos.
- **⏱ Tiempo agotado**: penaliza a quienes no respondieron.
- **MatchBoard** (cuando hay enfrentamiento activo):
  - Botones de puntuación rápida (+100, +200, +500, -50, -100, -200).
  - Comodines: **50/50** (oculta dos opciones incorrectas), **Cap** (+5 segundos), **Cmb** (cambia pregunta por otra del mismo nivel/etapa).
  - **PASO**: salta la pregunta sin penalización (solo una vez por enfrentamiento).
- **Terminar enfrentamiento**: guarda el resultado y pasa al siguiente.
- **Botones inferiores**:
  - **⛶ Abrir proyector** (ventana independiente para público).
  - **💾 Guardar** (guarda manualmente el estado actual).
  - **🔄 Nueva** (reinicia todo el torneo).
  - **🎵 / 🎶** (enciende o apaga la música de fondo).

### Columna derecha – Gestión del torneo
- **Ronda actual** y número de equipos activos.
- **⚡ Desempate – Palabra por Palabra** (sirve para definir empates).
- **ENFRENTAMIENTOS**: lista de duelos generados. Haz clic en **"▶ Iniciar"** para comenzar un enfrentamiento.
- **POSICIONES**: tabla de puntuaciones actuales.
- **HISTORIAL**: registro de rondas y eliminaciones.
- **⚡ Generar enfrentamientos** (una vez que la ronda está lista).

## 4. Proyector (pantalla para el público)
- Se abre en una ventana nueva.
- Muestra la pregunta, el temporizador grande, las opciones y las selecciones de los equipos con sus colores y letras fijas (A, B, C, D…).
- El operador no necesita interactuar con ella; se actualiza automáticamente.

## 5. Pestaña "🎵 Sonidos" – Configuración de audio avanzada
### Sonidos de eventos
- **Acierto**, **Error**, **Tiempo agotado**, **Tick (cuenta regresiva)**, **Fanfarria (desempate)**, **Comodín**, **Inicio de cuenta regresiva**.
- Para cada uno: botón **🔊 Probar**, **📁 Cargar archivo** (MP3, WAV, OGG), **🔄 Restaurar predefinido**.

### Música por fases (5 pistas independientes)
- **Música de inicio**: suena cuando se abre el sistema y en el proyector inactivo.
- **Música ronda inicial / cuartos**: durante las fases iniciales del torneo.
- **Música semifinal**: cuando se alcanza la semifinal.
- **Música final**: durante la ronda final.
- **Música desempate**: suena automáticamente al abrir un desempate.
- Cada una puede cargarse con su propio archivo. Si no se carga, se usa la música sintética por defecto (estilo "¿Quién quiere ser millonario?" pero adaptada a la tensión de cada fase).

### Persistencia
- Todos los sonidos y músicas personalizadas se guardan en **IndexedDB** y se restauran automáticamente al recargar la página.

## 6. Pestaña "📋 Preguntas" (administración)
- Permite **crear, editar, eliminar, importar y exportar** preguntas.
- Las preguntas se guardan automáticamente en el navegador.
- **⟳ Restablecer predeterminadas** vuelve a las preguntas de ejemplo.

## 7. Consejos para el evento
- Antes del evento, abre el sistema una vez con conexión a internet para que el Service Worker cachee los recursos (React, fuentes, etc.). Luego funcionará sin conexión.
- Realiza una prueba con un par de preguntas para verificar el sonido y el proyector.
- Carga tus propias canciones para cada fase en la pestaña de sonidos. El sistema las recordará incluso después de cerrar el navegador.
- Usa el botón de música (🎵/🎶) para pausar o reanudar la música en cualquier momento.