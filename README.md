## Serverless Doc

## Build `slate` doc

```shell
docker run --rm -v ${pwd()}/source:/srv/slate/source -v ${pwd()}/static:/srv/slate/build slatedocs/slate
```
