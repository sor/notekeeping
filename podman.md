
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

### List running (or even non running?)
`podman ps`

### Create?
`podman run -it --name steamrt4-direct -v "$PWD:/src" -w /src registry.gitlab.steamos.cloud/steamrt/steamrt4/sdk /bin/bash`

### Run?
`podman exec -it steamrt4 /bin/bash`

### Run once then delete
`podman run --rm -it --name steamrt4-direct -v "$PWD:/src" -w /src registry.gitlab.steamos.cloud/steamrt/steamrt4/sdk /bin/bash`

### Start
`podman start steamrt4-direct`

### Enter
`podman exec -it steamrt4-direct /bin/bash`


## Usage of Distrobox

### Prerequirement

#### Fix ~/.bashrc
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
