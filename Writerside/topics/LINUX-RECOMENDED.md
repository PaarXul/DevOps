# LINUX RECOMENDED

## 1.- permisos de IDE en Linux como root, es necesario para docker y otros servicios

``` Bash
sudo chown -R $USER:$USER /opt/idea-IC-213.6461.48

## Nos ubicamos en la carpeta del IDE por ejemplo o donde se encuentre el IDE 
cd ~/.config/JetBrains$
## Luego ejecutamos el siguiente comando
sudo chmod -R 755 .
```