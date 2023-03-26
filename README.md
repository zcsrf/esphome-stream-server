# Pylontech BMS console server for ESPHome

Extension of [Oxan van Leeuwen's](https://github.com/oxan) generic stream server customized for exposing Pylontech BMS console over WIFI.

To allow remote connection to the BMS console, the UART connection has to be first set up with baud rate 1200, then a magic console wake-up string has to be entered and then the UART can be switched to target baude rate of 115200. (See the pylontech_setup() method, which is actually the only change to upstream Stream Server code.)

Original stream server's README is below.

#

# Stream server for ESPHome

Custom component for ESPHome to expose a UART stream over WiFi or Ethernet. Provides a serial-to-wifi bridge as known
from ESPLink or ser2net, using ESPHome.

This component creates a TCP server listening on port 6638 (by default), and relays all data between the connected
clients and the serial port. It doesn't support any control sequences, telnet options or RFC 2217, just raw data.

## Usage

Requires ESPHome v2022.3.0 or newer.

```yaml
external_components:
  - source: github://oxan/esphome-stream-server

stream_server:
```

You can set the UART ID and port to be used under the `stream_server` component.

```yaml
uart:
  id: uart_bus
  # add further configuration for the UART here

stream_server:
  uart_id: uart_bus
  port: 1234
```

## Sensors

The server provides a binary sensor that signals whether there currently is a client connected:

```yaml
binary_sensor:
  - platform: stream_server
    connected:
      name: Connected
```

It also provides a numeric sensor that indicates the number of connected clients:

```yaml
sensor:
  - platform: stream_server
    connection_count:
      name: Number of connections
```
