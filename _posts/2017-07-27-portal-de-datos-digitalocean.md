---
title: 'Cómo configurar tu propio portal de datos abiertos DKAN en DigitalOcean'
layout: post
date: '2017-07-27'
permalink: /blog/setup-dkan-digitalocean/
published: true
share-img: "/img/posts/portaldatos/opendata_graph_PC.png"
tags:
- Español
- Spanish
- open-data
- DKAN
- DigitalOcean
- linux
- MySQL
- PHP
- Apache2
---

> Si necesitas ayuda configurando tu propia plataforma de datos abiertos, un acompañamiento detallado de estas instrucciones, o solo consultoría en general sobre análisis de datos, estaremos a disposición de ofrecerte este [servicio]({{ site.url }}/aboutme/#contact).

Si has buscado en internet y no has hallado la forma de configurar tu propia plataforma de publicación, análisis y visualización de datos abiertos, [DigitalOcean](https://m.do.co/c/b8c84bc5d55a) (DO) y [DKAN](http://getdkan.com/) son un buen primer paso para configurarla. Por ejemplo, el [Portal de datos abiertos GANA de la Gobernación de Nariño](https://datos.narino.gov.co/) fue configurado utilizando DKAN como plataforma de publicación de datos.

DigitalOcean provee servidores virtuales privados "en la nube" llamados *droplets*. Por *$5 dólares al mes* puedes tener tu propio servidor para alojar en él lo que quieras y acceder a él donde quieras. Utiliza mi [link de referidos](https://m.do.co/c/b8c84bc5d55a) para obtener $10 dolares en crédito, suficientes para mantener un servidor con DKAN durante un mes.

[DKAN](http://getdkan.com/) es una plataforma para publicar, contar historias y graficar datos abiertos. Es un proyecto de código abierto basado en Drupal y liderado por [Granicus](https://granicus.com/). Puede instalarse y publicarse en un droplet (recuerda: droplet = tu máquina/servidor en la nube) de DO.

Este blog cubrirá la instalación y configuración de un portal de datos abiertos usando DKAN en un servidor Ubuntu 14.04.05 x64 (Linux) con el fin de apoyar entidades gubernamentales, ONG's y personas naturales en su compromiso de publicar datos abiertos.

# Tabla de Contenidos

- [Paso 1: Regístrate en DigitalOcean](#registro)
- [Paso 2: Crea tu propio droplet](#crea-droplet)
- [Paso 3: Loggeate en tu propio servidor](#loggeate)
- [Paso 4: Asegúrate de no estar usando el usuario root](#usuario-root)
- [Paso 5: Aumenta los protocolos de seguridad](#protocolos-seguridad)
- [Paso 6: Accede a tu servidor desde un navegador](#accede-navegador)
  - [Nota 6.1: Referencias rápidas de Apache2](#referencias-apache2)
- [Paso 7: Instala MySQL, PHP y Git](#instala-LAMP)
- [Paso 8: Configura MySQL y Apache2](#configura-LAMP)
  - [Paso 8.1: MySQL](#configura-mysql)
  - [Paso 8.2: Apache2](#configura-apache2)
- [Paso 9: Descarga el DKAN](#descarga-dkan)
- [Paso 10: Prepara la instalación del DKAN](#prepara-dkan)
- [Paso 11: Instala el DKAN](#instala-dkan)
- [Paso 12: Actualiza los módulos de DKAN](#actualiza-dkan)
- [Paso 13: Cambia los permisos](#cambia-permisos)
- [Recursos](#recursos)
- [Exoneración de responsabilidades](#exoneracion)


# Paso 1: Regístrate en DigitalOcean {#registro}

Ve a [DigitalOcean](https://m.do.co/c/b8c84bc5d55a) (¡usa este link para obtener tus créditos!) y regístrate. Usualmente el registro es rápido y al final, para verificar tu cuenta (y obtener tus 10 dólares), debes proveer información de tarjeta de crédito. Si hiciste bien, verás en la seccion ["Billing"](https://cloud.digitalocean.com/settings/billing) tu crédito.

# Paso 2: Crea tu propio droplet {#crea-droplet}

Configurar tu propia máquina es tan sencillo que ni siquiera necesitas mis instrucciones. Dale click en el botón "Create Droplet" y escoge tu configuración. Para DKAN, es recomendable utilizar la máquina de mínimo 1 GB de RAM, es decir la de 10 dólares. La ubicación de tu servidor también es relativa, en este ejemplo se escogió San Francisco. En cuanto al sistema operativo prefiero utilizar Ubuntu 14.04.05 x64. Te recomiendo agregar una [llave SSH](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets) en el último paso si sabes cómo hacerlo. Sino, encuentra un tutorial [aquí](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-putty-on-digitalocean-droplets-windows-users) o procede sin la llave SSH. 

*Nota: los siguientes pasos asumen que estarás usando las mismas configuraciones del servidor recomendadas. Si escogiste una configuración diferente, las instrucciones en general aplicarán. Sin embargo, algunos comandos específicos o URLs podrían requerir cambios.*

La explicación de cómo configurar tu propio servidor en DO está también en inglés gracias a un tutorial de Dean Attali sobre [cómo crear tu propio servidor  Shiny Server](http://deanattali.com/2015/05/09/setup-rstudio-shiny-server-digital-ocean/). A continuación los pasos previamente descritos:

[![Crear droplet](http://deanattali.com/img/blog/digital-ocean/do-create.gif)](http://deanattali.com/img/blog/digital-ocean/do-create.gif)

# Paso 3: Loggeate en tu propio servidor {#loggeate}

Una vez tu droplet está listo, la plataforma te redireccionará a una página que te muestra información sobre el nuevo droplet, incluyendo su dirección IP. De ahora en adelante, usaremos la IP `123.123.1.2` como ejemplo. No olvides reemplazar este valor por la IP de tu nueva máquina.

Una opción para iniciar sesión en tu droplet es a través de la pestaña "Access" en la página a la que fuiste redireccionado. Sin embargo, es lenta y fea. Yo prefiero conectarme desde mi propia máquina. Si estás en una máquina UNIX, como es mi caso, puedes correr `ssh 123.123.1.2` en el terminal. Si eres usuario de Windows, puedes usar [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/) para hacer un puente (SSH) hacia tu servidor e iniciar sesión. Utiliza la dirección IP que aparece en tu página de DO con el nombre de usuario `root`. Si usaste una llave SSH entonces no necesitarás de una contraseña; de lo contrario, la contraseña debería haber sido enviada a tu email.

Deberías ser saludado con un mensaje de bienvenida que debería verse así:

[![Ventana de inicio de sesión](http://deanattali.com/img/blog/digital-ocean/login.png)](http://deanattali.com/img/blog/digital-ocean/login.png)

# Paso 4: Asegúrate de no estar usando el usuario root {#usuario-root}

El usuario raíz o `root` es el primero que se activa cuando se instala Ubuntu. Tiene todos los permisos dentro de la máquina y es una buena práctica limitar su uso a menos que sea estrictamente necesario. Vamos a adicionar el usuario "camilo" y le daremos poderes de _administrador_. Cuando crees este usuario, se te preguntará información básica: Nombre Completo, Teléfono, etc.

~~~
adduser camilo
gpasswd -a camilo sudo
~~~

De ahora en adelante me voy a loggear como "camilo" en vez de "root". Para correr comandos que requieran permisos del administrador, basta con anteponer `sudo` al comando. Vamos a intercambiar el usuario "root" por el usuario "camilo".

~~~
su - camilo
~~~

# Paso 5: Aumenta los protocolos de seguridad {#protocolos-seguridad}

Ahora que estás usando tu usuario, aumentamos los niveles de seguridad de tu plataforma:

1. Emparejando tu computador (cliente) con el servidor a través de una llave SSH para que sólo tu máquina se pueda conectar a él.
  - Creamos una llave de emparejamiento en __tu computador personal__ y la copiamos al portapapeles. Encuentra un manual para Windows, Mac y Linux [aquí](https://confluence.atlassian.com/bitbucketserver/creating-ssh-keys-776639788.html).
  - Creamos la carpeta `.ssh` en el __directorio personal de tu usuario ("camilo" en mi caso) en el servidor__: `mkdir -p /home/camilo/.ssh`
  - Copiamos la llave de tu computador personal en el registro de llaves autorizadas `authorized_keys` del servidor: `echo tu_llave_personal >> ~/.ssh/authorized_keys`
2. Autorizando el acceso al servidor exclusivamente a través de llaves SSH: `nano /etc/ssh/sshd_config`. Esto abrirá un editor de texto llamado "nano". Cambia la linea `PasswordAuthentication yes` por `PasswordAuthentication no`. Una vez realizada la modificación, salimos del editor de texto oprimiendo `Control (Command) + X`, luego `Y` para guardar los cambios y finalmente `Enter` para confirmar el nombre del archivo.
3. Deshabilitando la contraseña del usuario `root` para limitar su acceso: `nano /etc/ssh/sshd_config` y cambias la linea `PermitRootLogin yes` por `PermitRootLogin no`. Guardar y salir.

# Paso 6: Accede a tu servidor desde un navegador {#accede-navegador}

En estos momentos si visitas `http://123.123.1.2` en un navegador, encontrarás algo parecido a "Error: página no disponible". Vamos a habilitar el servicio de páginas web de Apache2. 

~~~
sudo apt-get update
sudo apt-get -y upgrade
sudo apt-get install -y apache2
~~~

Ahora, si visitas `http://123.123.1.2`, deberías poder ver el mensaje de bienvenida de Apache2. ¡Felicitaciones!

## Nota 6.1: Referencias rápidas de Apache2 {#referencias-apache2}

El archivo que carga el navegador por defecto se encuentra en la carpeta `/var/www/html/` de tu droplet. Para modificar la página web basta con editar el archivo "index.html" usando el comando `sudo nano var/www/html/index.html`. El archivo `/etc/apache2/apache2.conf` es el archivo global de configuración de Apache2.

Cuando editas un archivo HTML, podrás ver los cambios inmediatamente cuando refresques la página. Si haces cambios en la configuración, tendrás que reiniciar Apache2. En el futuro, puedes reiniciar/detener/iniciar Apache2 con:

~~~
sudo service apache2 restart
sudo service apache2 stop
sudo service apache2 start
~~~

# Paso 7: Instala MySQL, PHP y Git {#instala-LAMP}

DKAN viene con una distribución core de Drupal. Esto quiere decir que con instalar DKAN se instala Drupal. Para correrlo, es necesario primero tener 1 sistema operativo y 3 softwares catalogados como [LAMP](https://clients.javapipe.com/knowledgebase/133/Installing-Apache-MySQL-PHP-LAMP-on-Ubuntu-1404.html) (Linux, Apache2, MySQL y PHP). Usaremos 1 software adicional para descargar DKAN llamado Git. Tendremos entonces 5 conceptos:

1. __Linux__ es la base en la que corre nuestro sistema operativo Ubuntu 14.04.05 x64. Ya está. 
2. __Apache2__ es un software que habilita ver el contenido de un servidor desde el navegador. 
3. __MySQL__ es un software (motor) de base de datos y servirá para almacenar [datos estructurados](http://smarterworkspaces.kyocera.es/blog/diferencia-datos-estructurados-no-estructurados/) de la plataforma.
4. __PHP__ es un [lenguaje de programación](http://php.net/manual/en/intro-whatis.php) diseñado y usado especialmente para ejecutar, desde el servidor, código embebido en archivos HTML.
5. __Git__ es un software para controlar y versionar código. Lo usaremos únicamente para descargar DKAN desde una carpeta pública en Github. Conoce más sobre GitHub [aquí]({{ site.url }}/2017-05-14-github-red-social-meritocracia/).

~~~
# Instala MySQL
sudo apt-get install -y mysql-server

# Instala PHP
sudo apt-get install -y php5-mysql php5 libapache2-mod-php5 php5-mcrypt php5-gd php5-curl libssh2-php

# Instala Git
sudo apt-get install -y git
~~~

Durante la instalación de MySQL, digita la contraseña que quieras usar para el usuario raiz (`root`) de tu motor de base de datos.

# Paso 8: Configura MySQL y Apache2 {#configura-LAMP}

Luego de instalar LAMP y Git, configuramos MySQL y Apache2. 

## Paso 8.1: MySQL {#configura-mysql}

Creamos un directorio de base de datos y aumentamos los protocolos de seguridad removiendo cuentas de usuario anónimas y bases de datos de prueba, deshabilitando la conexión remota al usuario root y recargando las tablas de privilegios. Los comandos de respuesta en su orden son: `n`, `Y`, `Y`, `Y`:

~~~
sudo mysql_install_db
sudo mysql_secure_installation
~~~

Abrimos MySQL y creamos una nueva base de datos. Aquí digitas la clave del usuario `root` de MySQL. ¡No olvides el __punto y coma `;` al final de cada línea__ en MySQL!

~~~
# Iniciamos sesión en MySQL desde el terminal
mysql -u root -p

# En MySQL, creamos la base de datos dkan
CREATE DATABASE dkan;

# Creamos el usuario "dkan" con su respectiva contraseña "password123"
CREATE USER dkan@localhost IDENTIFIED BY 'password123';

# Habilitamos los permisos necesarios para el usuario en la base de datos
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,INDEX,ALTER,CREATE TEMPORARY TABLES,LOCK TABLES ON dkan.* TO dkan@localhost;

# Reiniciamos el sistema
FLUSH PRIVILEGES;

# Salimos del sistema
QUIT;
~~~

## Paso 8.2: Apache2 {#configura-apache2}

Por defecto, cuando un navegador consulta el servidor, el primer archivo que busca Apache2 es `index.html`. Si este no existe, busca archivos con el mismo nombre "index" que tenga las extensiones `.cgi`, `.pl`, `.php`, `.xhtml` y `.htm`, en ese orden. En vista que DKAN corre sobre PHP, el primer archivo que Apache2 debería abrir es "index.php". Editamos entonces el archivo `dir.conf` del Apache2 para que "index.php" abra antes que "index.html".

~~~
sudo nano /etc/apache2/mods-enabled/dir.conf
~~~

El texto debería verse así: `DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm`. Para  guardar y salir, se oprime la secuencia de comandos: `Control (Command) + X`, `Y` y `Enter`.

Como buena práctica, se recomienda editar el archivo `php.ini` de la ruta `/etc/php5/apache2/php.ini` para que limite código php y también la apertura de archivos desde una URL. 

~~~
sudo nano /etc/php5/apache2/php.ini
~~~

Oprime `Control (Command) + W` y digita "expose_php" para buscar esta palabra. Reemplazas "On" por "Off" y realizamos el mismo procedimiento de búsqueda a la variable "allow_url_fopen". Al final deberían verse así (no necesariamente en líneas seguidas):

~~~
expose_php = Off
allow_url_fopen = Off
~~~

Guardar y salir: `Control (Command) + X`, `Y` y `Enter`. Modificamos el archivo `.htaccess`:

~~~
sudo nano /etc/apache2/sites-enabled/000-default.conf
~~~

Y adicionamos estas lineas justo debajo de `DocumentRoot /var/www/html`:

~~~
  <Directory /var/www/html>
    AllowOverride All
  </Directory>
~~~

Guardar y salir. Habilitamos Apache2 para que pueda hacer "URL bonitas" (modificarlas para mejor entendimiento del usuario) y reiniciamos el servicio de Apache para refrescar los cambios.

~~~
sudo a2enmod rewrite
sudo service apache2 restart
~~~

# Paso 9: Descarga el DKAN {#descarga-dkan}

Descargamos en el servidor la distribucion DKAN usando Git y la movemos al directorio raíz de Apache2 para verlo desde el navegador:

~~~
sudo git clone --branch master https://github.com/nuams/dkan-drops-7.git dkan
sudo cp -a dkan/. /var/www/html/
~~~

Abre tu dirección `123.123.1.2`. Ya puedes ver que DKAN está listo para ser instalado en inglés. 

# Paso 10: Prepara la instalación del DKAN {#prepara-dkan}

![]({{ site.url }}/img/posts/portaldatos/error-instalacion.png "¡Aún falta modificar los permisos!")

Si te apresuraste a instalar el DKAN, te habrás dado cuenta que aún faltan pasos por hacer: 

- Descargar el paquete de instalación en español

~~~
sudo mkdir /var/www/html/profiles/dkan/translations
sudo wget -O /var/www/html/profiles/dkan/translations/drupal-7.56.es.po http://ftp.drupal.org/files/translations/7.x/drupal/drupal-7.56.es.po
~~~

- Copiar el archivo de configuración por defecto de Drupal: `sudo cp /var/www/html/sites/default/default.settings.php /var/www/html/sites/default/settings.php`.

- Cambiar los permisos de la carpeta "default" para DKAN tenga los permisos de lectura, escritura y ejecución necesarios para instalar la plataforma en el servidor: `sudo chmod 777 -R /var/www/html/sites/default`. Se modificarán más adelante.

Refresca tu página desde el navegador y verás la opción de instalación en español. Procede con la instalación.

# Paso 11: Instala el DKAN {#instala-dkan}

Un vez carga la página y la instalación inicia, ingresamos los parámetros de la base de datos que configuramos antes.

- Base de datos: `dkan`
- Usuario: `dkan`
- Contraseña: `password123`

![]({{ site.url }}/img/posts/portaldatos/setup-instalacion.png "Configuración de la base de datos")

Click en "Guardar y Continuar".

Digita la información de tu sitio web. El _Nombre del sitio_ va a ser el que aparece en la pestaña de tu navegador cuando cargue la página. Asegúrate de pasar también un correo electrónico válido. 

![]({{ site.url }}/img/posts/portaldatos/setup.png "Configuración de la página por primera vez")

Continúa con la creación del usuario y contraseña del administrador de la plataforma y finaliza la instalacion. 

# Paso 12: Actualiza los módulos de DKAN {#actualiza-dkan}

![]({{ site.url }}/img/posts/portaldatos/actualizaciones.png "Actualizaciones disponibles")

Revisa si hay módulos por actualizar. Estos aparecen en `http://123.123.1.2/admin/reports/updates/update`. Para realizar actualizaciones de módulos existen varias altenartivas, encontré tres: 

1. Conexión FTP o SSH desde la plataforma al mismo servidor. Esta alternativa implicaría ingresar las credenciales de acceso al servidor desde el navegador; haciéndolo vulnerable a ataques. Además, en nuestra configuración inicial restringimos por seguridad el acceso a la plataforma a través de nombres de usuario y contraseñas.
2. Colocar manualmente cada actualización en la carpeta `/sites/all/modules/` en el servidor. Según la documentación de DKAN, la plataforma primero leerá los módulos en esta carpeta y luego procederá a utilizar los módulos de la instalación. Esto implica riesgos asociados a la manipulación manual de módulos en el servidor desde las linea de comando.
3. Según [esta](https://www.drupal.org/node/1036494) discusión, la forma de automatizar la actualización de módulos desde el navegador, sin digitar credenciales, es cambiando el dueño de la carpeta "sites" y "themes" a Apache2 (`www-data`). 

~~~
sudo chown www-data: /var/www/html/sites/
sudo chown www-data: /var/www/html/themes/

# En algunos casos hay que aplicar cambios en los permisos de la carpeta "sites"
sudo chmod 777 -R /var/www/html/sites/
~~~

__¡Cuidado!__ Las actualizaciones de módulos pueden generar cambios en la plataforma que restringen su uso o limitan funcionalidades. Lee _muy bien_ qué cambios se van a ejecutar antes de actualizar la plataforma y asegúrate de [hacerle un backup a tu droplet](https://www.digitalocean.com/community/tutorials/digitalocean-backups-and-snapshots-explained) antes de comenzar.

# Paso 13: Cambia los permisos {#cambia-permisos}

El último paso para no vulnerar la seguridad en tu portal de datos es limitar los permisos de lectura, edición y ejecución en las carpetas que hayas modificado. 

~~~
sudo chmod 744 -R /var/www/html/sites/
sudo chmod 744 -R /var/www/html/themes/
~~~

¡Eso es todo!

# Recursos {#recursos}

A continuación una lista de las fuentes bibliográficas.

- [Dean Attali: How to get your very own RStudio Server and Shiny Server with DigitalOcean](http://deanattali.com/2015/05/09/setup-rstudio-shiny-server-digital-ocean/#host-rmd)
- [DigitalOcean: ssh keys](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-putty-on-digitalocean-droplets-windows-users)
- [Github: Connecting to GitHub with SSH](https://help.github.com/articles/connecting-to-github-with-ssh/)
- [Media Temple: Using SSH keys on your server](https://mediatemple.net/community/products/dv/204644740/using-ssh-keys-on-your-server)
- [How to Ubuntu: How to install LAMP on Ubuntu](http://howtoubuntu.org/how-to-install-lamp-on-ubuntu)
- [DKAN: Installation](http://dkan.readthedocs.io/en/latest/introduction/installation.html)
- [DigitalOcean: How To Install MySQL on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-14-04)
- [GitHub: DKAN Open Data Portal](https://github.com/NuCivic/dkan)
- [Drupal: Module installation requires FTP access?](https://www.drupal.org/node/1036494)
- [Bitbucket: Creating SSH keys](https://confluence.atlassian.com/bitbucketserver/creating-ssh-keys-776639788.html)

# Exoneración de responsabilidades {#exoneracion}

No soy un _sysadmin_ y muchas cosas en este tutorial fueron aprendidas en Google, por lo que puede haber maneras más rápidas de realizar la misma tarea. Si tienes comentarios sobre este documento, ¡me gustaría [escucharlos]({{ site.url }}/aboutme#contact)!
