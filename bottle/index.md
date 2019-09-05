# Python Bottle

- [Python Bottle](#Python-Bottle)
  - [Introducción](#Introducci%C3%B3n)
  - [Crear Proyecto](#Crear-Proyecto)
  - [Crear ambiente virtual](#Crear-ambiente-virtual)
  - [Agregar dependencia de Python Bottle](#Agregar-dependencia-de-Python-Bottle)
  - [Configurando Bottle](#Configurando-Bottle)

## Introducción

Lo que se va a realizar es una aplicación web con Python Bottle con las siguientes características:

+ Uso de templates

Para el tutorial se necesitará tener instalado el siguiente software:

+ Python3
+ Python PIP
+ Git

Para el caso del presente tutorial ha sido usando el sistema operativo <b>Ubuntu 16.04.5</b>.

## Crear Proyecto

Para crear un proyecto en Python vamos a crear una carpeta vacía. Dento de esta carpta vamos a crear los siguientes archivos:

+ requirements.txt
+ .gitignore
+ .editorconfig

El contenido del archivo '.gitignore' será:

```
*.pyc
.DS_Store
__pycache__
env
.vscode
```

El contenido del archivo '.editorconfig' será:

```
# editorconfig.org

root = true

[*]
charset = utf-8
end_of_line = lf
indent_size = 4
indent_style = space
insert_final_newline = true
trim_trailing_whitespace = true

[*.md]
trim_trailing_whitespace = false
```

Por ahora el archivo 'requirements.txt' lo dejaremos vacío.

## Crear ambiente virtual

El uso de los ambientes virtuales en python tiene como fin aislar los recursos de desarrollo a usar en el desarrollo en python de los entornoes y librerías de python del sistema principal [2]. 

Para crear un ambiente virtual usaremos 'pip'. Vamos a instalar la herramienta para crear ambientes virtuales usando 'pip' con el siguiente comando:

    $ sudo pip install virtualenv

Una ves instalado 'virtualenv', nos debemos situar por medio de la línea de comandos en el directorio creado en el paso anterior [Crear Proyecto](#Crear-Proyecto). Una vez en el directorio, usar el siguiente comando:

    $ virtualenv -p python3 env

Para poder activar el ambiente virtual debemos usar el siguiente comando, estando situados en la raíz del proyecto:

    $ source env/bin/activate

## Agregar dependencia de Python Bottle

En el archivo 'requirements.txt' agregamos la dependencia de Bottle editando el archivo mencionado con el siguiente código:

```
bottle
```

Una vez grabado el archivo a editar, deberemos instalar la dependencia agregada:

    $ pip install -r requirements.txt

Para probar que la instalación ha sido exitosa, vamos a crear un archivo llamado 'app.py' en la raiz de la carpeta. Luego agreamos el siguiente código a dicho archivo:

```python
import bottle

app = bottle.app()

@app.route('/', method='GET')
def index():
    return 'Hola mundo!'

if __name__ == '__main__':
    bottle.run(
        app=app, 
        host='localhost', 
        port=3000, 
        debug=True
    )
```

Para ejecutar la aplicación web usaremos el siguiente código:

    $ python app.py

Si la ejecución fue exitosa, en la línea de comandos deberíamos de ver un resultado similar:

```
Bottle v0.12.17 server starting up (using WSGIRefServer())...
Listening on http://localhost:3000/
Hit Ctrl-C to quit.
```

El código usado es importante resaltar que la función "@route('/')" es la ruta pública que usaremos para acceder un recurso mediante el protocolo HTTP. Otro punto importante es notar los parámetros usados en la función "run", los parámetros "localhost" y "port". Dichos parámetros pueden ser modificados en función a la necesidad del ambiente de desarrollo y producción.

Si entramos en un web browser podremos ver el siguiente resultado si colocamos la url 'http://localhost:3000/'

![img01](resources/img01.png)

## Configurando Bottle

Las configuraciones que vamos a realizar son las siguientes:

+ Autorecarga de inicio para la aplicación una vez se modifiquen los arhivos.

En la sección de código dónde arracamos la aplicación, debemos agregar el atributo 'reloader=True'.

```python
bottle.run(
    app = app, 
    host='localhost', 
    port=3000, 
    debug=True, 
    reloader=True
)
```

+ Carpeta de archivos estáticos.

Primero vamos a crear un carpeta llamada 'static' en la raíz del proyecto. Esta carpeta contendrá los archivos estáticos del sitio web. Una vez creado los archivos se usará el siguiente código para indicarle a la aplicación web el uso que le daremos a esa carpeta, este código deberá estar debajo de '@app.route('/', method='GET')'

```python
@app.route('/:filename#.*#')
def send_static(filename):
    return bottle.static_file(
        filename, 
        root='./static/'
    )
```

---

Fuentes:

[1] https://github.com/pepeul1191/github-python <br>
[2] https://medium.com/@m.monroyc22/configurar-entorno-virtual-python-a860e820aace <br>
[3] https://bottlepy.org/docs/dev/ <br>