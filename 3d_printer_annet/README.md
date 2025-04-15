# Anet A8 with Sprite Extruder and Klipper Slow Print Configuration

This directory contains configurations for a modified Anet A8 printer with an Ender 3 motherboard, Klipper firmware, and Creality Sprite extruder. The configurations are specifically optimized for slow, stable printing to resolve glitching and fast speed issues.

## Files Included

- `printer.cfg` - Klipper configuration with reduced speed/acceleration settings
- `orca_slicer/print_settings.ini` - Orca Slicer configuration file with slow print profiles

## Klipper Setup Instructions

1. **Update Your Klipper Configuration**:
   - Make a backup of your existing `printer.cfg` file
   - Replace or update your configuration with the settings in the provided `printer.cfg`
   - Key changes include:
     - Reduced max_velocity (100 mm/s)
     - Reduced max_accel (500 mm/s²)
     - Reduced Z axis speeds
     - Added pressure advance settings for the Sprite extruder
     - Added PRINT_START and PRINT_END macros

2. **Restart Klipper**:
   - After updating your configuration, restart Klipper to apply the changes
   - Check for any errors in the Klipper logs

## Orca Slicer Setup Instructions

1. **Download and Install Orca Slicer** (if not already installed):
   - Download from: https://github.com/SoftFever/OrcaSlicer/releases
   - Install on your computer

2. **Import Print Settings**:
   - Open Orca Slicer
   - Go to "File" → "Import" → "Import Config"
   - Select the `orca_slicer/print_settings.ini` file
   - This will import the "Slow and Stable" print profile, "PLA Slow" filament profile, and "Anet A8 Klipper" printer profile

3. **Verify Settings**:
   - Check that your printer profile has these settings:
     - G-code flavor: Klipper
     - Start G-code: `START_PRINT EXTRUDER=[first_layer_temperature] BED=[first_layer_bed_temperature]`
     - End G-code: `PRINT_END`
   - Check that the "Slow and Stable" print profile has:
     - Perimeter speeds: 30 mm/s
     - External perimeter speed: 25 mm/s
     - First layer speed: 15 mm/s
     - All accelerations set to 500 mm/s² or lower

## Printing Instructions

1. **First Print Test**:
   - Slice a simple test model (like a 20mm calibration cube)
   - Use the "Slow and Stable" print profile and "PLA Slow" filament profile
   - Monitor the first print carefully

2. **Fine-tuning**:
   - If you still experience issues, consider further reducing speeds
   - If printing is successful but too slow, you can gradually increase speeds
   - For the Sprite extruder, you may need to calibrate the pressure advance setting

3. **Troubleshooting**:
   - If you experience layer shifting, reduce acceleration further
   - If you experience under-extrusion, check filament path and extruder calibration
   - If first layer adhesion is poor, reduce first layer speed further

## Additional Notes

- The Sprite extruder works best with lower retraction settings (0.5mm)
- Consider enabling "Z hop on retraction" if you have z-banding issues
- PLA works best at lower speeds when cooling is limited
- For more information about Klipper configurations, see: https://www.klipper3d.org/Config_Reference.html