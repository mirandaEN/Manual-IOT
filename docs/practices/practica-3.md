# Práctica 3 - Encender y aapagar 4 LED's

**Nivel:** Fácil  
**Duración:** 12 minutos

## Objetivo
Encender y apagar 4 leds en intervalos de 1 segundo, ½ segundo y ¼ de segundo de izquierda utilizando el Web IDE.

## Material
- 1 × Particle Photon 2
- 1 x Proto Board
- 4 × LED (cualquier color)
- 4 × Resistencia 220Ω
- Cables jumper
- Conexión a Internet

## Conexión

**LED → Pin D7**

**LED → Pin D2**

**LED → Pin D3**

**LED → Pin D4**

| Componente     | Pin Photon2   |
|----------------|---------------|
| LED (ánodo)    | D0            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D0|
| LED (ánodo)    | D1            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D1|
| LED (ánodo)    | D2            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D2|
| LED (ánodo)    | D3            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D3|

## Ver Simulación

<div style="max-width: 700px; margin: 30px auto; padding: 25px; background: #0f172a; border-radius: 20px; text-align: center; color: white; position: relative;">
  <h3 style="color: #00f7ff; margin-bottom: 15px;">🔬 Simulación Interactiva – Particle Photon 2</h3>
  
  <div style="position: relative; display: inline-block;">
    <img src="/manual-iot/assets/images/practica2-diagrama.jpeg" 
         alt="Circuito 4 LEDs" 
         style="width: 100%; max-width: 680px; border-radius: 15px; box-shadow: 0 0 25px rgba(0, 247, 255, 0.6);">

    <!-- LED 1 (D0) -->
    <div id="led1" style="position: absolute; top: 20%; left: 36%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 2 (D1) -->
    <div id="led2" style="position: absolute; top: 45%; left: 36%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 3 (D2) -->
    <div id="led3" style="position: absolute; top: 20%; left: 55%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 4 (D3) -->
    <div id="led4" style="position: absolute; top: 45%; left: 55%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
  </div>

  <div style="margin-top: 25px;">
    <button onclick="toggleSimulation()" 
            id="btnSim"
            style="padding: 14px 40px; font-size: 18px; font-weight: bold; background: #00f7ff; color: #0f172a; border: none; border-radius: 50px; cursor: pointer; box-shadow: 0 0 20px #00f7ff;">
      ▶️ Iniciar Simulación
    </button>
  </div>

  <p id="statusSim" style="margin-top: 12px; font-size: 15px; color: #64748b;">
    Fase 1: 1 segundo → Fase 2: ½ segundo → Fase 3: ¼ segundo (se repite)
  </p>
</div>

<script>
let running = false;
let interval = null;

function toggleSimulation() {
  const btn = document.getElementById('btnSim');
  const status = document.getElementById('statusSim');
  const leds = [
    document.getElementById('led1'),
    document.getElementById('led2'),
    document.getElementById('led3'),
    document.getElementById('led4')
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

    let phase = 0;                    // 0=1s, 1=0.5s, 2=0.25s
    const delays = [1000, 500, 250];
    let ledIndex = 0;

    interval = setInterval(() => {
      if (!running) return;

      // Apagar todos
      leds.forEach(led => {
        led.style.boxShadow = '0 0 8px 4px rgba(255, 60, 60, 0.4)';
        led.style.background = 'radial-gradient(circle at 30% 30%, #ff4444, #cc0000)';
      });

      // Encender el LED actual
      leds[ledIndex].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)';
      leds[ledIndex].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';

      // Siguiente LED
      ledIndex++;

      // Si terminó los 4 LEDs → cambiar de fase
      if (ledIndex >= 4) {
        ledIndex = 0;
        phase = (phase + 1) % 3;           // vuelve al inicio después del ¼
      }
    }, delays[phase]);
  }
}
</script>

## Código

**include "Particle.h"**

**SYSTEM_MODE(AUTOMATIC);**

**SerialLogHandler logHandler(LOG_LEVEL_INFO);**


# void setup() {
    pinMode(D0,OUTPUT);
    pinMode(D1,OUTPUT);
    pinMode(D2,OUTPUT);
    pinMode(D3,OUTPUT);
}
# int leds[] = {
    D0, D1, D2, D3
  };
# void loop() {
    digitalWrite(D0, HIGH); 
    delay(1000); 
    digitalWrite(D0, LOW);
    digitalWrite(D1, HIGH); 
    delay(1000); 
    digitalWrite(D1, LOW);
    digitalWrite(D2, HIGH); 
    delay(1000); 
    digitalWrite(D2, LOW);
    digitalWrite(D3, HIGH); 
    delay(1000); 
    digitalWrite(D3, LOW);

    digitalWrite(D0, HIGH); 
    delay(500); 
    digitalWrite(D0, LOW);
    digitalWrite(D1, HIGH); 
    delay(500); 
    digitalWrite(D1, LOW);
    digitalWrite(D2, HIGH); 
    delay(500); 
    digitalWrite(D2, LOW);
    digitalWrite(D3, HIGH); 
    delay(500); 
    digitalWrite(D3, LOW);


    digitalWrite(D0, HIGH); 
    delay(250); 
    digitalWrite(D0, LOW);
    digitalWrite(D1, HIGH); 
    delay(250); 
    digitalWrite(D1, LOW);
    digitalWrite(D2, HIGH); 
    delay(250); 
    digitalWrite(D2, LOW);
    digitalWrite(D3, HIGH); 
    delay(250); 
    digitalWrite(D3, LOW);
}

## Procedimiento
1. Colocar el Particle Photon 2 a un extremo del protoboard
2. Colocar los 4 LED's en cualqueira de las lineas de conexión que estén libres
3. Conectar el catodo de los 4 led's a la linea de tierra del protoboard. NOTA: puede ser directo o con un jumper
4. Colocar una resistencia de 220Ω frente al otro extremo de los 4 LED's (ánodos) NOTA: no importa la direccion de la resistencia, asegurate de que la resistencia este en la lina que tiene continuidad con el LED
5. Conectar el extremo de las resistencias que quedaron libres un cable JUMPER para llevarlo a los pines elegidos (D7, D2, D3, D4)
6. Conectar con un cable de tipo MICRO-USB el Particle Photon 2 a tu PC 
7. Conectar el Particle Photon 2 a Internet, puedes usar este enlace: (https://docs.particle.io/tools/developer-tools/configure-wi-fi/)

## Resultado Esperado
Se espera que los cuatro LEDs conectados a los pines D0, D1, D2 y D3 se enciendan y apaguen de manera secuencial, uno después del otro, con 3 intervalos de tiempo diferentes: 1/2 segundo, 1/4 segundo, 1 segundo

## Evidencia
![Practica 3](../assets/images/practica3.jpeg)

![Captura Web IDE](../assets/images/practica3-captura.jpeg)

## Ver Video
<video width="50%" controls>
  <source src="/manual-iot/assets/videos/practica3.mp4" type="video/mp4">
  Tu navegador no soporta video.
</video>