.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 12 - PGE 2015
===================

Ejemplos de ejercicios del primer parcial
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Ejercicio 1:**

- Se adjunta el código fuente para que sea compilado en una librería estática.
- Separar en distintas carpetas los archivos de cabecera y la librería estática (por ejemplo, en las carpetas 'includes' y 'libs'

.. figure:: images/clase12/descargarFuenteEjercicio1.png
   :target: sources/clase12/LineaDeTextoLib.rar

**Ejercicio 2:**

- En un proyecto nuevo, utilizar la librería generada en el ejercicio 1 para crear la GUI de la siguiente figura.
- Los 3 campos de texto son objetos de LineaDeTexto promovidos a QLineEdit dentro del QtDesigner.
- Presionando el QPushButton se realiza la concatenación de los textos con el operator+ de LineaDeTexto y se muestra el resultado en el tercer campo de texto.

.. figure:: images/clase12/ejercicio1.png

**Ejercicio 3:**

- Incorporar a la clase LineaDeTexto del ejercicio 1 la característica del popup para sugerencias.
- Compilarlo como librería estática en el archivo libLineaConSugerenciaLIB.a
- Usarlo nuevamente en otro proyecto.




