# Tasmota-AWS-IoT

Enable AWS IoT and MQTT TLS on Tasmota for ESP8266  devices.

The firmware is built automatically via [GitHub Actions](https://github.com/dlime/Tasmota-AWS-IoT/blob/main/.github/workflows/compile_firmware_image.yml) using latest [Tasmota release](https://github.com/arendst/Tasmota/releases/latest) and the following build flags:
```
-DUSE_MQTT_TLS 
-DUSE_MQTT_AWS_IOT_LIGHT 
-DUSE_MQTT_TLS_CA_CERT
```

## How to use it?

* Create a backup of your current device by going to Tasmota's WebUI -> `Configuration` -> `Backup configuration`
* Download latest built firmware [tasmota-aws-iot.bin.gz](https://github.com/dlime/Tasmota-AWS-IoT/releases/latest)
* Open your Tasmota's webUI and go to `Firmware Upgrade` -> `Choose file` and select `tasmota-aws-iot.bin.gz`
* Wait until upgrade is over and that your device has rebooted

### Troubleshoot
![screenshot_upload_failed.png](docs%2Fscreenshot_upload_failed.png)

In case uploading the firmware fails with "Not enough space" error, follow these steps:
* Upgrade via OTA to `tasmota-minimal` using this URL:
  * `http://ota.tasmota.com/tasmota/release/tasmota-minimal.bin.gz`
* Wait till reboot
* Repeat `Firmware Upgrade` -> `Choose file` -> `tasmota-aws-iot.bin.gz`

## How to test?
* Setup your device and  `AWS IoT Authorizer` following [AWS IoT Tasmota documentation](https://tasmota.github.io/docs/AWS-IoT/#how-to-configure)
* Once MQTT TLS is setup on the device, reboot
* When device boots expect a similar result in Tasmota's WebUI -> `Console` (example for Athom Plug V2)
```
MQT: Attempting connection...
MQT: TLS connected in 1753 ms, max ThunkStack used 4596
MQT: MFLN not supported by TLS server
MQT: Connected
```
* From `AWS IoT Core` -> `MQTT test client` subscribe to the topic `tele/tasmota_XXXX/SENSOR`
* You should start receiving device messages in `MQTT test client` console

## How to uninstall?
* Open your Tasmota's WebUI and go to `Firmware Upgrade` -> `OTA` and `Start Upgrade` using the default OTA URL.
