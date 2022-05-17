# PRACTICA DOCKER - NGINX

Se debe desplegar un HTML sobre un servidor web dockerizado Nginx.; 
el HTML debe contener vuestro nombre, como es habitual, 
deberá ser documentado en un fichero README.MD de un repositorio de github.

## 1 ¿QUE ES?

-DOCKER:Docker es un proyecto de código abierto que automatiza el despliegue de aplicaciones dentro de contenedores de software, proporcionando una capa adicional de abstracción y automatización de virtualización de aplicaciones en múltiples sistemas operativos.

-NGINX: (pronunciado en inglés «ényin-ex», /ˈɛndʒɪn-ɛks/)2​ es un servidor web/proxy inverso ligero de alto rendimiento y un proxy para protocolos de correo electrónico

## 2 INSTALACIÓN

Iniciamos nuestro terminal de linux nos autenticamos como usuario administrador o superusuario y una vez hecho, hacemos un "docker pull nginx" para descargarnos el contenedor de nginx. tal y como se informa en la siguiente imagen.

<img src="https://i.gyazo.com/ecf5f02f3f7f5081520a3358be307cfe.png">

Con el siguiente comando ejecutamos la imagen de nginx.

```docker run --rm -d -p 8080:80 --name web nginx```

Con este comando comienza la ejecución del contenedor como un demonio  ```-d``` y publica el ```puerto 8080``` en la red del host. También llamó web al contenedor usando la opción ```--name```.

<img src="https://i.gyazo.com/8cb121c513b5f9272f73dc125accc3c5.png">

Visitamos http://localhost:8080 y comprobamos si teneomos listo nginx.

<img src="https://i.gyazo.com/4d7b1011ba38e40013c2b06cff699033.png">

Lo sabemos por que estamos viendo la pagina por defecto de nginx.

A continuacion pararemos el contenedor web con el comando

```docker stop web```

<img src="https://i.gyazo.com/51425dbae931e4070c29ee30a3a55c51.png">

## AGREGAR NUESTRO HTML

De forma predeterminada, Nginx busca en el directorio ```/usr/share/nginx/```html dentro del contenedor los archivos para servir. Necesitamos poner nuestros archivos html en este directorio.

Creamos una página html personalizada y luego la publicamos usando la imagen Nginx.

<img src="https://i.gyazo.com/ec89598f1243491e8db843edfbdb6fc9.png">

Creamos un directorio en Documentos llamado nginx, y dentro de este otro directorio llamado site-content.

<img src="https://i.gyazo.com/906839e07365e6893e13dd90c03c3549.png">

<img src="https://i.gyazo.com/1b2da77eeee53b627821d996627d47bb.png">

Ahora ejecuta el siguiente comando, que es el mismo que el anterior, pero ahora agregamos la marca ```-v```para crear un volumen y la direccion de nuestro html, en total queda asi:

```docker run --rm -d -p 8080:80 --name web -v ~/Documentos/nginx/site-content:/usr/share/nginx/html nginx```

<img src="https://i.gyazo.com/85d306e948a1ce0c07181d4b218224ef.png">

Y finalmente tenemos el servidor enseñando lo que queremos

<img src="https://i.gyazo.com/d24b4789e60c3979fbf14cf922aa8965.png">

## IMAGEN PERSONALIZADA

Para crear una imagen personalizada, necesitaremos crear un Dockerfile para posteriormente añadirle nuestras ordenes. Para esto, crearemos un archivo llamado Dockerfile dentro de site-content para luego agregarle el comando:

FROM nginx:latest

COPY ./site-content/index.html /usr/share/nginx/html/index.html



Una vez hecho esto, ejecutamos el siguiente comando:

docker build -t webserver .


Para finalizar, haremos un docker run, pero esta vez usando la imagen que hemos creado.

```docker run --rm -d -p 8080:80 --name web webserver```