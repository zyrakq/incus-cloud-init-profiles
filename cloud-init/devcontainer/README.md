# README

```sh
yq eval-all 'select(fileIndex == 0) *+ select(fileIndex == 1)' \
    tools/dind.yaml \
    lang/rust.yaml
```

## Profile

```sh
incus profile set default cloud-init.vendor-data="$(cat general.yaml)"
```

```sh
incus profile set default cloud-init.network-config="$(cat network/lan-macvlan.yaml)"
```

```sh
USERDATA=$(yq eval-all 'select(fileIndex == 0) *+ select(fileIndex == 1)' tools/dind.yaml lang/rust.yaml)
```

```sh
incus profile set default cloud-init.user-data="$(echo $USERDATA)"
```

```sh
incus launch images:archlinux/cloud devcontainer
```

## Instanse

```sh
incus launch images:archlinux/cloud devcontainer \
    --config=cloud-init.vendor-data="$(cat general.yaml)" \
    --config=cloud-init.network-config="$(cat network/lan-macvlan.yaml)"
```

```sh
USERDATA=$(yq eval-all 'select(fileIndex == 0) *+ select(fileIndex == 1)' tools/dind.yaml lang/rust.yaml)
```

```sh
incus launch images:archlinux/cloud devcontainer \
    --config=cloud-init.vendor-data="$(cat general.yaml)" \
    --config=cloud-init.network-config="$(cat network/lan-macvlan.yaml)" \
    --config=cloud-init.user-data="$(echo $USERDATA)"
```
