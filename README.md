## Function Doc

## Local Build

### Build `slate` doc

```shell
docker build -t slate-builder:latest slate/ --build-arg local=true
docker run --rm -v (pwd)/static:/srv/slate/build slate-builder:latest
```

### Build `bootstrap`

```shell
go build -o bootstrap
```
