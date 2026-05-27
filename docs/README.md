# Dilo Como Es – ACTIVATE 2026

**Sistema de concurso bíblico para eventos juveniles.**  
Diseñado para operar en una laptop (operador) y una pantalla externa (público).  
Totalmente offline después de la primera carga. Persistencia completa de partidas, preguntas y sonidos personalizados.

## Características principales
- 🎮 Torneo por rondas (Inicial → Cuartos → Semifinal → Final) con eliminación del equipo de menor puntaje.
- ⏱️ Temporizador por pregunta (10s, 15s, 20s) con cuenta regresiva audible.
- 🃏 Comodines: 50/50, Ayuda Capitán (+5s), Cambio de pregunta.
- 🎵 **Música por fases**: pista independiente para inicio, ronda inicial, semifinal, final y desempate. Cada fase puede tener su propia canción personalizada.
- 🔊 **Sonidos personalizables**: todos los efectos (acierto, error, timeout, tick, fanfarria, comodín, countdown) se pueden reemplazar por archivos MP3/WAV/OGG.
- 💾 **Persistencia total en IndexedDB**: los sonidos y músicas cargados no se pierden al recargar la página.
- 👥 Dos modos de respuesta en Etapa 1: todos responden o un equipo por turno.
- 🖥️ Vista proyector en ventana independiente sincronizada.
- 💾 Guardado automático y manual del estado del juego (localStorage).
- 📋 Banco de preguntas persistente (importar/exportar JSON).
- 📡 Modo offline total mediante Service Worker.

## Tecnologías utilizadas
- React 18 (sin build, usando Babel standalone)
- Web Audio API
- IndexedDB (para sonidos personalizados)
- localStorage (para partidas y preguntas)
- Service Worker (caché de dependencias)
- CSS Grid / Flexbox (responsive)

## Requisitos del sistema
- Navegador moderno (Chrome 90+, Edge 90+, Firefox 88+).
- Conexión a internet solo para la primera carga (para cachear recursos).

## Instalación y uso
1. Descarga el archivo `DiloComoEs_ACTIVATE2026_v4.html`.
2. Ábrelo con tu navegador.
3. (Opcional) Configura tus propios sonidos y músicas en la pestaña **"🎵 Sonidos"**.
4. Configura los equipos y comienza el torneo.

## Personalización avanzada
- **Sonidos propios**: ve a la pestaña "🎵 Sonidos" y carga archivos para cada evento o fase musical.
- **Preguntas**: edita el banco de preguntas desde la pestaña "📋 Preguntas" (puedes importar/exportar JSON).
- **Colores de equipos**: modificar el array `TC` en el código.
- **Tiempos y puntajes**: modificar el objeto `SC` en el código.

## Autores
Desarrollado para Palabra de Vida.  
Mantenido por Ernesto Gregori.