# Práctica 1 - Encender un LED

**Nivel:** Fácil  
**Duración:** 10 minutos

## Objetivo
Encender y apagar un LED conectado al Particle Photon 2 utilizando el Web IDE.

## Material
- 1 × Particle Photon 2
- 1 x Proto Board
- 1 × LED (cualquier color)
- 1 × Resistencia 220Ω
- Cables jumper
- Conexión a Internet

## Conexión

**LED → Pin D7**

| Componente     | Pin Photon2   |
|----------------|---------------|
| LED (ánodo)    | D7            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D7|


## Ver Simulación

<div style="max-width: 700px; margin: 30px auto; padding: 25px; background: #0f172a; border-radius: 20px; text-align: center; color: white; position: relative;">
  <h3 style="color: #00f7ff; margin-bottom: 15px;">🔬 Simulación Interactiva – Particle Photon 2</h3>
  
  <div style="position: relative; display: inline-block;">
    <img src="/manual-iot/assets/images/practica1-diagrama.jpeg" 
         alt="Circuito Particle Photon 2" 
         style="width: 100%; max-width: 620px; border-radius: 15px; box-shadow: 0 0 25px rgba(0, 247, 255, 0.6);">

    <!-- CÍRCULO DEL LED QUE PARPADEA -->
    <div id="led-particle" 
         style="position: absolute; 
                top: 21%; 
                left: 76%; 
                width: 58px; 
                height: 58px; 
                background: radial-gradient(circle at 30% 30%, #ff4444, #cc0000); 
                border-radius: 50%; 
                box-shadow: 0 0 8px 4px rgba(255, 60, 60, 0.4);
                display: none;
                transition: all 0.15s ease;">
    </div>
  </div>

  <div style="margin-top: 25px;">
    <button onclick="toggleLedSimulation()" 
            id="btnSim"
            style="padding: 14px 40px; font-size: 18px; font-weight: bold; background: #00f7ff; color: #0f172a; border: none; border-radius: 50px; cursor: pointer; box-shadow: 0 0 20px #00f7ff;">
      ▶️ Iniciar Simulación
    </button>
  </div>

  <p id="statusSim" style="margin-top: 12px; font-size: 15px; color: #64748b;">
    Presiona el botón para ver el LED parpadeando cada 1 segundo
  </p>
</div>

<script>
let blinking = false;
let interval = null;

function toggleLedSimulation() {
  const led = document.getElementById('led-particle');
  const btn = document.getElementById('btnSim');
  const status = document.getElementById('statusSim');

  if (blinking) {
    // DETENER
    clearInterval(interval);
    led.style.display = 'none';
    btn.innerHTML = '▶️ Iniciar Simulación';
    status.textContent = 'Simulación detenida';
    blinking = false;
  } else {
    // INICIAR
    led.style.display = 'block';
    btn.innerHTML = '⏹️ Detener Simulación';
    status.innerHTML = '✅ <strong>LED parpadeando</strong> (1 segundo)';
    blinking = true;

    interval = setInterval(() => {
      if (!blinking) return;
      
      // Encendido brillante
      led.style.boxShadow = '0 0 45px 30px rgba(255, 80, 80, 0.95)';
      led.style.background = 'radial-gradient(circle at 30% 30%, #ff6666, #ff2222)';
      
      setTimeout(() => {
        if (!blinking) return;
        // Apagado suave
        led.style.boxShadow = '0 0 8px 4px rgba(255, 60, 60, 0.4)';
        led.style.background = 'radial-gradient(circle at 30% 30%, #ff4444, #aa0000)';
      }, 450);
    }, 1000);
  }
}
</script>

## Código

**include "Particle.h"**

**SYSTEM_MODE(AUTOMATIC);**

**SerialLogHandler logHandler(LOG_LEVEL_INFO);**


# void setup() {
    pinMode(D7, OUTPUT);
}

# void loop() {
    digitalWrite(D7, HIGH); //Enciende
    delay(1000); //Retraso 1 segundo
    digitalWrite(D7, LOW); //Apaga
    delay(1000);
}

## Procedimiento
1. Colocar el Particle Photon 2 a un extremo del protoboard
2. Colocar el LED en cualqueira de las lineas de conexión que estén libres
3. Conectar el catodo del led a la linea de tierra del protoboard. NOTA: puede ser directo o con un jumper
4. Colocar una resistencia de 220Ω frente al otro extremo del LED (ánodo) NOTA: no importa la direccion de la resistencia, asegurate de que la resistencia este en la lina que tiene continuidad con el LED
5. Conectar el extremo de la resistencia que quedo libre un cable JUMPER para llevarlo al pin elegido (D7)
6. Conectar con un cable de tipo MICRO-USB el Particle Photon 2 a tu PC 
7. Conectar el Particle Photon 2 a Internet, puedes usar este enlace: (https://docs.particle.io/tools/developer-tools/configure-wi-fi/)

## Resultado Esperado
El LED comenzará a encender y apagarse con un retraso de 1 segundo

## Evidencia
![Practica 1](../assets/images/practica1.jpeg)

![Captura Web IDE](../assets/images/practica1-captura.jpeg)

## Ver Video
<video width="50%" controls>
  <source src="/manual-iot/assets/videos/practica1.mp4" type="video/mp4">
  Tu navegador no soporta video.
</video>
