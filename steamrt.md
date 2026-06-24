# Steam Runtime / SDK

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
