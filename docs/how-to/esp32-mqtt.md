# How-to — ESP32 → MQTT (mini-sketch)

**Status:** stable  
**Slug:** how-to/esp32-mqtt

---

## První funkční publikační sketch pro ESP32 → Mosquitto s LWT a základní konvencí topiců

### Konvence topiců (minimum)
```
v1/<org>/<site>/<device>/<channel>/<metric>
# Příklad
ev1/home/lab/esp32-01/tele/temperature
```
- QoS: 0 (start)
- retain: jen pro „poslední známý stav“

**LWT:**
- `v1/<...>/meta/status` → OFFLINE (retained), při připojení publikuj ONLINE (retained).

---

## Knihovny
- `WiFi.h`
- `PubSubClient` (Arduino Library Manager)

---

## Konfigurace (nahraď podle sebe)
```cpp
#include <WiFi.h>
#include <PubSubClient.h>

// --- Wi‑Fi & MQTT ---
const char* WIFI_SSID     = "<ssid>";
const char* WIFI_PASS     = "<pass>";
const char* MQTT_HOST     = "<broker-ip-nebo-hostname>"; // např. 192.168.1.100 nebo "raspberrypi"
const uint16_t MQTT_PORT  = 1883;
const char* CLIENT_ID     = "esp32-01"; // unikátní

// --- Namespace ---
const char* ORG   = "home";   // org/site/device jen příklad
const char* SITE  = "lab";
const char* DEV   = "esp32-01";

String tTele(const char* metric){ return String("v1/")+ORG+"/"+SITE+"/"+DEV+"/tele/"+metric; }
String tMeta(const char* key){ return String("v1/")+ORG+"/"+SITE+"/"+DEV+"/meta/"+key; }

WiFiClient wifi;
PubSubClient mqtt(wifi);

unsigned long lastPub = 0;

void connectWifi(){
	WiFi.mode(WIFI_STA);
	WiFi.begin(WIFI_SSID, WIFI_PASS);
	while(WiFi.status()!=WL_CONNECTED) { delay(400); }
}

void ensureMqtt(){
	while(!mqtt.connected()){
		// LWT → OFFLINE (retained)
		String willTopic = tMeta("status");
		const char* willMsg = "OFFLINE";
		mqtt.setServer(MQTT_HOST, MQTT_PORT);
		if(mqtt.connect(CLIENT_ID, NULL, NULL, willTopic.c_str(), 0, true, willMsg)){
			// ONLINE (retained)
			mqtt.publish(willTopic.c_str(), "ONLINE", true);
		} else {
			delay(1000);
		}
	}
}

void setup(){
	connectWifi();
	mqtt.setKeepAlive(60);
	ensureMqtt();
}

void loop(){
	if(WiFi.status()!=WL_CONNECTED) connectWifi();
	if(!mqtt.connected()) ensureMqtt();
	mqtt.loop();

	unsigned long now = millis();
	if(now - lastPub > 5000){
		lastPub = now;
		// Dummy „teplota“ (nahraď reálným čidlem)
		float value = 20.0 + (now % 1000)/100.0;
		char buf[32];
		dtostrf(value, 0, 2, buf);
		String topic = tTele("temperature");
		mqtt.publish(topic.c_str(), buf, false); // QoS 0, retain false
	}
}
```

---

## Ověření
- MQTT Explorer: subscribe na `v1/#` nebo přesně na `v1/home/lab/esp32-01/tele/temperature`.
- Po resetu ESP32: na .../meta/status uvidíš ONLINE; po odpojení (kill Wi‑Fi / vypni ESP) se objeví OFFLINE (retained).

---

## Nejčastější potíže
- Špatná IP/hostname brokera → ESP32 nikdy connected().
- Duplikované CLIENT_ID → broker dropne jedno z připojení.
- Nestabilní Wi‑Fi → prodluž keepalive v Mosquittu i v klientovi.
# How-to: ESP32 → MQTT

Návod na odesílání dat ze senzoru ESP32 do MQTT brokeru.

## Předpoklady
- ESP32 s firmwarem (např. ESPHome, MicroPython)
- MQTT broker

## Postup
1. Nakonfiguruj ESP32 pro připojení k WiFi a MQTT brokeru.
2. Odesílej data (např. teplotu) do tématu `test/sensor/temperature`.

## Ověření
- Data z ESP32 jsou vidět v MQTT Exploreru
