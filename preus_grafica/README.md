# Gráfica de Precios PVPC - ESP32 TFT

Configuración ESPHome para visualizar precios de electricidad PVPC en tiempo real en una pantalla TFT ILI9342.

## 📊 Funcionalidad

Esta configuración muestra:
- **Precio actual** de electricidad PVPC
- **Gráfica de barras** con precios por hora (00h-23h)
- **Precio medio del día**
- **Indicadores visuales** de franjas horarias (valle, llano, punta)

## 🖼️ Captura de Pantalla

![Gráfica de Precios](preus_grafica.jpg)

## ⚙️ Configuración

### Hardware
- **ESP32-2432S028** con pantalla TFT ILI9342
- **Conexiones SPI** configuradas en el archivo YAML

### Requisitos Home Assistant
- Sensor de precios PVPC: `sensor.esios_pvpc`
- Entidades con atributos de precios por hora

### Configuración de Red
Las credenciales WiFi y configuración de red se manejan a través del archivo `secrets.yaml` en la raíz del proyecto. Asegúrate de que esté configurado correctamente.

### Compilación y Flasheo

```bash
# Desde la carpeta raíz del proyecto
esphome run preus_grafica/preus_grafica.yaml --device /dev/ttyUSB0

# O desde esta carpeta
esphome run preus_grafica.yaml --device /dev/ttyUSB0
```

### Logs

```bash
esphome logs preus_grafica/preus_grafica.yaml
```

## 🎨 Personalización

### Colores de Franjas
- **Valle**: Verde (00ff00)
- **Llano**: Azul (51c0f2) 
- **Punta**: Rojo (ff0000)

### Fuentes
- **Arimo Regular** en diferentes tamaños (8, 10, 12, 14)
- Caracteres especiales incluidos: €, ó, Ó, É, í, etc.

## 📡 Sensores

- `precio_actual`: Precio actual PVPC
- `precio_00h` a `precio_23h`: Precios por hora
- `precio_mitja_dia`: Precio medio del día

## 🔧 Mantenimiento

### Actualización OTA
```bash
esphome run preus_grafica/preus_grafica.yaml --device 192.168.68.45
```

### Backup
Hacer backup del archivo `preus_grafica.yaml` antes de cambios importantes.

---

**Parte del proyecto ESP32 Sensor de Presencia** 