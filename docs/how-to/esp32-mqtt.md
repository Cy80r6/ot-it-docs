# ESP32 → MQTT

## Účel
Posílat data ze senzoru v ESP32 do MQTT brokeru.

## Předpoklady
- Funkční broker a topic (viz [Instalace Mosquitto](instalace-mosquitto.md))
- Znalost flashování ESP32 v PlatformIO
- Knihovna PubSubClient

## Kroky
1. Vytvoř nový projekt v PlatformIO pro ESP32.
2. Přidej knihovnu `PubSubClient`.
3. Vlož minimální kód:
    ```cpp
    #include <WiFi.h>
    #include <PubSubClient.h>
    const char* ssid = "YOUR_WIFI";
    const char* pass = "YOUR_PASS";
    const char* host = "192.168.50.10"; // IP brokeru
    WiFiClient espClient; 
    PubSubClient client(espClient);

    void reconnect() {
      while (!client.connected()) {
        client.connect("esp32-lab");
      }
    }

    void setup() {
      WiFi.begin(ssid, pass);
      while (WiFi.status()!=WL_CONNECTED) delay(500);
      client.setServer(host, 1883);
    }

    void loop() {
      if (!client.connected()) reconnect();
      client.loop();
      float t = 20.0 + random(-50,50)/10.0;
      String payload = String("{\"ts\":") + String((uint32_t)time(NULL)) + 
                       ",\"value\":" + String(t,1) + ",\"unit\":\"C\"}";
      client.publish("lab/esp32/telemetry/temperature", payload.c_str());
      delay(5000);
    }
    ```
4. Nahraj do ESP32 a sleduj sériový monitor pro připojení k Wi-Fi.

## Ověření
- MQTT Explorer vidí zprávy na `lab/esp32/telemetry/temperature`.
- Node-RED dashboard grafuje hodnoty.

## Rollback
- Flash prázdný sketch nebo odpoj zařízení.
