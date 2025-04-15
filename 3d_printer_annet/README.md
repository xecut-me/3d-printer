# Anet A8 with Sprite Extruder and Klipper Slow Print Configuration

This directory contains configurations for a modified Anet A8 printer with an Ender 3 motherboard, Klipper firmware, and Creality Sprite extruder. The configurations are specifically optimized for slow, stable printing to resolve glitching and fast speed issues.

## Files Included

- `printer.cfg` - Klipper configuration with reduced speed/acceleration settings
- `orca_slicer/print_settings.ini` - Orca Slicer configuration file with slow print profiles
- `orca_slicer/xecut_tuned_v1.orca_printer` - Bundled Orca Slicer configuration (printer, filament, and process settings)

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
     - Added gcode_arcs support for G2/G3 commands

2. **Restart Klipper**:
   - After updating your configuration, restart Klipper to apply the changes
   - Check for any errors in the Klipper logs

## Orca Slicer Setup Instructions

### Method 1: Direct Import (Recommended)

1. **Download and Install Orca Slicer** (if not already installed):
   - Download from: https://github.com/SoftFever/OrcaSlicer/releases
   - Install on your computer

2. **Import Configuration Bundle**:
   - Open Orca Slicer
   - Go to "File" → "Import" → "Import Config Bundle"
   - Select the `orca_slicer/xecut_tuned_v1.orca_printer` file
   - This will import all printer, filament, and process profiles at once

### Method 2: Using the INI File

1. **Import Print Settings**:
   - Open Orca Slicer
   - Go to "File" → "Import" → "Import Config"
   - Select the `orca_slicer/print_settings.ini` file
   - This will import the "Slow and Stable" print profile, "PLA Slow" filament profile, and "Anet A8 Klipper" printer profile

### Method 3: Command Line Extraction and Manual Import

If you need to examine or modify the configuration files before importing, you can extract and view them using the command line:

1. **Extract the Configuration Files**:
   ```bash
   # Create an extraction directory
   mkdir -p /tmp/orca_extract

   # Copy the configuration bundle
   cp path/to/xecut_tuned_v1.orca_printer /tmp/orca_extract/

   # Change to the extraction directory
   cd /tmp/orca_extract

   # Extract the files (the .orca_printer file is actually a zip archive)
   unzip xecut_tuned_v1.orca_printer
   ```

2. **Examine the Extracted Files**:
   - `printer/xecut_tuned_v1.json` - Printer settings
   - `filament/` - Various filament profiles
   - `process/` - Print process profiles
   - `bundle_structure.json` - Metadata about the configuration bundle

3. **Manual Import in Orca Slicer**:
   After reviewing/modifying the files, you can:
   - Import individual configurations via "File" → "Import" → "Import Config"
   - Or repackage the files into a zip archive and rename it with the `.orca_printer` extension

## Configuring G2/G3 Arc Commands

If you experience "Unknown command: G2" or "Unknown command: G3" errors:

1. **Enable Arc Support in Klipper**:
   - Make sure your `printer.cfg` includes:
     ```
     [gcode_arcs]
     resolution: 0.1
     ```
   - Restart Klipper after adding this section

2. **OR Disable Arc Commands in Orca Slicer**:
   - Go to "Print Settings" → "Expert" → "G-code options"
   - Uncheck "Use G2/G3 commands for curves"
   - Save the profile

## Verify Settings

- Check that your printer profile has these settings:
  - G-code flavor: Klipper
  - Start G-code: `START_PRINT EXTRUDER=[first_layer_temperature] BED=[first_layer_bed_temperature]`
  - End G-code: `PRINT_END`
- Check that the "Slow and Stable" print profile has:
  - Perimeter speeds: 30 mm/s
  - External perimeter speed: 25 mm/s
  - First layer speed: 15 mm/s
  - All accelerations set to 500 mm/s² or lower



## Additional Notes

- The Sprite extruder works best with lower retraction settings (0.5mm)
- Consider enabling "Z hop on retraction" if you have z-banding issues
- PLA works best at lower speeds when cooling is limited
- For more information about Klipper configurations, see: https://www.klipper3d.org/Config_Reference.html