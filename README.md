# api-practical-governance
Workshop práctico de governance automatizado de APIs

## Dependencias

### nvm
Yo usé homebrew para instalar nvm
`brew install nvm`

Las opciones para instalarlo en sus distibuciones de OS las pueden encontrar acá: https://www.freecodecamp.org/news/node-version-manager-nvm-install-guide/

### Docker
Yo usé colima para docker
`brew install colima`

Las opciones para instalarlo en sus distibuciones de OS las pueden encontrar acá: https://github.com/abiosoft/colima/blob/main/docs/INSTALL.md

### Python
Yo usé homebrew para instalar pyenv
`brew install pyenv`

Las opciones para instalarlo en sus distibuciones de OS las pueden encontrar acá: https://github.com/pyenv/pyenv


## Paso 1 - Iniciar docker
`colima start`

## Paso 2 - Correr la última imagen de swagger editor:
```
docker pull swaggerapi/swagger-editor:latest
docker run -d -p 80:8080 swaggerapi/swagger-editor:latest
```

Abrir en el browser http://localhost/

Dar clic en File > Save as YAML e incluir este archivo en su repo

## Paso 3 - Instalar spectral-cli
```
nvm use --lts
npm install -g @stoplight/spectral-cli
```

## Documentación de las herramientas utilizadas

https://github.com/swagger-api/swagger-editor
https://editor-next.swagger.io/
https://stoplight.io/open-source/spectral