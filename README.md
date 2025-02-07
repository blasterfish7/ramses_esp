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