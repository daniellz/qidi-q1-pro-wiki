# Stepper Driver Temperature Monitoring on Qidi Q1 Pro

![Fluidd](/docs/images/fluid-temp.jpg)

## Description

This modification enables stepper driver temperature monitoring in the Fluidd interface by upgrading to a specific version that supports this feature while maintaining system stability.

## Why Version 1.28.1?

- **1.28.1** is the latest stable version that doesn't break existing functionality
- Newer versions may cause issues (e.g., camera functionality may fail)
- This version specifically supports stepper driver temperature display
- Maintains compatibility with Qidi Q1 Pro hardware

## Prerequisites

- SSH access to your printer
- Basic command line knowledge
- **Important:** This will replace your current Fluidd installation

## Installation

### Step 1: Backup Current Installation

Before proceeding, create a backup of your current Fluidd installation:

```bash
cd ~
mv fluidd fluidd-bak
```

### Step 2: Install Fluidd v1.28.1

Execute the following commands to upgrade Fluidd:

```bash
cd ~
wget --no-check-certificate wget --no-check-certificate https://raw.githubusercontent.com/billkenney/revert_qidi_software/main/fluidd-1.28.tgz
tar -xzf fluidd-1.28.tgz
rm fluidd-1.28.tgz
```

**Command breakdown:**
- `cd ~` - Navigate to home directory
- `mv fluidd fluidd-bak` - Backup current Fluidd installation
- `wget --no-check-certificate https://...` - Download Fluidd v1.28.1 (specific hash 066ece3128af69661ff83a7f181a715175f35fcb) prepared by [@billkenney](https://github.com/billkenney) 
- `tar -xzf fluidd-1.28.tgz` - Extract the archive
- `rm fluidd-1.28.tgz` - Clean up downloaded file

### Step 3: Restart Printer

After installation, power off and power on the entire printer to ensure all services restart properly.

### Step 4: Verify Installation

1. Open your Fluidd web interface
2. Check that the interface loads correctly
3. Verify that stepper driver temperatures now appear in the temperature monitoring section

## Result

After successful installation, you will see additional temperature sensors in Fluidd:

- **Stepper driver temperatures** - Individual temperature readings for each stepper motor driver
- **Enhanced monitoring** - Better visibility of all thermal components
- **Stable operation** - All existing functionality remains intact

## Troubleshooting

### Rollback to stock version

If you encounter issues, you can restore your original installation:

```bash
cd ~
rm -rf fluidd
mv fluidd-bak fluidd
```

Then power off and power on the printer.

## Important Notes

- **Version warning:** Do not upgrade to versions newer than 1.28.1 without testing
- **Backup importance:** Always keep backups before system modifications
- **Compatibility:** This method is specifically tested on Qidi Q1 Pro
- **Support:** This uses a community-maintained package from billkenney's repository

## What This Enables

With this upgrade, your Fluidd interface will display:

- All standard temperature sensors (MCU, Host CPU)
- **NEW:** Individual stepper driver temperatures
- Enhanced thermal monitoring capabilities
- Better system health visibility

---

**Note:** This modification enhances monitoring capabilities without affecting printer operation. The stepper driver temperature monitoring helps prevent thermal issues and extends hardware lifespan.
