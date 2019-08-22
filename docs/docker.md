Build the docker container:

```bash
docker build --build-arg http_proxy=$http_proxy --build-arg https_proxy=$https_proxy --rm  --tag=waymo .
```

Start a Jupyter notebook with `waymo_open_dataset` as a dependency:

```bash
docker run --env="http_proxy=$http_proxy" --env="https_proxy=$https_proxy" --rm -p 8888:8888 waymo
```
