# Práctica 10 - Sistema de secuencias múltiples con control digital y PWM por ciclos programados

**Nivel:** Medio  
**Duración:** 20 minutos

## Objetivo
Programar el Particle Photon 2 para ejecutar diferentes patrones de iluminación y secuencias de encendido y apagado organizados en ciclos for, combinando control digital y modulación PWM, para reforzar el conocimiento

## Material
- 1 × Particle Photon 2
- 1 x Proto Board
- 4 × LED (cualquier color)
- 4 × Resistencia 220Ω
- Cables jumper
- Conexión a Internet

## Conexión

**LED → Pin MISO**

**LED → Pin D3**

**LED → Pin A2**

**LED → Pin D1**

| Componente     | Pin Photon2   |
|----------------|---------------|
| LED (ánodo)    | MISO          |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y MISO|
| LED (ánodo)    | D3            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D3|
| LED (ánodo)    | A2            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y A2|
| LED (ánodo)    | D1            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D1|

## Ver Simulación

<div style="max-width: 700px; margin: 30px auto; padding: 25px; background: #0f172a; border-radius: 20px; text-align: center; color: white; position: relative;">
  <h3 style="color: #00f7ff; margin-bottom: 15px;">🔬 Simulación Interactiva – Particle Photon 2</h3>
  
  <div style="position: relative; display: inline-block;">
    <img src="/manual-iot/assets/images/practica2-diagrama.jpeg" 
         alt="Circuito 4 LEDs - Práctica 10" 
         style="width: 100%; max-width: 680px; border-radius: 15px; box-shadow: 0 0 25px rgba(0, 247, 255, 0.6);">

    <!-- LED 1 (MISO) - arriba izquierda -->
    <div id="led1" style="position: absolute; top: 20%; left: 36%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 2 (D3) - abajo izquierda -->
    <div id="led2" style="position: absolute; top: 45%; left: 36%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 3 (A2) - arriba derecha -->
    <div id="led3" style="position: absolute; top: 20%; left: 55%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 4 (D1) - abajo derecha -->
    <div id="led4" style="position: absolute; top: 45%; left: 55%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
  </div>

  <div style="margin-top: 25px;">
    <button onclick="toggleSimulation10()" 
            id="btnSim"
            style="padding: 14px 40px; font-size: 18px; font-weight: bold; background: #00f7ff; color: #0f172a; border: none; border-radius: 50px; cursor: pointer; box-shadow: 0 0 20px #00f7ff;">
      ▶️ Iniciar Simulación
    </button>
  </div>

  <p id="statusSim" style="margin-top: 12px; font-size: 15px; color: #64748b;">
    Ciclo 1 → Ciclo 2 → Ciclo 3 (se repite)
  </p>
</div>

<script>
let running = false;
let interval = null;
let currentCycle = 0;

function toggleSimulation10() {
  const btn = document.getElementById('btnSim');
  const status = document.getElementById('statusSim');
  const leds = [
    document.getElementById('led1'), // MISO
    document.getElementById('led2'), // D3
    document.getElementById('led3'), // A2
    document.getElementById('led4')  // D1
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
    currentCycle = 0;
    runNextCycle(leds, status);
  }
}

function runNextCycle(leds, status) {
  if (!running) return;

  const cycleNames = [
    "1. Secuencia progresiva (ida y vuelta)",
    "2. PWM simultáneo en MISO y A2",
    "3. Encendido y apagado individual (ambos sentidos)"
  ];

  status.innerHTML = `✅ <strong>${cycleNames[currentCycle]}</strong>`;

  let i = 0;

  interval = setInterval(() => {
    if (!running) return;

    // Apagar todos
    leds.forEach(led => {
      led.style.boxShadow = '0 0 8px 4px rgba(255, 60, 60, 0.4)';
      led.style.background = 'radial-gradient(circle at 30% 30%, #ff4444, #cc0000)';
    });

    if (currentCycle === 0) {
      // Ciclo 1: ida y vuelta en bloque
      if (i < 4) {
        for (let k = 0; k <= i; k++) leds[k].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)';
        for (let k = 0; k <= i; k++) leds[k].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
      } else {
        const off = i - 4;
        for (let k = 0; k < 4 - off; k++) leds[k].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)';
        for (let k = 0; k < 4 - off; k++) leds[k].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
      }
      i = (i + 1) % 8;
    } 
    else if (currentCycle === 1) {
      // Ciclo 2: PWM breathing en led1 (MISO) y led3 (A2)
      const brightness = Math.sin(Date.now() / 300) * 0.5 + 0.5;
      const intensity = Math.floor(brightness * 255);
      leds[0].style.boxShadow = `0 0 ${20 + intensity * 0.12}px ${10 + intensity * 0.08}px rgba(255, 100, 100, 0.9)`;
      leds[0].style.background = 'radial-gradient(circle at 30% 30%, #ff8888, #ff3333)';
      leds[2].style.boxShadow = `0 0 ${20 + intensity * 0.12}px ${10 + intensity * 0.08}px rgba(255, 100, 100, 0.9)`;
      leds[2].style.background = 'radial-gradient(circle at 30% 30%, #ff8888, #ff3333)';
    } 
    else {
      // Ciclo 3: encendido/apagado individual ida y vuelta
      leds[i].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)';
      leds[i].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
      i = (i + 1) % 8;   // simula ida y vuelta
    }
  }, currentCycle === 1 ? 40 : 125);

  // Cambiar de ciclo cada ~6 segundos
  setTimeout(() => {
    if (running) {
      clearInterval(interval);
      currentCycle = (currentCycle + 1) % 3;
      runNextCycle(leds, status);
    }
  }, 5800);
}
</script>

