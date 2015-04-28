# Docker Registry 2.0 Proxy

Use this image to run Docker Registry 2.0 behind nginx proxy, with SSL and basic_auth enabled.

[Tutorial](http://www.container-solutions.com/2015/04/running-secured-docker-registry-2-0/)

## Settings

First run official Docker Registry 2.0 container:

```
#!/bin/bash
docker run --name docker-registry \
	-v $(pwd)/data:/tmp/registry-dev \
	registry:2.0
```

Then run the proxy:

```
#!/bin/bash
docker run -p 443:443 \
	-e REGISTRY_HOST="docker-registry" \
	-e REGISTRY_PORT="5000" \
	-e SERVER_NAME="localhost" \
	--link docker-registry:docker-registry \
	-v $(pwd)/.htpasswd:/etc/nginx/.htpasswd:ro \
	-v $(pwd)/certs:/etc/nginx/ssl:ro \
	containersol/docker-registry-proxy
```

## Volumes

- .htpasswd file with basic_auth credentials
- a directory with `docker-registry.crt` and `docker-registry.key`

## Environment Variables

- REGISTRY_HOST ... hostname of the registry container
- REGISTRY_PORT ... port number of the registry container
- SERVER_NAME ... nginx server_name directive value

