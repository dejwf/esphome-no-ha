# esphome-no-ha
An attempt to docker compose for ESPHome, without HA

For services' ports see docker-compose.yml
# Deployed web UIs

http://localhost:6052 - esphome
http://localhost:1880 - node-red
http://localhost:1880/ui/ - node-red-dashboard

## Mqtt broker (server)
for whatever reason, naming convention uses `broker` terminology, even though
it is technically a server :shrug:

localhost:1883 (has no webUi)
If you'd like to interact with mqtt, either use node-red
or use something like https://hivemq.github.io/mqtt-cli/

## To make ESPs to publish into mqtt, add following into device's yaml
### At 1st, outcomment the HA API - it clashes with some non-HA's mqtt
```
# Enable Home Assistant API
# api:
#   encryption:
#     key: "RpO/FdUbqrPATNrWeO01SN7IO+8nErSVAQuRrZe9JRA="

```

### Set up the broker (server) connection
```
mqtt:
  broker: 192.168.0.74
  # if you're using my docker, do not specify the user/pwd
  # username:
  # password:
  client_id: mqtt_test_esp32 #name of your ESP as it appears in the mqtt
  birth_message:
    topic: demo
    payload: online_and_kicking
  will_message:
    topic: demo
    payload: offline_and_leaving
```

### Subscribe (listen) to a topic
```
sensor:
  - platform: mqtt_subscribe
    name: "Data from topic"
    id: mysensor
    topic: my/great/topic
```

### Publish (send) to a topic on value change
```
  - platform: copy # Reports the WiFi signal strength in %
	...
	on_value:
      - mqtt.publish_json:
          topic: demo
		  # send json with both values (dB & %)
          payload: |-
            root["wifi_signal_percentage"] = id(wifi_signal_percentage).state;
            root["wifi_signal_db"] = id(wifi_signal_db).state;
```