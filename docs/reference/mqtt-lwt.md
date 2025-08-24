# LWT (Last Will and Testament) v MQTT

LWT je mechanismus MQTT, který umožňuje brokeru automaticky informovat ostatní klienty o nečekaném odpojení zařízení. Typicky se používá pro signalizaci stavu zařízení (ONLINE/OFFLINE).

- Klient při připojení nastaví LWT zprávu (např. OFFLINE) na konkrétní topic.
- Pokud klient vypadne, broker tuto zprávu publikuje.

Podrobné návody a příklady najdeš v:
- [How-to: MQTT namespace](../how-to/mqtt-namespace.md)
- Projektové příklady (např. ESP32, Node-RED)

---

*Pro detailní implementaci a doporučené postupy viz odkazy výše.*

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
