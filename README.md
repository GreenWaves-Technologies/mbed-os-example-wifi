# mbed-os-example-wifi #

This guide reviews the steps required to get external wifi module working on an mbed OS platform in GAP8, it only support ESP8266 now. For more details, please find in [mbed-os-example-wifi](https://github.com/ARMmbed/mbed-os-example-wifi)

Please install [mbed CLI](https://github.com/ARMmbed/mbed-cli#installing-mbed-cli).

## Import the example application

From the command-line, get the example:

```
mbed import https://github.com/GreenWaves-Technologies/mbed-os-example-wifi
cd mbed-os-example-wifi
```

For problem of `hg` tool
```
sudo apt-get install mercurial
```
or
```
sudo yum install mercurial
```

For problem of `python missing module`, try following commands according to your python version :
```
sudo pip install mbed-cli
sudo pip install -r mbed-os/requirements.txt

sudo pip2 install mbed-cli
sudo pip2 install -r mbed-os/requirements.txt

sudo pip3 install mbed-cli
sudo pip3 install -r mbed-os/requirements.txt
```

## Or use `git clone` to get the example :
```
git clone https://github.com/GreenWaves-Technologies/mbed-os-example-wifi
cd mbed-os-example-wifi
mbed deploy
mbed config -G GCC_RISCV_PATH "/usr/lib/gap_riscv_toolchain/bin"
```

If you find that the mbed-os git verison size (which includes all git history about 400 MB) is too large to download,
you can download [gwt-mbed-os-wifi](https://github.com/GreenWaves-Technologies/mbed-os/releases/tag/gwt-mbed-os-wifi) or :

```
wget https://codeload.github.com/GreenWaves-Technologies/mbed-os/zip/gwt-mbed-os-wifi
mv gwt-mbed-os-wifi gwt-mbed-os-wifi.zip
unzip -qq gwt-mbed-os-wifi.zip
mv mbed-os-gwt-mbed-os-wifi mbed-os
```
which is only 20 MB, then uncompress it and rename it to `mbed-os` in your directory.

### Configure the Wi-Fi shield and settings.

   Edit ```mbed_app.json``` to include the correct Wi-Fi shield, SSID and password:

```json
{
    "config": {
        "wifi-ssid": {
            "help": "WiFi SSID",
            "value": "\"SSID\""
        },
        "wifi-password": {
            "help": "WiFi Password",
            "value": "\"PASSWORD\""
        }
    },
    "target_overrides": {
        "*": {
            "platform.stdio-convert-newlines": true,
            "esp8266.provide-default" : false
        }
    }
}
```

   For build-in WiFi, you do not need to set any `provide-default` values. Those are required
   if you use external WiFi shield.

   Sample ```mbed_app.json``` files are provided for ESP8266 (```mbed_app_esp8266.json```), X-NUCLEO-IDW04A1 (```mbed_app_idw04a1.json```) and X-NUCLEO-IDW01M1 (```mbed_app_idw01m1```).

### Now compile

Invoke `mbed compile`, and specify the name of your platform and your favorite toolchain (`GCC_ARM`, `ARM`, `IAR`, `GCC_RISCV`). For example, for the RISC-V GCC Compile :

```
mbed compile -m GAP8 -t GCC_RISCV
```

Your PC may take a few minutes to compile your code. At the end, you see the following result:

```
Elf2Bin: mbed-os-example-wifi
| Module          |      .text |    .data |     .bss |
|-----------------|------------|----------|----------|
| BUILD/GAP8      |  49212(-2) |  768(+0) | 4688(-8) |
| [fill]          |      2(+0) |    0(+0) |   32(+0) |
| [lib]/c.a       |  53786(+0) | 2480(+0) |   60(+0) |
| [lib]/gcc.a     |  17716(+0) |    0(+0) |    0(+0) |
| [lib]/stdc++.a  |      0(+0) |    0(+0) |    0(+0) |
| mbed-os/targets |    288(+0) |    4(+0) |   28(+0) |
| Subtotals       | 121004(-2) | 3252(+0) | 4808(-8) |
Total Static RAM memory (data + bss): 8060(-8) bytes
Total Flash memory (text + data): 124256(-2) bytes

Image: ./BUILD/GAP8/GCC_RISCV/mbed-os-example-wifi.bin
```

### Program your board

1. Connect your device (with sensor board) to the computer over USB.
1. Execute the script (make sure you have already install the [gap_sdk](https://github.com/GreenWaves-Technologies/gap_sdk)) :

```
source ./USER_PATH/gap_sdk/sourceme.sh
run_mbed ./BUILD/GAP8/GCC_RISCV/mbed-os-example-wifi.elf
```

### Result
The terminal should display a similar output to below, indicating a successful Wi-Fi connection:
```
WiFi example
Mbed OS version 5.10.0

Scan:
Network: Dave Hot Spot secured: Unknown BSSID: 00:01:02:03:04:05 RSSI: -58 Ch: 1
1 network available.

Connecting to `SSID`...
Success

MAC: 00:01:02:03:04:05
IP: 192.168.0.5
Netmask: 255.255.255.0
Gateway: 192.168.0.1
RSSI: -27

Sending HTTP request to www.arm.com...
sent 38 [s]
recv 64 [s]

Done
```

## Troubleshooting

If you have problems, you can review the [documentation](https://os.mbed.com/docs/latest/tutorials/debugging.html) for suggestions on what could be wrong and how to fix it.