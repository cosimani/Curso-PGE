.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 16 - PGE 2015
===================

**Clase QThread**

- Permite crear hilos de ejecución para realizar varias tareas a la vez. 
- Proporciona el método start() para iniciar el hilo.
- Emite señales para indicar el inicio y fin de la ejecución del hilo.
- Se necesita reimplementar el método run() en una clase derivada de QThread.
- El código dentro de run() se ejecuta en un hilo y finaliza cuando retorna.
- La programación miltihilo es un paradigma útil para realizar tareas que consumen tiempo sin congelar la interfaz de usuario.

.. code-block:: c++

	class MiHilo : public QThread  {
	    Q_OBJECT

	protected:
	    void run();
	};

	void MiHIlo::run()  {

	    ...

	}

	
- Las clases no GUI (QTimer, QTcpSocket, QFtp, etc.) fueron diseñadas para funcionar en un hilo independiente.
- Las clases GUI (QWidget y derivadas) sólo se puede usar desde el hilo principal.
- Para consultar el estado del hilo podemos utilizar isFinished() o isRunning().
- Podríamos terminar un hilo a fuerza bruta con terminate().
- Dormimos el hilo con: sleep(int seg) o msleep(int miliseg) o usleep(int microseg)
	
**Ejercicio 1:**

- Diseñar una aplicación GUI que escriba en un archivo muchísimos caracteres de tal forma se note que la interfaz de usuario se bloquea hasta finalizar la escritura.
- Luego de esto, utilizar un hilo distinto para escribir la misma cantidad de caracteres.


Función callback
^^^^^^^^^^^^^^^^

- Función que se llama a través de un puntero a función.
- Se puede utilizar como parámetro de otra función.
- Cuando la función que recibe este puntero a función hace uso de este, se dice que hace una retrollamada (callback).
- Si en la clase Listado deseamos que se ordenen los datos pero no queremos incluir (en Listado) la lógica de un método de ordenamiento, podemos pedir al programador que nos pase como parámetro un puntero a su propia función de ordenamiento.
- Se podría utilizar para una simple notificación o comunicación de dos vías (similar a las signals y slots).
- Cuando un diseñador de bibliotecas quiere notificar al programador sobre algún suceso, puede solicitar un puntero a función.

**Declaraciones de punteros a funciones:**

.. code-block:: c++

	void (*fptr)();  
	// puntero a una función sin parámetros que devuelve void.

	void (*fptr)(int);	
	// puntero a función que recibe int y devuelve void.

	int (*fptr)(int, char);		
	// acepta int y char y devuelve un int.

	int * (*fptr)();	
	// puntero a función, sin argumentos y devuelve puntero a int


**Declaraciones de punteros a funciones (o métodos) de clases:**

.. code-block:: c++

	void (C::*puntero) (int);  // puntero a método de la clase C

	int (C::*puntero) ();

- Antes de usar un puntero a función es necesario definirlo (asignarle un valor).
- El valor es la dirección de memoria donde inicia una función concreta.

.. code-block:: c++

	char funcion(int);  // Declara una función concreta

	char (*puntero_funcion) (int);  // Declara un puntero a función

	puntero_funcion = &funcion;  // Asigna al puntero la dirección de memoria de funcion(int)


**Luego de declarado y definido, podemos usarlo de dos formas:**

- Acceder (invocar), a la función que representa
- Usarlo como parámetro de otra función.

**Invocación**

.. code-block:: c++

	char funcion(int);  // Declara función concreta. Suponemos que está definida en otro lugar.

	char (*puntero_funcion)(int);  // Declaramos puntero a función

	puntero_funcion = &funcion;  // Asigna la dirección de memoria

	int i = 10;
	char c;

	c = (*puntero_funcion)(i);

**Ejemplo**

.. code-block:: c++

	#include <iostream>

	void funcion() {  std::cout << "Una funcion cualquiera" << std::endl; }
	void (*puntero_funcion)() = &funcion; 

	int main ()  {      
	    funcion();     
	    (*puntero_funcion)(); 
	    puntero_funcion();   

	    return 0;
	}

	// Salida:
	// Una funcion cualquiera
	// Una funcion cualquiera
	// Una funcion cualquiera

**Paso de funciones como argumento**

.. code-block:: c++

	void funcion(void (*puntero_funcion)() ) {  
	    // Código de este método

	    (*puntero_funcion)();  // Llama a la función apuntada
	}

**Ejemplo:**

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

**Ejercicio 1:** Definir la siguiente clase:

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

**Ejercicio 2:** Realizar la misma aplicación de la clase pasada pero que la funcionalidad de sugerencias se encuentre dentro de una clase Line

**Ejercicio 3:**

- Realizar la misma aplicación de la clase pasada pero que las sugerencias las busque en Google
- http://qt-project.org/doc/qt-4.8/network-googlesuggest.html


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


