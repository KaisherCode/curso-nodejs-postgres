# Curso de Backend con Node.js: Base de Datos con PostgreSQL

## Platzi Store: instalación y presentación del proyecto

Clonar el proyecto.

  `git clone git@github.com:platzi/curso-nodejs-postgres.git`

Ingresaamos a la carpeta del proyecto.

  `cd curso-nodejs-postgres`

  Instalamos dependencias.

  `npm install`

## Instalación de Docker

### Instalación en Windows con WSL (Recomendada) 🐧

Debes descargar el instalador desde la página de [docker for windows](https://docs.docker.com/desktop/install/windows-install/)

- Cuando ya tienes instalado Docker Desktop dentro de tus programas debes abrirlo y debes asegurarte que la opción “Use the WSL 2 based engine” está habilitada:

- Luego en la sección “Resources > WSL Integration”, asegurarate que la opcion “Enable integration with my default WSL distro”, este habilitada:

Nota: para el 24 de mayo de 2023 ya viene por defecto las configuraciones.

Puedes ver más detalles de Docker con WLS 👉 [](https://docs.docker.com/desktop/wsl/)


## Configuraciones de Postgres en Docker


Creamos el archivo docker-compose.yml

```jsx

version: '3.3'

services:
  postgres:
    image: postgres:13
    environment:
      - POSTGRES_DB=my_store
      - POSTGRES_USER=nico
      - POSTGRES_PASSWORD=admin123
    ports:
      - 5432:5432
    volumes:
      - ./postgres_data:/var/lib/postgresql/data


```

Para levantar nuestro el docker

 `docker-compose up -d postgres`

Verificamos is realmente está corriendo

  `docker-compose ps`

  Para bajar los servicios

  `docker-compose down`

