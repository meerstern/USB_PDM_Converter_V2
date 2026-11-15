# USB-PDM Converter V2  
  * Converts a digital MEMS microphone into a USB audio device.
  * Resolution 16bit, Sampling Rate 16kHz/24kHz/32kHz/48kHz
  * The sampling rate is defined by `SAMPLES_PER_MS` in `tusb_config.h`.  
  * If you change the sampling rate, be sure to uninstall the driver; otherwise, the device will not function correctly.  
  * In versions such as TinyUSB 0.18.0, the microphone device may fail to initialize properly.
  * Please apply the following command to fix the bug in TinyUSB : `cd $PICO_SDK_PATH/lib/tinyusb && git cherry-pick 334ac80 -m 1`
  * [The sample code forked originally](https://github.com/delingren/mems_mic/tree/stereo_no_encoding) obtained the data line from two separate I/O pins(GPIO3&4).
  * This repo is modified it to align with the typical PDM microphones by consolidating the data line(Only GPIO3) using the SEL pin.

## Sampling Rate vs PDM Clock
| PCM Sampling Rate | PDM Clock  |
| -------------------------- | ----------------- |
| 16kHz	| 1.024MHz |
| 24kHz	| 1.536MHz |
| 32kHz	| 2.048MHz |
| 48kHz	| 3.072MHz |

## Hardware

 * RP2040 board or RP2350 board 
   * [Raspberry Pi Pico](https://www.raspberrypi.org/products/raspberry-pi-pico/)
 * Microphones
   * PDM
     * [Syntiant Digital MEMS Microphone SPH0641LU4H-1](https://github.com/meerstern/SPH0641LU)

### Default Pinout

#### Two PDM Microphone

| Raspberry Pi Pico / RP2040 | PDM Microphone Left | PDM Microphone Right |
| -------------------------- | ----------------- |----------------- |
| 3.3V | VCC |VCC |
| GND | GND | GND |
| - | SEL->GND | SEL->VCC |
| GPIO 3 | DAT | DAT |
| GPIO 5 | CLK | CLK |



#### One PDM Microphone

| Raspberry Pi Pico / RP2040 | PDM Microphone  |
| -------------------------- | ----------------- |
| 3.3V | VCC |
| GND | GND | 
| - | SEL->GND or VCC |
| GPIO 3 | DAT | 
| GPIO 5 | CLK | 

GPIO pins are configurable in examples or API.  
For monaural operation, connect SEL to either VCC or GND.  
Do not leave the SEL pin floating. 

## Examples

See [examples](examples/) folder.


## Building

1. [Set up the Pico C/C++ SDK](https://datasheets.raspberrypi.org/pico/getting-started-with-pico.pdf)
2. Set `PICO_SDK_PATH`
```sh
export PICO_SDK_PATH=/path/to/pico-sdk
```
3. Create `build` dir, run `cmake` and `make`:
```
mkdir build
cd build
cmake .. -DPICO_BOARD=pico
make
```
4. Copy example `.uf2` to Pico when in BOOT mode.

## License

[Apache-2.0 License](LICENSE)

## Acknowledgements

This project was created on behalf of the [Arm Software Developers](https://developer.arm.com/) team, follow them on Twitter: [@ArmSoftwareDev](https://twitter.com/armsoftwaredev) and YouTube: [Arm Software Developers](https://www.youtube.com/channel/UCHUAckhCfRom2EHDGxwhfOg) for more resources!

The [OpenPDM2PCM](https://os.mbed.com/teams/ST/code/X_NUCLEO_CCA02M1//file/53f8b511f2a1/Middlewares/OpenPDM2PCM/) library is used to filter raw PDM data into PCM. The [TinyUSB](https://github.com/hathach/tinyusb) library is used in the `usb_microphone` example.

---

Disclaimer: This is not an official Arm product.
