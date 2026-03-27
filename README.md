# Smart Home System README (English)

An embedded smart home system developed based on the NXP iMX6ULL development board and Qt 5.14.2 framework. It implements 9 core functions including doorbell control, smart light adjustment, environmental sensing, and home appliance monitoring, with 10 visual interactive pages. Adopting a layered architecture of **UI Layer - Business Logic Layer - Hardware Interface Layer**, combined with SQLite database and Qt multi-threading technology, it realizes an embedded intelligent control solution featuring software-hardware collaboration and multi-module linkage.

## Project Overview

This project is a course design work for embedded systems. Taking the iMX6ULL Cortex-A7 development board as the hardware core, it is developed based on Qt Widgets under Linux 4.1.15 system, with lightweight UI design adapted for embedded devices. At the hardware layer, it drives peripherals such as buzzers, AP3216C sensors, and LED lights through GPIO/I2C/UART interfaces. At the software layer, it implements functions such as device control, data collection, status monitoring, and information management, fully meeting the core requirements of local smart home control.

### Core Features

- Full coverage of 9 functional modules, covering basic control and sensing scenarios for smart homes

- Flexible page switching and real-time status saving based on Qt Signal-Slot mechanism

- Hardware operations implemented via Linux system calls, directly operating device files under `/sys/class` for easy driver adaptation

- Lightweight SQLite database for persistent storage of resident information and timing tasks

- Multi-thread isolation of UI and hardware operations to avoid interface freezes and ensure real-time performance

- Custom Qt widgets (Arc Graph, Glow Text, Digital Selector) to enhance embedded UI interaction experience

- Dual-environment compatibility: supports cross-compilation for iMX6ULL and local debugging on Ubuntu desktop

## Hardware Environment

### Core Hardware

- Main Control Board: NXP iMX6ULL Development Board (512MB RAM + 4GB EMMC)

- Operating System: Linux 4.1.15 Embedded System

### Peripheral Modules

|Peripheral Name|Function Purpose|Communication Interface|
|---|---|---|
|Buzzer|Doorbell ring reminder|GPIO|
|LED Lights|Smart light on/off control|GPIO|
|AP3216C Sensor|Ambient light/Proximity/Infrared data collection|I2C1|
|UART Module|Serial data transceiver debugging|UART|
|Curtain Motor (Simulated)|Curtain opening adjustment|GPIO|
|HDMI Display|UI interface display|HDMI|
|USB Mouse/Keyboard|Human-computer interaction|USB|
## Software Environment

### Development Environment

- Host System: Ubuntu 20.04 LTS

- Cross-Compilation Toolchain: arm-linux-gnueabihf-gcc 7.5.0

- Qt Version: Qt 5.14.2 (Qt Creator + Qt Widgets)

- Database: SQLite 3.31.1

- Version Control: Git

### Runtime Environment

- Target Board: iMX6ULL Embedded Linux 4.1.15

- Qt Runtime: Qt 5.14.2 for ARM

- Dependencies: `libqt5widgets5`, `libqt5multimedia5`, `libsqlite3-0`

## Functional Module Description

The system includes **1 main page + 9 functional pages**. The main page serves as the function entry, and each functional page supports one-click return to the main interface. The core functional modules are as follows:

