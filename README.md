# mhi-ac-ctrl-esp32
Control Mitsubishi Heavy Industries Airconditioning locally through ESPHome!

This repository contains a custom component for ESPHome. It is inspired by [@absalom-muc's MHI-AC-Ctrl](https://github.com/absalom-muc/MHI-AC-Ctrl), [@mriksman's esp32 homekit implementation](https://github.com/mriksman/esp32_homekit_mhi/blob/e4a8a4382b990c8e64463411c47e911d1741d9d1/main/main.c) and [@ginkage's MHI-AC-Ctrl-ESPHome](https://github.com/ginkage/MHI-AC-Ctrl-ESPHome). Compared to the popular implementation of @absalom-muc and @ginkage, this project is a fully redesigned implementation aiming for high reliability. It achieves that by using the hardware SPI peripheral instead of a software based implementation.

## Hardware

### Officialy supported (tested by @hberntsen):
* [ESP-C3-32S(4M)-KIT](https://nl.aliexpress.com/item/1005002983050962.html).
  Requires an additional PCB for the AC interface. See the [kicad folder](kicad) folder for the PCB design (manufactured via [Aisler](https://aisler.net)).
* [Universal Air Conditioning Controller](https://www.tinytronics.nl/en/development-boards/microcontroller-boards/with-wi-fi/universal-air-conditioning-controller-esp32-s3) + [JST cable](https://www.tinytronics.nl/en/cables-and-connectors/cables-and-adapters/jst-compatible/jst-xh-female-to-dupont-female-compatible-cable-5p-15cm)
* 2022 models of these indoor AC units:
  * SRK50ZS-W
  * SRK25ZS-W

### Community supported:
* [ESP32-C6](https://github.com/hberntsen/mhi-ac-ctrl-esp32-c3/pull/25)
* Other ESP32-C3 boards (e.g. [ESP32-C3 super mini](https://github.com/xangin/mhi-ac-ctrl-esp32-c3/blob/master/Hardware.md)).
* Other ESP32-S3 boards should work, since the Universal Air Controller has one.
* The ESP32 [has been reported to work](https://github.com/hberntsen/mhi-ac-ctrl-esp32-c3/issues/12#issuecomment-2594627493). Make sure you use the right pins, as discussed in [#11](https://github.com/hberntsen/mhi-ac-ctrl-esp32-c3/issues/11).
* The AC units listed at https://github.com/absalom-muc/MHI-AC-Ctrl?tab=readme-ov-file#prerequisites should work. Some older units have to use the `use_long_frame: false` setting.

Note that it is also possible to re-use custom PCBs designed for the ESP8266 with a supported ESP32 series chip. For example with the Lilygo T-OI Plus, see [this discussion](https://github.com/hberntsen/mhi-ac-ctrl-esp32/discussions/37).

### Not supported
[ESP32 chips without RMT peripheral](https://github.com/esphome/esphome/blob/dev/esphome/components/esp32_rmt/__init__.py#L7). At the moment of writing that's the ESP32-C2 and ESP32-C61. The RMT is used to derive an SPI CS signal from the clock. Users of these chips might try an older version of the code, i.e. commit 3ddb8cb, that uses timers and interrupts for this.

## Getting started

The [ESPHome documentation](https://esphome.io/install/getting-started/) will help you getting started with an ESPHome setup that integrates with your Home Assistant, MQTT or whatever. Then you can merge the relevant parts of the [`example.yaml`](esphome/example.yaml) configuration file into your own. Once you got that going and want to use an external temperature sensor, have a look at [`example-external-sensor.yaml`](esphome/example-external-sensor.yaml) for extra settings to do so. The example shows a Bluetooth sensor but any ESPHome temperature sensor will do.

In general, the latest version of ESPHome should work. See the [build workflow](.github/workflows/build-example.yml#L17) for the latest version of ESPHome that was tested.
