# ExpressJS

## Crear un proyecto Node

Nos ubicamos en una carpeta, de preferencia sin archivos, ejecutamos el siguiente comando:

    > npm init -y

Luego de ejecutado dicho comando, se habrá creado un archivo llamado <b>package.json</b>. En este archivo instalaremos las dependencias de las librerías que en futuro usaremos.

```
{
  "name": "expp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

El código de las librerías que descarguen serán ubicadas en una carpeta llamada <b>node_modules</b> en la raíz del proyecto. Esta carpeta no debe ser tenida en cuenta al momento de llevar un control de versiones.

## Crear ExpressJS y EJS

ExpressJs es el framework web que usaremos y EJS es el motor de plantillas que usará ExpressJs para mostrar las vistas HTML. Para instalar ambas librerías tenemos en que ubicarnos dentro de la carpeta donde se encuentra el archivo <b>package.json</b>. Para dicho fin usaremos el siguiente comando:

    > npm install express ejs

Una vez ejecutado el comando, vamos a crear en la raiz del proyecto un archivo llamado <b>index.js</b>, en dicho archivo vamos a colocar el siguiente código.

``` js
const express = require('express');
const app = express();
app.set('view engine', 'ejs');

app.get('/', (req, res) => {
  var locals = {
    title: 'Bienvenido',
  };
  res.render('home', locals);
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Servidor Express escuchando en el puerto ${PORT}`);
});
```

Antes de darle ejecutar al proyecto, vamos a crear una carpeta llamada <b>views</b> en la raiz del proyecto y vamos a crear un archivo llamada <b>home.ejs</b>, cual tendrá el siguiente código:

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  title: <%= title %>
</body>
</html>

```

Para ejecutar el proyecto vamos a ejecutarlo con el siguiente código:

    > node index.js