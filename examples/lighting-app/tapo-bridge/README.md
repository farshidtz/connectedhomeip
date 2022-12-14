# Tapo Matter Bridge - Party mode demo

## Dependencies

Assuming you have Ubuntu 22.04 and Python 3.10, install the following
dependencies:

```
sudo apt install git gcc g++ libdbus-1-dev \
  ninja-build python3-venv python3-dev \
  python3-pip libgirepository1.0-dev libcairo2-dev
# maybe:
# sudo apt install pkg-config libssl-dev libglib2.0-dev libavahi-client-dev libreadline-dev
```

Install the bridge app dependencies:

```
sudo apt install alsa mpg123
```

## Installation

Build the Python/C library:

```shell
cd ~/connectedhomeip/
scripts/checkout_submodules.py --shallow --platform linux
source scripts/activate.sh

./scripts/build_python_device.sh --chip_detail_logging true
```

Install the Python dependencies inside the virtual env:

```shell
source ./out/python_env/bin/activate
pip install -r requirements.txt
```

## Usage

Go to the bridge directory:

```shell
cd examples/lighting-app/tapo-bridge
```

Copy and update the config file:

```shell
cp config.json.example config.json
nano config.json
```

In the configuration file, the endpoint numbers 1 to 4 correspond to the matter
endpoints, which can be passed to the `chip-tool` to control the devices.

If the Python env isn't active, run the following:

```shell
source ../../../out/python_env/bin/activate
```

Run the Python lighting matter device:

```shell
python lighting.py
```

### Control with Chip Tool

Commissioning:

```bash
chip-tool pairing ethernet 110 20202021 3840 192.168.1.110 5540
```

where:

-   `110` is the assigned node id
-   `20202021` is the pin code for the bridge app
-   `3840` is the discriminator id
-   `192.168.1.110` is the IP address of the host for the bridge
-   `5540` the the port for the bridge

Alternatively, to commission with discovery which works with DNS-SD:

```bash
chip-tool pairing onnetwork 110 20202021
```

Switching on/off:

```bash
chip-tool onoff toggle 110 1 # toggle is stateless and recommended
chip-tool onoff on 110 1
chip-tool onoff off 110 1
```

where:

-   `onoff` is the matter cluster name
-   `on`/`off`/`toggle` is the command name. The `toggle` command is RECOMMENDED
    because it is stateless. The bridge does not synchronize the actual state of
    devices.
-   `110` is the node id of the bridge app assigned during the commissioning
-   `1` is the endpoint of the configured device
