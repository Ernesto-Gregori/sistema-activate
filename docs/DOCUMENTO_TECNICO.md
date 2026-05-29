# Documento técnico – Dilo Como Es ACTIVATE 2026

Este documento describe la arquitectura interna, los flujos de datos y las APIs utilizadas.

---

# 🏗️ Arquitectura general

La aplicación es un **Single Page Application (SPA)** implementada en un único archivo HTML.

Utiliza:

* **React 18** (vía CDN) con JSX transformado por Babel en tiempo real.
* **IndexedDB** para persistencia offline.
* **Web Audio API** para sonidos y música.
* **BroadcastChannel** para comunicación entre pestañas (control remoto y proyector).
* **Service Worker** para caché de recursos estáticos y modo offline.

El código se organiza en componentes funcionales con hooks:

* `useState`
* `useEffect`
* `useCallback`
* `useMemo`
* `useRef`

---

# 📦 Módulos principales

## `AudioEngine`

Objeto global que maneja toda la reproducción de audio.

### Almacenamiento

Los buffers de audio se guardan en `DiloSoundDB` bajo las claves:

* `correct`
* `wrong`
* `timeout`
* `tick`
* `fanfare`
* `joker`
* `countdown`
* `roundEnd`

Y también:

* `music_*` para las fases.

### Métodos clave

```js
correct()
wrong()
timeout()
tick()
fanfare()
joker()
countdown()
roundEnd()

startMusic(phase)
stopMusic()
toggleMusic()
setMusicPhase(phase)

setCustomSound(type, file)
resetToDefaultSound(type)

setCustomMusic(phase, file)
resetToDefaultMusic(phase)

playTestSound(type)
```

### Música por fases

Internamente, si no hay buffer personalizado, genera melodías simples con osciladores (`sine`).

### Reanudación automática

En navegadores que suspenden `AudioContext`, se reanuda al primer `playSound` o al hacer clic en un botón.

---

## `GameView`

Componente principal del juego.

Maneja:

* Estado del torneo (`teams`, `roundMatches`, `matchIdx`, `roundName`, `history`)
* Pregunta actual (`curQ`)
* Temporizador (`running`, `remaining`, `extraTime`)
* Respuestas de equipos (`sel`)
* Modo de respuesta
* Comodines
* Sincronización con el proyector mediante `sendUpdateToProjector()`
* Persistencia en IndexedDB

### Flujo de datos

1. El usuario selecciona una pregunta → `setCurQ`
2. Inicia temporizador → `setRunning(true)`
3. Los equipos seleccionan opciones → `handleSelect`
4. Confirmación → `handleReveal`
5. Actualización de puntajes → `setTeamsState`
6. Animaciones → `triggerAnim`
7. Registro de historial → `addToHistory`
8. Finalización de enfrentamiento → `endMatch`
9. Eliminación de equipos → `elimLowest`

Cada cambio relevante dispara:

```js
sendUpdateToProjector()
```

mediante un `useEffect` que depende de estados descompuestos en primitivas para evitar loops.

---

## `ProjectorView`

Componente que se renderiza cuando la URL contiene:

```txt
?projector=1
```

Escucha mensajes `postMessage` tipo:

```js
PROJECTOR_UPDATE
```

y actualiza su estado local.

### Renderiza:

* `showWait` → pantalla de espera
* `showTie` → interfaz de desempate
* `projectorClear` → pantalla “PREPÁRATE”
* `winner` → pantalla de campeón
* Vista normal → pregunta, opciones, temporizador y selecciones

### Hooks utilizados

* `useEffect` para sincronizar fuentes
* `useEffect` para efectos visuales y confeti

---

## `TieBreaker`

Modal para gestionar desempates.

### Props principales

```js
teams
teamIndexes
onAwardPoints
```

### Comunicación con `GameView`

Utiliza:

```js
onStateChange()
```

para notificar cambios en:

* `phase`
* `word`
* `running`
* etc.

De esta forma el proyector refleja el estado actual del desempate.

---

# 💾 Persistencia

## Funciones principales

* `openGameDB`
* `saveGameStateToDB`
* etc.

Utilizan:

```js
indexedDB.open()
```

### Stores definidas

#### `gameState`

Guarda:

```js
'current'
```

con el objeto completo del estado.

#### `questions`

Cada pregunta utiliza su `id` como `keyPath`.

#### `settings`

Guarda:

* `stageConfig`
* `waitConfig`

#### `questionHistory`

Store autoincremental para historial.

### Migración

Al iniciar la app se migran datos desde `localStorage` si existían.

---

# 📡 BroadcastChannel

## Canal `'dilocontrol'`

El control remoto envía comandos:

```js
{ command: 'start' }
```

`GameView` los recibe y ejecuta mediante:

```js
voiceCommandHandler()
```

---

