# Práctica 9 - Control de brillo de un LED mediante señal PWM

**Nivel:** Fácil  
**Duración:** 15 minutos

## Objetivo
Para variar progresivamente la intensidad luminosa de un LED utilizando la función analogWrite(). NOTA: asegurate de que el pin elegido tenga soporte para PWM

## Material
- 1 × Particle Photon 2
- 1 x Proto Board
- 1 × LED (cualquier color)
- 1 × Resistencia 220Ω
- Cables jumper
- Conexión a Internet

## Conexión

**LED → Pin D1**


| Componente     | Pin Photon2   |
|----------------|---------------|
| LED (ánodo)    | D1            |
| LED (cátodo)   | GND           |
| Resistencia    | Entre LED y D1|


## Ver Simulación

<div style="max-width: 700px; margin: 30px auto; padding: 25px; background: #0f172a; border-radius: 20px; text-align: center; color: white; position: relative;">
  <h3 style="color: #00f7ff; margin-bottom: 15px;">🔬 Simulación Interactiva – PWM en LED (D1)</h3>
  
  <div style="position: relative; display: inline-block;">
    <img src="/manual-iot/assets/images/practica1-diagrama.jpeg" 
         alt="Circuito PWM - Particle Photon 2" 
         style="width: 100%; max-width: 620px; border-radius: 15px; box-shadow: 0 0 25px rgba(0, 247, 255, 0.6);">

    <!-- LED con PWM (brillo variable) -->
    <div id="led-pwm" 
         style="position: absolute; 
                top: 21%; 
                left: 76%; 
                width: 58px; 
                height: 58px; 
                background: radial-gradient(circle at 30% 30%, #ff6666, #ff2222); 
                border-radius: 50%; 
                box-shadow: 0 0 20px 10px rgba(255, 80, 80, 0.6);
                display: none;
                transition: box-shadow 0.4s ease, background 0.4s ease;">
    </div>
  </div>

  <div style="margin-top: 25px;">
    <button onclick="togglePWM()" 
            id="btnSim"
            style="padding: 14px 40px; font-size: 18px; font-weight: bold; background: #00f7ff; color: #0f172a; border: none; border-radius: 50px; cursor: pointer; box-shadow: 0 0 20px #00f7ff;">
      ▶️ Iniciar Simulación PWM
    </button>
  </div>

  <p id="statusSim" style="margin-top: 12px; font-size: 15px; color: #64748b;">
    Brillo: 204 → 102 → 26 → 13 (se repite cada segundo)
  </p>
</div>

<script>
let running = false;
let interval = null;
const brightnessLevels = [204, 102, 26, 13];  // valores exactos de tu código

function togglePWM() {
  const led = document.getElementById('led-pwm');
  const btn = document.getElementById('btnSim');
  const status = document.getElementById('statusSim');

  if (running) {
    clearInterval(interval);
    led.style.display = 'none';
    btn.innerHTML = '▶️ Iniciar Simulación PWM';
    status.textContent = 'Simulación detenida';
    running = false;
  } else {
    led.style.display = 'block';
    btn.innerHTML = '⏹️ Detener Simulación';
    running = true;

    let index = 0;

    interval = setInterval(() => {
      if (!running) return;

      const level = brightnessLevels[index];

      // Cambiar brillo según el valor analogWrite
      const intensity = (level / 255) * 100;
      led.style.boxShadow = `0 0 ${30 + intensity * 1.2}px ${15 + intensity * 0.6}px rgba(255, 100, 100, ${0.7 + intensity * 0.003})`;
      led.style.background = `radial-gradient(circle at 30% 30%, #ff8888, #ff3333)`;

      // Mostrar el valor actual
      status.innerHTML = `✅ <strong>Brillo: ${level}</strong> (analogWrite)`; 

      index = (index + 1) % brightnessLevels.length;
    }, 1000);   // exactamente 1 segundo como en tu código
  }
}
</script>

## Código

**include "Particle.h"**

**SYSTEM_MODE(AUTOMATIC);**

**SerialLogHandler logHandler(LOG_LEVEL_INFO);**

# void setup() {
    pinMode(D1, OUTPUT);

}

# void loop() {
    analogWrite(D1, 204);
    delay(1000);
    analogWrite(D1, 102);
    delay(1000);
    analogWrite(D1, 26);
    delay(1000);
    analogWrite(D1, 13);
    delay(1000);
}

## Procedimiento
1. Colocar el Particle Photon 2 a un extremo del protoboard
2. Colocar el LED en cualquiera de las lineas de conexión que estén libres
3. Conectar el catodo del led a la linea de tierra del protoboard. NOTA: puede ser directo o con un jumper
4. Colocar una resistencia de 220Ω frente al otro extremo del LED (ánodos) NOTA: no importa la direccion de la resistencia, asegurate de que la resistencia este en la lina que tiene continuidad con el LED
5. Conectar el extremo de la resistencia que quedo libre un cable JUMPER para llevarlo a el PIN elegido (D1)
6. Conectar con un cable de tipo MICRO-USB el Particle Photon 2 a tu PC 
7. Conectar el Particle Photon 2 a Internet, puedes usar este enlace: (https://docs.particle.io/tools/developer-tools/configure-wi-fi/)

## Resultado Esperado
Se espera que el LED aumente su intensidad de manera gradual hasta alcanzar el máximo brillo y posteriormente disminuya progresivamente hasta apagarse por completo.


## Evidencia
![Practica 9](../assets/images/practica9.jpeg)

![Captura Web IDE](../assets/images/practica9-captura.jpeg)

## Ver Video
<video width="50%" controls>
    <source src="/manual-iot/assets/videos/practica9.mp4" type="video/mp4">
    Tu navegador no soporta video.
</video>