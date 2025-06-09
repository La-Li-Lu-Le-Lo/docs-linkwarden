
unraid labels

```
services:
  postgres:
    labels:
      net.unraid.docker.icon: "https://raw.githubusercontent.com/sgraaf/Unraid-Docker-Templates/main/postgresql16/icon.png"
  linkwarden:
    labels:
      net.unraid.docker.icon: "https://raw.githubusercontent.com/UNRA1DUser/unraid-docker-templates/main/templates/img/linkwarden.png"
      net.unraid.docker.webui: http://[IP]:[PORT:3001]
  meilisearch:
    labels:
      net.unraid.docker.icon: "https://raw.githubusercontent.com/Collectathon/unraid-templates/main/icons/meilisearch.png"
      net.unraid.docker.webui: http://[IP]:[PORT:7700]
```