|Module Name|Corresponding Page|Core Function|
|---|---|---|
|Smart Light Control|Page 1|LED light on/off control, operating `/sys/class/leds` device file via GPIO|
|Doorbell (Buzzer) Control|Page 2|Buzzer on/off status switching, real-time device status reading and UI update|
|Home Audio Playback|Page 3|Local MP3 file scanning, play/pause/skip, progress bar display, loop mode|
|Kitchen/Bedroom Timer|Page 4|1-24 hour timing task setting, support add/delete/start/stop, tasks stored in SQLite|
|Curtain & Door Control|Page 5|0-100% curtain opening sliding adjustment, door/window status switching, color marking (Red=Open/Green=Closed)|
|Resident Information Registration|Page 6|CRUD operations for resident info (ID/Name/Age/Gender/Photo), persistent storage in SQLite|
|Environmental Sensing Hub|Page 7|AP3216C collects ambient light/proximity/infrared data, real-time display via Arc Graph & Glow Text, 500ms refresh|
|Serial Debug Assistant|Page 8|Serial parameter configuration (Baud Rate/Data Bit/Parity), data transceiver & display, supports up to 921600bps|
|Home Appliance Status Monitoring|Page 9|Polling monitoring for 5 types of appliances (Fridge/AC/Washer etc.), implemented with multi-threading, working devices highlighted|
## Project Structure

```Plain Text

SmartHome_System/
├── ap3216c.cpp/.h        # AP3216C sensor driver and data collection class
├── arcgraph.cpp/.h       # Custom Arc Graph widget (for environmental data display)
├── button.cpp/.h         # Custom switch button widget
├── digit.cpp/.h          # Custom Digital Selector widget (for timer)
├── first.cpp/.h          # Smart Light Control page
├── fourth.cpp/.h         # Timer page
├── fifth.cpp/.h          # Curtain & Door Control page
├── eighth.cpp/.h         # Serial function page
├── glowtext.cpp/.h       # Custom Glow Text widget (for environmental data values)
├── graph.cpp/.h          # Arc Graph widget implementation
├── main.cpp              # Project entry
├── mainwindow.cpp/.h     # Main page (function entry)
├── nine.cpp/.h           # Appliance Status Monitoring page
├── second.cpp/.h         # Doorbell (Buzzer) page
├── seventh.cpp/.h        # Environmental Sensing Hub page
├── six.cpp/.h            # Resident Information Registration page
├── switchbutton.cpp/.h   # Switch button implementation
├── text.cpp/.h           # Glow Text implementation
├── third.cpp/.h          # Home Audio page
├── top.cpp/.h            # Custom page header widget
├── photos/               # Directory for resident photos and UI image resources
├── myMusic/              # Directory for audio playback MP3 files
├── styles.qss/.qss       # Qt Style Sheet (UI beautification)
├── CMakeLists.txt        # CMake compilation configuration
└── README.md             # Project documentation
```

## Compilation & Running

### 1. Environment Preparation

#### Host Side (Ubuntu 20.04)

- Install cross-compilation toolchain: `sudo apt-get install gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf`

