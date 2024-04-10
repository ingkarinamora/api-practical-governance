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

### VS Code u otro IDE de preferencia
Yo usaré VS Code para mostrar el código

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

## Paso 4 - Ejecutar el cli para analizar openapi.yaml
`spectral lint openapi.yaml`

## Paso 5 - Crear el archivo de reglas .spectral.yaml vacío y volver a ejecutar el cli
```
touch .spectral.yaml
spectral lint openapi.yaml
```

## Paso 6 - Añadir una línea en el archivo .spectral.yaml para validar las reglas oas (open api)
`extends: ["spectral:oas"]`

Spectral tiene sets de reglas predefinidos que podemos utilizar como el que incluimos. Más información del set que incluimos se encuentra acá: https://docs.stoplight.io/docs/spectral/4dec24461f3af-open-api-rules

## Paso 7 - Volver a ejecutar el cli
`spectral lint openapi.yaml`

Acá dediquemos un tiempo para entender los warnings. Incluso pueden ver en su IDE si se muestran los warnings.

## Paso 8 - Dejemos sin efecto la regla operation-description incluyendo un override en .spectral.yaml
```
"overrides": [
  {
    "files": ["openapi.yaml"],
    "rules": {
      "operation-description": "off"
    }
  }
]
```

Y una vez mas volvamos a ejecutar el cli
`spectral lint openapi.yaml`

## Paso 9 - Ahora si generemos nuestra regla de alineamiento de dominio en .spectral.yaml

```
rules:
  x-domainInfo:
    description: Info object must have "x-domainInfo"
    given: $.info
    severity: error
    then:
      - field: x-domainInfo
        function: truthy
```

## Paso 10 - Modifiquemos el archivo openapi.yaml con este nuevo campo requerido dentro de Info
```
  x-domainInfo:
    someField: some value
```

Y una vez mas volvamos a ejecutar el cli
`spectral lint openapi.yaml`

## Paso 11 - Definamos una segunda regla que especifique campos deben existir relacionados al dominio en .spectral.yaml
Dentro de rules, debajo de la regla anteriormente creada incluir:
```
  x-domainInfo-fields:
    description: Custom domain properties object must have "domainName" and "subdomainName" fields
    given: $.info.x-domainInfo
    severity: error
    then:
      - field: domainName
        function: truthy
      - field: subdomainName
        function: truthy
```

Y una vez mas volvamos a ejecutar el cli
`spectral lint openapi.yaml`

## Paso 12 - Modifiquemos el archivo openapi.yaml con los campos requeridos dentro de x-domainInfo
```
  x-domainInfo:
    domainName: some value
    subdomainName: some value
```

Y una vez mas volvamos a ejecutar el cli
`spectral lint openapi.yaml`

## Paso 13 - Creemos una regla aún más específica para esta organización en spectral.yaml
Acá dediquemos primero un tiempo para revisar el Business Capability Map que usaremos de ejemplo (en las diapositivas) para entender mejor la regla

Dentro de rules, debajo de la regla anteriormente creada incluir:
```
  valid-domainName:
    description: Property "domainName"should have a valid value
    given: $.info.domainInfo
    severity: error
    then:
      - field: domainName
        function: enumeration
        functionOptions:
          values:
            - customer-services-management
            - staff-and-personnel-management
            - airflight-management
            - supporting-services-management
```

Y una vez mas volvamos a ejecutar el cli
`spectral lint openapi.yaml`

## Paso 14 - Modifiquemos el archivo openapi.yaml con los campos requeridos dentro de x-domainInfo
```
  x-domainInfo:
    domainName: customer-services-management
    subdomainName: some value
```

Y una vez mas volvamos a ejecutar el cli
`spectral lint openapi.yaml`

De esta manera podemos seguir iterando sobre las reglas que refelejen la necesidad de esta organización

## Paso 15 - Instalar la version 3.10 de python con pyenv
```
pyenv install 3.10
pyenv local 3.10
pyenv version
```

## Paso 16 - Instalar cookiecutter
```
pip install --upgrade pip
pip install cookiecutter
```

## Paso 17 - Crear el archivo cookiecutter.json con el siguiente contenido
```
{
    "data_product_name": "api-name",
    "domain_name": "customer-services-management"
}
```

## Paso 18 - Crear un folder llamado {{cookiecutter.api_name}} y generar una estructura base
`mkdir {{cookiecutter.api_name}}`

Mover los archivos .spectral.yaml y openapi.yaml a esta carpeta y copiar el archivo .gitignore

## Paso 19 - Reemplazar Petstore de la linea 6 del opeanpi.yaml
`  title: Swagger {{cookiecutter.api_name}} - OpenAPI 3.0`

## Paso 20 - Reemplazar el domain name de la linea 4 del opeanpi.yaml
`    domainName: {{cookiecutter.domain_name}}`

## Paso 21 - Crear un README.md en la carpeta {{cookiecutter.api_name}}
`touch README.md`

Y colocar una estructura base de README para una nueva API siguiendo el paradigma API First
```
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

```

## Documentación de las herramientas utilizadas

https://github.com/swagger-api/swagger-editor
https://editor-next.swagger.io/
https://stoplight.io/open-source/spectral
https://cookiecutter.readthedocs.io/en/stable/index.html
