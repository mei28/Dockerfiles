## Login 

```bash
docker login
```

## Build

```bash
docker build -t <username>/<image-name>:<tag> .
```

## Push

```bash
docker push <username>/<image-name>:<tag>
```

## Pull
```bash
docker pull <username>/<image-name>:<tag>
```

## Run

### Interactive

```bash
docker run -it <username>/<image-name>:<tag>
```

### Background

```bash
docker run -d <username>/<image-name>:<tag>
```

## Connect

```bash
docker exec -it <container-id> /bin/bash
```

