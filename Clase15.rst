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



