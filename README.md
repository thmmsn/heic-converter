`heic-converter` is designed to monitor a specified folder and its subfolders for image files with the extension `.heic` or `.HEIC`. It converts these files into `.jpg` format. Originaly this is supposed to be executed on a Synology NAS, that backups pictures from an iPhone (with Synology Photos / Synology Drive).

The application is not limited to running exclusively on a Synology NAS; it can be deployed on any system that supports Docker.

It runs on a ticker, that will restart the converting process every 1 hour (configurable)
by default, it will also delete the source-files and live-photos



## Environment variables

| Name              | Default     | Description |
|-------------------|-------------|-------------|
| `WATCH`           | `/opt/data` | 
| `TIME_BETWEEN`    | `1h`        | The duration between consecutive conversion processes. Use Golang duration notation (e.g., `24h`, `30m`, `1m`).
| `KEEP_ORIGINAL`   | `false`     | Whether to delete (`false`) or keep (`true`) the original photos after conversion. 
| `KEEP_LIVE_PHOTO` | `false`     | Whether to delete (`false`) or keep (`true`) the original live-photos after conversion.



# Deployment

## Docker on Synology NAS

1. Install Docker package on Synology NAS.
2. Add latest image from URL/Hub Page.
```html
559rvsuq/heic-converter
```
3. Launch image.
4. Select network `bridge`.
5. Give the application a name and go to Advanced Settings.
6. Define Environment variables and save.
7. Skip port settings.
8. In Volume Settings you select which folder to watch for `.heic` images.
9. Mount path must be equal to enviromnet variable `WATCH` `/opt/data` default.
10. Save and next and done.

### Screenshots

![](01_docker-heic-converter.png)
![](02_docker-heic-converter.png)
![](03_docker-heic-converter.png)
![](04_docker-heic-converter.png)
![](05_docker-heic-converter.png)
![](06_docker-heic-converter.png)
![](07_docker-heic-converter.png)

## Docker-compose

```yml
version: '3.9'
services:
  heic-converter:
    image: 559rvsuq/heic-converter
    container_name: my-heic-converter
    volumes:
    - ./folder/to/watch:/opt/data
    environment:
      - TIME_BETWEEN=1h
      - KEEP_ORIGINAL=false
      - KEEP_LIVE_PHOTO=false
```

## Docker run
```sh
docker run -v ./folder/to/watch:/opt/data --name my-heic-converter -e TIME_BETWEEN=1h -e KEEP_ORIGINAL=false -e KEEP_LIVE_PHOTO=false 559rvsuq/heic-converter  
```

# Example usage

## Monitor a SMB share
One use case for this application is to watch an SMB share for `.heic` files. You can set up an SMB share on your system and configure the application to monitor that specific share. Users with access to the SMB share can then convert pictures by moving `.heic` files into the shared folder. After `TIME_BETWEEN`, the application will automatically convert the photos and delete the original files.

## Monitor share on Synology NAS for converting backed up iPhone/iPad photes

Example goes here.
