# Práctica 13 - Lectura con potenciometro de voltaje real

**Nivel:** Fácil  
**Duración:** 12 minutos

## Objetivo
Convertir el Particle en un voltímetro digital. A diferencia del código de la practica 13 que solo te daba números en bruto, este realiza una operación matemática para decirte cuántos voltios reales están entrando por el pin A0.

## Material
- 1 × Particle Photon 2
- 1 x Proto Board
- 1 × Potenciometro
- Cables jumper
- Conexión a Internet

## Conexión

**Potenciometro → Pin A0**


| Componente     | Pin Photon2   |
|----------------|---------------|
| Pin Izquierdo  | GND           |
| Pin Derecho    | VUSB          |
| Pin Central    | A0            |


## Ver Simulación

<div style="max-width: 700px; margin: 30px auto; padding: 25px; background: #0f172a; border-radius: 20px; text-align: center; color: white; position: relative;">
  <h3 style="color: #00f7ff; margin-bottom: 15px;">🔬 Simulación Interactiva – Voltímetro Digital (A0)</h3>
  
  <div style="position: relative; display: inline-block;">
    <img src="/manual-iot/assets/images/practica12-diagrama.jpeg" 
         alt="Circuito con Potenciómetro - Voltímetro" 
         style="width: 100%; max-width: 620px; border-radius: 15px; box-shadow: 0 0 25px rgba(0, 247, 255, 0.6);">

    <!-- Potenciómetro virtual -->
    <div style="margin-top: 25px;">
      <label style="display: block; margin-bottom: 8px; color: #00f7ff; font-size: 15px;">
        🎛️ Potenciómetro (0 - 4095)
      </label>
      <input type="range" id="potSlider" min="0" max="4095" value="2048" 
             style="width: 100%; accent-color: #00f7ff;">
      <div style="display: flex; justify-content: space-between; font-size: 13px; margin-top: 4px; color: #64748b;">
        <span>0</span>
        <span id="valorActual" style="color: #00f7ff; font-weight: bold;">2048</span>
        <span>4095</span>
      </div>
    </div>
  </div>

  <!-- Monitor Serial falso (exactamente como se ve en particle serial monitor) -->
  <div style="margin-top: 25px; background: #111827; border: 2px solid #334155; border-radius: 12px; padding: 15px; text-align: left; font-family: monospace; max-height: 320px; overflow-y: auto;" id="serialMonitor">
    <div style="color: #22c55e; margin-bottom: 8px; font-size: 14px;">
      📡 Particle Serial Monitor
    </div>
    <div id="serialContent" style="color: #e0f2fe; line-height: 1.6; font-size: 15px;"></div>
  </div>

  <div style="margin-top: 20px;">
    <button onclick="toggleVoltmeter()" 
            id="btnSim"
            style="padding: 14px 40px; font-size: 18px; font-weight: bold; background: #00f7ff; color: #0f172a; border: none; border-radius: 50px; cursor: pointer; box-shadow: 0 0 20px #00f7ff;">
      ▶️ Iniciar Simulación
    </button>
  </div>

  <p id="statusSim" style="margin-top: 12px; font-size: 15px; color: #64748b;">
    Mueve el potenciómetro y observa la lectura + voltaje real
  </p>
</div>

<script>
let running = false;
let interval = null;

function toggleVoltmeter() {
  const btn = document.getElementById('btnSim');
  const status = document.getElementById('statusSim');
  const slider = document.getElementById('potSlider');
  const content = document.getElementById('serialContent');

  if (running) {
    clearInterval(interval);
    btn.innerHTML = '▶️ Iniciar Simulación';
    status.textContent = 'Simulación detenida';
    running = false;
  } else {
    btn.innerHTML = '⏹️ Detener Simulación';
    running = true;

    interval = setInterval(() => {
      if (!running) return;

      const lectura = parseInt(slider.value);
      const voltaje = (lectura * 3.3) / 4095.0;

      const timestamp = new Date().toLocaleTimeString('es-ES', {hour12: false});

      const linea = `
        <span style="color:#64748b">${timestamp} → </span>
        La lecura es de: <span style="color:#22c55e">${lectura}</span><br>
        El voltaje es de: <span style="color:#22c55e">${voltaje.toFixed(2)}</span><br><br>`;

      content.innerHTML += linea;

      // Mantener solo las últimas 8 líneas
      if (content.children.length > 8) {
        content.removeChild(content.children[0]);
      }

      content.scrollTop = content.scrollHeight;
    }, 1000);   // actualiza cada segundo como en tu código
  }
}

// Actualizar valor mostrado en tiempo real
document.getElementById('potSlider').addEventListener('input', function() {
  document.getElementById('valorActual').textContent = this.value;
});
</script>

## Código

**include "Particle.h"**

**SYSTEM_MODE(AUTOMATIC);**

**SerialLogHandler logHandler(LOG_LEVEL_INFO);**

double voltaje = 0.0;
double lectura = 0;

# void setup() {
    Serial.begin(9600);

}

# void loop() {

    lectura = analogRead(A0);
    Serial.print("La lecura es de:");
    Serial.print(lectura);
    Serial.println(" ");
    Serial.print("El voltaje es de:");
    voltaje = (lectura * 3.3) / 4095.0;
    Serial.println(voltaje);
    delay(1000);
}

## Procedimiento
1. Colocar el Particle Photon 2 a un extremo del protoboard
2. Colocar el POTENCIOMETRO en cualquiera de las lineas de conexión que estén libres
3. Conectar un extremo del potenciometro a GND
4. Conectar el extremo sobrante a el puerto con al fuente de 5v (VUSB)
5. Conectar pin central del potenciometro a el pin elegido (A0)
6. Conectar con un cable de tipo MICRO-USB el Particle Photon 2 a tu PC 
7. Conectar el Particle Photon 2 a Internet, puedes usar este enlace: (https://docs.particle.io/tools/developer-tools/configure-wi-fi/)

## Resultado Esperado
Cada segundo (1000ms), verás algo así en tu terminal:

- La lecura es de: 2048
- El voltaje es de: 1.65

- La lecura es de: 4095
- El voltaje es de: 3.30


## Evidencia
![Practica 13](../assets/images/practica13.jpeg)

![Captura Web IDE](../assets/images/practica13-captura.jpeg)

## Ver Video
<video width="50%" controls>
    <source src="/manual-iot/assets/videos/practica13.mp4" type="video/mp4">
    Tu navegador no soporta video.
</video>