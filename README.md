# MQTT Ingester
MQTT Ingester is a lightweight Go service designed to subscribe to one or more MQTT topics, buffer incoming messages, and process them asynchronously through a worker pool.
The project provides a clean separation between configuration loading, MQTT communication, and message ingestion logic.

## Features
- Loads configuration from a YAML file, with automatic defaults and validation.
- Connects to an MQTT broker with configurable QoS and retry policies.
- Subscribes to multiple topics and pushes received messages into a buffered channel.
- Uses a worker pool to process incoming messages concurrently.
- Supports graceful shutdown:
  - stops message ingestion
  - drains and closes workers
  - disconnects from the MQTT broker
 
## Configuration
The service reads its configuration from config.yaml.
Example fields include:
- broker
- clientId
- qos
- connectionInterval
- maxRetry
- maxDelay
- topics

If the file is missing, a default one is generated automatically.

## Extending Ingestion Logic
You can add custom topic handlers inside internal/ingestion/ProcessMessage.
Each topic can route to a different processing function, database writer, or transformation pipeline.

```
switch msg.Topic {
case "sensors/temperature":
    handleTemperature(msg)
case "metrics/power":
    handlePower(msg)
}
```
