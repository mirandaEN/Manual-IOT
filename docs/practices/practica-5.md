# Práctica 5 - Encender y apagar 5 LED's con cilos FOR

**Nivel:** Fácil  
**Duración:** 15 minutos

## Objetivo
Programar el Particle Photon 2 para controlar 5 LEDs utilizando un arreglo y ciclos for, generando un efecto de encendido secuencial de ida y vuelta, optimizando el código y aplicando estructuras repetitivas.

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
         alt="Circuito 5 LEDs - Knight Rider" 
         style="width: 100%; max-width: 680px; border-radius: 15px; box-shadow: 0 0 25px rgba(0, 247, 255, 0.6);">

    <!-- LED 1 (D10) - arriba izquierda -->
    <div id="led1" style="position: absolute; top: 20%; left: 35%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 2 (D7) - abajo -->
    <div id="led2" style="position: absolute; top: 45%; left: 36%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 3 (D6) - arriba -->
    <div id="led3" style="position: absolute; top: 20%; left: 55%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 4 (D5) - abajo -->
    <div id="led4" style="position: absolute; top: 45%; left: 55%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 5 (D4) - arriba derecha (más a la derecha) -->
    <div id="led5" style="position: absolute; top: 20%; left: 74%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
  </div>

  <div style="margin-top: 25px;">
    <button onclick="toggleKnightRider()" 
            id="btnSim"
            style="padding: 14px 40px; font-size: 18px; font-weight: bold; background: #00f7ff; color: #0f172a; border: none; border-radius: 50px; cursor: pointer; box-shadow: 0 0 20px #00f7ff;">
      ▶️ Iniciar Simulación
    </button>
  </div>

  <p id="statusSim" style="margin-top: 12px; font-size: 15px; color: #64748b;">
    Efecto Knight Rider (ida y regreso)
  </p>
</div>

<script>
let running = false;
let interval = null;

function toggleKnightRider() {
  const btn = document.getElementById('btnSim');
  const status = document.getElementById('statusSim');
  const leds = [
    document.getElementById('led1'),  // D10
    document.getElementById('led2'),  // D7
    document.getElementById('led3'),  // D6
    document.getElementById('led4'),  // D5
    document.getElementById('led5')   // D4
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
    status.innerHTML = '✅ <strong>Knight Rider activo</strong>';
    running = true;

    let direction = 1;   // 1 = derecha, -1 = izquierda
    let index = 0;

    interval = setInterval(() => {
      if (!running) return;

      // Apagar todos
      leds.forEach(led => {
        led.style.boxShadow = '0 0 8px 4px rgba(255, 60, 60, 0.4)';
        led.style.background = 'radial-gradient(circle at 30% 30%, #ff4444, #cc0000)';
      });

      // Encender el LED actual
      leds[index].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)';
      leds[index].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';

      // Mover al siguiente LED
      index += direction;

      // Cambio de dirección al llegar a los extremos
      if (index >= leds.length - 1) direction = -1;
      if (index <= 0) direction = 1;
    }, 120);   // velocidad del efecto (puedes bajar a 80 para que sea más rápido)
  }
}
</script>

## Código

**include "Particle.h"**

**SYSTEM_MODE(AUTOMATIC);**
# int misLeds[] = {
    D0, D7, D6, D5, D4
    };

**SerialLogHandler logHandler(LOG_LEVEL_INFO);**
int t=125;

# void setup() {
    pinMode(D0, OUTPUT);
    pinMode(D7, OUTPUT);
    pinMode(D6, OUTPUT);
    pinMode(D5, OUTPUT);
    pinMode(D4, OUTPUT);
}

# void loop() {
    for(int i = 0; i < 5; i++) {
        digitalWrite(misLeds[i], HIGH);
        delay(t);
        digitalWrite(misLeds[i], LOW);
        delay(t);
    }
    
    for(int i = 4; i >= 0; i--) {
        digitalWrite(misLeds[i], HIGH);
        delay(t);
        digitalWrite(misLeds[i], LOW);
        delay(t);
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
Se espera que los cinco LEDs se enciendan uno tras otro desde el primer pin hasta el último y posteriormente regresen en orden inverso, generando un efecto de desplazamiento continuo.

## Evidencia
![Practica 5](../assets/images/practica5.jpeg)

![Captura Web IDE](../assets/images/practica5-captura.jpeg)

## Ver Video
<video width="50%" controls>
  <source src="/manual-iot/assets/videos/practica5.mp4" type="video/mp4">
  Tu navegador no soporta video.
</video>