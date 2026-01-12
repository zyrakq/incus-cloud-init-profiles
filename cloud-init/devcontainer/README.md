# README

```sh
yq eval-all '. as $item ireduce ({}; . *+ $item)' \
    base.yaml \
    utils/pacman-mirror.yaml \
    utils/sshd.yaml \
    utils/zsh.yaml
```

```sh
STATIC_ADDRESS="192.168.1.100/24" GATEWAY4="192.168.1.1" envsubst < network/lan-macvlan.yaml
```

```sh
yq eval-all '. as $item ireduce ({}; . *+ $item)' \
    tools/dind.yaml \
    lang/rust.yaml
```

## Profile

```sh
VENDOR_DATA=$(yq eval-all '. as $item ireduce ({}; . *+ $item)' base.yaml utils/pacman-mirror.yaml utils/sshd.yaml utils/zsh.yaml)
```

```sh
incus profile set default cloud-init.vendor-data="$(printf '%s' "$VENDOR_DATA")"
```

```sh
NETWORK_CONFIG=$(STATIC_ADDRESS="192.168.1.100/24" GATEWAY4="192.168.1.1" envsubst < network/lan-macvlan.yaml)
```

```sh
incus profile set default cloud-init.network-config="$(printf '%s' "$NETWORK_CONFIG")"
```

```sh
USER_DATA=$(yq eval-all '. as $item ireduce ({}; . *+ $item)' tools/dind.yaml lang/rust.yaml)
```

```sh
incus profile set default cloud-init.user-data="$(printf '%s' "$USER_DATA")"
```

```sh
incus launch images:archlinux/cloud devcontainer
```

## Instance

```sh
VENDOR_DATA=$(yq eval-all '. as $item ireduce ({}; . *+ $item)' base.yaml utils/pacman-mirror.yaml utils/sshd.yaml utils/zsh.yaml)
```

```sh
NETWORK_CONFIG=$(STATIC_ADDRESS="192.168.1.100/24" GATEWAY4="192.168.1.1" envsubst < network/lan-macvlan.yaml)
```

```sh
USER_DATA=$(yq eval-all '. as $item ireduce ({}; . *+ $item)' tools/dind.yaml lang/rust.yaml)
```

```sh
incus launch images:archlinux/cloud devcontainer \
    --config=cloud-init.vendor-data="$(printf '%s' "$VENDOR_DATA")" \
    --config=cloud-init.network-config="$(printf '%s' "$NETWORK_CONFIG")" \
    --config=cloud-init.user-data="$(printf '%s' "$USER_DATA")"
```
