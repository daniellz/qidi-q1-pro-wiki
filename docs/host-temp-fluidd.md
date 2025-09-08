# MCU and Host Temperature Monitoring on Qidi Q1 Pro
[← Back to ToC](README.md)
## Description
![Fluidd](/docs/images/fluid-temp.jpg)

This modification allows you to monitor temperatures of all important components of your Qidi Q1 Pro printer in the Fluidd/Mainsail interface. This helps control equipment status and prevent overheating.

## What is monitored

- **Host CPU (MKSPI)** - single board computer temperature
- **Mainboard MCU (STM32)** - main control board temperature
- **Toolhead MCU (RP2040)** - extruder control board temperature

## Installation

### Step 1: Backup

Before making changes, create a backup of your `printer.cfg` file:

```bash
cp /home/mks/klipper_config/printer.cfg /home/mks/klipper_config/printer.cfg.backup
```

### Step 2: Edit configuration

Open the `printer.cfg` file via Fluidd/Mainsail web interface or SSH:

```bash
nano /home/mks/klipper_config/printer.cfg
```

### Step 3: Add monitoring sections

Add the following blocks to your `printer.cfg` file:

```ini
# MKSPI Temperature (Host CPU)
[temperature_sensor Host]
sensor_type: temperature_host
min_temp: 0
max_temp: 90

# STM32 MCU Temperature (Main Board)
[temperature_sensor Mainboard_MCU]
sensor_type: temperature_mcu
sensor_mcu: U_1
min_temp: 0
max_temp: 90

# RP2040 MCU Temperature (Toolhead Board)
[temperature_sensor Toolhead_MCU]
sensor_type: temperature_mcu
min_temp: 0
max_temp: 90
```

### Step 4: Restart Klipper

After saving the file, restart Klipper:

- In the web interface, click the **"RESTART"** button
- Or via SSH: `sudo systemctl restart klipper`

## Result

After successful configuration, three new temperature sensors will appear in the Fluidd/Mainsail interface:

- **Host CPU** - shows single board computer temperature
- **Mainboard MCU** - shows STM32 main board temperature
- **Toolhead MCU** - shows RP2040 extruder board temperature

## Configuration parameters

### Parameter description:

- `sensor_type: temperature_host` - sensor type for host CPU monitoring
- `sensor_type: temperature_mcu` - sensor type for MCU temperature monitoring
- `sensor_mcu: U_1` - specifies the particular MCU (for main board)
- `min_temp: 0` - minimum allowable temperature
- `max_temp: 90` - maximum allowable temperature

Full description of temperature sensor configuration in [Klipper](https://www.klipper3d.org/Config_Reference.html#temperature_sensor)

### Temperature limits setup:

You can change temperature limits according to your needs:

```ini
min_temp: 0    # Minimum temperature (°C)
max_temp: 90   # Maximum temperature (°C)
```

When these limits are exceeded, Klipper will issue a warning or stop printing.

## Inaccurate temperature readings

This is normal for MCU sensors - they show approximate chip temperature, not accurate ambient temperature.

**Note:** This modification does not affect printer operation, it only adds temperature monitoring for better equipment status control.
[← Back to ToC](README.md)
