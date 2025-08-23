# Last Will & Testament (LWT) v MQTT

LWT je mechanismus, jak automaticky signalizovat, že zařízení (klient) nečekaně vypadlo z provozu.

## Jak to funguje
- Při připojení klienta (např. ESP32) nastavíš LWT na brokeru.
- Pokud klient „umře“ (vypadne napájení, Wi-Fi, crash), broker automaticky publikuje zprávu na zvolený topic.

## Doporučený postup
- **Topic:** `v1/home/lab/esp32-01/meta/status`
- **Payload:** `OFFLINE`
- **Retain:** `true` (ať to uvidí i noví subscribeři)
- **QoS:** `1` (doporučeno)

A hned po úspěšném připojení pošli tzv. birth message:
- `ONLINE` na stejný topic, také retained.

Tím máš trvale k dispozici aktuální „stav života“ zařízení.

---

## Jak to zapnout (ESP32 + PubSubClient)
```cpp
#include <PubSubClient.h>

WiFiClient wifi;
PubSubClient client(wifi);

const char* mqttHost = "192.168.1.10";
const int   mqttPort = 1883;

const char* willTopic   = "v1/home/lab/esp32-01/meta/status";
const char* willPayload = "OFFLINE";
const bool  willRetain  = true;
const int   willQoS     = 1;

void setup() {
    // ... Wi‑Fi connect ...
    client.setServer(mqttHost, mqttPort);

    String clientId = "esp32-01";

    // LWT se nastavuje při connectu:
    // connect(clientId, willTopic, willQoS, willRetain, willMessage)
    if (client.connect(clientId.c_str(), willTopic, willQoS, willRetain, willPayload)) {
        // Birth message – jsme online (retained)
        client.publish(willTopic, "ONLINE", true);  // retain = true
    }
}

void loop() {
    if (!client.connected()) {
        // reconnect + znovu poslat birth po připojení
    }
    client.loop();
}
```

**Co se stane:** když se ESP32 „ztratí“ (vypadne napájení, Wi‑Fi, crash) a nestihne poslat OFFLINE, broker sám po timeoutu pošle retained OFFLINE na ten topic. Každý nový subscriber uvidí poslední stav (ONLINE/OFFLINE) okamžitě.

### Tipy a zvyky
- Graceful vypnutí (třeba před OTA nebo restartem): klidně publishni OFFLINE (retained) sám a pak disconnect().
- Keepalive ~60 s je rozumné; kratší = rychlejší detekce pádu, ale více provozu.
- Jeden LWT na klienta; drž „stav“ na jednom jasném topicu (.../meta/status).
- V Node‑RED si sleduj `v1/home/lab/+/meta/status` a vykresli health přehled.
