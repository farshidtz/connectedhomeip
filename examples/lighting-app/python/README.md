# Python based lighting example (bridge) device to Tapo L530.

## Installation

Build the Python/C library:

```shell
cd ~/connectedhomeip/
git submodule update --init
source scripts/activate.sh

./scripts/build_python_device.sh --chip_detail_logging true

source ./out/python_env/bin/activate
```

Install the python dependencies:

```shell
pip install -r requirements.txt
```

## Usage

Run the Python lighting matter device:

```shell
cd examples/lighting-app/python
IP="tapo dev ip" USER="tapo user" PASS="tapo pass" python lighting.py
```
