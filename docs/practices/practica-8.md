# Práctica 8 - Combinación de patrones de iluminación con diferentes tiempos en 5 LEDs

**Nivel:** Fácil  
**Duración:** 15 minutos

## Objetivo
Programar el Particle Photon 2 para generar múltiples patrones de iluminación combinados en cinco LEDs, utilizando ciclos for, arreglos y distintos tiempos de retardo, con el fin de aplicar secuencias variadas y control avanzado de salidas digitales.

## Material
- 1 × Particle Photon 2
- 1 x Proto Board
- 5 × LED (cualquier color)
- 5 × Resistencia 220Ω
- Cables jumper
- Conexión a Internet

## Conexión

**LED → Pin D0**

**LED → Pin D7**

**LED → Pin D6**

**LED → Pin D5**

**LED → Pin D4**

| Componente     | Pin Photon2   |
|----------------|---------------|
| LED (ánodo)    | D0            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D0|
| LED (ánodo)    | D7            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D7|
| LED (ánodo)    | D6            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D6|
| LED (ánodo)    | D5            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D5|
| LED (ánodo)    | D4            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D4|

## Ver Simulación

<div style="max-width: 700px; margin: 30px auto; padding: 25px; background: #0f172a; border-radius: 20px; text-align: center; color: white; position: relative;">
  <h3 style="color: #00f7ff; margin-bottom: 15px;">🔬 Simulación Interactiva – Particle Photon 2</h3>
  
  <div style="position: relative; display: inline-block;">
    <img src="/manual-iot/assets/images/practica4-diagrama.jpeg" 
         alt="Circuito 5 LEDs - Práctica 8" 
         style="width: 100%; max-width: 680px; border-radius: 15px; box-shadow: 0 0 25px rgba(0, 247, 255, 0.6);">

    <!-- LED 1 (D0) -->
    <div id="led1" style="position: absolute; top: 20%; left: 35%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 2 (D7) -->
    <div id="led2" style="position: absolute; top: 45%; left: 36%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 3 (D6) -->
    <div id="led3" style="position: absolute; top: 20%; left: 55%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 4 (D5) -->
    <div id="led4" style="position: absolute; top: 45%; left: 55%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 5 (D4) -->
    <div id="led5" style="position: absolute; top: 20%; left: 74%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
  </div>

  <div style="margin-top: 25px;">
    <button onclick="toggleSimulation8()" 
            id="btnSim"
            style="padding: 14px 40px; font-size: 18px; font-weight: bold; background: #00f7ff; color: #0f172a; border: none; border-radius: 50px; cursor: pointer; box-shadow: 0 0 20px #00f7ff;">
      ▶️ Iniciar Simulación
    </button>
  </div>

  <p id="statusSim" style="margin-top: 12px; font-size: 15px; color: #64748b;">
    Secuencia 1 → 2 → 3 → 4 (se repite)
  </p>
</div>

<script>
let running = false;
let interval = null;
let currentSeq = 0;

function toggleSimulation8() {
  const btn = document.getElementById('btnSim');
  const status = document.getElementById('statusSim');
  const leds = [
    document.getElementById('led1'), // D0
    document.getElementById('led2'), // D7
    document.getElementById('led3'), // D6
    document.getElementById('led4'), // D5
    document.getElementById('led5')  // D4
  ];

  if (running) {
    clearInterval(interval);
    leds.forEach(led => led.style.display = 'none');
    btn.innerHTML = '▶️ Iniciar Simulación';
    status.textContent = 'Simulación detenida';
    running = false;
  } else {
    leds.forEach(led => led.style.display = 'block');
    btn.innerHTML = '⏹️ Detener Simulación';
    running = true;
    currentSeq = 0;
    runSequence(leds, status);
  }
}

function runSequence(leds, status) {
  if (!running) return;

  const sequences = [
    "1. Todos encendidos 1s → apagados 1s",
    "2. Encendido por pares (D0+D6 y D7+D5)",
    "3. Secuencia rápida ida",
    "4. Secuencia rápida vuelta"
  ];

  status.innerHTML = `✅ <strong>${sequences[currentSeq]}</strong>`;

  let i = 0;

  interval = setInterval(() => {
    if (!running) return;

    // Apagar todos
    leds.forEach(led => {
      led.style.boxShadow = '0 0 8px 4px rgba(255, 60, 60, 0.4)';
      led.style.background = 'radial-gradient(circle at 30% 30%, #ff4444, #cc0000)';
    });

    if (currentSeq === 0) {
      // Todos encendidos/apagados
      if (i % 2 === 0) {
        leds.forEach(led => {
          led.style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)';
          led.style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
        });
      }
      i++;
    } 
    else if (currentSeq === 1) {
      // Pares
      if (i % 2 === 0) {
        leds[0].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)'; // D0
        leds[0].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
        leds[2].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)'; // D6
        leds[2].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
      } else {
        leds[1].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)'; // D7
        leds[1].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
        leds[3].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)'; // D5
        leds[3].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
      }
      i++;
    } 
    else if (currentSeq === 2) {
      // Secuencia rápida ida
      leds[i].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)';
      leds[i].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
      i = (i + 1) % 5;
    } 
    else {
      // Secuencia rápida vuelta
      leds[i].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)';
      leds[i].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
      i = (i + 4) % 5;   // va hacia atrás
    }
  }, currentSeq === 0 ? 1000 : (currentSeq === 1 ? 500 : 120));

  // Cambiar de secuencia cada ~5 segundos
  setTimeout(() => {
    if (running) {
      clearInterval(interval);
      currentSeq = (currentSeq + 1) % 4;
      runSequence(leds, status);
    }
  }, 4800);
}
</script>

