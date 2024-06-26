# Klipper for Kingroon KP3S 3.0

## Configuration Summary
- CHT Volcano 0.6mm nozzle
- BLTouch ABL Sensor support
- Input Shaper tuned
- Fly-ADXL345 Accelerometer support
- Stepper drivers UART enabled and configured with [TMC Autoune](https://github.com/andrewmcgr/klipper_tmc_autotune)
- Improved input shaping tuning and vibration testing with [Klippain-Shake&Tune](https://github.com/Frix-x/klippain-shaketune)
- Custom [Klipper](https://github.com/ozelentok/klipper) to support latest newlib
- Moonraker configured

## Installation under Arch Linux
1. Install the `klipper`, `moonraker`, `klippain-shaketune` and `klipper-tmc-autotune` using the supplied PKGBUILDs
```bash
# Manually
for f in  klipper moonraker/python-smart_open moonraker/python-streaming-form-data moonraker python-pywavelets klippain-shaketune klipper-tmc-autotune; do
  (cd $f; makepkg -si)
done

# With AUR helper
pikaur -Pi {klipper,moonraker/python-smart_open,moonraker/python-streaming-form-data,moonraker,python-pywavelets,klippain-shaketune,klipper-tmc-autotune}/PKGBUILD

# Enable services
sudo systemctl enable --now klipper moonraker
````
2. Copy Klipper's configuration files
```bash
sudo cp -r config/* /var/lib/klipper/config
sudo cp klipper/kp3s-config /usr/lib/klipper/.config
sudo chown -h -R klipper:klipper /var/lib/klipper/config /usr/lib/klipper/.config
```
3. To enable the accelerometer, uncomment `[include adxl.cfg]` in `/etc/klipper/printer.cfg` and restart Klipper
4. The klipper directory is owned by the klipper user to support the Moonraker Update Manager, although I prefer rebuilding the packages and writing PKGBUILDs for plugins
