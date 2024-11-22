# Klipper for Kingroon KP3S 3.0

## Configuration Summary
- CHT Volcano 0.6mm nozzle
- BLTouch ABL Sensor support
- Input Shaper tuned
- Fly-ADXL345 Accelerometer support
- Stepper drivers UART enabled and configured with [TMC Autotune](https://github.com/andrewmcgr/klipper_tmc_autotune)
- Improved input shaping tuning and vibration testing with [Klippain-Shake&Tune](https://github.com/frix-x/klippain-shaketune)
- Updates to all components through Moonraker's update manager

## Installation under Arch Linux
```bash
# Install `klipper` and `moonraker` using the supplied PKGBUILDs
for f in  klipper moonraker; do
  (cd $f; makepkg -si)
done

# Setup fluidd
sudo pacman -Syu nginx-mainline
curl -L https://github.com/fluidd-core/fluidd/releases/download/v1.30.6/fluidd.zip -o /tmp/fluidd.zip
# Link fluidd to nginx default public directory
PREFIX=/opt/3d-printer
sudo mkdir "${PREFIX}/fluidd"
(cd "${PREFIX}/fluidd"; sudo unzip /tmp/fluidd.zip)
sudo chown klipper:klipper "${PREFIX}/fluidd"
sudo ln -s "${PREFIX}/fluidd" /srv/http/fluidd


# Setup TMC Autotune
TMC_PATH="${PREFIX}/klipper_tmc_autotune"
KLIPPY_PATH="${PREFIX}/klipper/klippy"
sudo git clone https://github.com/andrewmcgr/klipper_tmc_autotune "${TMC_PATH}"
sudo ln -srfn "${TMC_PATH}/autotune_tmc.py"    "${KLIPPY_PATH}/extras/autotune_tmc.py"
sudo ln -srfn "${TMC_PATH}/motor_constants.py" "${KLIPPY_PATH}/extras/motor_constants.py"
sudo ln -srfn "${TMC_PATH}/motor_database.cfg" "${KLIPPY_PATH}/extras/motor_database.cfg"


# Setup Shake&Tune
ST_PATH="${PREFIX}/klippain_shaketune"
sudo cp shaketune-requirements.txt "${PREFIX}/"
sudo git clone https://github.com/frix-x/klippain-shaketune "${ST_PATH}"
sudo ln -srfn "${ST_PATH}/shaketune" "${KLIPPY_PATH}/extras/shaketune"


# Copy configuration files
sudo cp -r config/* /var/lib/klipper/config
sudo systemd-tmpfiles --create
sudo systemctl enable --now klipper moonraker nginx
```