## Código

**include "Particle.h"**

**SYSTEM_MODE(AUTOMATIC);**

**SerialLogHandler logHandler(LOG_LEVEL_INFO);**

//PINES DIGITALES
# int leds[] = {
    MISO, D3, A2, D1
    };
int numLeds = 4;

// Pines PWM
# int pwmPins[] = {
    MISO, A2
    };

int numPwm = 2;

// Contadores (se reinician al completar la ronda)
int primerCiclo = 4;
int segundoCiclo = 2;
int tercerCiclo = 2;

# void setup() {
    for (int i = 0; i < numLeds; i++) {
        pinMode(leds[i], OUTPUT);
        digitalWrite(leds[i], LOW);
    }
}

# void loop() {
    //PRIMER CICLO
    if (primerCiclo > 0) {
        // DE izq → der Y der → izq
        for (int i = 0; i < numLeds; i++) {
            digitalWrite(leds[i], HIGH);
            delay(125);
        }
        delay(500);

        for (int i = numLeds - 1; i >= 0; i--) {
            digitalWrite(leds[i], LOW);
            delay(125);
        }
        delay(500);

        primerCiclo--;
    }
    //SEGUNDO CICLO
    //PWM EN MISO Y A2 
    else if (segundoCiclo > 0) {
    for (int b = 0; b <= 255; b++) {
        analogWrite(MISO, b);
        analogWrite(A2,   b);
        delay(50);
    }
    
    for (int b = 255; b >= 0; b--) {
        analogWrite(MISO, b);
        analogWrite(A2,   b);
        delay(50);
    }
    
    delay(300);
    
    segundoCiclo--;
}

    
    else if (tercerCiclo > 0) {
        // CICLO 3
        //Encendido-apagado individual izq → der Y der → izq
        for (int r = 0; r < numLeds; r++) {
            digitalWrite(leds[r], HIGH);
            delay(125);
            digitalWrite(leds[r], LOW);
            delay(125);
        }
        for (int r = numLeds - 1; r >= 0; r--) {
            digitalWrite(leds[r], HIGH);
            delay(125);
            digitalWrite(leds[r], LOW);
            delay(125);
        }
        delay(500);

        tercerCiclo--;
    }
    else {
        // Todos los ciclos terminaron,reiniciar contadores para repetir todo
        primerCiclo = 4;
        segundoCiclo = 2;
        tercerCiclo = 2;

        delay(1000);  // pausa de 1 segundo entre ciclos completos
    }
}

## Procedimiento
1. Colocar el Particle Photon 2 a un extremo del protoboard
2. Colocar los 4 LED's en cualqueira de las lineas de conexión que estén libres
3. Conectar el catodo de los 4 led's a la linea de tierra del protoboard. NOTA: puede ser directo o con un jumper
4. Colocar una resistencia de 220Ω frente al otro extremo de los 4 LED's (ánodos) NOTA: no importa la direccion de la resistencia, asegurate de que la resistencia este en la lina que tiene continuidad con el LED
5. Conectar el extremo de las resistencias que quedaron libres un cable JUMPER para llevarlo a los pines elegidos (MISO, A2, A5, D1)
6. Conectar con un cable de tipo MICRO-USB el Particle Photon 2 a tu PC 
7. Conectar el Particle Photon 2 a Internet, puedes usar este enlace: (https://docs.particle.io/tools/developer-tools/configure-wi-fi/)

## Resultado Esperado
Se espera que los leds ejecuten los siguientes tres tipos de efectos en orden:
-	Secuencia progresiva de encendido y apagado.
-	Efecto PWM simultáneo en dos pines.
-	Encendido y apagado individual de cada LED en ambos sentidos.

## Evidencia
![Practica 10](../assets/images/practica10.jpeg)

![Captura Web IDE](../assets/images/practica10-captura.jpeg)

## Ver Video
<video width="50%" controls>
    <source src="/manual-iot/assets/videos/practica10.mp4" type="video/mp4">
    Tu navegador no soporta video.
</video>