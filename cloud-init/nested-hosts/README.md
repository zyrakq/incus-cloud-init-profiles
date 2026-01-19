# README

```sh
yq eval-all '. as $item ireduce ({}; . *+ $item)' \
    tantal.yaml \
    ../common/pacman-mirror.yaml \
    ../common/sshd.yaml
```

## Profile

```sh
VENDOR_DATA=$(yq eval-all '. as $item ireduce ({}; . *+ $item)' tantal.yaml ../common/pacman-mirror.yaml ../common/sshd.yaml tools/common-utils.yaml)
```

```sh
incus profile set default cloud-init.vendor-data="$(printf '%s' "$VENDOR_DATA")"
```

```sh
incus launch images:archlinux/cloud devcontainer
```

## Instance

```sh
VENDOR_DATA=$(yq eval-all '. as $item ireduce ({}; . *+ $item)' tantal.yaml ../common/pacman-mirror.yaml ../common/sshd.yaml)
```

```sh
incus launch images:archlinux/cloud devcontainer \
    --config=cloud-init.vendor-data="$(printf '%s' "$VENDOR_DATA")"
```
