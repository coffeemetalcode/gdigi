| 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| f0  | 00  | 00  | 10  | 00  | 5e  | 05  | 2c  | 67  | f7  |
| 240 | 0   | 0   | 16  | 0   | 94  | 5   | 44  | 103 | 247 |

Pushing correct message!\
f0 00 00 10 00 5e 05 2b 00 04 00 4a 45 46 46 48 00 20 52 48 00 00 02 1b f7

Pushing correct message!\
f0 00 00 10 00 5e 05 2b 00 04 00 4d 55 52 52 59 00 20 43 4c 20 20 00 00 00 02 
08 f7

## 12/22/2023

### USB Sniffing  w/ `usbmon`

- [tutorial](https://www.baeldung.com/linux/usb-sniffing)
- [`usbmon` documentation](https://www.kernel.org/doc/Documentation/usb/usbmon.txt)

#### Steps

1. Get bus/device info
    ```bash
    $~ lsusb

    Bus 002 Device 003: ID 045b:0210 Hitachi, Ltd 
    Bus 002 Device 002: ID 045b:0210 Hitachi, Ltd 
    Bus 001 Device 014: ID 248a:8367 Maxxter Telink Wireless Receiver
    Bus 001 Device 013: ID 248a:8367 Maxxter Telink Wireless Receiver
    Bus 001 Device 016: ID 1210:0016 DigiTech RP500 Guitar Multi-Effects Processor
    Bus 001 Device 009: ID 0c45:6366 Microdia Webcam Vitade AF

    $~
    ```

2. Stream device communication where `1u` is for `Bus 001` (for instance), and write to a file
    ```bash
    sudo cat /sys/kernel/debug/usb/usbmon/1u > Desktop/usb.out.txt
    ```

    (Use `Ctrl+c` to stop streaming)

3. Analyze the file
    ```bash
    Bus 001 Device 016: ID 1210:0016 DigiTech RP500 Guitar Multi-Effects Processor
    ```

    ```bash
    ffff88826a380200 2068274786 C Zo:1:002:1 0:1:15136:0 6 0:0:48 0:48:48 0:96:40 0:136:48 0:184:40 272 >
    ffff88826a380200 2068274790 S Zo:1:002:1 -115:1:15136 6 -18:0:48 -18:48:40 -18:88:48 -18:136:40 -18:176:48 264 = 00ecffff 00000000 00000000 00000000 00000000 00000000 00000000 00000000
    ffff8881233759c0 2068275125 C Bi:1:016:4 0 12 = 04f00000 0410005e 04057e00
    ffff8881233759c0 2068275129 S Bi:1:016:4 -115 16 <
    ffff88826a383e00 2068275544 C Zo:1:002:1 0:1:15136:0 6 0:0:40 0:40:48 0:88:40 0:128:48 0:176:40 264 >
    ffff88826a383e00 2068275549 S Zo:1:002:1 -115:1:15136 6 -18:0:48 -18:48:40 -18:88:48 -18:136:40 -18:176:48 272 = 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000
    ffff888123375000 2068275817 C Bi:1:016:4 0 4 = 074174f7
    ffff888123375000 2068275819 S Bi:1:016:4 -115 16 <
    ffff88826a380000 2068276247 C Zo:1:002:1 0:1:15144:0 6 0:0:40 0:40:48 0:88:40 0:128:48 0:176:40 264 >
    ffff88826a380000 2068276251 S Zo:1:002:1 -115:1:15144 5 -18:0:40 -18:40:48 -18:88:40 -18:128:48 -18:176:40 216 = 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00140000
    ```
    Searching for `:016` in results yields packets sent to or from `Device 016`