# Ender 3 V2 firmware

Following applies only for Creality v. 4.2.2.

## Prepare development enviroment

1. Download and install VS Code.
2. Install `Auto Build Marlin` extension
3. If not installed, install `PlatformIO` extension.

## Compile code
1. Download latest stable Marlin release from [here](https://marlinfw.org/meta/download/). 
2. Do not forget to download configuration.
3. Unpack Marlin to folder `Marlin_FW`.
4. Copy content of configuration folder (e.g. `Configurations-release-2.1.1\config\examples\Creality\Ender-3 V2\CrealityV422\`) to `Marlin_FW\Marlin` folder. 
5. Open VS Code, move to `PlatformIO` extension and open Marlin project.
6. Modify settings, if needed and execute build from `Auto Build Marlin` extension - build enviroment `STM32F103RE_creality (512K)`
7. Copy `.bin` file to SD card root location and flash Ender 3D printer.
8. From configuration zip file, copy LCD display FW (`Configurations-release-2.1.1\config\examples\Creality\Ender-3 V2\LCD Files\`) to SD card root location.

## Flash printer
1. First flash LCD display
2. Flash printer.

## Modify settings

### `Configuration.h`

`#define DWIN_CREALITY_LCD` - Ender 3 OEM display 

`#define PREHEAT_1_LABEL` and `#define PREHEAT_2_LABEL`- Preheat constants  

`#define LCD_BED_TRAMMING` - Add a menu item to move between bed corners for manual bed adjustment  

`#define NOZZLE_PARK_FEATURE` - Park the nozzle at the given XYZ position on idle or G27.

`#define FWRETRACT` - new menu during printing - to allow change retraction values

`#define ADVANCED_PAUSE_FEATURE` - filament change