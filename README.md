# Curso de Backend con Node.js: Base de Datos con PostgreSQL

## Platzi Store: instalación y presentación del proyecto

Clonar el proyecto.

  `git clone git@github.com:platzi/curso-nodejs-postgres.git`

Ingresaamos a la carpeta del proyecto.

  `cd curso-nodejs-postgres`

  Instalamos dependencias.

  `npm install`

## Base de datos

### Instalación de Docker

**Instalación en Windows con WSL (Recomendada) 🐧**

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

Conectando a docker vía terminal

  `docker-compose exec postgres bash`

  `psql -h localhost -d my_store -U nico`

 Para ver estructura del DB

  `\d+`

  Para salir de la base datos

  `\q`
  
  Luego para salir del contenedor

  `exit`


## Explorando Postgres: interfaces gráficas

```jsx

  pgadmin:
    image: dpage/pgadmin4
    environment:
    - PGADMIN_DEFAULT_EMAIL=admin@mail.com
    - PGADMIN_DEFAULT_PASSWORD=root
    ports:
      -5050.80

```

Levantamos el servicio de pgadmin

  `docker-compose up -d pgadmin`

Luego abrimos en el browser 

`http://localhost:5050/`

En la terminal checkamos ip del docker 

`docker ps`

Inspeccionar ip con el comando: 7197f6a352ab (id del docker)

`docker inspect 7197f6a352ab`


## Integración de Node-postgres

### Instalar

´$ npm install pg´

```jsx
CREATE TABLE task (
	id serial PRIMARY KEY,
	title VARCHAR ( 250 ) NOT NULL,
	completed boolean DEFAULT false
);
```

### Instalar dotenv

`npm i dotenv`


## Sequelize

### ¿Qué es ORM? Instalación y configuración de Sequalize ORM

Un ORM es un modelo de programación que permite mapear las estructuras de una base de datos relacionales.

Sequalize

 `npm install --save sequalize`

 `npm install --save pg pg-hstore # Postgres`


## Cambiando la base datos a mysql

En el archivo 'docker-compose' agregamos el siguiente código:

````jsx
mysql:
    image: mysql:5
    environment:
      - MYSQL_DATABASE=my_store
      - MYSQL_USER=nico
      - MYSQL_ROOT_PASSWORD=admin123
      - MYSQL_PORT=3306
    ports:
      - 3306:3306
    volumes:
      - ./mysql_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    environment:
      - MYSQL_ROOT_PASSWORD=admin123
      - PMA_HOST=mysql

    ports:
      - 8080:80
```

Luego leventamos la base de datos

  `docker-compose up -d mysql`

Una vez terminado el proceso lo podemos ver si está corriendo

`docker-compose ps`

Luego levantamos el phpmyadmin

`docker-compose up -d phpmyadmin`

Nuevamente verificamos que si está corriendo

`docker-compose ps`

y podemos ver funcionando 4 servicios: 2 bases de datos una en postgres y otra en msql Ademas tenemos 2 motores graficos uno en pgadmin y otro en phpmyadmin.

tenemos que instalar el driver

`npm install --save mysql2`

Editar el archivo .env DB_PORT=''
```jsx

PORT=3000
DB_USER='root'
DB_PASSWORD='admin123'
DB_HOST='localhost'
DB_NAME='my_store'
DB_PORT='3306'

```
Cambiamos el motor de base de datos en el archivo sequelize.js

```jsx
...
const URI = `mysql://${USER}:${PASSWORD}@${config.dbHost}:${config.dbPort}/${config.dbName}`;

//...
  dialect: 'mysql',
// ...
```

## Migraciones

Las migraciones son la forma en que Django propaga cambios en los modelos y los refleja en el esquema de bases de datos. - Django.

Las migraciones son como un sistema de control de versiones para la base de datos. - Laravel.

Es como un sistema de control de versiones para manejar los cambios desde el código y trackear los cambios en la base de datos. - Sequelize.

Básicamente, las migraciones mantienen el historial del esquema que se lleva en la base de datos. Es un sistema muy usado en ambientes de producción para trackear los cambios sin tener que replicar todo nuevamente (creación de tablas, llaves foráneas, etc). Es decir, permite saber en qué punto estaba para saber qué es lo que se tiene que modificar.

para correr migraciones sequelize tiene una librería que vamos a utilizar.

`npm i sequelize-cli -D`o `npm i sequelize-cli --save-dev`

luego creamos una archivo llamada '.sequelizerc'  
