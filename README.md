**Curso de VPS y NginX con Ubuntu 14.4**

**Inicio:**

- Creación del Droplet Ubuntu.
- Asociación del Dominio y los DNS.

**Conexión SSH:**

- Usuario por defecto &quot;root&quot;.
- Clave del usuario: enviada por **email**. ¡En la primera conexión pide el cambio de contraseña!

**Comando conexión SSH:**

$ ssh usuario@&quot;host-ip&quot; or &quot;Domain&quot;

**Configuración del dominio:**

En el proveedor del dominio ejemplo: **GoDaddy**. configurar los **DNS** del servidor VPS dentro del proveedor de servicio. Ejemplo: **ns1.digitalocean.com**

**Archivo de Zona:**

En **DIgitalOcean** hay que declarar y configurar el dominio con la herramienta de creación del archivo de Zona. ¡Se agregarán el dominio principal **ejemplo.com** y se asigna al VPS correspondiente!

Luego se registran los sub dominios como por ejemplo el www:

- Seleccionamos **CNAME**.
- En NAME colocamos: &quot; **www**&quot;
- En Hostname colocamos: el dominio principal con un punto al final **&quot;ejemplo.com.&quot;**

Configuración de respuesta a cualquier dominio:

- Seleccionamos **CNAME**.
- En NAME colocamos: **&quot;\*&quot;**
- En Hostname colocamos: el dominio principal con un punto al final **&quot;ejemplo.com.&quot;**

Aquí NGINX se encargaría de verificar si tiene que responder o no la solicitud.

**Seguridad básica del VPS:**

**Actualización del sistema:**

- ¡Comando **apt-get** , Comando para actualización del sistema! (Ubuntu)
- Comando **apt-get update** , comando para actualización de los repositorios del sistema. ¡Los repositorios de donde se descargarán las actualizaciones!
- ¡Comando **apt-get upgrade** , comando para actualizar el software del VPS según lo último que tengamos en los repositorios!
- Comando **apt-get install** , comando para la instalación de un paquete nuevo.
- Comando **apt-get remove** , comando para desinstalar un paquete ya instalado.
- Comando **apt-get autoremove** , comando para remover software que ya no se utiliza en el VPS.
- Comando **apt-get purge** , comando para desinstalar software del sistema y quitar también los archivos de configuración de ese software que estamos desinstalando.

**Manejo de servicios:**

Se usa el comando siguiente:

$ service NAME\_SERVICE status|start|stop|restart|reload

**Gestión de Usuarios:**

- Agregar un usuario: **adduser NAME\_USER** este comando agrega una carpeta en la ruta / **home** con el nombre del usuario
- Eliminar usuario: **deluser NAME\_USER** este comando elimina el usuario, pero no remueve la carpeta del usaurio de la ruta / **home**.

**Permisos Administrativos:**

Para dar permisos administrativos a un usuario y que el mismo pueda usar el comando &quot; **sudo**&quot; debemos usar el siguiente comando desde la cuenta **root** :

$ _sudo adduser NAME\_USER sudo_

**Generación de cables SSH:**

Para poder conectarnos a servicio como control de versiones entre otros, podemos usar claves **SSH**. Así no tenemos que exponer nuestros datos de usuario a la red.

El comando para generar las claves es el siguiente:

$ _ssh-keygen –t rsa –b 4096 –C &quot;USER\_NAME&quot;_

Este comando crea un directorio dentro de la carpeta del usuario llamada &quot;. **ssh**&quot;, en donde se generan dos archivos que pertenecen a la clave privada ( **id\_rsa** ) y la clave pública ( **id\_rasa.pub** ) asociada al usuario.

**Habilitación y configuración firewall:**

- Debemos corroborar si el **ufw** está instalado y actualizado en el sistema.
- Configuramos que deniegue todas las conexiones entrantes, pero sin habilitar el **ufw**.
- Comando: **$ sudo ufw default deny incoming**
- Configuramos que todas las conexiones salientes estén habilitadas.
- Comando: **$ sudo ufw default allow outgoing**
- Habilitar las conexiones SSH.
- Comando: **$ sudo ufw allow ssh**
- Habilitar el **ufw** :
- Comando: **$ sudo ufw enable**
- Habilitar conexión **http** :
- Comando: **$ sudo ufw allow www**
- Habilitar conexiones por **SSL** :
- Comando: **$ sudo ufw allow 443**
- Verificar estado del **ufw** :
- Comando: **$ sudo ufw status**

