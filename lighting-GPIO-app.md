## Hardware setup

-   LED
-   330Ω resistor
-   Raspberry Pi 4B

Connect them as below:

![](./circus.png)

## (Pi) Build lighting GPIO application

1. prepare environment

```
git checkout lighting-app-rpi-gpio
cd ~/connectedhomeip/examples/lighting-app/linux
source ~/connectedhomeip/scripts/activate.sh
```

2. build

```
gn gen out/build
ninja -C out/build
# FAILED: chip-lighting-app chip-lighting-app.map

# The above error could be ignored, let's continue
cd out/build

aarch64-linux-gnu-g++ -O0 -fPIC -Werror -Wl,--fatal-warnings -Wl,-z,defs -fdiagnostics-color -Wl,--gc-sections -pie -Wl,-Map,./chip-lighting-app.map @./chip-lighting-app.rsp -o ./chip-lighting-app -lwiringPi

touch obj/linux.stamp
touch obj/default.stamp
```

## (Pi) Run lighting GPIO application

```
sudo ./chip-lighting-app --wifi
```

## (Laptop) Pair the lighting GPIO application over WiFi

```
./scripts/examples/gn_build_example.sh examples/chip-tool ./build-examples
sudo ~/connectedhomeip/build-examples/chip-tool pairing onnetwork 123 20202021
```

## (Laptop) Control the lighting GPIO application

```
# This command will toggle the status of LED
sudo ~/connectedhomeip/build-examples/chip-tool onoff toggle 0x000000000000007B 1
```
