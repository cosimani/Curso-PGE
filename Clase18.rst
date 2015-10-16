.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 18 - PGE 2015
===================

**Ejercicio 1:**

- Modificar el ejercicio de la clase ListadoEnteros para usar funciones globales de ordenamiento, es decir, que no se encuentren dentro de Ordenador ni de ninguna clase.

.. code-block:: c++	

	class ListadoEnteros : public QVector<int>  {
	public:
	    void ordenar(void (*pFuncionOrdenamiento)(int *, int))  {
	        (*pFuncionOrdenamiento)(this->data(), this->size());
	    }
	};

.. code-block:: c++		
	///// Desde main se puede utilizar así:

    void (*ordenador)(int *, int) = &burbuja;

    listado.ordenar(ordenador);


**Ejercicio 2:**

- Necesitamos conocer el rendimiento de cada algoritmo de ordenamiento midiendo su tiempo.
- Utilizar un array de punteros a funciones que apunte a cada función global de ordenamiento.
- Utilizar Archivador para almacenar los tiempos en un archivo.
- Utilizar un ListadoEnteros de 50.000 números generados con qrand()

.. code-block:: c++		

	///// Desde main se puede utilizar así:

    void (*ordenador[2])(int *, int);
    ordenador[0] = &burbuja;
    ordenador[1] = &insercion;

    listado.ordenar(ordenador[1]);

**Ejercicio 3:** 

- Agregar la funcionalidad de sugerencias a la clase LineaDeTexto y que dichas sugerencias las busque desde Google.
- http://doc.qt.io/qt-5/qtnetwork-googlesuggest-example.html
- `Descargar LineaDeTexto desde aquí <https://github.com/cosimani/Curso-PGE-2015/blob/master/sources/clase18/lineadetexto.rar?raw=true>`_
- Crear un QtChrome que permita buscar en Google la sugerencia elegida. Con el siguiente aspecto:

.. figure:: images/clase18/navegador.png

Ejercicios para OpenGL y Procesamiento de Imágenes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Ejercicio 4:**

- Crear un QCameraViewfinder promovido a QWidget
- Un botón para capturar la imagen de la cámara
- Con el mouse se puede dibujar encima de la imagen como un lápiz
- Un botón para almacenar la imagen resultante.

**Ejercicio 5:**

- Con Archivador almacenar cada vez que se dibuja con el lápiz
- Almacenar con el siguiente formato:
	- Fecha y hora: 21.10.2014-20:53:42 - Píxel inicio: (153, 230) - Fin: (51, 76)
	
**Ejercicio 6:**

- Definir métodos para realizar procesamiento de las imágenes para:
	- Convertir a grises
	- Llevar a negativo
	- Eliminar algún color
- El prototipo puede ser:
	- QImage getGrayImage(QImage imagenOriginal);

**Ejercicio 7:**

- Imágenes de Google Street View en OpenGL

Usabilidad
^^^^^^^^^^

- Se refiere a la capacidad de ser comprendido, aprendido, usado y ser atractivo.

- El concepto de usabilidad involucra:
	- Aprendizaje
	- Eficiencia (que se logre la tarea o meta)
	- Recordación
	- Manejo de errores
	- Satisfacción

**Mensajes de error**

- Los errores ocurren por falta de conocimiento, comprensión incorrecta o equivocaciones involuntarias.
- Es probable que el usuario esté confundido.
- Mensajes de error demasiado genéricas no ayudan.
- Los sistemas se recuerdan más cuando las cosas van mal.
- Mejorar los mensajes de error es una buena forma de mejorar la interfaz.
- Los logs de errores permiten a los desarrolladores revisar procedimientos y mejorar la documentación.
- Se recomienda crear mensajes de error con tono positivo, especificidad y formato apropiado.

**Tono positivo**

- No condenar al usuario.
- Las palabras MAL, ILEGAL, ERROR deberían eliminarse.
- Los mensajes hostiles alteran a los usuarios no técnicos.
- Error 800405: Fallo del método string de objeto Sistema.

**Especificidad**

- ERROR DE SINTAXIS  ---->  Paréntesis izquierdo sin correspondencia
- ENTRADA ILEGAL     ---->  Escriba la primer letra Enviar, Leer o Eliminar
- DATOS INVÁLIDOS    ---->  Los días deben estar en el intervalo 1 - 31
- NOMBRE INVÁLIDO    ---->  El archivo C:\Datos\datos.txt no existe

**Formato apropiado**  

- Los mensajes que comienzan con un código numérico y misterioso no sirven a los usuarios comunes.
- Llamar la atención pero sin molestar al usuario.
- Mostrar un cuadro de texto cerca del problema pero sin ocultarlo.


