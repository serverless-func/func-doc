## Function Doc

## Local Build

### Build `slate` doc

```shell
docker build -t :latest slate/ --build-arg local=true
docker run --rm -v (pwd)/build:/srv/slate/build slate-builder:latest
```

### Build `bootstrap`

```shell
go build
```
