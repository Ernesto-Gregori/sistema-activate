# Guía de uso para el operador – Dilo Como Es

Esta guía está pensada para la persona que maneja el concurso en vivo.

## 🏁 Preparación antes del evento

### 1. Configurar equipos

- Abre `index.html`.
- Ve a la pestaña **⚙️ Equipos**.
- Escribe los nombres de los equipos (mínimo 2, máximo ilimitado).
- Elige un avatar para cada uno (emojis).
- Haz clic en **COMENZAR TORNEO**.

### 2. Cargar preguntas

- Ve a **📋 Preguntas**.
- Puedes usar las preguntas de demostración, importar desde Excel/JSON, o crear manualmente cada pregunta.
- **Tipos disponibles**:
  - *Opción múltiple*: requiere 4 opciones y una respuesta (A, B, C o D).
  - *Abierta*: respuesta libre, el operador decide si es correcta.
  - *Recitación*: similar a abierta, pero con un versículo.

### 3. Ajustar parámetros

En **⚙️ Ajustes** puedes modificar:

- Tiempo, puntaje por acierto y por error para cada etapa.
- Mensaje e imagen de la pantalla de espera.

## 🎮 Durante el concurso

### Pantalla principal (operador)

La interfaz se divide en tres columnas:

1. **Izquierda** – Banco de preguntas (filtrable por etapa/nivel). Haz clic en una pregunta para cargarla.
2. **Centro** – Control del temporizador, área de respuesta y registro de eventos.
3. **Derecha** – Gestión del torneo (enfrentamientos, posiciones, historial).

### Flujo típico de una ronda

1. **Generar enfrentamientos** – Haz clic en **⚡ Generar enfrentamientos** (solo una vez por ronda).
2. **Iniciar un enfrentamiento** – Presiona **▶ Iniciar** en el primer par de equipos.
3. **Seleccionar una pregunta** – Desde el banco izquierdo.
4. **Iniciar el temporizador** – Botón **▶ Iniciar** (arriba del temporizador).
5. **Los equipos responden** (según el modo):
   - **Modo "Todos responden"** (Etapa 1): cada equipo elige una opción (botones A/B/C/D).
   - **Modo "Un equipo responde"**: selecciona qué equipo tiene el turno (botones con nombres) y luego su opción.
6. **Confirmar respuestas** – Botón **✓ Confirmar respuesta(s)**. El sistema asigna puntaje automáticamente según la configuración.
   - Si ningún equipo seleccionó, presiona **⏱ Tiempo agotado**.
   - Para preguntas abiertas/recitación, primero muestra la respuesta con **Revelar respuesta** y luego asigna puntaje manualmente (+ o -).
7. **Finalizar enfrentamiento** – Botón **Terminar enfrentamiento →**. El resultado se registra y pasa al siguiente.
8. **Cuando se completa la ronda**, aparece el botón **✕ Eliminar menor**. Confirma la eliminación del equipo con menos puntos.
9. **Automáticamente** se crea la siguiente ronda (Cuartos, Semifinal, Final). Aparecerá la pantalla de espera (puedes cerrarla con **CONTINUAR TORNEO**).

### Uso de comodines

Durante un enfrentamiento activo, en la columna derecha o en el **MatchBoard** central (botones):

- **50/50**: Oculta dos opciones incorrectas en preguntas de opción múltiple.
- **Ayuda Capitán**: Añade 5 segundos al temporizador.
- **Cambio**: Sustituye la pregunta actual por otra del mismo nivel y etapa.

### Gestión del proyector

- Abre el proyector con el botón **⛶ Abrir proyector**. Aparecerá una ventana independiente.
- El proyector muestra exactamente lo mismo que ves en la pantalla principal (pregunta, temporizador, opciones, selecciones de equipos).
- Puedes ocultar temporalmente la pregunta con **🌙 Ocultar pregunta** (muestra "PREPÁRATE") y restaurarla con **☀️ Mostrar pregunta**.
- El proyector también refleja la pantalla de espera y el desempate.

### Control remoto (desde un móvil)

1. En el móvil, abre la misma URL que usas en la computadora pero añadiendo `?remote=1` al final.  
   Ejemplo: `http://192.168.1.10:8080/index.html?remote=1`
2. Verás un panel con botones táctiles (Iniciar, Pausar, Correcto, etc.).  
   También puedes usar atajos de teclado (1–0) si conectas un teclado externo.

### Desempate (cuando hay empate en una ronda)

- En la sección derecha, haz clic en **⚡ Desempate – Palabra por Palabra**.
- Escribe el versículo o palabra a recitar.
- Prepara y luego **COMENZAR**. El temporizador de 30 segundos corre.
- Cuando un equipo recite correctamente, haz clic en su botón. El equipo gana 500 puntos y cierra el desempate.

## 📊 Exportar resultados

Al final del torneo (o en cualquier momento), haz clic en **📊 Exportar resultados**. Se descargará un archivo Excel con todas las estadísticas.

## ⚠️ Notas importantes

- **Salva frecuentemente** con el botón **💾 Guardar** (el juego también guarda automáticamente cada 30 segundos).
- Si cierras la ventana sin querer, al volver a abrir el juego te preguntará si quieres restaurar la partida.
- Para reiniciar completamente: en la pantalla principal, botón **🔄 Nueva** (te pedirá confirmación).
- El proyector debe estar en la misma ventana de navegador (mismo origen) para que la sincronización funcione. Si se desconecta, ciérralo y ábrelo de nuevo.