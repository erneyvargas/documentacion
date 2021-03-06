![](https://www.mct.com.co/bundles/portalpaginaweb/img/grupo_horz_fondo4.png)

# Ejecuci贸n de proyectos Python en contenedores Docker


<div align="center">
<img src="https://img.icons8.com/fluency/144/000000/python.png"/>
<img src="https://img.icons8.com/carbon-copy/100/000000/advance.png"/>
<img src="https://img.icons8.com/fluency/144/000000/docker.png"/>
</div>

## _Comenzando_ 馃殌
El objetivo de este manual, es indicar los pasos para la correcta ejecuci贸n de proyectos hechos en el lenguaje Python utilizando contenedores Docker  estos proyectos pueden ser ejecutados desde crontabs en el  mismo servidor **172.24.1.123** el cual cuenta con SO **Centos7** o desde scripts desde alguna otra plataforma

#### Desarrollo del proyecto 馃搵

 1. ##### Creaci贸n entorno Virtual 

     Un virtualenv o entorno virtual de Python es un ambiente creado con el objetivo de aislar recursos como librer铆as y entornos de ejecuci贸n del sistema principal o de otros entornos virtuales. para crear un entorno Virtual ir a: [Entornos Virtuales]("/projects_python/Creaci贸n_de_Entornos_Virtuales.md")

 2. ##### Estructura de directorios y archivos del proyecto Python
    Se recomienda dejar en la ra铆z del programa los siguientes archivos

    <div align="center">
    <img src="../assets/python_docker/carpetas.png"/>
    </div>

    - ***Ejecutable o iniciador del proyecto**** con el objetivo de que le sea m谩s f谩cil la identificaci贸n de este archivo para el equipo de infraestructura
    - ***requierements.txt*** El cual est谩 conformado por las librer铆as requeridas para la ejecuci贸n del proyecto, este archivo ser谩 el que buscara infraestructura para la instalaci贸n de las librer铆as.
        _Ejemplo de requirements .txt :_
        ```
        pandas == 1.3.0
        psycopg2-binary == 2.9.1
        openpyxl == 3.0.7
        Pillow == 8.2.0

        ```
        Utilizando EL manejador de paquetes conda:

        ```bash
        conda list -e > requirements.txt    
        ```
        
        Utilizando EL manejador de paquetes pip
        ```bash
        pip install -r requirements.txt
        ```   

## _Ejecutando las pruebas_ 鈿欙笍

Las pruebas se deber谩 realizar desde consola y fuera del proyecto, esto garantiza que el proyecto se podr谩 ejecutar desde cualquier ambiente o servidor

***Nota:*** Si el proyecto usa rutas relativas, es necesario la implementaci贸n de: **os.path.dirname(__file__)** El cual es un m茅todo en Python que se usa para obtener el nombre del directorio de la ruta especificada.

_***Ejemplo de uso de rutas absolutas:***_
    El proyecto tiene una ruta donde guarda o consulta un archivo:
```python
    save_route = assets/informe_polar.xlsx'
```
Se debe cambiar a:
```python
    path_abs = os.path.dirname(__file__)
    save_route = path_abs + '/assets/informe_polar.xlsx'
```
El uso de este 煤ltimo m茅todo, garantiza que sin importar desde donde ejecute el proyecto, buscara o guardara los archivos en la ruta especificada.


## _Despliegue_ 馃摝
Una vez finalizado el proyecto y realizado las pruebas se realiza commit a al proyecto especificado
_este paso aun esta pendiente por generar repositorio git....._
La creaci贸n del contenedor y el crontab se debe realizar mediante un ticket al equipo de infraestructura si se desea indagar como se ejecuta el c贸digo en producci贸n ver.....



## _Construido con_ 馃洜锔?

* [Python](https://www.python.org/) - Lenguaje utilizado
* [pycharm](https://www.jetbrains.com/es-es/pycharm/) - El IDE usado

## _Autores_ 鉁掞笍

_Menciona a todos aquellos que ayudaron a levantar el proyecto desde sus inicios_

* **Erney Vargas** - *Trabajo Inicial* - [erney.vargas](http://git.mct.com.co/erney.vargas)
* **Erney Vargas** - *Documentaci贸n* - [erney.vargas](http://git.mct.com.co/erney.vargas)

