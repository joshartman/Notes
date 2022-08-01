# Heltec LoRa ESP32 V2 under PlatformIO

## Register an application under the Application in TTN Console.

1. AppEUI = 00 00 00 00 00 00 00 00
2. DevEUI = auto generate  (this is in MSB format)
3. AppKey = auto generate  (this is in MSB format)

## PlatformIO

1. Install `MCCI LoRaWAN LMIC library` by IBM
2. Edit `lmic_pinmap` as follows:
   ```
   #define SPI_LORA_CLK 5
   #define SPI_LORA_MISO 19
   #define SPI_LORA_MOSI 27
   #define LORA_RST 14
   #define LORA_CS 18
   #define LORA_DIO_0 26
   #define LORA_DIO_1 35
   #define LORA_DIO_2 34
   
   // Pin mapping
   const lmic_pinmap lmic_pins = {
    .nss = LORA_CS,
    .rxtx = LMIC_UNUSED_PIN,
    .rst = LORA_RST,
    .dio = {LORA_DIO_0, LORA_DIO_1, LORA_DIO_2},
   };

   ```
4. Edit the file hal.c and hal.h where hal_init() function failed. I just renamed the function to hal_init2().
5. Edit the file `arduino-lmic/project_config/lmic_project_config.h` and define the correct radio type:
   ```
   #define CFG_eu868 1
   ```
   Mine is for Europe. Choose the correct one for your location.
7. pio run -t clean
8. pio run -t upload
