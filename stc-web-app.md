# Running STC Web App on any Linux Host

[TOC]

## Pre-Requisites

1. Install docker
2. Make sure there is no firewall rule that stops incoming connections on TCP port 5000

## Copy the STC Web App code in a directory

**STC Web App** code must have the following files / directories to function properly

- `Dockerfile` - To create docker image
- `app.py` - Entry point into Flask 
- `certificates.zip` - Contains templates to generate image and PDF certificates
- `requirements.txt` - Contains dependencies. Used by `Dockerfile`
- `site.db` - Contains user database (for authentication)
- `wsgi.py` - Gateway Interface for Flask
- `flaskapp` - Source code of the Application

> From now on, this directory is referred to as "project directory"

### Create Docker Image

Run the below command from the project directory

```bash
docker build -t <image-name> .
```

`<image-name>` can be anything

`.` indicates local directory (should be project directory)

### Check if Docker Image is created

```bash
docker images
```

### Run the Docker Image

```bash
docker run -d --name <app-name> --network="host" <image-name>
```

`<app-name>` can be anything

`<image-name>` should correspond to an image in `docker images` (as created in the earlier step)

#### For Windows / Mac Host

In Windows / Mac, host networking may not work. Hence use port forwarding

```bash
docker run -d --name <app-name> -p 5000:5000 <image-name>
```

### Check if app is running

```bash
docker ps
```

You should see the `<app-name>` you created in "Up" status

### If "Up" - Check logs

Check logs to see if the app's WSGI is working as expected

```bash
docker logs <container-id>
```

You can find the `<container-id>` from `docker ps` command

You should see that the app is listening on `http://0.0.0.0:5000` and around 8 threads are started

### Check if the port is open on the host

Whichever host you are running the app on, must have `5000` open

```bash
# Works for Mac and Linux hosts
netstat | grep 5000
```

You should see that the port `5000` is open for `TCP`

### Access the app on any browser

Go to any browser and access the app at `localhost:5000`  
