![](https://www.mct.com.co/bundles/portalpaginaweb/img/grupo_horz_fondo4.png)

# Ejecución de Crontabs en Producción


<div align="center">
<img src="https://img.icons8.com/external-vitaliy-gorbachev-flat-vitaly-gorbachev/116/000000/external-server-internet-security-vitaliy-gorbachev-flat-vitaly-gorbachev.png"/>
<img src="https://img.icons8.com/external-others-cattaleeya-thongsriphong/128/000000/external-equipment-artificial-intelligence-blue-others-cattaleeya-thongsriphong-13.png"/>
<img src="https://img.icons8.com/dusk/128/000000/system-task.png"/>
</div>

## _Comenzando_ 🚀
El objetivo de este manual, es instruir como se ejecutan los crontabs cuando es usando el cron del anfitrión es decir el host que ejecuta su el motor Docker en el presente documento se usara como ejemplo el servidor 172.24.1.123 el cual cuenta con SO CentOS 7.

**¿Qué es cron y para qué sirve?**

Cron es un administrador de tareas de Linux que permite ejecutar comandos en un momento determinado, por ejemplo, cada minuto, día, semana o mes. Si queremos trabajar con cron, podemos hacerlo a través del comando crontab.

**¿Qué es crontab?**

Es un archivo de texto donde se listan todas las tareas que deben ejecutarse y el momento en el que deben hacerlo.


1. ### Ejecución Crontab En 172.24.1.123 :wrench:
    
    
    La localización de los scripts que ejecuta el cron se encuentra alojados en el host 172.24.1.123 en la ruta: **/scripts**, para este ejemplo el script tiene como nombre **ejecuta_crontab_project.sh**

    ```python
        /scripts/ejecuta_crontab_project.sh
    ```

   - Contenido de este script **ejecuta_crontab_project.sh**
   
        ```bash
        #!/bin/bash
            echo "inicia contenedor"
            cd /home/docker/centos7/
            pwd
            /usr/bin/sh ./arranca_reportAPP.sh
            sleep 15
            echo  "ejecuta crontab"
            sudo /usr/bin/sh /scripts/crontabs_project.sh "/opt/projects_py/report_app/report_app.sh"
            sleep 20
            echo "finaliza crontab"
            docker rm -f reportAPP
            echo "fin"
        ```
        Donde:
        - **cd /home/docker/centos7/:** Se cambiará a la posición donde se encuentra el script que ejecuta el contenedor Docker.
        
        - **/usr/bin/sh ./arranca_reportAPP.sh":** arranca el Contenedor Docker
        - **sudo /usr/bin/sh /scripts/crontabs_project.sh "/opt/projects_py/report_app/report_app.sh":** Ejecuta script y le envía parámetros al siguiente Script:
        -  **docker rm -f reportAPP:** Elimina el Contenedor después de terminar la ejecución del crontab
       


        Contenido de **crontabs_project.sh**
        ```bash
        #!/bin/bash
        #echo "comienza"
        fecha=$(date)
        echo "$fecha $1"
        /usr/bin/docker exec --privileged reportAPP /bin/sh "$1"
        ```
        Donde:
        - **/usr/bin/docker exec --privileged reportAPP /bin/sh:** Ejecuta el contendor que para este caso es reportAPP y se le envía la variable "$1" que para este caso este argumento es:

        - **"$1 " :** Es el argumento "/opt/projects_py/report_app/report_app.sh" 
        
        EL comando /opt/projects_py/report_app/report_app.sh  Se ejecutará dentro del contenedor, el script **report_app.sh** contiene lo siguiente:

        ```bash
        #!/bin/bash
        echo "Inicio Ejecucion Archivo Python"
        /usr/local/bin/python3.8 /opt/projects_py/report_app/informe_tiempos_polar.py
        echo "Finalizo Ejecucion Archivo Python"
        ```
        Donde 
        - informe_tiempos_polar.py. Es El archivo que ejecuta el proyecto Python.


2. ## _Ejecución de imagen Docker_ :whale2:

    **¿Qué Es Docker?**

    Docker es una plataforma de software que le permite crear, probar e implementar aplicaciones rápidamente. Docker empaqueta software en unidades estandarizadas llamadas contenedores que incluyen todo lo necesario para que el software se ejecute, incluidas bibliotecas, herramientas de sistema, código y tiempo de ejecución. Con Docker, puede implementar y ajustar la escala de aplicaciones rápidamente en cualquier entorno con la certeza de saber que su código se ejecutará.


	1. ##### Ejecución de Imagen Docker 

        Los archivos arranca.sh se encargan de iniciar los contenedores

        La ubicación de los archivos arranca.SH para el caso del servidor **172.24.1.123** se encuentran alojados en la ruta:
        ```python
        /home/docker/centos7/
        ```


        Un archivo **arranca.sh** está conformado por:
        ```bash
       #!/bin/bash
        cd $(dirname "$0")
        local=$(pwd)
        echo "inicio"
    
        docker run --privileged -e "container=docker" -d --name reportAPP \
        -p 172.24.1.123:8600:80 -p 172.24.1.123:8622:22 --dns 172.24.1.45 \
        --add-host git.mct.com.co:192.168.102.31 \
        --add-host galerand1.mct.com.co:192.168.102.135 \
        --add-host bd-test.mct.com.co:192.168.102.23 \
        --add-host bd.mct.com.co:192.168.102.24 \
        --add-host bd-select.mct.com.co:192.168.102.29 \
        --add-host notificaciones.mct.com.co:172.24.1.97 \
        -v centos_ssh-root:/root \
        -v $local/phpexcel1.7:/usr/local/phpexcel1.7 \
        -v $local/Rmail:/usr/local/Rmail \
        -v $local/projects_py:/opt/projects_py \
        -v centos7_ssh-usr-local-lib-python3.8:/usr/local/lib/python3.8 \
        -v /var/lib/docker/volumes/documentos_cli/local2/jpgraph:/usr/local/jpgraph \
        -v /var/lib/docker/volumes/documentos_cli/local2/toxtylab:/usr/local/toxtylab \
        -v /var/lib/docker/volumes/documentos_cli/local2/jasperreports-1.3.0:/usr/local/jasperreports-1.3.0 \
        -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
        --env PATH=/usr/local/pgsql/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/php/bin:/usr/lib/x86_64-linux-gnu/ \
        -it centos_projects
        docker exec --privileged -it reportAPP postmap /etc/postfix/sasl_passwd
        sleep 3
        docker exec --privileged -it reportAPP systemctl stop sendmail
        docker exec --privileged -it reportAPP systemctl start postfix
        ```
        Este archivo de ejecución se puede dividir en los siguientes segmentos:

		1. #### Docker Run
            El comando Docker run crea y arranca un contenedor con los argumentos especificados,
			para la ejecución del proyecto **informe polar**  se le envían los siguientes argumentos:

            
            ```bash
            docker run --privileged -e "container=docker" -d --name reportAPP 
            ```
            donde:
		             * **privileged:** otorga capacidades root de contenedor de Docker a todos los dispositivos en el sistema host.
		             * **container=docker** Tipo de contenedor
		             * **d:**  Modo detach (sin mostrar output)
		             * **name**= Nombre específico
		          

		2. #### Segmentos de red
	        En este nivel se declara las conexiones de redes: 
	        ```bash
	        -p 172.24.1.123:8600:80 -p 172.24.1.123:8622:22 --dns 172.24.1.45 
	        ```
	        Donde:
		        *	**-p 172.24.1.123:8600:80 :** Puerto de escucha
		        *	**-p 172.24.1.123:8622:22 :** Puerto por defecto de SH
		        *	**--dns 172.24.1.45  :** Servidor de resolución de nombres
		       
		 3. #### Rutas Estáticas
			 En este nivel es donde se resuelven las IP, al agregar las rutas estáticas
			  ```bash
		    --add-host git.mct.com.co:192.168.102.31 \
	        --add-host galerand1.mct.com.co:192.168.102.135 \
	        --add-host bd-test.mct.com.co:192.168.102.23 \
	        --add-host bd.mct.com.co:192.168.102.24 \
	        --add-host bd-select.mct.com.co:192.168.102.29 \
	        --add-host notificaciones.mct.com.co:172.24.1.97 \ 
		      ```
		      Donde:
					* **add-host**:  Agregar un host a la asignación de la IP dada
					*  git.mct.com.co: El nombre de host o el dominio que le gustaría usar
					*  **192.168.102.31:** Dirección IP que el nombre de host debe resolver
		3.  #### Volúmenes
			Los **volúmenes** son como discos duros virtuales administrados por **Docker**
			  ```bash
		    -v $local/phpexcel1.7:/usr/local/phpexcel1.7 \
	        -v $local/Rmail:/usr/local/Rmail \
	        -v $local/projects_py:/opt/projects_py \
	        -v centos7_ssh-usr-local-lib-python3.8:/usr/local/lib/python3.8 \
	        -v /var/lib/docker/volumes/documentos_cli/local2/jpgraph:/usr/local/jpgraph \
	        -v /var/lib/docker/volumes/documentos_cli/local2/toxtylab:/usr/local/toxtylab \
	        -v /var/lib/docker/volumes/documentos_cli/local2/jasperreports-1.3.0:/usr/local/jasperreports-1.3.0 \
	        -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
		      ```
		      Donde:
					* **-v $local/phpexcel1.7:**:  Es un fichero existente en el host, en este caso el host es *172.24.1.123*
					*  **/usr/local/phpexcel1.7:** Lugar donde se va a alojar este recurso en el contenedor
		      
		      
		4.  #### Variables de entorno (PATH)
			En ella se especifican las rutas en las cuales el intérprete de comandos debe buscar los programas a ejecutar
			  ```bash
			  PATH=/usr/local/pgsql/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/php/bin:/usr/lib/x86_64-linux-gnu/																																			   				     
			```
		      Donde:
					* **/usr/local/pgsql/:** Permite Ejecutar comandos de PosgreSQL

		5.  #### Imagen
			Las Imágenes e Docker son esencialmente una instantánea de un contenedor
					
			```bash
			-it centos_projects																																		   				     
			```
			Donde:
			* **-it :** Se ejecuta de forma interactiva (`-i` mantiene STDIN abierto y `-t` asigna un terminal virtual en el contenedor)
			* **-it centos_projects:** Imagen que ya fue creada con la especificación necearía	

		6.  #### Ejecución de comandos Docker
			Las Imágenes e Docker son esencialmente una instantánea de un contenedor
					
			```bash
			docker exec --privileged -it reportAPP postmap /etc/postfix/sasl_passwd																																	   				     
			```
			Donde:
			* **docker exec:** Ejecutar comandos en un contenedor en ejecución
			* **privileged:** otorga capacidades root de contenedor de Docker a todos los dispositivos en el sistema host.
			* **-it :** Se ejecuta de forma interactiva (`-i` mantiene STDIN abierto y `-t` asigna un terminal virtual en el contenedor)
			* **reportAPP	:** Nombre o ID del contenedor
			* **postmap /etc/postfix/sasl_passwd :**  comando que se ejecutará en este contenedor