# ESP32 Automated Watering System

An object-oriented Python simulation of an automated plant watering system using ESP32 microcontroller with various sensors and hardware components.

## Overview

This project simulates a smart watering system that automatically monitors soil moisture and humidity levels, then activates a water pump when needed. The system includes safety features like voltage regulation, buzzer alerts, and manual override capabilities.

## Features

- **Automated Watering**: Monitors soil moisture and humidity to automatically water plants
- **Voltage Regulation**: Built-in 7805 voltage regulator with safety checks
- **Alert System**: Buzzer notifications for low voltage and pump errors
- **Manual Override**: Push button to manually stop the pump
- **Remote Control**: Mock WiFi client for remote system control
- **Hardware Abstraction**: Clean OOP design with abstract base classes

## Hardware Components

### Main Components
- **ESP32 Microcontroller**: Central control unit
- **DHT11 Sensor**: Temperature and humidity monitoring
- **Soil Moisture Sensor**: Monitors soil water content
- **Water Pump**: Automated watering mechanism
- **Relay Module**: Controls pump power switching
- **7805 Voltage Regulator**: Provides stable 5V power supply
- **Buzzer**: Audio alerts and notifications
- **Push Button**: Manual pump control

### Passive Components
- **Resistors**: Current limiting and voltage division
- **Capacitors**: Power supply filtering and decoupling

## Pin Configuration

| Component | ESP32 Pin |
|-----------|-----------|
| DHT11 Sensor | Pin 0 |
| Soil Sensor | Pin 1 |
| Relay | Pin 2 |
| Pump | Pin 3 |
| AC Power | Pin 4 |
| Voltage Regulator | Pin 5 |
| Buzzer | Pin 6 |
| Push Button | Pin 7 |
| Ground Pins | Pins 8-12 |

## System Architecture

### Class Hierarchy

```
ESP32 (Main Controller)
├── HardwareModule (Abstract Base Class)
│   ├── DHT11_sensor
│   ├── SoilSensor
│   ├── Relay
│   ├── Pump
│   ├── VoltageRegulator
│   ├── PowerSupply12V
│   └── Buzzer
├── PassiveComponent (Abstract Base Class)
│   ├── Resistor
│   └── Capacitor
└── WateringSystem (Main System Controller)
```

### Key Classes

- **`ESP32`**: Hardware abstraction layer defining pin assignments
- **`HardwareModule`**: Abstract base class for all hardware components
- **`WateringSystem`**: Main system orchestrator
- **`MockESP32WiFiClient`**: Simulates remote control functionality

## Installation & Setup

1. **Clone or download** the project files
2. **Ensure Python 3.6+** is installed
3. **Run the simulation**:
   ```bash
   python watering_system.py
   ```

## Usage

### Basic Operation

The system automatically:
1. Initializes all hardware components
2. Monitors soil moisture and humidity levels
3. Activates the pump when moisture < 10% AND humidity < 16%
4. Provides voltage regulation and safety checks
5. Alerts user with buzzer for error conditions

### Manual Controls

- **Push Button**: Press to manually stop the pump
- **Remote Commands**: Send commands via the mock WiFi client
  - `"TURN_ON_PUMP"`: Manually start the pump
  - `"TURN_OFF_PUMP"`: Manually stop the pump
  - `"Let_current"`: Enable power supply
  - `"STOP_current"`: Disable power supply

### Safety Features

- **Voltage Monitoring**: Automatically shuts down if voltage is too low or too high
- **Error Alerts**: Buzzer patterns for different error conditions
- **Manual Override**: Emergency stop via push button

## Configuration

### Moisture Thresholds
```python
# In check_moisture() method
if moisture < 10 and humidity < 16:  # Adjust these values as needed
    # Activate watering
```

### Voltage Limits
```python
# In VoltageRegulator class
output_voltage = 5  # 7805 outputs 5V
# Input voltage should be between 5V and 20V
```

### Component Values
```python
# Passive components example
resistor1 = Resistor(1, 5)  # 1Ω, ±5% tolerance
capacitor1 = Capacitor(100, "uF", 10)  # 100µF, ±10% tolerance
```

## Customization

### Adding New Components
1. Inherit from `HardwareModule` or `PassiveComponent`
2. Implement required abstract methods (`initialize()`, `operate()`)
3. Add to the `WateringSystem` initialization

### Modifying Behavior
- **Watering Logic**: Edit `check_moisture()` method
- **Safety Checks**: Modify `check_voltage()` method
- **Alert Patterns**: Customize `beep_pattern()` in Buzzer class

## Error Handling

The system includes several error detection mechanisms:

- **Low Voltage**: Buzzer alert + pump shutdown
- **High Voltage**: Buzzer alert + system protection
- **Pump Errors**: Specific buzzer pattern
- **Hardware Failures**: Component-specific error handling

## Development Notes

### Design Patterns Used
- **Abstract Factory**: Hardware component creation
- **Template Method**: Hardware module initialization
- **Observer**: Component state monitoring

### Future Enhancements
- Real hardware integration with actual ESP32
- WiFi connectivity implementation
- Data logging and analytics
- Mobile app integration
- Multiple zone support

## Troubleshooting

### Common Issues
1. **No watering activation**: Check moisture/humidity thresholds
2. **Continuous buzzer**: Verify voltage regulator operation
3. **Pump not responding**: Check relay state and power supply

### Debug Mode
Enable verbose logging by adding print statements in component methods for detailed system state information.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Implement changes with proper documentation
4. Test thoroughly
5. Submit a pull request

## License

This project is open source and available under the MIT License.

## Contact

For questions, issues, or contributions, please create an issue in the project repository.

---

**Note**: This is a simulation/prototype. For actual hardware implementation, replace mock sensor readings with real GPIO operations and sensor libraries appropriate for your ESP32 development environment.
