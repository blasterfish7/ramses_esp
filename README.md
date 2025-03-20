To get [printing working](http://esp32.io/viewtopic.php?t=38971):
I found out a way to work with ESP32-C6 USB.
I'm using usb_serial_jtag_driver_install() followed by "usb_serial_jtag_read_bytes()" and "usb_serial_jtag_write_and_flush()" to read and write. I set sdkconfig "Channel for console output" to UART0 and "Channel for secondary output" to USB/JTAG. With this settings I'm able to use USB to send/receive data I need to exchange with the PC and I can see logs (aka ESP_LOGx) on UART0.
I'm not shure this is a good way to do things and I'm opened to suggestions or ideas.

On the C6, SPI0 and SPI1 are used internally to access the ESP32-C6’s attached flash memory. But the documentation says that "SPI2 is a general purpose SPI controller. It has an independent signal bus with the same name. The bus has 6 CS lines to drive up to 6 SPI slaves."

How to use ESP32-C6 in platformIO is explained on [reddit](https://www.reddit.com/r/esp32/comments/1ananz5/esp32_c6_in_platformio_for_arduino_framework/). The *platformio.ini* file is already configured accordingly. However, you should also change the *boards\esp32-c6-devkitc-1.json* file to contain the following text:
```
{
  "build": {
    "core": "esp32",
    "f_cpu": "160000000L",
    "f_flash": "80000000L",
    "flash_mode": "qio",
    "mcu": "esp32c6",
    "variant": "esp32c6"
  },
  "connectivity": [
    "wifi"
  ],
  "frameworks": [
    "arduino",
    "espidf"
  ],
  "name": "Espressif ESP32-C6-DevKitC-1",
  "upload": {
    "flash_size": "8MB",
    "maximum_ram_size": 524288,
    "maximum_size": 8388608,
    "require_upload_port": true,
    "speed": 460800
  },
  "url": "https://docs.espressif.com/projects/espressif-esp-dev-kits/en/latest/esp32c6/esp32-c6-devkitc-1/index.html",
  "vendor": "Espressif"
}
```

```
{
  "build": {
    "core": "esp32",
    "f_cpu": "160000000L",
    "f_flash": "80000000L",
    "flash_mode": "qio",
    "mcu": "esp32c6",
	"variant": "esp32c6"
  },
  "connectivity": [
    "wifi"
  ],
  "frameworks": [
	"arduino",
    "espidf"
  ],
  "name": "Espressif ESP32-C6-DevKitC-1",
  "upload": {
    "flash_size": "4MB",
    "maximum_ram_size": 327680,
    "maximum_size": 4194304,
    "require_upload_port": true,
    "speed": 460800
  },
  "url": "https://docs.espressif.com/projects/espressif-esp-dev-kits/en/latest/esp32c6/esp32-c6-devkitc-1/index.html",
  "vendor": "Espressif"
}
```

PlatformIO Library Dependency Finder will find automatically dependent
libraries scanning project source files.

More information about PlatformIO Library Dependency Finder
- https://docs.platformio.org/page/librarymanager/ldf.html

You can use serial commands to change parameters of the ramses_esp, explained [here](https://github.com/IndaloTech/ramses_esp/wiki/Serial-Interface#mqtt-configuration)

To use mqtt: 
First
Fill out timezone: timezone CET-1CEST,M3.5.0/2,M10.5.0/3
SNTP configuration: sntp server pool.ntp.org
Then, mqtt details:
mqtt user mqtt_user
mqtt password secret
mqtt broker mqtt://192.168.253.251:1883

Next, in home assistant, you can enter the url of your broker:
    serial_port: "mqtt://mqtt_user:mqtt_p%40ss@homeassistant.local"  # password is 'mqtt_p@ss'
    serial_port: "mqtt://username:password@172.27.0.1:1883"
    serial_port: "mqtt://username:password@172.27.0.1" 

This was very tricky for me because I had multiple characters that need percent encoding in my password.

i added tzset();

Bugs in:
main -> platform function made all functionality seize.
cmd -> cmd_work, console didn't work nicely
ramses_sntp -> sntp_state_machine, state never reached.
In frame state machine, multiple break statements are missing.
In frame, added watch dog reset in fifo prime
in radio, added watch dog reset in task

Invoked the panic handler in menuconfig
set component config -> log -> log level to verbose

added esp_timer in requirements of frame



