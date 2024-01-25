# esphome-no-ha
An attempt to docker compose for ESPHome, without HA

For services' ports see docker-compose.yml

http://localhost:8443 - vscode
http://localhost:6052 - esphome
http://localhost:9090 - prometheus (reads esp32 data)
http://localhost:3000 - grafana (visualizes prometheus data)

ESPs need to have this properites in their yaml files:
```
# Example configuration entry
web_server:
  
# Activates prometheus /metrics endpoint
prometheus:
```