## Código

**include "Particle.h"**

**SYSTEM_MODE(AUTOMATIC);**
# int misLeds[] = {
    D0, D7, D6, D5, D4
    };

**SerialLogHandler logHandler(LOG_LEVEL_INFO);**


int tp1 = 1000;
int tfp1 = 250;
int tp2 = 125;
int tfp2 = 250;
int tp3 = 500;
int tfp3 = 500;

# void setup() {
    pinMode(D0, OUTPUT);
    pinMode(D7, OUTPUT);
    pinMode(D6, OUTPUT);
    pinMode(D5, OUTPUT);
    pinMode(D4, OUTPUT);
}

# void loop() {
    //  un segundo
    for(int i = 0; i < 5; i++) {
        digitalWrite(misLeds[i], HIGH);
    }
    delay(1000); 
    
    
    for(int i = 0; i < 5; i++) {
        digitalWrite(misLeds[i], LOW);
    }
    delay(1000); 
    

    digitalWrite(D0, HIGH);
    digitalWrite(D6, HIGH);
    delay(tp3);
    digitalWrite(D0, LOW);
    digitalWrite(D6, LOW);
    delay(tp3);
    
    digitalWrite(D7, HIGH);
    digitalWrite(D5, HIGH);
    delay(tfp2);
    digitalWrite(D7, LOW);
    digitalWrite(D5, LOW);
    delay(tfp2);
    
    for(int i = 0; i < 5; i++) {
        digitalWrite(misLeds[i], HIGH);
        delay(tp2);
        digitalWrite(misLeds[i], LOW);
        delay(tp2);
    }

    for(int i = 4; i >= 0; i--) {
        digitalWrite(misLeds[i], HIGH);
        delay(tp2);
        digitalWrite(misLeds[i], LOW);
        delay(tp2);
    }
    
    for(int i = 4; i >= 0; i--) {
        digitalWrite(misLeds[i], HIGH);
        delay(tp2);
        digitalWrite(misLeds[i], LOW);
        delay(tp2);
    }
    
    for(int i = 0; i < 5; i++) {
        digitalWrite(misLeds[i], HIGH);
        delay(tp2);
        digitalWrite(misLeds[i], LOW);
        delay(tp2);
    }
}

## Procedimiento
1. Colocar el Particle Photon 2 a un extremo del protoboard
2. Colocar los 5 LED's en cualqueira de las lineas de conexión que estén libres
3. Conectar el catodo de los 5 led's a la linea de tierra del protoboard. NOTA: puede ser directo o con un jumper
4. Colocar una resistencia de 220Ω frente al otro extremo de los 5 LED's (ánodos) NOTA: no importa la direccion de la resistencia, asegurate de que la resistencia este en la lina que tiene continuidad con el LED
5. Conectar el extremo de las resistencias que quedaron libres un cable JUMPER para llevarlo a los pines elegidos (D10, D7, D6, D5, D4)
6. Conectar con un cable de tipo MICRO-USB el Particle Photon 2 a tu PC 
7. Conectar el Particle Photon 2 a Internet, puedes usar este enlace: (https://docs.particle.io/tools/developer-tools/configure-wi-fi/)

## Resultado Esperado
Se espera que el sistema ejecute varios efectos de iluminación de manera continua:
-	Primero, todos los LEDs se encienden y apagan al mismo tiempo.
-	Después, se encienden en combinaciones por pares.
-	Luego, se generan secuencias rápidas en ambos sentidos.


## Evidencia
![Practica 8](../assets/images/practica8.jpeg)

![Captura Web IDE](../assets/images/practica8-captura.jpeg)

## Ver Video
<video width="50%" controls>
  <source src="/manual-iot/assets/videos/practica8.mp4" type="video/mp4">
  Tu navegador no soporta video.
</video>