- Install Qt 5.14.2: Download [Qt 5.14.2 for Linux](https://download.qt.io/archive/qt/5.14/5.14.2/), select the `arm-linux-gnueabihf` compilation kit during installation

- Install dependencies: `sudo apt-get install libsqlite3-dev qt5multimedia5-dev`

#### Target Board (iMX6ULL)

- Flash Linux 4.1.15 system to the development board, enable GPIO/I2C/UART peripherals

- Copy Qt 5.14.2 ARM runtime libraries to the board's `/usr/lib/` directory

- Install SQLite runtime: `opkg install libsqlite3-0`

### 2. Compilation Methods

#### Method 1: Qt Creator Cross-Compilation

1. Open Qt Creator, import the `CMakeLists.txt` in the project root directory

2. Select the compilation kit as `arm-linux-gnueabihf-qt5`

3. Click **Build** to generate the ARM architecture executable file `SmartHome`

#### Method 2: Command Line Cross-Compilation

```bash

# Enter project root directory
cd SmartHome_System
# Create build directory
mkdir build && cd build
# CMake configuration for cross-compilation
cmake .. -DCMAKE_C_COMPILER=arm-linux-gnueabihf-gcc -DCMAKE_CXX_COMPILER=arm-linux-gnueabihf-g++ -DQT5_DIR=/path/to/qt5-arm/lib/cmake/Qt5
# Compile
make -j4
```

### 3. Running Steps

1. Copy the compiled executable `SmartHome` to the development board's `/home/root/` directory

2. Copy the `photos/` and `myMusic/` directories from the project to the board's `/home/root/`, ensure the MP3 files and photo resource paths are correct

3. Grant execution permission on the target board: `chmod +x SmartHome`

4. Run the program: `./SmartHome`

5. Connect the HDMI display and USB mouse/keyboard, then you can operate the UI interface

## Key Technical Points

1. **Qt Page Switching**: Based on `QMainWindow` and custom `back()` signal, the main page initializes all functional page instances. Page hide/show is implemented via Signal-Slot, and status is saved through class member variables.

2. **Hardware Operations**: Peripherals exist as files in Linux system. Use `open/read/write/close` to operate GPIO/I2C/UART, for example, buzzer control operates `/sys/devices/platform/leds/leds/beep/brightness`.

3. **Multi-thread Design**: Time-consuming operations such as appliance monitoring and environmental data collection are placed in independent Qt threads. Communicate with the UI main thread via Signal-Slot to avoid interface freezes.

4. **Custom Qt Widgets**: Implement Arc Graph, Glow Text, Digital Selector and other widgets based on `QPainter`, adapting to the lightweight display requirements of embedded devices.

5. **SQLite Database**: Implement persistent storage of resident information and timing tasks. Database operations and UI table linkage are realized via `QSqlDatabase`/`QSqlQueryModel`.

6. **UI Beautification**: Customize control styles, layout and interaction effects through Qt Style Sheet (QSS), adapting to the resolution of embedded displays.

## Function Demo

### Main Page

9 function buttons correspond to 9 modules, click to jump to the corresponding functional page. The page also displays developer information and project logo.

### Environmental Sensing Hub

The AP3216C sensor collects data in real time. Three arc graphs display data proportion, and glow text shows specific values, with automatic refresh every 500ms.

### Home Appliance Status Monitoring

Click **Start Monitoring** to start multi-thread polling, working appliances are highlighted in real time. Click **Stop Monitoring** to terminate polling and reset status.

### Resident Information Registration

The table displays resident information, the corresponding photo is displayed in real time when selecting a list item. Support adding/modifying information and synchronizing with the database.

## Future Improvements & Extensions

1. **Remote Control**: Add WiFi/Bluetooth modules (such as ESP8266), develop mobile APP to realize remote control and status viewing of smart home.

2. **Scene Linkage**: Implement automated scenarios based on environmental data, such as automatically turning on lights when ambient light is too dark, or opening curtains when human proximity is detected.

3. **Data Visualization**: Add QChart widgets to realize trend chart display of environmental data and appliance usage duration.

4. **Touch Interaction**: Adapt to touch screen, optimize control size and click area, add touch vibration feedback.

5. **Power Optimization**: Implement low-power mode for peripherals, turn off sensors and display when idle to reduce development board power consumption.

6. **More Peripherals**: Extend with temperature and humidity sensors (DHT11/DHT22), smoke sensors, relay control and other peripherals to enrich smart home functions.

## Notes

1. Ensure correct peripheral wiring when running on the development board, no conflict in GPIO pin allocation (the project uses static allocation, no interface multiplexing).

2. When using the Home Audio module, please put MP3 files into the `myMusic/` directory, ensure the files are in MP3 format with UTF-8 encoding.

3. The environmental sensing module only works on the iMX6ULL board (detected via the `#if __arm__` macro). No sensor data will be collected during desktop debugging.

4. Ensure the development board serial port is not occupied by other programs before using the serial module, pay attention to cross-connection of TX/RX when wiring.

5. Ensure the Qt cross-compilation kit path is correct during compilation, otherwise compilation errors or runtime failures on the target board may occur.

## License

This project is a course design work, for learning and communication purposes only. Commercial use is prohibited.

## Acknowledgements

Thanks to NXP official for providing iMX6ULL development materials. Thanks to the Qt community and embedded open source community for technical support.
