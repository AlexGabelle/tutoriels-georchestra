# Tuto : deploy georchestra avec docker

On va expliuqer commentt adapter une composition docker bla bla bla

## Step 1: clone the official repo
``` bash
git clone 
--recurse-submodules https://github.com/georchestra/docker.git 
Ensuite, commande :'cd docker'
git checkout 23.0 && git submodule update
``` 

## Step 2: change treafik configuration file

On va ajouter qqch :

*resources/traefik_custom.yaml:*
```yaml 
global: 
  sendAnonymousUsage: false
  checkNewVersion: false

entryPoints:
  web:
    address: ":80"
    http:
      redirections:
        entryPoint:
          to: websecure
          scheme: https

  websecure:
    address: ":443"

providers:
  docker:
    watch: true
    exposedByDefault: false
    endpoint: unix:///var/run/docker.sock

api:
  dashboard: true

log:
  level: INFO

certificatesResolvers:
  letsEncrypt:
    acme:
      # comment this line when going to production
      caServer: https://acme-staging-v02.api.letsencrypt.org/directory
      email: jean.pommier@pi-geosolutions.fr
      storage: /acme/acme.json
      httpChallenge:
        entryPoint: web

ping: {}
```

## Step 3: Update accordingly traefik config in docker-compose.override.yml 
In docker-compose.override.yml, 
- On peut commenter le bloc 'traekif-me-certifacte-downloader'
- pour le service 'georchestra-127-0-11.traefik.me' : 
  - commente la section 'depends_on' 
  - change la section des volumes ... blablabla pour : 

  