**Securizacion de base de datos MySQL:**

Por defecto en la instalación de **MySQL** en su instalación estándar deja algunas configuraciones que no son necesarias para el funcionamiento y que pueden comprometer la seguridad del VPS y la base de datos. Por ejemplo, se elimina:

- usuarios anónimos.
- Se elimina la posibilidad de conexión remota del usuario root.
- Se eliminan las bases de datos de test y el acceso a ellas.
- Se recarga los privilegios para las tablas actuales.

Comando: **$ sudo mysql\_secure\_instalation**

**Diferencias entre chmod y chown:**

El comando &quot; **chmod**&quot; sirve para dar permisos de lectura, escritura y ejecución a un fichero o carpeta, estos permisos son aplicados a todos los usuarios del VPS. No es aconsejable utilizar este comando a menos que estemos seguros de lo que estamos haciendo.

Ejemplo: **$ chmod 777 prueba/** (damos todos los permisos de acceso a todos los usuarios sobre la carpeta prueba)

El comando &quot; **chown**&quot; sirve para dar permisos de lectura, escritura y ejecución a un fichero o carpeta, pero solo a un usuario determinado pudiendo así asignar granularmente los permisos del sistema de ficheros y ejecución a un único usuario. Esto desde el punto de vista de seguridad es más optimo que hacerlo a todos los usuarios.

Ejemplo: **$ chown –R USER\_NAME prueba/** (damos permisos al usuario de manera recursiva al árbol de ficheros que cuelgue de la carpeta prueba)

El usuario administrador por el cual corre el servidor tipo apache o nginx de manera estándar es &quot; **www-data**&quot;. En caso que este usuario necesite permisos para de lectura, escritura y ejecución dentro de una carpeta especifica podemos usar el comando &quot; **chown**&quot; para asignar estos permisos sin comprometer la seguridad del VPS.

**NginX:**

Estructura de carpetas:

Dentro de la carpeta &quot; **etc**&quot; en la raíz del VPS podemos encontrar la siguiente estructura para el servidor de **Nginx** :

- etc/
  - nginx/
    - nginx.conf
    - sites-available/
    - sites-enabled/

**nginx.conf** : archivo de configuración principal del servidor.

**sites-available** : carpeta de sitios disponibles para el servidor. Aquí se guarda todos los archivos de configuración de los dominios configurados para que sirva el servidor.

**sites-enabled** : carpeta de sitios habilitados para el servidor. Esta carpeta contiene enlaces simbólicos de los archivos que están en la carpeta **sites-available.** Todos los enlaces que se encuentren en esta carpeta serán los sitios que el servidor podrá servir, si no están incluidos en esta carpeta de **sites-enabled** el servidor no servirá nada que no esta en esta carpeta.

**Los Logs del servidor:**

Nginx tiene dos tipos de archivos de logs está el de tipo acceso y el de errores.

La ruta donde están serian:

Log de Acceso: **/var/log/nginx/access.log**

Log de errores: **/var/log/nginx/error.log**

Comando para verificar la sintaxis de los archivos de configuración del servidor **NginX:**

$ _sudo nginx -t_

Habilitar **Gzip** en la configuración de los servidores **:**

Con la configuración siguiente podemos configurar la compresión del algoritmo **Gzip** para de esta manera ahorrar ancho de banda en el servidor según los datos que enviemos.

**# Gzip Settings**

**##**

**gzip on;**

**#gzip\_disable &quot;msie6&quot;;**

**gzip\_disable &quot;MSIE [1-6].(?!.\*SV1)&quot;; #Disable compression for MSIE and SVG fonts**

**gzip\_vary on;**

**gzip\_proxied any;**

**gzip\_comp\_level 4;**

**gzip\_buffers 16 8k;**

