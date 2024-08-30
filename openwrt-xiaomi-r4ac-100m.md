# Install OpenWRT/ImmortalWRT on Xiaomi R4AC 100M (Chinese version)

## Step 1 - Prepare

1. Reset Router to default

1. Access `192.168.31.1` and set WiFi SSID + password. This password also managed password

1. Connect router to Internet

1. Download firmware

   - OpenWRT: https://archive.openwrt.org/releases/23.05.3/targets/ramips/mt76x8/

   - ImmortalWRT: https://firmware-selector.immortalwrt.org/?version=23.05.3&target=ramips%2Fmt76x8&id=xiaomi_mi-router-4a-100m

   - Stock firmware (for recovery): include in this guide https://hoddysguides.com/xiaomi-debrick-tools-all/

## Step 2 - Exploit router for ssh, telnet with root user

Instruction:

- https://hoddysguides.com/installing-openwrt-xiaomi4a/

- https://www.youtube.com/watch?v=VxzEvdDWU_s&t=143s

1. Following these instruction to exploit: https://github.com/acecilia/OpenWRTInvasion

1. Copy firmware to `/tmp` folder on Router (by `scp` command or ftp client program, ex: `filezilla`, `cyberduck`)

1. ssh or telnet to Router and flash kernel firmware

   ```bash
   mtd -e OS1 -r write openwrt-23.05.4.bin OS1
   ```

   Wait the flash process complete. Then we can connect to Router at `192.168.1.1`

1. Continue update to ImmortalWRT with sysupgrade firmware via OpenWRT interface (LuCI)

## Recovery to stock firmware

Following this guild https://hoddysguides.com/xiaomi-debrick-tools-all/

1. Connect LAN port on Router and set IP 192.168.1.2 on PC

1. Run tinypxe (run on Win 10 if there is error: Access Denied)

1. Hold Reset button and plug power on Router. Router will start on recovery mode and download firmware to it

1. Reboot router and wait for recovery process complete
