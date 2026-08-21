# Mokulate: local ZMK build

## First-time setup

1. Install Docker Desktop and the VS Code **Dev Containers** extension.
2. Open this repository in VS Code and choose **Dev Containers: Reopen in Container**.
3. The container runs `west init -l config` (when needed) and `west update` automatically. This downloads ZMK, Zephyr, and the modules declared in `config/west.yml`.

The manifest deliberately pins the ZMK Studio/input-processor modules to their ZMK v0.3-compatible releases, because this keyboard uses the `v0.3-branch+dya` ZMK fork.

For a terminal-only setup, run the same commands from the repository root inside the ZMK build container:

```sh
west init -l config
west update
```

## Build

In VS Code, run **Tasks: Run Build Task** and choose one of the following:

- `West Build (Mokulate Left)`
- `West Build (Mokulate Right)`
- `West Build (Settings Reset)`
- `West Build (All)`

The UF2 files are produced at:

```text
build/Mokulate_left-xiao_ble-mk.uf2
build/Mokulate_right-xiao_ble-zmk.uf2
build/settings-reset/zephyr/zmk.uf2
```

The original per-build files (`build/left/zephyr/zmk.uf2` and
`build/right/zephyr/zmk.uf2`) are also retained.

Use the settings-reset firmware only when you need to erase the keyboard's saved Bluetooth settings; it is not normal daily firmware.

## Manual commands

```sh
west build -p=always -s zmk/app -d build/left -b seeeduino_xiao_ble -- \
  -DZMK_CONFIG="$PWD/config" -DSHIELD=Mokulate_left

west build -p=always -s zmk/app -d build/right -b seeeduino_xiao_ble -- \
  -DZMK_CONFIG="$PWD/config" -DSHIELD=Mokulate_right \
  -DSNIPPET=studio-rpc-usb-uart
```