**gzip\_http\_version 1.1;**

**gzip\_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript font/ttf font/opentype application/vnd.ms-fontobject image/svg+xml;**

algunas de las configuraciones a tener en cuenta son:

**gzip\_comp\_level:** mientras más alto sea este valor más se incrementa el uso de **CPU** del **VPS** para la compresión de los archivos y más ancho de banda ahorramos en el servidor. Hay que tener en cuenta que para **VPS** pequeños de **CPU** este valor debe ser moderado.

**gzip\_buffers:** tamaño del buffer para generar la compresión de los archivos.

**gzip\_types:** aquí se configuran los archivos que van a ser comprimidos por el módulo de **Gzip.**

configurar caberas de expiración de archivos servidos:

con la siguiente configuración le decimos al navegador que cacheara todos los archivos con la extensión que estamos configurando:

**server
{
        location ~\* .(jpg|jpeg|png|gif|ico|css|js|eot|svg|ttf|woff)$
              {
                             expires 365d;
             }
}**

Configuración para evitar que el servidor Nginx envié la versión y el sistema operativo en la que se está ejecutando:

En el archivo de **nginx.conf** debemos descomentar la siguiente línea:

**server\_tokens off**

cuando esta comentado este parámetro en el navegador la cabecera de respuesta seria como la siguiente:

