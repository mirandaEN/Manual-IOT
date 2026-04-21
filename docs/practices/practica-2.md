# Práctica 2 - Encender y aapagar 4 LED's

**Nivel:** Fácil  
**Duración:** 12 minutos

## Objetivo
Programar el Photon 2 y prender y apagar 4 leds utilizando el Web IDE.

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
| LED (ánodo)    | D7            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D7|
| LED (ánodo)    | D2            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D2|
| LED (ánodo)    | D3            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D3|
| LED (ánodo)    | D4            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D4|

## Ver Simulación

<div style="max-width: 700px; margin: 30px auto; padding: 25px; background: #0f172a; border-radius: 20px; text-align: center; color: white; position: relative;">
  <h3 style="color: #00f7ff; margin-bottom: 15px;">🔬 Simulación Interactiva – Particle Photon 2</h3>
  
  <div style="position: relative; display: inline-block;">
    <img src="/Manual-IOT/assets/images/practica2-diagrama.jpeg" 
         alt="Circuito 4 LEDs" 
         style="width: 100%; max-width: 680px; border-radius: 15px; box-shadow: 0 0 25px rgba(0, 247, 255, 0.6);">

    <!-- LED 1 (D7) -->
    <div id="led1" style="position: absolute; top: 20%; left: 36%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 2 (D2) -->
    <div id="led2" style="position: absolute; top: 45%; left: 36%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 3 (D3) -->
    <div id="led3" style="position: absolute; top: 20%; left: 55%; width: 18px; height: 18px; background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); border-radius: 50%; box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4); display: none; transition: all 0.12s ease;"></div>
    
    <!-- LED 4 (D4) -->
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
    LED1 → LED2 → LED3 → LED4 (125 ms cada cambio)
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
    status.innerHTML = '✅ <strong>Secuencia activa</strong>';
    running = true;

    let index = 0;
    interval = setInterval(() => {
      if (!running) return;

      // Apagar todos
      leds.forEach(led => {
        led.style.boxShadow = '0 0 8px 4px rgba(255, 60, 60, 0.4)';
        led.style.background = 'radial-gradient(circle at 30% 30%, #ff4444, #cc0000)';
      });

      // Encender solo el LED actual
      leds[index].style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)';
      leds[index].style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';

      // Pasar al siguiente LED
      index = (index + 1) % 4;
    }, 250);   // 125ms ON + 125ms OFF = 250ms total por LED
  }
}
</script>

## Código

**include "Particle.h"**

**SYSTEM_MODE(AUTOMATIC);**

**SerialLogHandler logHandler(LOG_LEVEL_INFO);**


# void setup() {
    pinMode(D7,OUTPUT);
    pinMode(D2,OUTPUT);
    pinMode(D3,OUTPUT);
    pinMode(D4,OUTPUT);
}

# void loop() {
    digitalWrite(D7,HIGH); //Enciende
    delay(125);    //Retraso
    digitalWrite(D7,LOW);   //Apaga
    delay(125);
    digitalWrite(D2,HIGH);
    delay(125);
    digitalWrite(D2,LOW);
    delay(125);
    digitalWrite(D3,HIGH);
    delay(125);
    digitalWrite(D3,LOW);
    delay(125);
    digitalWrite(D4,HIGH);
    delay(125);
    digitalWrite(D4,LOW);
    delay(125);
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
Se espera que los cuatro LEDs conectados a los pines D7, D2, D3 y D4 se enciendan y apaguen de manera secuencial, uno después del otro, con un intervalo de 125 milisegundos entre cada cambio

## Evidencia
![Practica 2](../assets/images/practica2.jpeg)

![Captura Web IDE](../assets/images/practica2-captura.jpeg)

## Ver Video
<video width="50%" controls>
  <source src="/manual-iot/assets/videos/practica2.mp4" type="video/mp4">
  Tu navegador no soporta video.
</video>