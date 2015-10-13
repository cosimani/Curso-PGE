.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 17 - PGE 2015
===================

**Ejemplo: Función callback**

.. code-block:: c++

	#ifndef BOTONES_H
	#define BOTONES_H

	class Boton{
	public:
	    virtual void click()  {  }
	};

	template <class T> class BotonCallBack : public Boton  {
	private:
	    T *destinatario;
	    void (T::*callback)(void);
	public:
	    BotonCallBack(T *otro, void (T::*puntero_funcion)(void))
	        : destinatario(otro), callback(puntero_funcion)  {  }
	
	    void click()  {
	        (destinatario->*callback)();
	    }
	};

	#endif // BOTONES_H

.. code-block:: c++

	#ifndef REPRODUCTOR_H
	#define REPRODUCTOR_H

	#include <QDebug>

	class MP3Player{
	public:
	    void play()  {
	        qDebug() << "Escuchando...";
	    }
	};

	#endif // REPRODUCTOR_H

.. code-block:: c++

	#include <QApplication>
	#include "botones.h"
	#include "reproductor.h"

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    MP3Player mp3;
	    BotonCallBack<MP3Player> *boton;

	    //Conecta un MP3Player a un botón
	    boton = new BotonCallBack<MP3Player>(&mp3, &MP3Player::play);

	    boton->click();

	    return 0;
	}

**Array de punteros a función**

- Los punteros a funciones se pueden agruparse en arreglos

.. code-block:: c++	

	int (* afptr[10])(int);    // array de 10 punteros a función

- Los 10 punteros apuntan a funciones con el mismo prototipo
- Permiten muchas variantes para invocar funciones

.. code-block:: c++	

	int a = afptr[n](x);
	
**Resolución Ejercicio 1: Ordenador** 

.. code-block:: c++

	class Ordenador  {
	public:
	    void burbuja(int * v, int n)  {  /* código */  }
	    void insercion(int * v, int n)  {  /* código */  }

	    void seleccion(int * v, int n)  {  /* código */  }
	};

- Esta clase tendrá distintos métodos de ordenamiento.
- Cada método ordena un array de n cantidad de enteros
- Definir la clase ListaDeEnteros
	- Herede de QVector
	- Que no sea un template
	- Que sólo mantenga elementos del tipo int
	- Definir un método:
	
.. code-block:: c++	
		
	void ordenar(Ordenador::*puntero_funcion)(int * v, int n))
	// Este método ordenará los elementos

**Ejercicio 2:** Realizar la misma aplicación de la clase pasada pero que la funcionalidad de sugerencias se encuentre dentro de una clase LineaDeTexto

**Ejercicio 3:**

- Agregar la funcionalidad a la clase LineaDeTexto para que busque sugerencias en Google
- http://doc.qt.io/qt-5/qtnetwork-googlesuggest-example.html

Ejercicios para OpenGL y Procesamiento de Imágenes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Ejercicio 1:**

- Crear un QCameraViewfinder promovido a QWidget
- Un botón para capturar la imagen de la cámara
- Con el mouse se puede dibujar encima de la imagen como un lápiz
- Un botón para almacenar la imagen resultante.

**Ejercicio 2:**

- Con Archivador almacenar cada vez que se dibuja con el lápiz
- Almacenar con el siguiente formato:
	- Fecha y hora: 21.10.2014-20:53:42 - Píxel inicio: (153, 230) - Fin: (51, 76)
	
**Ejercicio 3:**

- Definir métodos para realizar procesamiento de las imágenes para:
	- Convertir a grises
	- Llevar a negativo
	- Eliminar algún color
- El prototipo puede ser:
	- QImage getGrayImage(QImage imagenOriginal);

**Ejercicio 4:**

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


