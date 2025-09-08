# 🧯 Known Issues with Qidi Q1 Pro (Out of the Box)
[← Back to Table of Contents](README.md)

## ⚡ Electrical

### Fan short when print detaches
If a large print detaches from the bed, the head can crash into it and rip off the front cover. This can cause the fan wires to short (insulation wears through), often killing the head board.

**Solutions:**
- Use a magnetic connector for the fan
- Or secure the front cover with a screw on the side

---

### Poor 24V terminal soldering on PSU
Power terminals on the stock PSU can desolder or break off due to vibration.

**Solutions:**
- Resolder the terminals properly
- Or move wires to adjacent PSU terminals

---

### No cooling fan for head driver
The stepper driver on the head board overheats — no fan is installed by default, although there’s space and a 2-pin header.

**Fix:**
- Install a 2006 5V fan, XH2.54 2-pin, with small screws

---

## ⚙️ Mechanical & Build Issues

### Auto Z-offset fails
Error: `no trigger on probe after full movement`. Caused by faulty piezo sensors under the bed — detachment, bad wiring, bad tuning, or uneven preload.

**Solutions:**
- Disable the failing sensor (there are 3 total)
- Re-glue the piezo element
- Check and adjust leveling knobs
- Adjust thresholds and offsets in Klipper config

---

### Loose linear bearing in print head
The lower linear bearing of the print head often rattles in its mount.

**Fixes:**
- Wrap foil around the bearing to tighten fit
- Or secure it with a screw through the back of the housing

---

### Faulty extruder gear
The extruder gear may bind even without filament — manufacturing defect.

**Check:**
- Remove the extruder
- Rotate the gear by hand — it should spin smoothly

---

### Warped aluminum bed
Stock bed is made of soft, thin aluminum. Warping over 0.25 mm is common when heated.

**Quick fix:**
- Stick Kapton tape on the magnetic sheet under low spots
- Aim for less than 0.25 mm deviation when hot

---

### Idler pulleys start squeaking
Over time, the idler pulleys that guide the belts start to squeak.

**Fix:**
- Lubricate pulley axles using a syringe or small brush
- Use silicone grease (e.g., PMS-1000)

---

### Bent Z leadscrews
Sometimes Z-axis leadscrews come bent from factory. Since they are integrated with the motor, replacement is difficult.

**Support comment:** "within tolerance"

---

## 🖥️ Software

### Wrong time zone / region
Incorrect system time prevents QidiLink from connecting.

**Fix:**
- Follow the instructions in the [official Qidi wiki](https://wiki.qidi3d.com/en/Memo/System-Time-Modification)

---

### Outdated Klipper
The stock firmware does not support `G17` (arc mode), leading to console errors.

**Fix:**
- Avoid G-code arcs and helical Z-hop in your slicer

---

## 📷 Camera & Print Bed

### Camera loses focus
The USB camera loses focus after prolonged heat exposure in the chamber. The lens is glued in place.

**Fix:**
- Carefully remove the glue and manually adjust the focus

---

### Magnetic bed surface peels off
When printing large parts from shrink-prone materials with the heated chamber active, the magnetic sheet may start delaminating from the aluminum bed.

---

### Head crashes during nozzle wipe
After belt tensioning or enabling `skew_correction`, the head may crash into the frame during nozzle wipe → missed steps and loud knocking.

**Fixes:**
- Check your wipe macros and `G28`
- Adjust `position_endstop` in config
- Make sure nothing physically blocks the parked position

---

Based on this post: https://t.me/qidixmax3/1221/71718  
Author: https://t.me/microshur

[← Back to Table of Contents](README.md)
