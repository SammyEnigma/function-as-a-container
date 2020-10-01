---
path: tree/master
source: syntax-tools/json/show-struct/Dockerfile
---

# show-struct

[![Docker Image Size (tag)](https://img.shields.io/docker/image-size/donaldrich/function/show-struct?color=blue&label=donaldrich/function:show-struct&logo=docker&style=flat-square)](https://hub.docker.com/r/donaldrich/function/show-struct)

## Documentation

### Show-Struct

- [:octicons-mark-github-16: ilyash/show-struct](https://github.com/ilyash/show-struct)

## Image Commands

### Container shell

```sh
docker pull donaldrich/function:show-struct
docker run -it --rm \
-v /var/run/docker.sock:/var/run/docker.sock \
-v "$(pwd)":"/work" -w "/work" \
--hostname=show-struct \
--entrypoint="/bin/sh" \
--net="host" \
donaldrich/function:show-struct
```

### Check versions

```sh
docker pull donaldrich/function:show-struct && docker run -it --rm  donaldrich/function:show-struct validate
```

### See config options

```sh
docker pull donaldrich/function:show-struct && docker run -it --rm  donaldrich/function:show-struct help
```

### Dive into Image

```sh
docker pull donaldrich/function:show-struct && dive donaldrich/function:show-struct
```

### See Layer Info

```sh
docker pull donaldrich/function:show-struct && docker history donaldrich/function:show-struct
```

## Image Details

??? info ""

    === "Image"
        ```json
        --8<--
        image-info/show-struct.md
        --8<--
        ```

    === "Layers"
        ```md
        --8<--
        layers/show-struct.md
        --8<--
        ```