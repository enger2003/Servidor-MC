# Servidor-MC
Instrucciones para preparar un servidor de minecraft en linux

Antes de nada, estas instrucciones están preparadas para el montaje en un sistema "linux", en mi caso se va a montar en una Raspberri Pi 5, por lo que usaremos "ssh" para conectarnos de fora cómoda.

Trabajaremos con las opciones básicas a su vez que podremos gestionar algunos errores referentes a java y preparar scripts para poder modificar, apagar y encender el servidor de forma remota
```
.
├── 📄 README.md                <-- Confiugración y arranque del servidor
├── 📄 VPN.md                   <-- Instalación y configuración VPN (Zero Tier)
├── 📄 VPN.md                   <-- Monitorización remota ()
├── ⚙️ restart.sh               <-- Script para reiniciar servidor Linux
├── 🔧 server.properties        <-- Configuración principal (explicación de cada campo)
```

## Descarga del servidor
Se tiene que descargar desde la página oficial de mojang, ya sea buscando "minecraft server mojang" o aquí -> https://www.minecraft.net/es-es/download/server

## Instalación java
Tenemos que instalar la versión correcta de java. Para ver la versión instalada simplemente abrimos una terminal de linux y pegamos el siguiente comando.
```
java --version
```

En nuestro caso vamos a instalar java 25, ya que a fecha de creación de este repositorio no se puede instalar java 25 de forma cómoda, lo haremos delegando en otro repositorio
```
sudo apt update
sudo apt install -y wget apt-transport-https gpg
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo gpg --dearmor -o /usr/share/keyrings/adoptium.gpg

sudo apt update
sudo apt install temurin-25-jdk
```

## Arranque inicial del servidor
Ahora con java instalado, procedemos a arrancar el servidor previamente descargado [Miencraft Server](https://www.minecraft.net/es-es/download/server)

```
java -Xmx4G -Xms4G -jar minecraft_server.26.3.jar nogui
```

Vamos a separar este comando en varias partes:
* -Xmx4G: Significa la ram **máxima** que queremos darle al servidor expresada en Gigabytes (en este caso 4)
* -Xms4G: Significa la ram **minima** que queremos darle al servidor expresada en Gigabytes (en este caso 4)
* -jar: Opciones para indicarle que es un archivo .jar
* minecraft_server.26.3.jar: El nombre del servidor (el archivo)
* nogui: Sin interfaz gráfica, solo terminal

Si quisieramos por ejemplo ponerle 6 Gigabytes de ram mínima y 8 Gigabytes de ram máxima, y el archivo se llamara "server.jar" usaríamos el siguiente comando

```
java -Xmx8G -Xms6G -jar server.jar nogui
```

## Arranque servidor

Tras realizar el arranque inicial, se nos crearán varios archivos

```
java -Xmx4G -Xms4G -jar minecraft_server.26.3.jar nogui
```

1) El servidor se cerrará y tendrás que aceptar/activar "eula". Dentro de los archivos creados es cambiar un campo de "false" a "true", estos cambios los haremos con "nano" pero se pueden hacer con cualquier editor de texto, la aclaración es por si hace falta la instrucción inicial para como salir de la edición de "nano" :)

```
sudo apt install nano
nano eula.txt

("Ctrl + X" para salir, "Y" para decirle que se guarde, "enter" para que se quede con el nombre)
```

2) Una vez cambiado y guardado el archivo, arrancamos de nuevo el servidor (java -Xmx4G -Xms4G -jar minecraft_server.26.3.jar nogui). El servidor se abrirá y se crearán los archivos necesarios para su funcionamiento. Entre los archivos creados hay uno llamado "server.properties", en este repositorio he puesto uno de prueba (el oficial hasta la fecha) para poder verlo

En este archivo es donde se hacen las modificaciones del mundo (hay que reiniciar el servidor para que se apliquen), dificultad de la partida, pvp, altura max, whitelist y blacklist, etc...
A su vez encontraremos las opciones necesarias de Ip, rcon y demás para poder configurarlo para la conexión

3) Ahora mismo el servidor está operativo y listo para entrar **estando en la red Local**, pero queremos que se pueda conectar cualquiera desde donde sea, para lo cual usaremos una VPN a elegir por el usuario (todos los jugadores la misma, al igual que la del servidor) Zero Tier(recomendada), Hamachi, RadminVPN... Tiene que ser una que se pueda instalar tanto en linux como en el sistema operativo de los clientes (normalmente windows)

4) Con esto listo (lo puede probar primero el que hace el servidor en local sin vpn para ver que todo funciona), se abre el cliente de Minecraft en la versión especificada en la que se ha creado.
Añadir servidor:
    - Nombre: Indiferente
    - IP: La ip de la máquina donde se ejecute, como se mira esto, al estar en linux, se usa el comando
    ```
    ifconfig
    ```
    ![imagen_ifconfig](./img/ifconfig.png)

    ```
    Pequña aclaración: las 2 marcas rojas son las interfaces, si tienes 2 tarjetas de red y una tarjeta  
    Bluetooth te deberían salir mínimo 3 interfaces, la ip que tienes que buscar es la de la interfaz  
    que estés usando, si estas conectado por "ethernet", por cable, deberás buscar en la de "ethernet".  
    Igualmente solo deberías poder ver una ip válida, la cual se encuentra en el recuadro azul  
    correspondiente a su interfaz.
    ```

Los clientes (con la VPN activa o estando en local) deberán esa ip para ponerla en los servidores para poder conectarse. Con esto hecho, el servidor está listo para funcionar cuando se abra :)