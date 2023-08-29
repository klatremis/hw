# Klatremis HW
![image](https://github.com/klatremis/hw/assets/22115157/144229b1-1ca8-4c09-a4f9-ac1b1156f769)


## Indledning og formål
Chippen i klatremis modulet bruger en lidt anden hardware. Derfor er det nødvendigt at tilføje lidt til koden som 
normalt findes på github. https://github.com/klatremis/esphome-for-deye/tree/main

Den generiske kode + ændringerne kan findes i "config.yaml" filen.

## Ny opsætning
 Hvis du ikke allerede har en Esp32 kørende
 
* Tilslut enheden til pc
* Gå til https://web.esphome.io/ og connect til enheden
![image](https://github.com/klatremis/hw/assets/22115157/e8acec6a-e01f-4af3-b4e4-2b9b652ef946)
Installer BIN filen
![image](https://github.com/klatremis/hw/assets/22115157/3cca71cc-a593-4170-b9bc-742955cc2a95)
Når den er installeret, tryk på de 3 ... til højre og ændre wifi ssid og password  til dit.
Nu bør den komme frem i ESPhome hvor den kan "adoptes", skip installation. Nu downloades config til enheden og den er klar til brug.
![image](https://github.com/klatremis/hw/assets/22115157/89434773-006c-4cb6-a1c2-1c25f670147a)

# Eksisterende ESP32 installation
Hvis du allerede har en ESP32 kørende med modbus til inverteren og vil erstatte dette hardware med den nye.
Dette sletter kun entitys midlertidigt, men statestikken der ligger gemt i databasen bagved vil blive beholdt så længe 
man bruger samme sensor navn i den nye enhed. Man skriver derefter bare videre i databasen som intet er hændt.
* Tilgå din Home Assistant via. En HTTPS(nødvendig for at uploade via. Usb til enheden).
* Afmonter dit gamle esp32 hardware
* Gå til integrationer og enheder i HOME ASSISTANT, find din enhed, tryk de 3 dots og slet. (fjern ikke 
enheden i ESPhome!).
* Tilføj de nye modbus indstillinger i din nuværende kode i ESPhome.(Se herunder)
* Plug in den nye enhed og installer koden via usb.
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
Princippet ses herunder hvordan det skal opstilles.
![image](https://github.com/klatremis/hw/assets/22115157/2ee8a79d-8b8c-47cc-ad43-56c0512be632)

