# Taller 1

Taller #1 para la electiva profesional Big Data & Analytics de la Universidad El Bosque

**Estudiante:** Álvaro Stagg  
**Profesor:** Fabian Peña

## Parte 1

En esta sección del taller se realizó la instalación de una máquina virtual
utilizando VMware como software de virtualización y la versión 34 de la
distribución de Fedora.

### Instalación del OpenJDK

Para instalar una versión libre de la máquina virtual de Java se eligió OpenJDK
y se instaló utilizando el siguiente comando:

```sh
$ sudo dnf install java-11-opendk-devel
```

![Versión de Java](./imgs/java.png)

### Creación y Preparación de Usuario SSH

Primero se creó un usuario llamado `hdoop` y se le asignó una contraseña.
Luego, autenticado como este usuario, se generó una llave SSH utilizando el
siguiente comando:

```sh
sh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
```

![Llave SSH para hdoop](./imgs/hdoop_ssh.png)

Después de obtener la llave, se añadió a la lista de claves autorizadas y se le
cambió el permiso al archivo por un `0600`.

![Llave autorizada](./imgs/ssh_authorized.png)

### Descarga de Hadoop

Primero, se descargó la versión 3.3.1 de Hadoop desde la [página oficinal del
proyecto](https://hadoop.apache.org/) utilizando la herramienta `cURL`:

```sh
$ curl -O https://dlcdn.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz
```

Luego se descomprimió utilizando el comando `tar xzf hadoop-3.3.1.tar.gz`.

![Hadoop Descargado](./imgs/hadoop_download.png)

### Configuración de Hadoop

Para instalar Hadoop en modo pseudo-distribuido, se modificó el archivo `.bashrc`
que está presenten el el directorio `$HOME` del usuario `hdoop`. El archivo
quedó de la siguiente manera:

![Configuración Hadoop](./imgs/hdoop_bashrc.png)

El siguiente paso es indicarlo a Hadoop qué versión de Java debe utilizar para
ejecutarse. Para ello, se editó el archivo `hadoop-env.sh`, específicamente
la siguiente linea:

![Hadoop Java Version](./imgs/hadoop_java_version.png)

El próximo archivo que se modificó fue el archivo `core-site.xml` el cual
quedó de la siguiente forma:

![Hadoop Core Site](./imgs/hadoop_core-site.png)

El siguiente archivo que se modificó fue `hdfs-site.xml` el cual quedó de la
siguiente manera:

![Hadoop HDFS Site](./imgs/hadoop_hdfs-site.png)

En la anterior configuración se indicó utilizar dos directorios que no existen,
por ende se deben crearon utilizando los siguientes comandos:

```sh
$ mkdir -p $HOME/dfsdata/namenode
$ mkdir -p $HOME/dfsdata/datanode
```

Luego, se indicó a Hadoop cuál framework de MapReduce utilizar, para este taller
se utilizará `yarn`. Esto se consigue editando el archivo `mapred-site.xml` de
la siguiente manera:

![Hadoop MapReduce Site](./imgs/hadoop_mapred.png)

Lo siguiente es editar el archivo `yarn-site.xml` de la siguiente manera:

![Hadoop Yarn Site](./imgs/hadoop_yarn-site.png)

### Preparación del Sistema de Archivos

Una vez todas las configuraciones se aplicaron, se formateron los directorios
indicados para el uso de Hadoop utilizando el siguiente comando:

```sh
hdfs namenode -format
```

El output de este comando es demasiado largo pero sabemos que se ejecutó con
éxito gracias a su mensaje final.

![HDFS Format](./imgs/hadoop_hdfs_format.png)

### Inicialización del Cluster

Para iniciar el cluster previamente configurado de Hadoop se ejecutó el archivo
`start-dfs.sh` dentro del directorio `sbin/` contenido en el directorio descomprimido
de Hadoop.

![Hadoop Cluster Init](./imgs/hadoop_cluster_init.png)

### Preparación De Nodos

Para inicializar los nodos, se utiliza el script `start-yarn.sh` presente en el
directorio `sbin/` contenido en el directorio descomprimido de Hadoop.

![Hadoop Start Yarn](./imgs/hadoop_yarn_start.png)

Se puede observar que todo levantó de manera satisfactoria ejecutando el comando `jps`:

![JPS](./imgs/hadoop_jps.png)

### Monitoreo de Hadoop en Navegador

En las siguientes imágenes se puede apreciar los diferentes paneles que habilita la suite
de Hadoop para el monitoreo de tareas, sistema de archivos, aplicaciones, etc.

![](./imgs/hadoop_nadenode_information.png)
![](./imgs/hadoop_datanode_information.png)
![](./imgs/hadoop_all_apps.png)

## Parte 2
