# :construction: Práctica Integradora :construction:


<br>

## Índice

* [Contexto](#contexto)

+ [Creación y conexión de la máquina virtual](#hammer-creación-y-conexión-de-la-máquina-virtual-entorno-linux)

  * [Limpieza de la máquina virtual (opcional)](#limpieza-de-la-máquina-virtual-opcional)

* [Clonación del Repositorio](#calling-clonación-del-repositorio-inicio-y-ejecución-de-los-servicios-docker-composeyml)

* [HDFS](#floppy_disk-hdfs---hadoop-distributed-file-system)

* [Hive](#bookmark_tabs-hive)

* [Formatos de Almacenamiento](#file_cabinet-formatos-de-almacenamiento)

* [SQL](#computer-sql)

+ [No-SQL](#card_file_box-no-sql)
  * [H-base](#h-base)

  * [Mondodb](#mongodb)

  * [Neo4j](#neo4j)

+ [Spark](#magic_wand-spark)

  * [Spark y Scala](#spark-y-scala)

  * [Kafka](#kafka)

  * [Comparativa Datasets y Dataframe en Scala](#comparativa-datasets-y-dataframe-en-scala)

* [Carga Incremental con Spark](#hourglass-carga-incremental-con-spark)

* [Herramientas de Orquestación de flujo de datos](#abacus-herramientas-de-orquestación-de-flujo-de-datos)

* [Participantes y compañeros del pair programming](#people_hugging-participantes-y-compañeros-del-pair-programming)



<br>


### Contexto

Durante esta practica la idea es emular un ambiente de trabajo, desde un área de innovación solicitan construir un MVP(Producto viable mínimo) de un ambiente de Big Data donde se deban cargar unos archivos CSV que anteriormente se utilizaban en un datawarehouse en MySQl, pero ahora en un entorno de Hadoop.

Desde la gerencia de Infraestructura no están muy convencidos de utilizar esta tecnología por lo que no se asigno presupuesto alguna para esta iniciativa, de forma tal que por el momento no es posible utilizar un Vendor(Azure, AWS, Google) para implementar dicho entorno, es por esto que todo el MVP se deberá implementar utilizando Docker de forma tal que se pueda hacer una demo al sector de infraestructura mostrando las ventajas de utilizar tecnologías de Big Data.

<br>

## :hammer: Creación y conexión de la máquina virtual (entorno linux)


Luego de descargar Virtual Box y Putty desde sus páginas oficiales, se procede a la creación y configuración de un nuevo entorno virtual... 

<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Conexion_VB.png" alt="imagen de la creación de una nueva máquina virtual" width="650" height="420">
</p>

*Se selecciona el símbolo celeste para crear una NUEVA máquina virtual, y se asegura que tengamos habilitado el modo expero (se puede seleccionar el modo experto o modo guiado, en la barra menú que se encuentra en el borde inferior).*

<br>
<br>
<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Creacion_imagen_ubuntu.png" alt="imagen de creación de espacio en disco y montado de imagen ubuntu." width="650" height="420">
</p>

*Luego de colocarle nombre a la maquina virtual, se selecciona la pestaña de disco duro, y se selecciona la opción de Usar un archivo de disco duro virtual existente. En la ventana que se abre a continuación, se selecciona añadir, y se busca la imagen de linux ubuntu previamente descargada.*

<br>
<br>
<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Configuracion_servidor.png" alt="imagen de la configuración del servidor de la máquina virtual" width="650" height="420">
</p>

*Luego de haber montado la imagen del sistema operativo, se procede a la configuración del servidor. Ésta configuración radica en la cantidad de núcleos del procesador que se le asignará a la máquina virtual, asi como tambien la cantidad de GB de memoria RAM y se habilitarán los permisos y conexiones de red correspondientes para poder trabajar con el entorno local de windows.*

<br>
<br>
<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Conexion_putty.png" alt="imagen de conexión de máquina virtual con putty" width="650" height="420">
</p>

*Luego de haber configurado todo, se enciende la máquina virtual. Luego de que carguen todos los archivos y dependencias, se ingresa usuario y contraseña Ubuntu, y se ingresa el comando hostname -I para obtener la dirección IP del entorno local. Con ésta dirección IP se procede a conectar Putty, que lo que nos permite es una mayor libertad y mejor interacción con lo que estemos haciendo en el mismo entorno virtual.*

<br>

### Limpieza de la máquina virtual (opcional)

Este paso es opcional, y aplica en el caso de que la máquina virtual ya exista y se quieran borrar todos los contenedores, volúmenes e imágenes, para liberar por completo el espacio asignado al entorno virtual.

#### Limpieza del disco duro

Borrar todo: 

```
  docker system prune -a
```

#### Limpiar Carpetas

Visualizar las carpetas:

```
  ls
```
 
#### Eliminar carpetas (una por una):

```
  rm -r <Nombre de carpeta> 
```

#### Limpiar Contenedores

Visualizar contenedores:

```
    sudo docker ps
```

Eliminar TODOS los contenedores:

```
  sudo docker rm -f $(sudo docker ps -a -q)
```

#### Limpiar Volumen

Visualizar volumen:

    sudo docker volume ls

Eliminar TODO los volumenes:

```
  sudo docker volume prune
```

#### Limpiar Imágenes

Visualizar Imágenes:

```
  sudo docker image ls
```

Eliminar TODAS las imagenes:

```
  sudo docker image prune
```

#### Verificar si está limpio maquina virtual (MV)

Visualizar espacio disponible:

```
  df -h
```

<br>

## :calling: Clonación del repositorio, inicio y ejecución de los servicios docker-compose.yml

Para empezar, clonamos el repositorio, entramos dentro de la carpeta, e iniciamos, y ejecutamos los servicios dentro de docker-compose.yml, según el siguiente código...
```
  git clone https://github.com/lopezdar222/herramientas_big_data
  cd herramientas_big_data
  sudo docker-compose -f docker-compose-v1.yml up -d 
```

<br>

<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Clonando_repo.png" alt="imagen dentro de putty, clonación de repositorio de github" width="450" height="290">
</p>

<br>

## :floppy_disk: HDFS - Hadoop Distributed File System

Se puede utilizar el entorno docker-compose-v1.yml

Se ejecuta el contenedor "namenode" y dentro de la carpeta "home" se crea la carpeta "Datasets". Luego de salir del contenedor, se copian y pegan los archivos ubicados en la carpeta "Datasets", dentro de la carpeta "Datasets" del contenedor "namenode", respectivamente.

```
  sudo docker exec -it namenode bash
  cd home
  mkdir Datasets
  exit
  sudo docker cp Datasets namenode:/home
```

Ejecutamos el contenedor "namenode" y creamos un directorio en HDFS llamado "/data". Luego copiamos los archivos csv provistos a HDFS.

```
  sudo docker exec -it namenode bash
  hdfs dfs -mkdir -p /data
  hdfs dfs -put /home/Datasets/* /data
  exit
```

<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/hadoop.png" alt="imagen copia de archivos del Datasets" width="450" height="290">
</p>

Este proceso de creación de la carpeta data y copiado de los arhivos, debe poder ejecutarse desde un shell script.

## :bookmark_tabs: Hive

Se puede utilizar el entorno docker-compose-v2.yml

Primero, copiaremos el script necesario, dentro de la carpeta "home" del contenedor "hive-server".

Luego Crearemos tablas en Hive, a partir de los csv ingestados en HDFS.
Para esto, nos ubicamos dentro del contenedor correspondiente a Hive-server, y ejecutamos desdea allí los scripts necesarios.

```
  sudo docker cp Paso02.hql hive-server:/home
  sudo docker exec -it hive-server bash
  cd /home
  hive -f Paso02.hql
```

Este proceso de creación las tablas debe poder ejecutarse desde un shell script.

<br>
<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/paso02_hql.png" alt="imagen de script hql copiado" width="450" height="290">
</p>

<br>

Luego de ejecutar el script Paso02.hql se puede proceder a ejecutar el motor de consultas hive con el comando "hive" y realizar consultas SQL, como si nos encontraramos en mySQL workbench, o bien, ejecutar el comando "exit" para salir del contenedor.


## :file_cabinet: Formatos de Almacenamiento

A partir de las tablas creadas en el punto 2 con los archivos en formato csv, se deben almacenar en formato Parquet + Snappy.
Para realizar esto se debe copiar y ejecutar el script.hql correspondientes al paso03, previamente copiadolos en la carpeta "home" del contenedor "hive-server", dado que para almacenar las tablas en formato Parquet se utiliza la cláusula STORED AS PARQUET al crear la tabla en Hive. Luego de la LOCATION del archivo, se coloca TBLPROPERTIES ('parquet.compression'='SNAPPY') para comprimirlo con SNAPPY.

```
  sudo docker cp Paso03.hql hive-server:/home
  sudo docker exec -it hive-server bash
  cd /home
  hive -f Paso03.hql
  exit
```

<br>
<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/parquet_snappy.png" alt="imagen de cláusulas sql para guardado parquet + snappy" width="450" height="290">
</p>

<br>

## :computer: SQL

La mejora en la velocidad de consulta que puede proporcionar un índice tiene el costo del procesamiento adicional para crear el índice y el espacio en disco para almacenar las referencias del índice.
Se recomienda que los índices se basen en las columnas que utiliza en las condiciones de filtrado. El índice en la tabla puede degradar su rendimiento en caso de que no los esté utilizando.
Crear índices en alguna de las tablas cargadas y probar los resultados.
Se debe ejecutar el script Paso04.hql para crear los índices.


```
  sudo docker cp Paso04.hql hive-server:/home
  sudo docker exec -it hive-server bash
  cd /home
  hive -f Paso04.hql
  exit
```
<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Creacion_index.png" alt="imagen de creacion de indice en sql" width="350" height="250">
</p>

## :card_file_box: No-SQL

Se puede utilizar el entorno docker-compose-v3.yml

### HBase:

Ingresamos a la carpeta...

```
  cd herramientas_big_data 
  sudo docker-compose -f docker-compose-v3.yml up -d
  sudo docker exec -it hbase-master hbase shell
```

Dentro del contenedor, ejecutamos el codigo SQL:

<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/H-base.png" alt="imagen de creacion de indice en sql" width="450" height="290">
</p>

<br>

### MongoDB

Se utiliza el mismo sudo docker-compose -f docker-compose-v3.yml up -d descargado para el punto anterior.
Se copian los archivos iris.csv e iris.json dentro de la carpeta data del contenedor de mongodb.

```
  cd herramientas_big_data
  sudo docker cp iris.csv mongodb:/data/iris.csv
  sudo docker cp iris.json mongodb:/data/iris.json
  sudo docker exec -it mongodb bash 
```
  
Copiamos los archivos de nuestro contenedor, al directorio 'dataprueba', que esta dentro del entorno virtual de 'mongosh' para poder trabajar con él. 

```
  mongoimport /data/iris.csv --type csv --headerline -d dataprueba -c iris_csv 											     
  mongoimport --db dataprueba --collection iris_json --file /data/iris.json --jsonArray 
```
												
Entramos al entorno virtual de 'mongosh':

```
  use dataprueba
  show collections
  db.iris_csv.find() 
  db.iris_json.find()
  exit
```

Guardamos en el directorio data el archivo iris_export.csv e iris_export.json:

```
  mongoexport --db dataprueba --collection iris_csv --fields sepal_length,sepal_width,petal_length,petal_width,species --type=csv --out /data/iris_export.csv 
  mongoexport --db dataprueba --collection iris_json --fields sepal_length,sepal_width,petal_length,petal_width,species --type=json --out /data/iris_export.json
  exit
```

Páginas de descarga:
	https://search.maven.org/search?q=g:org.mongodb.mongo-hadoop 
	https://search.maven.org/search?q=a:mongo-hadoop-hive 
	https://search.maven.org/search?q=a:mongo-hadoop-spark


Copiamos los archivos desde Mongodb al contenedor Hive-server:

```
  cd herramientas_big_data
  cd Mongo (entramos al directorio Mongo ya que aqui estan los archivos .jar que nos indican copiar en la practica)
  sudo docker cp mongo-hadoop-hive-2.0.2.jar hive-server:/opt/hive/lib/mongo-hadoop-hive-2.0.2.jar 
  sudo docker cp mongo-hadoop-core-2.0.2.jar hive-server:/opt/hive/lib/mongo-hadoop-core-2.0.2.jar
  sudo docker cp mongo-hadoop-spark-2.0.2.jar hive-server:/opt/hive/lib/mongo-hadoop-spark-2.0.2.jar
  sudo docker cp mongo-java-driver-3.12.11.jar hive-server:/opt/hive/lib/mongo-java-driver-3.12.11.jar
  sudo docker cp mongo-scala-driver-0.8.15.jar hive-server:/opt/hive/lib/mongo-scala-driver-0.8.15.jar
```

Copiamos el archivo irir.hql que está en el directorio herramientas_big_data:

```
  cd ..
  sudo docker cp iris.hql hive-server:/opt/iris.hql 
  sudo docker exec -it hive-server bash 
  exit
```

Entramos al servidor de consultas del contenedor hive-server:

```
  sudo docker exec -it hive-server bash
  hiveserver2 
  chmod 777 iris.hql 
  hive -f iris.hql
  exit
```


### Neo4J
Ejemplo de búsqueda del camino más corto:
		https://neo4j.com/docs/graph-data-science/current/algorithms/dijkstra-source-target/
<br>

<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Neo4j-1.png" alt="imagen de código Neo4j" width="300" height="450">
</p>

<br>
			
		
		
Ejemplo de logística: 
		https://neo4j.com/docs/graph-data-science/current/alpha-algorithms/minimum-weight-spanning-tree/

<br>

<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Neo4j-2.png" alt="imagen de código de Neo4j-2" width="400" height="300">
</p>

<br>				

## :magic_wand: Spark

Se pueden utilizar los entornos docker-compose-v4.yml y docker-compose-kafka.yml

```
  cd herramientas_big_data 
  sudo docker-compose -f docker-compose-v4.yml up -d 
```

### Spark y Scala:

Ubicarse en la línea de comandos del Spark master y comenzar PySpark.

```
  sudo docker exec -it spark-master bash 
  /spark/bin/pyspark --master spark://spark-master:7077 
```
  
Cargar raw-flight-data.csv desde HDFS:

<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Spark.png" alt="imagen de código de carga de raw-flight-data-csv" width="300" height="450">
</p>

<br>

Ubicarse en la línea de comandos del Spark master y comenzar Scala.

```
  sudo docker exec -it spark-master bash
  spark/bin/spark-shell --master spark://spark-master:7077
```

Cargar raw-flight-data.csv desde HDFS.

<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Spark-Scala.png" alt="imagen de código de carga de raw-flight-data-csv" width="400" height="350">
</p>

<br>

### Kafka

```
	sudo docker-compose up -d
	sudo docker exec -it kafka_container bash
	cd /opt/kafka/bin
	sh kafka-topics.sh --create --bootstrap-server kafka:9092 --replication-factor 1 --partitions 100 --topic demo
	sh kafka-topics.sh --list --bootstrap-server kafka:9092
	sh kafka-topics.sh --describe --bootstrap-server kafka:9092 --topic demo 
	sh kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic demo --from-beginning
	sh kafka-console-producer.sh --broker-list localhost:9092 --topic demo
```				

Escribir desde la consola del productor "Esto es una Prueba 1" y enviar.

Acceder a <IP_Anfitrion>:9000	
	
Desde Scala:

```
	val df = spark.readStream
			.format("kafka")
			.option("kafka.bootstrap.servers", "192.168.1.100:9092")
			.option("subscribe", "json_topic")
			.option("startingOffsets", "earliest") // From starting
			.load()

	df.printSchema()
```		
			
Más ejemplos:
		https://github.com/dbusteed/kafka-spark-streaming-example
						
Otra forma de ejecutar:

```			
  docker-compose exec kafka kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic TenMinPsgCnts --from-beginning
```

### Comparativa Dataset y Dataframe en Scala:
    
```  
  sudo docker cp pruebaPySpark.py spark-master:pruebaPySpark.py
  sudo docker cp pruebaScala.scala spark-master:pruebaScala.scala

	sudo docker exec -it spark-master bash
		
	/spark/bin/spark-submit --master spark://spark-master:7077 pruebaPySpark.py
	/spark/bin/spark-shell --master spark://spark-master:7077 -i pruebaScala.scala
		
	/spark/bin/pyspark --master spark://spark-master:7077
	/spark/bin/spark-shell --master spark://spark-master:7077
```


## :hourglass: Carga incremental con Spark 

Ahora resta evaluar qué sucede cuando en los sistemas fuente, se genere más dato, es decir, siguiendo los datos de esta práctica, qué pasa cuando se carguen más ventas. Se debería tomar las novedades e ingestar en el modelo existente cada día, de modo que la tabla venta, irá creciendo en cantidad de registro de manera diaria.
Para este fin, se provee un script en spark que realiza la generación de nuevas ventas, de manera aleatoria, para poder crear una situación, donde se cuenta con novedades para la tabla de venta. El script "Paso06_GeneracionVentasNuevasPorDia.py" utiliza los datasets provistos en la carpeta "Datasets\data_nvo" para generar las novedades de forma automática. Revisar la variable "fecha_nvo" que contiene la fecha para la que se quiere generar información, como tenemos datos hasta el año 2020, la fecha de ejemplo tomada es '2021-01-01'.
Es necesiario entonces generar, un script tal que tome las novedades en csv, y las cargue al modelo.

```	
	sudo docker exec -it spark-master /spark/bin/spark-submit --master spark://spark-master:7077 /home/Paso06_GeneracionVentasNuevasPorDia.py
```	
Supongamos que tenemos nuestro script, y ahora se quiere programar su ejecución:

```	
	/spark/bin/spark-submit --master spark://spark-master:7077 Paso06_IncrementalVentas.py
```	

Con crontab, para que ejecute cada día a las 5 AM:

<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Cron.png" alt="imagen de página crontab.guru modificando horario" width="450" height="350">
</p>

<br>

## :abacus: Herramientas de orquestación de flujos de datos

Para la orquestación de flujo de datos, una herramienta vista en clases y muy interesante es Airflow.

<p align="center">
<img src="https://github.com/SebitaElGordito/Integrador_M4/blob/main/Imagenes_proyecto/Airflow.png" alt="imagen de Airflow creando un DAG" width="600" height="300">
</p>

<br>

Con esta herramienta, se puede automatizar las tareas de flujo de carga de trabajo, pudiendo realizar reportes automáticos en cuanlquier momento, aun fuera de horario laboral. Es muy interesante para los reportes anuales, mensuales o diarios para las empresas, y para poder visualizar los DAGs con una interfaz gráfica intuitiva.

<br>

## :people_hugging: Participantes y compañeros del pair programming

* **`Sebastian Vazquez`**..................   [![linkedin](https://img.shields.io/badge/linkedin-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/sebastian-vazquez-67353722b/)

* **`Ronnie Escobar Ccanto`**.........   [![linkedin](https://img.shields.io/badge/linkedin-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/ronnie-escobar-ccanto7461/)

* **`Aldo Federico Mamani`**...........   [![linkedin](https://img.shields.io/badge/linkedin-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/afederico-mamani-mgg/)

* **`Maria Eva Bichi`**......................   [![linkedin](https://img.shields.io/badge/linkedin-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/mar%C3%ADa-eva-bichi-264443203/)

<br>
 
