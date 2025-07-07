# ESP32 Menu Principal - Sistema de Navegación

Este proyecto implementa un sistema de navegación completo para ESP32 con pantalla TFT ILI9342, que permite acceder a diferentes pantallas de información desde un menú principal.

## 📋 Descripción

El sistema incluye:

1. **Pantalla Principal** - Menú con botones de navegación
2. **Pantalla de Precios PVPC** - Visualización de precios de electricidad con gráfica de barras
3. **Pantalla de Consumos** - Monitoreo de consumos por franjas horarias
4. **Control de Switch** - Botón para controlar un sensor/switch desde la pantalla principal

### Características

- **Navegación táctil** entre pantallas
- **Gráfica de precios PVPC** en tiempo real
- **Visualización de consumos** por franjas (valle, llano, punta)
- **Control de switch** integrado
- **Sensores ambientales** (temperatura y humedad)
- **Interfaz intuitiva** con iconos y colores

## 🛠️ Hardware Requerido

- **ESP32-2432S028** (ESP32 con pantalla TFT integrada)
- **Conexión WiFi** para comunicación con Home Assistant
- **Fuente de alimentación** 5V/2A recomendada

### Conexiones SPI

```
Display SPI:
- CLK: GPIO14
- MOSI: GPIO13  
- MISO: GPIO12
- CS: GPIO15
- DC: GPIO2

Touch SPI:
- CLK: GPIO25
- MOSI: GPIO32
- MISO: GPIO39
- CS: GPIO33
- INT: GPIO36

Backlight:
- PWM: GPIO21
```

## 📁 Estructura del Proyecto

```
menu_principal/
├── menu_principal.yaml      # Archivo de configuración ESPHome
├── secrets.yaml            # Credenciales (no incluido en Git)
├── fonts/                  # Fuentes TTF necesarias
└── README.md               # Este archivo
```

## ⚙️ Configuración

### 1. Archivo secrets.yaml

Crea un archivo `secrets.yaml` en el directorio del proyecto con tus credenciales:

```yaml
# Credenciales WiFi
wifi_ssid: "TU_WIFI_SSID"
wifi_password: "TU_WIFI_PASSWORD"

# Configuración de red estática
static_ip: "192.168.68.45"
gateway: "192.168.68.1"
subnet: "255.255.255.0"
```

### 2. Configuración de Home Assistant

Asegúrate de tener configurados en Home Assistant:

#### Sensores de Precios PVPC:
```yaml
# Ejemplo de configuración de sensores PVPC
sensor:
  - platform: esios
    api_key: "tu_api_key"
    name: "PVPC"
    # Los sensores se crearán automáticamente para cada hora
```

#### Sensores de Consumo:
```yaml
# Ejemplo de configuración de sensores de consumo
sensor:
  - platform: template
    name: "Consumo Valle"
    unit_of_measurement: "kWh"
    # Configura tu lógica de cálculo
    
  - platform: template
    name: "Consumo Llano"
    unit_of_measurement: "kWh"
    
  - platform: template
    name: "Consumo Punta"
    unit_of_measurement: "kWh"
```

#### Switch de Control:
```yaml
# Ejemplo de switch para controlar
switch:
  - platform: template
    name: "Sensor Control"
    turn_on_action:
      - service: script.tu_script_on
    turn_off_action:
      - service: script.tu_script_off
```

#### Sensores Ambientales:
```yaml
# Ejemplo de sensores de temperatura y humedad
sensor:
  - platform: dht
    pin: GPIO4
    temperature:
      name: "Temperatura Comedor"
    humidity:
      name: "Humedad Comedor"
```

### 3. Compilación y Flasheo

```bash
# Compilar y flashear
esphome run menu_principal.yaml --device /dev/ttyUSB0

# O actualizar por OTA
esphome run menu_principal.yaml --device 192.168.68.45
```

## 🎮 Funcionalidad

### Pantalla Principal (Screen 0)

- **Botón "PRECIOS PVPC"** (azul) - Navega a la pantalla de precios
- **Botón "CONSUMOS"** (verde) - Navega a la pantalla de consumos
- **Botón "SWITCH"** (naranja) - Controla el switch de sensor
- **Indicador de estado** - Muestra ON/OFF del switch

### Pantalla de Precios (Screen 1)

- **Precio actual** en la parte superior
- **Gráfica de barras** con precios de las 24 horas
- **Colores dinámicos**:
  - Verde: < 0.1 €/kWh
  - Naranja: 0.1-0.2 €/kWh
  - Rojo: > 0.2 €/kWh
- **Botón "VOLVER"** para regresar al menú principal

### Pantalla de Consumos (Screen 2)

- **Consumos por franjas**:
  - Valle (verde)
  - Llano (naranja)
  - Punta (rojo)
- **Sensores ambientales**:
  - Temperatura y humedad del comedor
  - Temperatura y humedad del despacho
- **Botón "VOLVER"** para regresar al menú principal

## 🎨 Personalización

### Colores

Puedes modificar los colores en la sección `color:`:

```yaml
color:
  - id: black
    hex: '000000'
  - id: white
    hex: 'ffffff'
  - id: blue
    hex: '51c0f2'
  # Añade más colores según necesites
```

### Fuentes

Las fuentes están configuradas para usar Arial. Puedes cambiar a otras fuentes:

```yaml
font:
  - file: "fonts/tu_fuente.ttf"
    id: title_font
    size: 20
    glyphs: "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789.,:;!?()[]{}<>-_=+@#$%&*"
```

### Layout

El layout se define en la sección `lambda:` del display. Puedes modificar:

- Posiciones de botones y texto
- Tamaños de elementos
- Colores y estilos
- Lógica de navegación

## 🔧 Mantenimiento

### Logs

```bash
esphome logs menu_principal.yaml
```

### Actualizaciones OTA

```bash
esphome run menu_principal.yaml --device 192.168.68.45
```

### Backup

Es recomendable hacer backup de la configuración YAML antes de cambios importantes.

## 🐛 Solución de Problemas

### Pantalla no responde al tacto
- Verifica la calibración del touchscreen
- Comprueba las conexiones SPI del touch
- Revisa la configuración de threshold

### No se conecta a WiFi
- Verifica credenciales en secrets.yaml
- Comprueba que la IP estática no esté en conflicto
- Revisa la configuración de gateway y subnet

### Datos no se actualizan
- Verifica la conexión con Home Assistant
- Comprueba que los entity_ids sean correctos
- Revisa los logs de ESPHome

### Navegación no funciona
- Verifica que los botones táctiles estén bien configurados
- Comprueba las coordenadas x_min, x_max, y_min, y_max
- Revisa la variable global current_screen

## 📝 Notas Importantes

1. **Entity IDs**: Asegúrate de que todos los entity_ids en la configuración existan en tu Home Assistant
2. **Fuentes**: Las fuentes TTF deben estar en el directorio `fonts/`
3. **Calibración**: El touchscreen puede necesitar recalibración según tu hardware específico
4. **Memoria**: El ESP32 tiene memoria limitada, evita usar demasiadas fuentes o iconos grandes

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor:

1. Fork el proyecto
2. Crea una rama para tu feature
3. Commit tus cambios
4. Push a la rama
5. Abre un Pull Request

---

**Desarrollado con ❤️ para el control domótico inteligente** 