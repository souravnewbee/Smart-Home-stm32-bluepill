# Smart Home Automation — STM32 Blue Pill

> Wireless home appliance control using Bluetooth,
> with automatic temperature-based fan control and IR security alert.

## Demo
![Project photo](images/project_photo.jpg)

## Features
- Bluetooth control via HC-05 + mobile app
- DHT11 temperature & humidity monitoring
- Auto fan ON when temp > 30°C
- IR motion detection → buzzer alert
- 4-channel relay for AC appliance control

## Hardware
| Component | Pin |
|-----------|-----|
| HC-05 TX  | PA10 (USART1_RX) |
| HC-05 RX  | PA9  (USART1_TX) |
| DHT11     | PA1  |
| IR Sensor | PA0  |
| Relay IN1 | PB9  |
| Relay IN2 | PB8  |
| Buzzer    | PB12 |

## Block Diagram
![Block diagram](images/block_diagram.png)

## How to Build
1. Open in STM32CubeIDE
2. Flash to STM32F103C8T6 via ST-Link
3. Pair HC-05 with Bluetooth app (default PIN: 1234)

## License
MIT