**Server:** **Nginx/1.2.6 (CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16**

Si lo descomentamos estaría de la siguiente manera:

**Server:**  **Nginx**

Configuración para mejorar la seguridad contra ataques de tipo **CSS o XSS:**

Agregamos las siguientes líneas en el archivo de **nginx.conf** :

**#Avoiding XSS, CSS and similar attacks**

**##**

**add\_header X-Frame-Options SAMEORIGIN;**

**add\_header X-Content-Type-Options nosniff;**

**add\_header X-XSS-Protection &quot;1; mode=block&quot;;**

**add\_header X-Frame-Options:** agrega esta cabecera en la respuesta al navegador indicándole que si la web está siendo agregada o mostrada en un elemento de tipo i-frame solo puede ser cargada desde el mismo sitio y no desde una web o dominio distinto.

**add\_header X-Content-Type-Options** : esta cabecera se relaciona con el tipo de respuesta enviada al navegador (texto, json, xml, html…), aquí le indicamos al navegador que solo va a tratar la respuesta como le indicamos a través del servidor y que no utilice ninguna otra que pueda estar utilizando un atacante.

**add\_header X-XSS-Protection:** aquí le decimos al navegador el tipo de protección debe usar contra ataques xss y en caso de que lo detecte que bloquee la carga del sitio.

Configuración para evitar o aplacar ataques de tipo de DOS o DDOS:

Agregamos las siguientes líneas en el archivo de **nginx.conf** :

**# DDoS Protection Settings**

**##**

**client\_body\_buffer\_size      128k;**

**large\_client\_header\_buffers  4 256k;**

**limit\_conn\_zone $binary\_remote\_addr zone=conn\_limit\_per\_ip:10m;**

**limit\_req\_zone $binary\_remote\_addr zone=req\_limit\_per\_ip:10m rate=1r/s;**

**limit\_conn conn\_limit\_per\_ip 15;**

**limit\_req zone=req\_limit\_per\_ip burst=20;**

**server\_names\_hash\_bucket\_size 64;**

**# server\_name\_in\_redirect off**

**client\_body\_buffer\_size:** limitamos el tamaño de las cabeceras enviadas al cliente.

**limit\_conn\_zone:** limita la cantidad de conexiones desde una misma ip.

**limit\_req\_zone** : aquí limitamos la cantidad de peticiones por segundo desde una misma ip.

**limit\_conn** : limitamos la cantidad de conexiones simultaneas desde una misma ip.

**limit\_req zone:** limita la cantidad de peticiones simultaneas en total desde una misma ip.

Para probar estas configuraciones podemos usar un programa que se encuentra en el framework Xamp que se llama **ab.exe** para poder probar conexiones y peticiones simultaneas que nuestro sitio puede soportar.

Comando desde windows:

$ _ab –t 30 –c 10 http://dominio.com_

Donde –t es el tiempo que dura la prueba y –c el nivel de concurrencia de la prueba.

**Obtención de Certificados de seguridad tipo SSL y TLS:**

**Lets Encrypt:** es un proyecto de código abierto para generación y validación de certificados SSL y TLS de manera automática y gratuita. Se pueden generar certificados para uno a varios dominios con una valides de hasta 90 días.

SSL o TLS: ambas son tecnologías para cifrado de conexiones http, **SSL** (Secure Sockets Layer protocol) es una tecnología más antigua pero que sigue en vigencia, aunque sea actualmente vulnerable a varios tipos de ataques como Heartbleed es la más difundida y extendida, aunque ahora mismo está siendo reemplazado por **TLS** (Transport Layer Security) el cual usa distintos protocolos criptográficos para mantener la comunicación cifrada.

Para la instalación en VPS debemos clonar el repositorio desde GitHub en caso de que yo esté instalado en el VPS:

$ sudo apt-get update

$ sudo apt-get install software-properties-common

$ sudo add-apt-repository ppa:certbot/certbot

$ sudo apt-get update

$ sudo apt-get install python-certbot-nginx

Si en el VPS ya tenemos Lets Encrypt instalado podemos usar el siguiente comando para la generación de los certificados SSL para nuestro servidor en Nginx:

$ sudo Letsencrypt certonly - -non-interactive - -email [email@email.com](mailto:email@email.com) - -agree-tos - -webroot –w /path/to/web/ -d domain.com

**Certonly:** le indicamos al comando que solo deseamos generar los certificados sin instalarlos en la configuración de nuestro servidor.

**Non-interactive** : aquí le decimos al comando que deseaos una instalación desatendida, pero para que este argumento tenga efecto debemos proporcionar información como argumento del comando, en este caso el email ya que este email será el encargado de asociar los certificados a nuestra cuenta de correo.

**Ogree-tos** : indicamos que aceptamos todos los términos y condiciones de lets encrypt.

**Webroot** : la ruta física dentro de nuestro VPS en donde tendrá acceso para la verificación del dominio.

- **-w** : path de la web.
- **-d** : nombre del dominio.

Los certificados deberán generarse dentro de la carpeta de instalación de lets encrypt:

Deberíamos tener una carpeta nueva llamada **live** y otra que ya estaba llamada **keys:**

**Keys:** debieron crearse dos archivos los cuales serían los certificados de nuestro dominio. En esta carpeta siempre se guardaran todos los certificados que generemos a lo largo del tiempo.

**Live** : tendremos una sub carpeta con el nombre del dominio puesto como argumento en el comando de generación de los certificados. En esta carpeta siempre estarán los certificados vigentes para nuestro servidor.

- **Sub-carpeta example.com** : aquí encontraremos los siguientes archivos:
  - **oo**** Cert.pem:**
  - **oo**** Chain.pem:** cadena del certificado.
  - **oo**** Fullchain.pem:** cadena completa de los certificados en caso que tengamos sub dominios.
  - **oo**** Privkey.pem:** llave privada del certificado.

**Configuración de los certificados dentro de cada uno de los archivos de configuración de los dominios:**

Dentro de los archivos de configuración de los dominios en la carpeta **site-available** debemos poner estas líneas de código:

**listen 443 ssl;
ssl\_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
ssl\_certificate\_key /etc/letsencrypt/live/example.com/privkey.pem;

 ssl\_protocols TLSv1 TLSv1.1 TLSv1.2;
 ssl\_prefer\_server\_ciphers on;
 ssl\_ciphers &#39;EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH&#39;;**

**listen 443 ssl:** declaración del puerto y tipo de protocolo a utilizar SSL.

**ssl\_certificate:** ruta del certificado a utilizar.

**ssl\_certificate\_key:** ruta de la llave del certificado a utilizar.

**ssl\_protocols:** protocolos soportados por el servidor.

**ssl\_prefer** \_server\_ciphers: habilitación del cifrado en la configuración.

**ssl\_ciphers:** tipo de cifrado soportados.
