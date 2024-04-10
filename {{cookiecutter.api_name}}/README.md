# {{cookiecutter.api_name}}
{{cookiecutter.api_description}}

## Dependencias
Para validar de manera local la especificación de la API es necesario contar con npm, para ello es recomendable usar un manejador de versiones de Node como es nvm 

### nvm
Instalar nvm (por ejemplo usando homebrew)
`brew install nvm`

Otras opciones de instalación las pueden encontrar acá: https://www.freecodecamp.org/news/node-version-manager-nvm-install-guide/

## Validación de la especificación de la API

## Instalar spectral-cli
```
nvm use --lts
npm install -g @stoplight/spectral-cli
```

## Ejecutar el cli para analizar openapi.yaml
`spectral lint openapi.yaml`