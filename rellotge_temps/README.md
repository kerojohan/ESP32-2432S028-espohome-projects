# rellotge_temps

Este proyecto muestra una interfaz gráfica avanzada en una pantalla TFT controlada por un ESP32 (modelo ESP32-2432S028R, también conocido como Cheap Yellow Display), orientada a la visualización de información horaria, meteorológica y de sensores de temperatura/humedad.

## Funcionalidad principal
- **Reloj digital** grande y centrado, con hora, fecha, día de la semana y año.
- **Iconos a color** para interior (casa) y para la meteorología exterior (dinámico según el estado de Met.no en Home Assistant).
- **Visualización de sensores**:
  - Temperatura y humedad interior (sensor local o de Home Assistant).
  - Temperatura y humedad exterior (sensor local o de Home Assistant).
- **Brillo del backlight** controlado por software (por defecto al 1%).

## Integración meteorológica dinámica
- El icono de la temperatura exterior cambia automáticamente según el estado del sensor meteorológico de Met.no en Home Assistant (`weather.metno`).
- Se incluyen iconos BMP a color para las condiciones más habituales: soleado, nublado, lluvia, tormenta, nieve, niebla, etc.

## Estructura del proyecto
- `rellotge_temps.yaml`: Configuración principal de ESPHome.
- `fonts/`: Fuentes utilizadas (DS-DIGITAL para los valores, otras para iconos si es necesario).
- `icons/`: Iconos BMP a color para interior y todas las condiciones meteorológicas exteriores.

## Pines y hardware
- **Pantalla TFT**: SPI (MISO: GPIO12, MOSI: GPIO13, CLK: GPIO14, CS: GPIO15, DC: GPIO2, RESET: GPIO4)
- **Backlight**: GPIO21 (PWM, controlado por `ledc`)

## Personalización
- Puedes añadir o modificar iconos BMP en la carpeta `icons/` para nuevas condiciones meteorológicas.
- El brillo del backlight se puede ajustar cambiando el valor en el bloque `on_boot` del YAML.

## Requisitos
- ESPHome 2025.x o superior
- Home Assistant con integración Met.no y sensores de temperatura/humedad expuestos
- Fuentes y BMPs incluidos en las carpetas correspondientes

## Instalación y Actualización

### Primera vez (OTA o USB)

1. **Conecta el ESP32 al ordenador** mediante cable USB

2. **Compila y sube el firmware:**
   ```bash
   cd rellotge_temps
   esphome run rellotge_temps.yaml
   ```

3. **Selecciona el puerto USB** cuando te lo pida (normalmente `/dev/ttyUSB0` o similar)

4. **Espera a que termine** la compilación y carga. El proceso incluye:
   - Compilación del firmware
   - Generación del bootloader
   - Creación de particiones
   - Carga al ESP32
   - Verificación

5. **Verifica la conexión:**
   - El ESP32 se conectará automáticamente al WiFi configurado en `secrets.yaml`
   - Se conectará a Home Assistant vía API
   - La pantalla mostrará el reloj, sensores y meteorología

### Actualizaciones siguientes (OTA - Over The Air)

Una vez el ESP32 está conectado a tu red WiFi, puedes actualizar el firmware de forma inalámbrica:

```bash
cd rellotge_temps
esphome run rellotge_temps.yaml
```

Selecciona la opción OTA cuando te lo pida (normalmente `esp32-rellotge.local`)

### Configuración de secrets

Asegúrate de tener configurado el archivo `secrets.yaml` en la raíz del proyecto con:

```yaml
wifi_ssid: "TuSSID"
wifi_password: "TuContraseña"
```

### Verificación de funcionamiento

El ESP32 está funcionando correctamente cuando:
- ✅ La pantalla muestra el reloj con la hora actual
- ✅ Los sensores de temperatura/humedad muestran valores reales
- ✅ El icono meteorológico cambia según el estado del tiempo
- ✅ El dispositivo aparece en Home Assistant como `esp32-rellotge`

### Troubleshooting

- **No compila**: Verifica que tienes instalado ESPHome 2025.x o superior
- **No encuentra el puerto**: Asegúrate de que el ESP32 está conectado y que tienes permisos sobre `/dev/ttyUSB*`
- **No se conecta al WiFi**: Verifica las credenciales en `secrets.yaml`
- **Sensores en 0 o NaN**: Verifica que los entity_id en Home Assistant están correctos y expuestos

---

**Autor:** Joan
**Última actualización:** Noviembre 2025 