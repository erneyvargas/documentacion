![](https://www.mct.com.co/bundles/portalpaginaweb/img/grupo_horz_fondo4.png)

# Creación de Entornos Virtuales


<div align="center">
<img src="https://img.icons8.com/fluency/144/000000/anaconda--v2.png"/>
<img src="https://img.icons8.com/color/144/000000/python--v2.png"/>
</div>

## _Comenzando_ 🚀
El objetivo de este manual, es indicar los pasos para la creación de entornos virtuales en PYTHON

**¿Que es un entorno Virtual?**

Los entornos virtuales se pueden describir como directorios de instalación aislados. Este aislamiento te permite localizar la instalación de las dependencias de tu proyecto, sin obligarte a instalarlas en todo el sistema.


## Entorno virtual con Conda :snake:
- #### _Instalación de Anaconda_ 

    Para descargar anaconda dirigirse a https://www.anaconda.com/products/ind... y elegir el SO correspondiente, en este caso se instalará para sistemas Linux basados en debían.


    Con este instalador estamos listos para instalarlo en Ubuntu, para esto accedemos a la terminal y allí ejecutamos lo siguiente:

    _Permiso de ejecución:_
    ```sh
    Chmod +X Anaconda3-xxxx.xx-linux-x86_64.sh
    ```


    _Se ejecuta el instalador:_
    ```sh
    ~/Downloads/Anaconda3-xxxx.xx-Linux-x86_64.sh
    ```
    > ***Nota:*** el campo Anaconda3-xxxx.xx debe ser reemplazado por la última versión que hayamos descargado.

    * Pulsamos la tecla Enter para continuar con la instalación de Anaconda y luego aceptamos los términos de licencia

    * Nuevamente, pulsamos Enter para finalizar este proceso

    * Una vez finalizado podemos Ejecutar Anaconda con el comando:
    ```python
    anaconda-navigator
    ```
    Para mayor información o instalación en otro sistema operativo  visitar: https://conda.io/projects/conda/en/latest/user-guide/install/index.html

- #### Habilitar comandos de Anaconda

    Editamos el archivo .bashrc

    ```python
    sudo vim ~/.bashrc
    ```

    Dentro del archivo pegamos el path de la ruta de anaconda:

    ```python
    export PATH="<<ruta_instalacion_anaconda>>:$PATH"

    "Ejemplo"
    export PATH="/opt/anaconda3/bin/:$PATH"
    ```

    Actualizamos la terminal:
    ```
    source ~/.bashrc

    ```

    Si es necesario actualizar conda
    ```
    conda update -n base -c defaults conda
    ```
- #### Creación de entorno virtual


    ```python
    conda create --name <<name_environment>>

    "Ejemplo"
    conda create --name reporting_mct
    ```
    > Si desea crear un entorno virtual con una versión especifica de Python

    ```python
    conda create -n <<name_environment>> python=3.6

    ```

    Inicialización de conda para activar el entorno base

    `conda init <<shell>>`


    Los shells soportados actualmente son:
    - bash
    - fish
    - tcsh
    - xonsh
    - zsh
    - powershell

    Listar entornos virtuales existentes
    ```python
    conda env list
    ```
    Activar entorno virtual

    ```python
    conda activate <<name_environment>>
    "Ejemplo"
    conda activate reporting_mct
    ```
    Desactivar entorno virtual

    ```python
    conda deactivate 
    ```
    Listar las diferentes librerías o dependencias instaladas sobre el entorno virtual (solo si el entorno virtual deseado se encuentra activado)

    ```python
    conda list 
    ```

    Instalar paquetes

    ```python
    conda install <<name_dependencies>>
    "Ejemplo"
    conda install pandas
    ```

## _Entorno virtual con Virtualenv_ :computer:

virtualenv es una herramienta que se utiliza para crear entornos Python aislados. Crea una carpeta que contiene todos los ejecutables necesarios para usar los paquetes que necesitaría un proyecto de Python.

- #### _Instalación de virtualenv_ 

    Se debe tener instalado pip

    **pip** es un sistema de gestión de paquetes utilizado para instalar y administrar paquetes de software escritos en Python
    ```python
    pip install virtualenv 
    ```
 - #### _Crear un entorno_

    Para crear un entorno virtual utiliza:
    ```python
    virtualenv <<nombre_entorno>>
    ```

    Esto genera una carpeta en el directorio actual con el nombre del entorno (nombre_entorno/). Esta carpeta contiene los directorios para instalar módulos y ejecutables de Python.


    También puedes especificar la versión de Python con la que quieres trabajar. Simplemente, se emplea el argumento --python=/ruta/a/la/version/de/python. Por ejemplo, python2.7:
    ```python
    histvirtualenv --python=/usr/bin/python2.7 <<nombre_entorno>>
    ```

- #### _Activar un entorno_

Antes de utilizar el entorno, debes activarlo:
```python
source my_env/bin/activate
```

- #### _Instalar paquetes_

    Puede instalar paquetes uno por uno.

    ```python
    pip install algun-paquete
    ```
    Instalar paquetes configurando un archivo `requirements.txt` para tu proyecto.

    ```python
    pip install -r requirements.txt
    ```


## _Construido con_ 🛠️

* [Python](https://www.python.org/) - Lenguaje utilizado
* [pycharm](hhttps://www.jetbrains.com/es-es/pycharm/) - El IDE usado

## _Autores_ ✒️


* **Erney Vargas** - *Trabajo Inicial* - [erney.vargas](http://git.mct.com.co/erney.vargas)
* **Erney Vargas** - *Documentación* - [erney.vargas](http://git.mct.com.co/erney.vargas)
