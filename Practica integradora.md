<h1> Resolución de la practica integradora

<h2> 0. Pasos Preliminares

<h3> Limpieza del disco duro

<br>

    
+ Borrar todo: 

  `docker system prune -a`
    
<h3> Limpiar Carpetas
<br>

    
+ Visualizar las carpetas:
    
    `ls`

+ Eliminar carpetas (una por una):
    
    `rm -r <Nombre de carpeta>` 
    
<h3> Limpiar Contenedores
<br>

+ Visualizar contenedores:
    
    `sudo docker ps`

+ Eliminar TODOS los contenedores:
    
    `sudo docker rm -f $(sudo docker ps -a -q)`

<h3> Limpiar Volumen
<br>
    
+ Visualizar volumen:
    
    `sudo docker volume ls`

+ Eliminar TODO los volumenes:
    
    `sudo docker volume prune`

<h3> Limpiar Imágenes
<br>

+ Visualizar Imágenes:

    `sudo docker image ls`

+ Eliminar TODAS las imagenes:

    `sudo docker image prune`

Verificar si está limpio maquina virtual (MV)
<br>

+ Visualizar espacio disponible:

    `df -h`

### Finalmente, Clonamos y levantamos los contenedores
Clonamos el repositorio, nos posicionamos en la carpeta y levantamos el compose
```
Clonar repositorio
    git clone https://github.com/lopezdar222/herramientas_big_data

Posicionamos en la carpeta:
    cd herramientas_big_data

Levantamos los contenedores con el compose
    sudo docker-compose -f docker-compose.yml up -d
```

### 1) HDFS
Trabajamos en el HDFS
```
Ejecutamos el contenedor, nos posicionamos ahí y abrimos una linea de comandos 
    sudo docker exec -it namenode bash          
    -- (sudo docker exec -it <nombre del contenedor> bash)

Nos posicionamos en la carpeta home
    cd home
    -- cd <nombre de carpeta>

Creamos el directorio Datasets
    mkdir Datasets
    -- mkdir <Nombre de directorio>

Salimos del contenedor
    exit

Copiamos el archivo hace la carpeta del contenedor
    sudo docker cp Datasets namenode:/home
    sudo docker cp <path><archivo> namenode:/home/Datasets/<archivo>
```
Ubicarse en el contenedor "namenode"
    sudo docker exec -it namenode bash
    -- sudo docker exec -it <nombre de contenedor> bash

Crear un directorio en HDFS llamado "/data".

```
    hdfs dfs -mkdir -p /data
    --  hdfs dfs -mkdir -p /<Nombre de carpeta>
```

Copiar los archivos csv provistos a HDFS:
```
    hdfs dfs -put /home/Datasets/* /data
    -- hdfs dfs -put <ruta del archivo>
```
Salimos del contenedor
    exit

## 2) Hive

Crear tablas en Hive, a partir de los csv ingestados en HDFS.

Para esto, se puede ubicar dentro del contenedor correspondiente al servidor de Hive, y ejecutar desdea allí los scripts necesarios


Desde herramientas Big data llevamos los archivos al contenerdor
    sudo docker cp Paso02.hql hive-server:/home
    sudo docker cp Paso03.hql hive-server:/home
    sudo docker cp Paso03.hql hive-server:/home
```

```
Nos posicionamos en Herramientas BigData
Vamos hacia el contenedor
    sudo docker exec -it hive-server bash

Vamos a la la carpeta /home
    cd /home
```
Este proceso de creación las tablas debe poder ejecutarse desde un shell script.

Nota: Para ejecutar un script de Hive, requiere el comando:
```
Cargamos las tablas:
    hive -f Paso02.hql
    hive -f Paso03.hql
    hive -f Paso04.hql
    -- hive -f <script.hql>

Retrocedemos para iniciar hive
    cd ..

Inicializamos Hive
    hive

Ahora podemos usar comandos SQL
    show databases;
    use integrador;
    show tables;
