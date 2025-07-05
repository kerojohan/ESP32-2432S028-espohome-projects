# Gráfica de Consumos por Franjas - ESP32 TFT

Configuración ESPHome para visualizar consumos eléctricos por franjas horarias en tiempo real en una pantalla TFT ILI9342.

## 📊 Funcionalidad

Esta configuración muestra:
- **Consumos por franjas horarias** (valle, llano, punta)
- **Datos ambientales** (temperatura, humedad, sensación térmica)
- **Voltaje de batería** del sensor Oregon
- **Gráfica de barras** comparativa de consumos
- **Precio actual** de electricidad

## 🖼️ Captura de Pantalla

![Gráfica de Consumos](consums_franges.jpg)

## ⚙️ Configuración

### Hardware
- **ESP32-2432S028** con pantalla TFT ILI9342
- **Conexiones SPI** configuradas en el archivo YAML
- **Sensores de temperatura y humedad** integrados

### Requisitos Home Assistant
- Sensores de consumo por franjas:
  - `sensor.acumulador_casa_franges_horari_valle`
  - `sensor.acumulador_casa_franges_horari_punta`
  - `sensor.acumulador_casa_franges_horari_llano`
- Sensores ambientales:
  - `sensor.menjador_temperature_humidity_sensor_temperatura`
  - `sensor.menjador_temperature_humidity_sensor_humitat`
  - `sensor.t_h_sensor_temperatura`
  - `sensor.t_h_sensor_humitat`
- Sensor de precios: `sensor.esios_pvpc`

### Configuración de Red
Las credenciales WiFi y configuración de red se manejan a través del archivo `secrets.yaml` en la raíz del proyecto. Asegúrate de que esté configurado correctamente.

### Compilación y Flasheo

```bash
# Desde la carpeta raíz del proyecto
esphome run consums_franges/consums_franges.yaml --device /dev/ttyUSB0

# O desde esta carpeta
esphome run consums_franges.yaml --device /dev/ttyUSB0
```

### Logs

```bash
esphome logs consums_franges/consums_franges.yaml
```

## 🎨 Personalización

### Colores de Franjas
- **Valle**: Verde (00ff00)
- **Llano**: Azul (0000ff)
- **Punta**: Rojo (ff0000)

### Fuentes
- **Arimo Regular** en diferentes tamaños (10, 12, 14, 48)
- Caracteres especiales incluidos: °, %, €, etc.

## 📡 Sensores

### Consumos
- `consum_valle`: Consumo en franja valle
- `consum_punta`: Consumo en franja punta
- `consum_llano`: Consumo en franja llano

### Ambientales
- `temp_menjador`: Temperatura del comedor
- `hum_menjador`: Humedad del comedor
- `st_menjador`: Sensación térmica del salón
- `temp_despatx`: Temperatura del despacho
- `hum_despatx`: Humedad del despacho
- `st_despatx`: Sensación térmica del despacho
- `voltatge_oregon`: Voltaje de batería Oregon

### Precios
- `preu_actual`: Precio actual PVPC
- `preu_mitja_dia`: Precio medio del día

## 🔧 Mantenimiento

### Actualización OTA
```bash
esphome run consums_franges/consums_franges.yaml --device 192.168.68.45
```

### Backup
Hacer backup del archivo `consums_franges.yaml` antes de cambios importantes.

## 🐛 Solución de Problemas

### Datos no se actualizan
- Verificar que todos los entity_ids de Home Assistant estén correctos
- Comprobar la conexión WiFi
- Revisar los logs de ESPHome

### Pantalla en blanco
- Verificar conexiones SPI
- Comprobar que el backlight esté encendido
- Revisar la configuración de rotación (180°)

---

**Parte del proyecto ESP32 Sensor de Presencia** 