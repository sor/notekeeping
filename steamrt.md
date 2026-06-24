# Steam Runtime / SDK
https://gitlab.steamos.cloud/steamrt/steamrt4/sdk

## List installed Packages
apt list --installed|tail +2|less

## Freshup Podman
```sh
podman run --rm --pull=always \
  -v .:/src:Z \
  -w /src \
  registry.gitlab.steamos.cloud/steamrt/steamrt4/sdk/amd64:latest \
  make
```

## Configure auto pull new
:warning: Untested

```sh
mkdir -p ~/.config/distrobox
nano ~/.config/distrobox/distrobox.ini
```

```ini
[steamrt4]
image="registry.gitlab.steamos.cloud/steamrt/steamrt4/sdk/amd64:latest"
pull_on_create=true
additional_packages="gdb strace ltrace valgrind" # add more here
```

## Recreate Distrobox
distrobox-assemble create --replace --file ~/.config/distrobox/distrobox.ini

## Test Game with Pressure-Vessel
```sh
/path/to/pressure-vessel-launch \
  --runtime-path=/path/to/SteamLinuxRuntime_sniper/ \
  -- /path/to/your/game_executable
```

## RT4 SDK installed files
https://repo.steampowered.com/steamrt4/images/latest-container-runtime-depot/com.valvesoftware.SteamRuntime.Sdk-amd64,i386-steamrt4.manifest.dpkg

RT3 for comparison:
https://repo.steampowered.com/steamrt-images-sniper/snapshots/latest-container-runtime-depot/com.valvesoftware.SteamRuntime.Sdk-amd64%2Ci386-sniper.manifest.dpkg
