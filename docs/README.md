# 🎤 Dilo Como Es – ACTIVATE 2026

Sistema de concurso bíblico para eventos juveniles, totalmente offline después de la primera carga.
Diseñado para **operadores en vivo** con pantalla de proyector independiente, control remoto desde móvil y persistencia completa.

---

## ✨ Características principales

* Torneo por rondas (Inicial → Cuartos → Semifinal → Final) con eliminación del equipo de menor puntaje.
* Tres etapas de preguntas:

  * **Etapa 1** – Opción múltiple (4 opciones, respuesta única)
  * **Etapa 2** – Pregunta abierta (evaluación manual)
  * **Etapa 3** – Recitación de versículos (evaluación manual)
* Temporizador con sonido de tick en los últimos 3 segundos.
* Comodines:

  * 50/50
  * Ayuda Capitán (+5s)
  * Cambio de pregunta
* Modos de respuesta en Etapa 1:

  * Todos responden
  * Un equipo por turno
* Racha de aciertos (contador 🔥).
* Sonidos y música personalizables (se guardan en IndexedDB).
* Pantalla de espera entre rondas con mensaje e imagen personalizables.
* Proyector en ventana independiente sincronizada en tiempo real.
* Control remoto desde otro dispositivo/móvil (BroadcastChannel).
* Persistencia automática en IndexedDB (estado, preguntas, sonidos).
* Importación/exportación de preguntas desde Excel y JSON.
* Exportación de resultados a Excel.
* Funciona completamente offline (Service Worker).

---

## 🖥️ Requisitos

* Navegador moderno (Chrome, Edge, Firefox, Safari) con soporte para:

  * IndexedDB
  * Web Audio API
  * BroadcastChannel
  * Speech Recognition (opcional)
* No se necesita servidor web (puede ejecutarse desde archivo local, pero el Service Worker requiere HTTPS o localhost).

---

## 🚀 Instalación y puesta en marcha

1. Descarga el archivo `index.html` y guárdalo en tu computadora.
2. **Para el correcto funcionamiento del Service Worker**:

   * Coloca el archivo en un servidor local (ej. `npx http-server`)
   * O súbelo a un hosting estático (GitHub Pages, Netlify, etc.)
3. Abre `index.html` en tu navegador.
4. Configura los equipos, las preguntas y los ajustes.
5. ¡Listo! El juego funcionará incluso sin conexión después de la primera carga.

---

## 📦 Estructura del proyecto

```text
index.html      # Código completo del juego (HTML, CSS, JS, React)
debugger.html   # Herramienta de depuración (opcional)
```

---

## 🎮 Uso rápido

1. **Pestaña Equipos** – Define nombres y avatares.
2. **Pestaña Preguntas** – Importa/crea preguntas de los tres tipos.
3. **Pestaña Ajustes** – Personaliza tiempos, puntajes y pantalla de espera.
4. **Iniciar torneo** – El sistema genera enfrentamientos automáticamente.
5. **Abrir el proyector** (botón ⛶) – Se mostrará en otra ventana para el público.
6. **Controlar desde móvil** – Abre `?remote=1` en otro dispositivo.

---

## 📱 Control remoto

Añade `?remote=1` a la URL para abrir el panel de control táctil.

Ejemplo:

```text
http://localhost:8080/index.html?remote=1
```

Características del panel remoto:

* Botones táctiles rápidos
* Atajos de teclado (1–0)
* Sincronización en tiempo real vía BroadcastChannel

---

## 🐛 Depuración

Abre `debugger.html` en la misma ventana/origen para:

* Monitorear el estado del juego
* Ver tráfico de BroadcastChannel
* Ejecutar pruebas automáticas
* Revisar eventos y errores

---

## 📄 Licencia

Uso interno para eventos de iglesia.
No se permite redistribución comercial sin autorización.