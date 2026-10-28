# C6 Smart Home updates

Signed update downloads for C6 Smart Home: Windows, Android, and ESP32-C6 Zigbee firmware. This repository distributes update files only; device backups, network credentials, signing keys, and source code are not published here.

## Downloads

Open the [latest release](https://github.com/mangkron3001-boop/c6-smart-home-updates/releases/latest) and download:

- `C6SmartHome-v5.exe` — Windows app, version 5.0.0.
- `C6SmartHome-direct.apk` — Android app, version 5.0.1, Android 8 or later. Install over the previous app using Android's installer.
- `C6SmartHome-ESP32C6-OTA.bin` — ESP32-C6 application firmware, version 4.0.1, for the project's **16 MB flash board**.

## Update the apps

Both apps use this signed update feed by default:

`https://github.com/mangkron3001-boop/c6-smart-home-updates/releases/latest/download/update-feed.json`

On Windows select **ตั้งค่า → อัปเดตแอปและเฟิร์มแวร์บอร์ด → อัปเดตแอป**. On Android select **ตั้งค่า → อัปเดตแอปมือถือ**. A newer build must be available before the download/install button is enabled. The current release reports that it is up to date. Downloaded app files are checked against the signed release metadata. Android also requires the normal system installation confirmation.

## ESP32 firmware OTA

The board must first receive the OTA-capable firmware **and its 16 MB partition table over USB**. Previous single-application or blank firmware cannot install this layout over Wi-Fi. Do not use this layout on a 4 MB board.

For an initial USB install with esptool 5, download the four firmware-related binaries from the same release, replace `COM14` with the actual port, and run:

```powershell
esptool --chip esp32c6 --port COM14 --baud 460800 write-flash --flash-size 16MB 0x0 ESP32C6.bootloader.bin 0x8000 ESP32C6.partitions.bin 0xe000 ESP32C6.boot_app0.bin 0x10000 C6SmartHome-ESP32C6-OTA.bin
```

This layout keeps NVS and the existing Zigbee storage at their original offsets. It does not require erasing the entire flash. Keep an existing full flash backup before migrating.

After setup, use **ตั้งค่า → อัปเดตเฟิร์มแวร์บอร์ด** in Android, or **ตั้งค่า → อัปเดตแอปและเฟิร์มแวร์บอร์ด → เฟิร์มแวร์ ESP32** on Windows. Choose only the application file `C6SmartHome-ESP32C6-OTA.bin`. Do not upload bootloader, partition-table, or merged images through the OTA page. The page waits for a new boot identifier and a matching running-image hash before reporting success.

The device checks the application image before switching boot slots. Automatic rollback after a firmware logic failure is not enabled. Hardware OTA, long-term Wi-Fi/Zigbee coexistence, and Android installation still require testing on the actual devices; the release has passed build and software-level checks only.