## Canal `'dilo_debugger'`

`GameView` envía el payload del proyector para que:

```txt
debugger.html
```

intercepte y muestre el estado.

---

# ⚙️ Service Worker

Registrado en el evento:

```js
load
```

### Funcionalidades

* Cachea recursos estáticos
* Funciona offline
* Usa estrategia `cache-first`
* Actualización en segundo plano

### Recursos cacheados

* React
* Babel
* SheetJS
* Fuentes
* `index.html`

---

# 🔄 Flujo de sincronización del proyector

1. `GameView` define:

```js
sendUpdateToProjector()
```

2. Los cambios relevantes ejecutan un `useEffect`
3. `sendUpdateToProjector`:

   * Envía `postMessage`
   * Envía datos por `BroadcastChannel`
4. `ProjectorView` escucha:

```js
window.addEventListener('message')
```

y actualiza su estado.

---

## ⚠️ Precaución contra loops infinitos

Las dependencias del `useEffect` se desglosan en valores primitivos:

```js
tieState.phase
tieState.word
```

en lugar del objeto completo:

```js
tieState
```

Esto evita cambios de referencia en cada render.

---

# 🧪 Utilidades de depuración

## `debugger.html`

Herramienta independiente que:

* Escucha `BroadcastChannel('dilo_debugger')`
* Escucha `postMessage`
* Muestra el árbol de estado
* Ejecuta tests automáticos
* Detecta loops infinitos
* Verifica:

  * timer
  * conexión del proyector
  * lógica de winner
  * IndexedDB
  * etc.

---

# 🛠️ Extensibilidad

## Agregar un nuevo tipo de pregunta

### 1. Añadir entrada en `QUESTION_TYPES`

```js
nuevo_tipo: {
  id: "nuevo_tipo",
  label: "Etiqueta",
  icon: "🆕",
  color: "#color",
  defaultTime: 10,
  defaultCorrect: 100,
  defaultIncorrect: -50,
  hasOptions: true,
  hasAnswer: true,
  answerIsLetter: false,
  timerVisible: true,
  description: "...",
  evaluate: (q, teamAnswer) => boolean || null,
  revealMode: "highlight_option",
  twoOptionsOnly: false,
  hasMedia: false
}
```

---

### 2. Actualizar migraciones

Modificar:

* `migrateQuestion`
* `STAGE_TO_TYPE`
* `TYPE_TO_STAGE`

si el nuevo tipo pertenece a una etapa existente.

---

### 3. Actualizar el editor (`QManager`)

Mostrar los campos correctos:

* opciones
* respuestas
* multimedia
* etc.

---

### 4. Actualizar lógica de renderizado

#### `ProjectorView`

El renderizado ya es genérico usando:

* `hasOptions`
* `revealMode`

#### `GameView`

La evaluación utiliza:

```js
getQType(q).evaluate
```

Si es `null`, se realiza evaluación manual.

---

# 🎵 Personalizar música por fases

Las fases válidas son exactamente:

```txt
start
initial
semifinal
final
tiebreaker
```

`AudioEngine` reconoce estas fases y permite sobrescribirlas mediante:

```js
setCustomMusic()
```

---

# 🔒 Consideraciones de seguridad

* Todas las operaciones de IndexedDB son asíncronas.
* Los comandos de `BroadcastChannel` no están autenticados.
* Cualquier pestaña podría controlar el juego.
* El entorno asume una red cerrada de evento.
* El Service Worker solo cachea recursos del mismo origen.

---

# 🚦 Rendimiento

* Componentes memoizados con `React.memo`
* Temporizador implementado con `setInterval`
* Limpieza correcta de intervalos
* `postMessage` para sincronización rápida
* Límite de logs:

  * 300 entradas máximas

---

# 📱 Compatibilidad

## Chrome / Edge 80+

Funcionamiento óptimo.

---

## Firefox 75+

Funciona correctamente, aunque el audio puede requerir interacción del usuario.

---

## Safari 14+

Requiere iniciar `AudioContext` dentro de un gesto del usuario.

Esto se maneja automáticamente al hacer clic en botones de sonido.

### Requisito adicional

El Service Worker requiere HTTPS en producción.

---

# 🧪 Pruebas sugeridas

## Ejecutar tests automáticos

Abrir:

```txt
debugger.html
```

y ejecutar:

```txt
Ejecutar todos los tests
```

---

## Validaciones recomendadas

### Offline

Simular pérdida de conexión tras la primera carga.

Debe seguir funcionando.

---

### Reconexión del proyector

Cerrar y abrir el proyector varias veces.

Debe reconectarse correctamente.

---

### Control remoto

Usar el control remoto desde otro navegador o dispositivo.

---

### Estrés de preguntas

Crear muchas preguntas y verificar:

* paginación
* filtros
* rendimiento general