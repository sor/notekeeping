
# Podman
- Container software
- Daemonless / Rootless
- alternative to **Docker** (which would need Daemon)
- usable with **Distrobox** (making it stateful)

## Install
distrobox-1.8.2.5-1-any  
podman-5.8.3-1-x86_64

## Usage of Podman

### Enable Management-Socket
```sh
systemctl --user enable podman.socket
systemctl --user start podman.socket
```

### Add Registries
`nano  ~/.config/containers/registries.conf`

`unqualified-search-registries = ["docker.io", "quay.io", "registry.access.redhat.com", "ghcr.io", "registry.gitlab.com", "registry.gitlab.steamos.cloud", "codeberg.org"]`

"nvcr.io" needs auth :(

### List running (or even non running?)
`podman ps`

### Create?
`podman run -it --name steamrt4-direct -v "$PWD:/src" -w /src registry.gitlab.steamos.cloud/steamrt/steamrt4/sdk /bin/bash`

Ubuntu 26.04

```
podman run -it --name ubuntu docker.io/library/ubuntu:resolute /bin/bash
```

Inside the Pod:
```sh
apt update

ln -fs /usr/share/zoneinfo/Europe/Berlin /etc/localtime

echo "keyboard-configuration keyboard-configuration/layout select German" | debconf-set-selections
echo "keyboard-configuration keyboard-configuration/variant select German" | debconf-set-selections
echo "keyboard-configuration keyboard-configuration/modelcode select pc105" | debconf-set-selections

echo "console-setup console-setup/charmap select UTF-8" | debconf-set-selections
echo "console-setup console-setup/codeset select Guess optimal character set" | debconf-set-selections
echo "console-setup console-setup/modelcode select pc105" | debconf-set-selections

DEBIAN_PRIORITY=critical apt install -y ubuntu-server-minimal build-essential clang cmake git ninja-build
apt purge -y snap;apt autoremove -y # not needed on 24.04 :bliss:
```

### Run?
`podman exec -it steamrt4 /bin/bash`

### Run once then delete
`podman run --rm -it --name steamrt4-direct -v "$PWD:/src" -w /src registry.gitlab.steamos.cloud/steamrt/steamrt4/sdk /bin/bash`

### Start
`podman start steamrt4-direct`

### Enter
`podman exec -it steamrt4-direct /bin/bash`

### Commiting a running session for future reuse
`podman commit ubuntu ubuntu-secure`

### Want lazygit on 24.04
```bash
LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | \grep -Po '"tag_name": *"v\K[^"]*')
LAZYGIT_ARCH=$(uname -m | sed -e 's/aarch64/arm64/')
curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/download/v${LAZYGIT_VERSION}/lazygit_${LAZYGIT_VERSION}_Linux_${LAZYGIT_ARCH}.tar.gz"
tar xf lazygit.tar.gz lazygit
sudo install lazygit -D -t /usr/local/bin/
```

### Problems
If disconnects happen enable this:
`loginctl enable-linger $USER`

## Usage of Distrobox

### Prerequirement
Fix `~/.bashrc`:
```bash
# Force correct locale inside Distrobox containers
if [ -n "$DISTROBOX_ENTER_PATH" ]; then
    export LANG=en_US.UTF-8
    export LANGUAGE=en_US
    export LC_ALL=en_US.UTF-8
    export LC_CTYPE=en_US.UTF-8
fi
```

### List images/containers
`distrobox create --compatibility`

### Before creation
`sudo nano /etc/locale.conf`

Add:
```bash
LANG=en_US.UTF-8
LANGUAGE=en_US
LC_ALL=en_US.UTF-8
```
:warning: == remove these lines again after creation == :warning:

### Create
`distrobox create --name steamrt4 --image registry.gitlab.steamos.cloud/steamrt/steamrt4/sdk`

### Enter
`distrobox enter steamrt4`  
`distrobox enter --verbose steamrt4`

### Delete
`distrobox rm steamrt4`
