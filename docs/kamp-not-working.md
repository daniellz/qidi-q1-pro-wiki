# KAMP Not Working (Scans Entire Bed)
[← Back to ToC](README.md)
## Problem

If your Qidi Q1 Pro scans the entire bed surface when building the height map (bed mesh), instead of scanning only the print area of the object, the issue may be in the slicer settings.

## Cause

[KAMP](https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging) (Klipper Adaptive Meshing & Purging) in Klipper works based on object polygons that should be included in the G-code. If this information is missing, the system defaults to scanning the entire bed.

## Solution in OrcaSlicer

### Step 1: Open G-code Settings

1. Open OrcaSlicer
2. Go to **"Process"** settings
3. Find the **"Others"** section

### Step 2: Enable Exclude Objects

Find and **enable the checkbox** `exclude_objects`:

![Exclude Objects Setting](/docs/images/exclude-object.png)

### Step 3: Save Settings

1. Save the printer settings
2. Re-slice the model for printing
3. Ensure that lines with `EXCLUDE_OBJECT_DEFINE` appear in the G-code

## Verify Result

### G-code should contain lines like:

```gcode
EXCLUDE_OBJECT_DEFINE NAME=object_1 CENTER=100,100 POLYGON=[[90,90],[110,90],[110,110],[90,110]]
```

### During printing:

- Height map will be built only in object areas
- Calibration time will be significantly reduced
- More accurate calibration in the needed area

## Additional Information

### How KAMP Works

KAMP (Klipper Adaptive Meshing & Purging) uses:
- **Object polygons** from G-code to determine scanning area
- **Adaptive mesh** instead of full bed scanning

---

**Note:** This setting should be enabled by default for optimal operation with modern versions of Klipper and KAMP.
[← Back to ToC](README.md)
