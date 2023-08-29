# Klatremis HW
## Indledning og formål
Chippen i klatremis modulet bruger en lidt anden hardware. Derfor er det nødvendigt at tilføje lidt til koden som 
normalt findes på github. https://github.com/klatremis/esphome-for-deye/tree/main

Den generiske kode + ændringerne kan findes i "config.yaml" filen.

## Ny opsætning
 Hvis du ikke allerede har en Esp32 kørende
 
* Tilgå din Home Assistant via. En HTTPS(nødvendig for at uploade via. Usb til enheden)
Opret en ny enhed i Esphome som ESP32, kald den noget relevant, skip installation.
* Klik Edit og kopier koden fra github config.yaml filen til enheden
* Sørg for at dit wifi ssid og password er angivet I “secret” I Esphome.
* Ret evt. ”name” til hvad den skal hedde i ESPhome.
* Ret evt. ”device_type” til hvad inverterens entitys skal have som præfix i Home Assistant.
* Installer via. USB fra ”this device. Vælg den rigtige com port.
* Når den er færdig tilslut ESPen til inverter.

# Eksisterende ESP32 installation
Hvis du allerede har en ESP32 kørende med modbus til inverteren og vil erstatte dette hardware med den nye.
Dette sletter kun entitys midlertidigt, men statestikken der ligger gemt i databasen bagved vil blive beholdt så længe 
man bruger samme sensor navn i den nye enhed. Man skriver derefter bare videre i databasen som intet er hændt.
* Tilgå din Home Assistant via. En HTTPS(nødvendig for at uploade via. Usb til enheden).
* Afmonter din gamle esp32 hardware fra inverteren.
* Gå til integrationer og enheder i HOME ASSISTANT, find din enhed i ESPhome, tryk de 3 dots og slet. (fjern ikke 
enheden i ESPhome!).
* Tilføj de nye modbus indstillinger i din nuværende kode i ESPhome.(Se herunder)
* Plug in den nye enhed og installer koden.
* Tilslut til inverteren
* Genstart Home Assistant, gå til ”integrationer og enheder” og tilføj den til home assistant ved at trykke 
konfigurer på enheden. Den skulle selv gerne dukke op igen.
Herved bruges samme navne som den gamle enhed og den nye er blot en erstatning. Statistik osv. brugt i energy 
boardet bør forsætte uberørt.

## Tilføjelser i forhold til generisk esp32 og ttl modul.
```
esphome:
  name: ${name}
  on_boot: 
    - priority: 600
      then: 
      - output.turn_on: pin16_high
      - output.turn_on: pin17_high
      - output.turn_on: pin19_high

output:
  - platform: gpio
    pin: 16
    id: pin16_high
    
  - platform: gpio
    pin: 17
    id: pin17_high
    
  - platform: gpio
    pin: 19
    id: pin19_high

uart:
  id: mod_bus
  rx_pin: 21
  tx_pin: 22
  baud_rate: 9600
  stop_bits: 1
```
Eksempel 
Princippet ses herunder hvordan det skal opstilles. Dertil vedhæfter jeg github filen modificeret med tilføjelserne
![image](https://github.com/klatremis/hw/assets/22115157/2ee8a79d-8b8c-47cc-ad43-56c0512be632)

