.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 15 - PGE 2015
===================

Tratamiento de excepciones
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. figure:: images/clase15/excepciones1.png

.. code-block:: c++

	#ifndef EXCEPCIONES_H
	#define EXCEPCIONES_H

	#include <QString>
	#include <QFile>

	class ExcRango  {
	private:
	    QString mensaje;
	public:
	    ExcRango(QString mensaje, int i) : mensaje(mensaje)  {   }
	    QString getMensaje()  {  return mensaje;  }
	};

	class ExcNoArchivo  {
	private:
	    QString archivo;
	    QString mensaje;

	public:
	    ExcNoArchivo(QString archivo) : archivo(archivo)  {
	        QFile file(archivo);
	        if (!file.exists())
	            mensaje.operator=("El archivo " + archivo + " no existe.");
	    }

	    QString getMensaje()  {  return mensaje;  }
	};

	#endif // EXCEPCIONES_H


.. code-block:: c++

	#ifndef ARCHIVADOR_H
	#define ARCHIVADOR_H

	#include <QFile>
	#include <QTextStream>
	#include "excepciones.h"

	class Archivador  {
	private:
	    static QFile *file;

	public:
	    static bool abrir(QString ruta)  {
	        file->setFileName(ruta);

	        if (!file->exists())  {
	            throw ExcNoArchivo(ruta);
	            return false;
	        }

	        return file->open(QIODevice::Append | QIODevice::Text);
	    } 

	    static bool almacenar(QString texto)  {
	        if (!file->isOpen())
	        return false;

	        QTextStream salida(file);
	        salida << texto;
 
	        return true;
	    }
	};

	QFile * Archivador::file = new QFile("./defecto.txt");

	#endif // ARCHIVADOR_H

.. code-block:: c++

	#include <QApplication>
	#include "archivador.h"
	#include <QDebug>

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    try  {
	        Archivador::abrir("./defecto.txt");
	        Archivador::almacenar("11111111");
	    }
	    catch(ExcNoArchivo e)  {
	        qDebug() << e.getMensaje();
	    }

	    return 0;
	}

**Ejercicio 1:**

- Modificar la clase listado para que cuando sea necesario lance la excepción ExcRango cuando se intente acceder a un index fuera de rango. Probarlo luego en la función main.

.. code-block:: c++

	template <class T> class Listado  {
	private:
	    int cantidad;
	    int libre;
	    T *v;

	public:
	    Listado(int n=10) : cantidad(n), libre(0), v(new T[n])  {  }
	    bool add(T nuevo);

	    T get(int i)  {
	        if (i>=libre)
	            throw ExcRango("Listado fuera de rango", i);
	        return v[i];
	    }

	    int length()  {  return libre;  }
	};

	template <class T> bool Listado<T>::add(T nuevo)  {
	    if (libre < cantidad)  {
	        v[libre] = nuevo;
	        libre++;
	        return true;
	    }
	    return false;
	}

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

Welcome to StackEdit!
===================


Hey! I'm your first Markdown document in **StackEdit**[^stackedit]. Don't delete me, I'm very helpful! I can be recovered anyway in the **Utils** tab of the <i class="icon-cog"></i> **Settings** dialog.


