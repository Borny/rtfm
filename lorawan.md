# LoRaWAN

- [LoRaWAN Overview](#lorawan-overview)
- [LoRa vs LoRaWAN](#lora-vs-lorawan)
- [Protocols used in IoT](#protocols-used-in-iot)
- [Frequencies](#frequencies)
- [Spred spectrum](#spread-spectrum)
- [Downlinks and Uplinks](#downlinks-and-uplinks)
- [Embedded Systems](#embedded-systems)
- [IoT](#iot)
- [MQTT](#mqtt)
- [Radio transmission and propagation](#radio-transmission-and-propagation)
- [LoRa Modulation](#lora-modulation)
- [LoRaWAN Protocol](#lorawan-protocol)

## LoRaWAN Overview

LoRaWAN is a `Low Power Wide Area Network` (LPWAN) specification intended for wireless battery operated Things in a regional, national or global network.  
LoRaWAN is a `media access control` (MAC) layer protocol designed for large-scale public networks with a single operator.  
The LoRaWAN specification provides seamless interoperability among smart Things without the need of complex local installations and gives back the freedom to the user, developer, businesses enabling the roll out of Internet of Things.

LoRaWAN network architecture is typically laid out in a star-of-stars topology in which gateways is a transparent bridge relaying messages between end-devices and a central network server in the backend.

## LoRa vs LoRaWAN

`LoRa` (Long Range) is a proprietary wireless technology developed by Semtech Corporation. **(Modulation)**  
`LoRaWAN` is a network protocol on top of LoRa. **(Protocol)**  
`LoRaWAN` belongs to the **LP WAN (Low Power Wide Area Network)** family

## Protocols used in IoT

- `WiFi` => high bitrate / short range / high power
- `Bluetooth/BLE` => low bitrate / short range / low power
- `Zigbee` => low bitrate / short range / low power
- `Z-Wave` => low bitrate / short range / low power
- `Wireless M-Bus` => low bitrate / short range / low power
- `LoRa` => low bitrate / long range / low power
- `Sigfox` => low bitrate / long range / low power
- `NB-IoT` => low bitrate / long range / low power
- `LTE-M` => low bitrate / long range / low power
- `NFC` => low bitrate / low power / short range
- `RFID` => low bitrate / low power / short range

## Frequencies

- 13.56 MHz => NFC
- 433 MHz => LoRa
- 868 MHz => LoRa, Sigfox, NB-IoT `most used frequency in Europe for LoRaWAN`
- 915 MHz => LoRa
- 2.4 GHz => Zigbee, BLE, WiFi
- 5 GHz => WiFi

For each message the device changes the freaquency.

## Spread spectrum

LoRa uses `8 channels` to spread the signal over a wide frequency band (e.g: 868Mhz).  
They range from 867.1 to 869.5 MHz (increment of 200 kHz: 867.1, 867.3, etc).
A `channel` is made of the `frequency` + `the spreading factor`.  

The `last 3 frequencies` are used for the `join message`.

It also has `6 colors` per channel to avoid interference with other LoRa devices which represents the `spreading factor`.

## Downlinks and Uplinks

- `Downlink`: from the gateway to the device
- `Uplink`: from the device to the gateway

**Only one `channel` is used for the downlink**.

## Embedded Systems

An `embedded system` is a computer system with a dedicated function within a larger mechanical or electrical system, often with real-time computing constraints.  
Embedded systems are the heart of IoT. They are the `devices` that collect data, process it and send it to the cloud.

## IoT

The `Internet of Things` (IoT) is a system of interrelated computing devices, mechanical and digital machines, objects, animals or people that are provided with unique identifiers and the ability to transfer data over a network without requiring human-to-human or human-to
computer interaction.

## Radio transmission and propagation

### Decibel (dB)

- `dBm` is a unit of power relative to 1mW
- `dB` is a unit of power relative to 1W

=> In Europe the dBm limit is 14.0 dBm.

`Pt` = Power transmitted  
`Pr` = Power received  
`Pn` = Noise power  
`Gt` = Gain of the transmitting antenna  
`Gr` = Gain of the receiving antenna  
`L` = Losses in the system

`Pr = Pt * Gt * Gr / L`

`RSSI` (Received Signal Strength Indicator) is a measurement of the power present in a received radio signal. (dBm)  
`Sensitivity` is the minimum power level at which the receiver can detect a signal. (dBm)  
`SNR` (Signal to Noise Ratio) is the ratio of the power of the signal to the power of the noise. (dB) => `SNR = RSSI(Pr) - Pn`

The RSSI **must be higher** than the sensitivity of the receiver to be able to receive the signal.

`Link budget` = Pt - sensitivity (dBm)

## LoRa Modulation

The higher the spreading factor, the longer the transmission time and the longer the range. But the lower the bitrate.

The spread factors go from 7 to 12.

Spread factor 7 => 125 kHz => 5469 bps  
Spread factor 12 => 125 kHz => 293 bps

A device will start with the highest spreading factor and will decrease it until the signal is enough for the gateway.

## LoRaWAN Protocol

### Flow

**Device => Gateway => Network server => Application server** (and the other way around).

The LoRaWAN frame is composed of:

- User payload
- Security information

### Security

- **Authenticity**: only allowed end-devices can send messages to the Network Server
- **Integrity**: Nobody can change the transmitted frame/data
- **Confidentiality**: Nobody can read the transmitted frame/data

The `authenticiy` and `confidentiality` are ensured by the `Device EUI` and the `Application EUI` => Network Session Key (`NwkSKey`), used for Authentication

The `integrity` is ensured by the `Message Integrity Code (MIC)`. => Application Session Key (AppSKey) => Encrypts the user payload

The Network Server stores the NwkSKey.
The Application Server stores the AppSKey and the DevAddr.

## End-device classes

`TIME ON AIR`: The time the device is transmitting or `Packet transmission time`.

When an uplink is made the device opens two receive windows (or time slots) called `Rx1` and `Rx2`.

`For Rx1` a downlink will be sent on the same channel as the uplink. e.g: SF9 - 867.5MHz - 14dBm  
`For Rx2` a downlink will be sent on a boosted channel: SF12 - 869.525MHz - 27dBm. e.g: In moshi, there are only 20 meters, so all of them use Rx2.

- **Class A**: Lowest power consumption, but the least flexible. It can receive a downlink message only after an uplink transmission.
- **Class B**: Adds scheduled receive windows at specific times. A `Time beacon` is sent by the gateway for the device to open the receive window and accept a downlink message.
- **Class C**: The most power-consuming, but the most flexible. The device is always listening, except when transmitting therefore they is no specific time slots.

## Activation modes

- **`OTAA` (Over The Air Activation)**: The device sends a join request to the `join server` then the join serverFadr sends a join accept to the device with the `NwkSKey` and the `AppSKey`. The device sends the following information: `DevEUI, AppEUI/JoinEUI`.  
<!-- TEMP: complete this information -->

- **`ABP` (Activation By Personalization)**: The device is pre-configured with the NwkSKey and the AppSKey.

## Network Server

The Network Server will drop duplicate messages.

## Application Server

The `DevAddr` is a 4-byte address used to identify the device. It is assigned by the network server during the join procedure. The `DevAddr` is used by the network server to identify the device and route the messages to the correct application server.

Difference between Devadrr and DevEUI ?

The `DevEUI` is a 64-bit unique identifier for the device. It is assigned by the device manufacturer and is used to identify the device on the network. The `DevEUI` is used by the network server to authenticate the device during the join procedure.

- DevEUI: Device EUI, unique identifier
- DevAdrr: Device Address, used to identify the device
- NwkSKey: Network Session Key, used for Authentication
- AppSKey: Application Session Key, used to encrypt the user payload

## Increase the network coverage

- Increase the power of the device (<14dBm) (but it will consume more energy)
- Increase the spreading factor (but it will decrease the bitrate)

The term `bitrate` refers to the number of bits that are processed or transmitted in a given amount of time, typically measured in bits per second (bps).  
In the context of LoRaWAN, which is a low-power wide-area networking protocol designed for Internet of Things (IoT) devices, bitrate is a crucial factor because it directly impacts both the range and power consumption of the communication.

## Adaptive Data rate

`ADR` optimizes the Spreading Factor and the Transmission Power of the device.  
The `ADR` needs to be enabled on the LoRa device, otherwise the Network Server will not make an optimization request and the device will always transmit data with the same data rate.  
If 16 messages are not received by the gateway then the device will increase the spreading factor by 1.

## Networks

### Types of networks

- `Private network`: The end device, the gateway and the servers are managed by the company.
- `Public network`: The end device is managed by the company and the gateway and the servers are managed by a network provider.
- `Hybrid network`: The end device and the gateway is managed by the company and the servers are managed by a network provider.

### Configuration

#### Gateway setup

Gateway configuration (_on the gateway_):

- `Frequency`: 868 MHz
- `Spreading factor`: 7 to 12
- `Bandwidth`: 125 kHz
- `Coding rate`: 4/5
- `Tx power`: 14 dBm

1. Install a `Packet Forwarder` on the gateway to forward the packets to the network server. (e.g: Semtech UDP Packet Forwarder)
2. Set the network server IP address
3. Set the UDP port (uplink and downlink ports)

Gateway registration (_on the server_):

- `Gateway EUI`: Unique identifier, usually written on the gateway
- `Gateway ID or name`: Unique identifier, make up an id
- `Region`: frequency plan
- `Gateway Key`: Used to authenticate the gateway

#### Device setup

Device registration (on the _server_):

For `ADB` mode:

- `Frequency plan`: 868 MHz
- `LoRaWan version`
- `Activation mode`: OTAA or ABP
- `Device EUI`: Unique identifier
- `Device Address`: Unique identifier
- `AppSKey`: Application Session Key
- `NwkSKey`: Network Session Key
- `Device ID`: Device ID

For `OTAA` mode:

- `Frequency plan`: 868 MHz
- `LoRaWan version`
- `Activation mode`: OTAA or ABP
- `DevEUI`: Unique identifier
- `AppKey`: Unique identifier

Device configuration (on the device)

For `ABP` mode:

- `DevAdrr`
- `nwkSKey`
- `appSKey`

For `OTAA` mode:

- `appEUI`
- `appKey`

## The LoRaWAN frame

LoRaWAN OSI model:

- `Physical layer` (PHY Layer): Reponsible for the LoRa modulation, Spreading factor, Bandwidth, Coding rate
- `MAC Layer`: LoRaWAN
- `App Layer`: User data

### LoRa frame

- `Preamble`: Synchronization
- `Header`: Length of the frame
- `PHY Payload`: User data
- `CRC`: Cyclic Redundancy Check

The `Preamble` is used to synchronize the receiver with the transmitter. It is composed of a series of alternating 0s and 1s.

The `Header` contains the length of the frame.

The `Payload` contains the user data.

### The LoRaWAN Frame

- `MHDR`: MAC Header
- `MAC Payload`: MAC Payload
- `MIC`: Message Integrity Code

```bash
PHYPayload
  ├── MHDR (MAC Header)
  ├── MACPayload
  │   ├── FHDR (Frame Header)
  │   │   ├── DevAddr (Device Address)
  │   │   ├── FCtrl (Frame Control)
  │   │   ├── FCnt (Frame Counter)
  │   │   ├── FOpts (Frame Options)
  │   ├── FPort (Frame Port)
  │   ├── FRMPayload (Frame Payload)
  │       ├── User data (encrypted, App Layer)
  ├── MIC (Message Integrity Code)
```

## Exporting data from the LoRaWAN server

LoRaWAN servers:

- `The Things Network`
- `Actility`
- `Loriot`
- `Chirpstack`
- `Wi6Labs`

### HTTP

#### GET request

The `IoT platform` will `send` a **GET request** to the server to get the data.  

Drawbacks of the HTTP GET rmethod:

- it is not a real-time protocol. It is a request-response protocol, which means that the client must send a request to the server to get the data
- HTTP is not suitable for real-time applications because it is not designed to handle a large number of requests at the same time
- A request will be made without a response from the server or with an empty response

#### POST request

The `LoRaWAN server` will send a **POST request** to the IoT platform to send the data.  
Then the IoT platform can in turn send a POST request to the LoRaWAN server to send an `order` or just an `acknowledgment` that the `message was received`.

Drawbacks of the HTTP POST rmethod:

- The message will be lost if the IoT server is down while the POST request is being sent
- If multiple IoT platforms are connected then all the configuration has to be made

### MQTT

MQTT (Message Queuing Telemetry Transport) is a lightweight messaging protocol that provides resource-constrained network clients with a simple way to distribute telemetry information in low-bandwidth environments.

Publisher > Broker > Subscriber

#### MQTT Subscriber

The `LoRaWAN server` will `publish` a `topic` on the `broker` to send the data.

#### MQTT Broker

The `IoT platform` will `subscribe` to a `topic` on the server to get the data.
