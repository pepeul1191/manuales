# Bottle ULima

## Instalar dependencias

Para crear las dependencias debemos de crear en la raiz del proyecto un archivo llamado <b>requirements.txt</b> y copiar las siguientes dependencias:

```
bottle
```

Y para instalar las dependencias ejecutaríamos el comando <b>pip</b>. En caso de estar en replit, no sería necesario instalar las dependencias con el siguiente comando porque en ese entorno de desarrollo se instalan de manera automática.

    > pip install -r requirements.txt

## HTTP Response: hola mundo

Para nuestro primer 'hola mundo' debemos de importar la librería <b>Bottle</b> y <b>run</b> de <b>bottle</b>. Luego cremos una instancia del servidor en la variable <b>app</b>. 

``` python
from bottle import Bottle, run

app = Bottle()

@app.route('/', method='GET')
def home():
  return 'hola mundo'

if __name__ == '__main__':
  run(
    app, 
    host='0.0.0.0', 
    port=8080, 
    debug=True, 
    reloader=True
  )
```
La función <b>home</b> es una función de endpoint, donde se estará mapeando la URL <b>/</b>, que sería la URL vacía de Home.

## HTTP Response: vista html + python 

Para crear las vistas vamos a primero una carpeta llamada <b>views</b>. Dentro de esta carpeta vamos crear un archivo llamado <b>home.tpl</b>. En ese archivo vamos a escribir el siguiente código: 

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
  <h1>Hola mundo</h1>
</body>
</html>
```

El cambio que vamos realizar será cambiar la respuesta de la petición HTTP del método <b>home</b>. Para esto debemos agregar una nueva función de <b>bottle</b> llamada <b>template</b>.

``` python
from bottle import Bottle, run, template
```

Ahora vamos a cambiar el retorno de la función de <b>home</b> de un string a a la vista que hemos creado.

``` python
@app.route('/', method='GET')
def home():
  return template('home')
```

## Pasando datos a la vista desde el backend

Vamos a pasar valores a la vista <b>home.tpl</b>. Se tiene mandar un segundo atributo en la función <b>template</b>. Este segundo atributo tiene que ser un diccionario, donde las llaves del diccionario se deben de encontrar en la vista del primer argumento.

``` python
@app.route('/', method='GET')
def home():
  locals = {
    'mensaje': 'Si se puede'
  }
  return template('home', locals)
```

De esta manera estamos mandando datos desde el servidor a la vista, pero en la vista hay que indicar el lugar de estas variables con la notación <b>{{nombre_de_variable}}</b>.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
  <h1>Hola mundo</h1>
  mensaje: {{mensaje}}
</body>
</html>
``` 

## Petición GET + query parameters

Vamos a crear una nueva ruta dónde le mandemos dos argumentos por la url, que en el contexto web se llama query parameters. Estos funcionan cuando usamos peticiones de tipo GET. Nuestra nueva url se llamará <b>suma</b> y los valores de sus query parameters serán <b>a</b> y <b>b</b>. La url final sería por ejemplo la siguiente: <b>BASE_URL + suma?a=1&b=12</b>, donde los valores de <b>a</b> y <b>b</b> serían cambiantes. 

Para empezar vamos a crear una nueva función asociada a la nueva URL y un archivo <b>suma.tpl</b> en la carpeta <b>views</b> dónde se mostrará el resultado de la suma de los argumentos enviados por la URL. Para poder recepcionar en la nueva función tenemos que importar una nueva librería, esta es <b>request</b>.

``` python
from bottle import Bottle, run, template, request
```

Recién ahora vamos a poder recepcionar el valor enviado por la URL. todos estos valores serán capturados en la función por el nombre del argumento de la url. Otro aspecto importante a tomar en cuenta, es que los datos recepcionados por este método son de tipo string, asi que si queremos hacer una suma, tenemos que parsearlo a entero.

``` python
@app.route('/suma', method='GET')
def suma_get():
  a = int(request.params.a)
  b = int(request.params.b)
  locals = {'suma': a + b}
  return template('suma', locals)
```
La vista <b>suma.tpl</b> sería la siguiente:

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
  la suma de a y b es: {{suma}}
</body>
</html>
``` 

## Pasando una lista a la vista

Vamos a crear un nuevo endpoint donde vamos a tener una variable que tenga una lista la cuál será enviada a una nueva lista llamada <b>lista.tpl</b> que deberá ser creada en la carpeta de vistas.

``` python
@app.route('/lista', method='GET')
def lista():
  selecciones = [
    {'id': 1, 'nombre': 'Brazil'},
    {'id': 2, 'nombre': 'Argentina'},
    {'id': 3, 'nombre': 'Uruguay'},
  ]
  locals = {'selecciones': selecciones}
  return template('lista', locals)
```

La vista se vería de la siguiente manera:
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
  % for s in selecciones:
  <p>{{s['id']}}, {{s['nombre]}}</p>
  % end
</body>
</html>
``` 

## Reutilizando código de las vistas

Como podemos ver en todas las vistas tpl que hemos creado, hay código repetido en las cabeceras y pies de página. Este código puede estar en otros archivos independientes, por ejemplo vamos a copiar la cabecera en un archivo llamado <b>_header.tpl</b>.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
```
Y en nuestra última vista(<b>lista.tpl</b>) vamos a quitar la cabecera y vamos a parcharlo con el código del arhcivo <b>_header.tpl</b>

``` html
% include('_header.tpl')
  % for s in selecciones:
  <p>{{s['id']}}, {{s['nombre]}}</p>
  % end
</body>
</html>
```
