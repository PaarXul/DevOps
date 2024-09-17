# Instalacion JAVA LINUX

Para Instalar JAVA en Linux se debe seguir los siguientes pasos:

``` Bash

#Descomprimimos el archivo tar.gz java17
tar -xvzf jdk-17.0.11_linux-x64_bin.tar.gz

https://download.oracle.com/java/17/archive/jdk-17.0.11_linux-x64_bin.tar.gz

#Movemos el directorio jdk-17 a /usr/local/
sudo mv jdk-17 /usr/local/

## agregamos el path de java al archivo .bashrc

```

# Agregar al archivo .bashrc 

``` Bash
#Accedemos al archivo .bashrc con el editor nano o VIM
nano ~/.bashrc

vim ~/.bashrc

```


### SE AGREGA ESTE PATH PARA EL ARCHIVO  bashrc ###
``` Bash
export JAVA_HOME=/usr/local/jdk-17
export PATH=$JAVA_HOME/bin:$PATH
```


### Luego de agregar el path de java al archivo .bashrc se debe ejecutar el siguiente comando

``` Bash    
source ~/.bashrc
```

