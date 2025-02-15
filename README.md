# docker-tarDRONE

I made this repo to dockerize scripts for drone RemoteID sniffing. See [zmqToTar1090](https://github.com/l0g-lab/zmqToTar1090) for more information.

** There has been an update to the Sniffle project which requires manually setting the baud rate for the sniff_receiver.py script. See the following from the Sniffle project [README](https://github.com/bkerler/Sniffle/blob/zmq/README.md)**
```
In 2022, due to COVID-19 pandemic chip shortages, some Sonoff CC2652P dongles were built with CP2102 (non-N) USB/UART bridge chips that are capped at 921600 baud. If you have one of these, you will need to flash a different firmware image that uses a slower baud rate of 921600. This special slower baud rate build is named sniffle_cc1352p1_cc2652p1_1M.hex (build variant CC2652P1F_1M). You will also need to invoke Sniffle utilities with the option -b 921600 to override the default baud rate of 2000000.
```

## Overview

First off, huge thanks to the following devs for their work in this realm:
- Tar1090 Dev: [wiedehopf](https://github.com/wiedehopf)
- DragonOS Dev: [alphafox02](https://github.com/alphafox02) - [YouTube](https://www.youtube.com/@cemaxecuter7783)
- Sniffle Dev: [nccgroup](https://github.com/nccgroup)

This project provides a dockerized way to track drones via RemoteID (FAA required on drones over 249 grams). It will keep a cache of drones in the local area that the BLE dongle can receive signals from and map them on tar1090. To accomplish this, it uses the following:

- a modified instance of [tar1090](https://github.com/wiedehopf/tar1090)
- [Sniffle](https://github.com/bkerler/Sniffle/tree/zmq): project for sniffing the BLE packets
- [DroneID](https://github.com/bkerler/DroneID) for decoding the BLE traffic
- [zmqToTar1090](https://github.com/l0g-lab/zmqToTar1090) for converting decoded data into specific JSON format for ingestion into tar1090

![drone_track](/images/output.gif)

## Requirements

- Docker with BuildKit enabled
- BLE5 dongle (I use a Sonoff Zigbee 3.0 with custom firmware from [nccgroup](https://github.com/nccgroup/Sniffle))
- Drone >249 grams or a RemoteID transmitter

## Installation

1. Clone this repo to your local machine:\
    `$ git clone https://github.com/l0g-lab/docker-tarDRONE`
2. Change directory into the project:\
    `$ cd docker-tarDRONE`
3. Modify the `.env` file within the project root directory with:\
    `TZ`: Your current TimeZone\
    `LAT/LONG`: The latitude & longitude of your current location or the RemoteID receiver\
    `USB_DEVICE`: The device of your sniffle receiver dongle (normally `/dev/ttyUSB0` if you have no other devices connected)

## How to use

1. Run the project with:\
    `$ DOCKER_BUILDKIT=1 docker-compose -f docker-compose-tarDRONE.yml up`

    This will take some time while the containers are downloaded and built.

2. Open your web browser to: `http://localhost:8078`

## License

```
MIT License

© 2024 l0g

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
