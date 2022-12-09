# Python based lighting example (bridge) device to Tapo L530.

## Dependencies

Ubuntu 22.04:

```
sudo apt install git gcc g++ libdbus-1-dev \
  ninja-build python3-venv python3-dev \
  python3-pip libgirepository1.0-dev libcairo2-dev
# maybe:
# sudo apt install pkg-config libssl-dev libglib2.0-dev libavahi-client-dev libreadline-dev
```

Install the bridge app dependencies:

```
sudo apt install alsa mpg321
```

## Installation

Build the Python/C library:

```shell
cd ~/connectedhomeip/
scripts/checkout_submodules.py --shallow --platform linux
source scripts/activate.sh

./scripts/build_python_device.sh --chip_detail_logging true

source ./out/python_env/bin/activate
```

Install the Python dependencies inside the virtual env:

```shell
pip install -r requirements.txt
```

## Usage

Copy and update the config file:

```shell
cp config.json.example config.json
nano config.json
```

Run the Python lighting matter device:

```shell
cd examples/lighting-app/tapo-bridge
python lighting.py
```

### Control with Chip Tool

Commissioning:

```bash
chip-tool pairing ethernet 110 20202021 3840 192.168.1.111 5540
```

where:

-   `110` is the assigned node id
-   `20202021` is the pin code for the bridge app
-   `3840` is the discriminator id
-   `192.168.1.111` is the IP address of the host for the bridge
-   `5540` the the port for the bridge

Alternatively, to commission with discovery which works with DNS-SD:

```bash
chip-tool pairing onnetwork 110 20202021
```

Switching on/off:

```bash
chip-tool onoff on 110 1
chip-tool onoff off 110 1
chip-tool onoff toggle 110 1
```
