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

---

**Autor:** Tu nombre o alias
**Última actualización:** Julio 2025 