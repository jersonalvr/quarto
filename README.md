## Primeros pasos

### [Tutorial para instalar Hadoop 3.4.1](https://medium.com/@charan.n_22122016/installing-hadoop-on-ubuntu-a-step-by-step-guide-a2f43dfdc4ac)

   ### Dar permisos de administrador al usuario hadoop
   ```bash
   sudo usermod -aG hadoop tu_usuario
   sudo usermod -aG sudo hadoop
   sudo chmod -R 755 /home/hadoop/hadoop
   start-all.sh # Detener stop-all.sh
   ```
   
   ### Si quieres iniciar Hadoop como tu usuario:

   1. Error de SSH:
      ```
      Permission denied (publickey,password)
      ```
   Este error aparece porque Hadoop necesita acceso SSH sin contraseña al localhost.

   2. Error de permisos de escritura:
      ```
      ERROR: Unable to write in /home/hadoop/hadoop/logs
      ```
   No tienes permisos de escritura en el directorio de logs.

   3. Cuando cambias al usuario "hadoop" (`su hadoop`), todo funciona correctamente.

   Para solucionar esto, tienes varias opciones:

   1. **Configurar tu usuario adecuadamente** (si necesitas usar este usuario):
      - Configurar SSH sin contraseña:
      ```bash
      ssh-keygen -t rsa -P ''
      cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
      chmod 0600 ~/.ssh/authorized_keys
      ```
      
      - Asegurarte que tienes los permisos necesarios:
      ```bash
      sudo usermod -aG hadoop jerson
      sudo chown -R hadoop:hadoop /home/hadoop/hadoop
      sudo chmod g+w -R /home/hadoop/hadoop
      ```

   2. **Modificar la configuración de Hadoop**:
      - Editar `etc/hadoop/hadoop-env.sh` para especificar el usuario correcto
      - Asegurarte que los directorios de logs tienen los permisos adecuados

   # Namenode
   http://localhost:9870/

   # Cluster
   http://localhost:8088/

   # Datanode
   http://localhost:9864
   ```

### Instalar Spark y configurar la variable de entorno SPARK_HOME.

1. Primero, descarga y descomprime Spark:

   ```bash
   # Descargar Spark
   wget https://dlcdn.apache.org/spark/spark-3.5.4/spark-3.5.4-bin-hadoop3.tgz

   # Descomprimir el archivo
   tar -xzf spark-3.5.4-bin-hadoop3.tgz

   # Mover a opt
   sudo mv spark-3.5.4-bin-hadoop3 /opt/spark
   ```

2. Configura las variables de entorno. Puedes añadirlas al archivo `.bashrc`:

   ```bash
   # Abre el archivo .bashrc
   nano ~/.bashrc

   # Añade estas líneas al final del archivo
   export SPARK_HOME=/opt/spark
   export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
   # Opcionalmente verificar su ip con ifconfig
   export SPARK_LOCAL_IP=192.168.18.80
   ```

   ```bash
   export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
   export HADOOP_HOME=/home/hadoop/hadoop
   export HADOOP_INSTALL=$HADOOP_HOME
   export HADOOP_MAPRED_HOME=$HADOOP_HOME
   export HADOOP_COMMON_HOME=$HADOOP_HOME
   export HADOOP_HDFS_HOME=$HADOOP_HOME
   export HADOOP_YARN_HOME=$HADOOP_HOME
   export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
   export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
   export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
   export SPARK_HOME=/opt/spark
   export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
   export SPARK_LOCAL_IP=192.168.18.80
   ```

3. Aplica los cambios:

   ```bash
   source ~/.bashrc
   ```

4. Verifica la instalación:

   ```bash
   echo $SPARK_HOME
   spark-shell --version
   ```

### Instalar [Quarto](https://quarto.org/docs/get-started/) tambien la extension de Quarto y Jupyter en VSCode

   ```bash
   # Instalar latex
   sudo apt-get install texlive-full
   ```

## Clona el repositorio
```bash
gh repo clone jersonalvr/quarto
cd quarto
git checkout Y1018-Y04AN1-2025-2-Big_Data_Aplicada
```

## Creación del entorno
* Instalar previamente entornos virtuales `sudo apt install python3-venv python3-full` y extension de Python en VSCode

   ```bash
   source spark_env/bin/activate
   python3 -m ipykernel install --user --name=spark_env
   ```

   - Presionar `Ctrl+Shift+P`
   - Escribir "Python: Select Interpreter" o "Jupyter: Select Kernel"
   - Seleccionar el kernel "spark_env"


