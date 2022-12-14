#base code with wifi manager and OTA update in captive portal with saving perams in json
#  
#  












#for esp8266 put this in platformio.ini


[platformio]
; point data_dir to nonexistent directory so that PIO doesn't bother building SPIFFS
data_dir = nonexistent

[env:d1_mini_lite_clone]
platform = espressif8266
upload_speed = 524288
monitor_speed = 115200
board = d1_mini
# Following is necessary for cheap Wemos D1 Lite clones.
# Without this line, flashing succeeds but programs simply don't run on the chip.
board_build.flash_mode = dout
framework = arduino
board_build.ldscript = eagle.flash.1m.ld

custom_nanopb_protos =
    +<proto/*.proto>
custom_nanopb_options =
    --error-on-unmatched

extra_scripts =
    pre:pio_tools/gen_data.py

build_flags =
  ; Disable global instances to save space
  -DNO_GLOBAL_INSTANCES
  ;-DDEBUG_ESP_PORT=Serial
  ;-DDEBUG_EEPROM_ROTATE_PORT=Serial
  ;-DDEBUG_ESP_CORE
  ;-DDEBUG_ESP_WIFI
  ;-DDEBUG_ESP_UPDATER
  ;-DDEBUG_ESP_PORT=Serial
  ;-DDEBUG_UPDATER=Serial


lib_deps =
    xoseperez/EEPROM_Rotate@^0.9.2
    ottowinter/ESPAsyncWebServer-esphome@^2.1.0
    nanopb/Nanopb@^0.4.6
    bblanchon/ArduinoJson@^6.19.4

[env:ota]
extends = env:d1_mini_lite_clone
extra_scripts =
    pre:pio_tools/gen_data.py
    pio_tools/platformio_upload.py
upload_url = http://owie-c024.lan/update
upload_protocol = custom
;board_build.gzip_fw = true

[env:native]
platform = native
