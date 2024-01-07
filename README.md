### PWM Fan Control Service on Raspberry Pi


#### Overview

This project consists of a Python script and a systemd service unit file to control the speed of a 40mm 5V PWM fan on a Raspberry Pi. The fan speed is adjusted proportionally to the CPU temperature, ideal for setups like the Pi Desktop Case with an OLED Stats Display. The project is based on [instructions]((https://www.the-diy-life.com/connecting-a-pwm-fan-to-a-raspberry-pi/)) and [code]((https://github.com/mklements/PWMFanControl)) by [Michael Klements](https://github.com/mklements).

#### Files

1. **FanProportional.py** - Python script to control the fan speed.
2. **systemd/pwmfancontrol.service** - Systemd service unit file to run the Python script as a service.

#### FanProportional.py

This Python script reads the CPU temperature of the Raspberry Pi and adjusts the speed of a connected PWM fan accordingly. The fan speed is set between a minimum and maximum speed, corresponding to a defined range of CPU temperatures.

- **Dependencies**: RPi.GPIO, time, subprocess
- **GPIO Mode**: BCM
- **PWM Frequency**: 100Hz
- **Temperature and Speed Range**: Customize the `minTemp`, `maxTemp`, `minSpeed`, and `maxSpeed` variables as needed.

#### pwmfancontrol.service

This systemd service file is used to run `FanProportional.py` as a service on the Raspberry Pi, ensuring that it starts automatically on boot.

- **Service Type**: simple
- **ExecStart**: Path to the Python interpreter and the `FanProportional.py` script.
- **Restart Policy**: The service will restart on failure.
- **User**: root
- **Environment**: PYTHONUNBUFFERED=1

#### Installation

1. Place `FanProportional.py` in `/opt/pwmfancontrol/`.
2. Place `pwmfancontrol.service` in `/etc/systemd/system/`.
3. Make `FanProportional.py` executable: `chmod +x /opt/pwmfancontrol/FanProportional.py`.
4. Reload systemd: `sudo systemctl daemon-reload`.
5. Enable the service: `sudo systemctl enable pwmfancontrol.service`.
6. Start the service: `sudo systemctl start pwmfancontrol.service`.

#### Usage

Once installed, the service will run automatically on boot. The fan speed will adjust based on the Raspberry Pi's CPU temperature. 

#### Troubleshooting

- Ensure the GPIO pin and PWM frequency in the Python script match your fan's specifications.
- Check the status of the service with `sudo systemctl status pwmfancontrol.service`.

#### Credits

- [Script](https://github.com/mklements/PWMFanControl) and [concept](https://www.the-diy-life.com/connecting-a-pwm-fan-to-a-raspberry-pi/) by [Michael Klements](https://github.com/mklements).
- Adaptation and systemd integration for Raspberry Pi by Michael Ramstein.
