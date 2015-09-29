.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 13 - PGE 2015
===================

Bluemix - Material para primer parcial
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**¿Cómo utilizar Bluemix?**

- Crear una cuenta en: https://console.ng.bluemix.net (gratis durante 30 días, luego se paga por lo que se usa)
- Crear una instancia de Node-Red
- Crear un host bajo el dominio mybluemix.net donde se alojará el sitio ( por ejemplo: misitio.mybluemix.net )
- Añadir un alias en el Git de Bluemix
- Con el botón "Edit Code" comenzar a editar el código fuente y archivos dentro de Git
- Desde https://www.dropbox.com/sh/5ngfvs2wkq7amnl/AAAMLK8XkgZQ9LEMEl-sYkGBa?dl=0 se puede descargar algunos documentos y código fuente de ejemplo
- Copiar los tres archivos (cloud.html, d3.layout.cloud.js y d3.v3.min.js) dentro del sitio (File - Import file or zip). Abrir el archivo cloud.html y eliminar el uso del puerto 1880.
- Tildamos "Edición en directo" (live edit) para que tome todos los cambios al instante
- Para poner el sitio online por primera vez clic en el ícono "play"
- Abrimos en un navegador http://misitio.mybluemix.net/ o directamente a http://misitio.mybluemix.net/cloud.html
- Luego podemos ir a http://misitio.mybluemix.net/red/ para el flow editor (aquí podemos importar el archivo wordcloud.txt para ver un ejemplo)
- Podemos usar módulos de twitter, mailing, visión artificial, etc.

- Más info en: http://node-red-ws.mybluemix.net/presentation/index.html#slide-12

**Ejemplo 1**

- En Node-RED dentro de una sheet
- Arrastrar un nodo entrada Inject y abrir su configuración
- Payload como string y colocar "Primer parcial"
- Arrastrar un nodo salida debug
- Conectar los dos nodos Inject y Debug
- Clic en Deploy
- Ir al tab Debug que está a la derecha
- Hacer clic en el nodo inject para que envíe el mensaje

**Ejemplo 2**

- Eliminar la conexión entre Inject y Debug
- Abrir configuración de Inject y seleccionar Payload blank
- Arrastrar un nodo function e ingresar el siguiente código: msg.name = 'Parcial'; return msg;
- Conectar los nodos.
- Arrastrar un nodo Template, abrir la configuración y setear lo siguiente: El texto enviado es: {{name}}
- Deploy y probar

**Ejemplo 3**

- Reemplazar el nodo Inject por un nodo http
- Elegir GET y en la URL poner /parcial
- Conectar el nodo http al nodo function
- En la configuración de function: msg.name = msg.req.query.name; return msg;
- Borrar el nodo Debug
- Agregar un nodo http response node y conectarlo al nodo temcplate
- Deploy y probar ingresando a http://misitio.mybluemix.net/parcial?name=Carlos 

**Ejercicio**

- Armar una base de datos con SQLite 
- La base tiene la tabla http con los siguientes campos: id campo1 campo2 campo3
- La tabla sólo tendrá un único registro
- Con una aplicación Qt leer la base de datos y pasar por GET los parámetros a http://misitio.mybluemix.net/parcial?campo1=dato1&campo2=dato2&campo3=dato3
- Crear la aplicación en Bluemix para que pueda publicar estos datos en una tabla HTML
- Finalmente mostrarlo en un QWebView



