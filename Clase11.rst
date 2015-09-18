.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 11 - PGE 2015
===================

**CLI >> GUI >> NUI**

.. figure:: images/clase11/cli_gui_nui.png
	:target: https://prezi.com/_iqvhrobpe0p/desarrollo/

	
Levantar frame por frame: Clase QAbstractVideoSurface
=====================================================

- QAbstractVideoSurface es una clase abstracta
- Proporciona streaming de video a través de la función virtual pura present()

.. code-block:: c++

	bool QAbstractVideoSurface::present ( const QVideoFrame & frame ) [pure virtual]


**Clase Capturador para obtener los frames de la cámara**

.. code-block:: c++

	class Capturador : public QAbstractVideoSurface  {
		Q_OBJECT

	public:
		Capturador(QObject *parent = 0);

		QList<QVideoFrame::PixelFormat> supportedPixelFormats(
				QAbstractVideoBuffer::HandleType handleType = QAbstractVideoBuffer::NoHandle) const;

		bool present(const QVideoFrame &frame);

		QVideoFrame getFrameActual()  {  return frameActual;  }

	private:
		QVideoFrame frameActual;
	};


- QVideoFrame encapsula los datos de video (bits, ancho, alto, etc.)
- Para acceder a los bits es necesario mapearlo con el método map()
- El mapeo deja en memoria los datos para se accedidos.

.. code-block:: c++

	bool Capturador::present(const QVideoFrame &frame)  {
		frameActual = frame;
		
		frameActual.map(QAbstractVideoBuffer::ReadOnly);
		
		return true;  // Con la idea de devolver true si este frame fue usado
	}


- La función virtual pura supportedPixelFormats() devuelve un listado de formatos soportados.

.. code-block:: c++

	QList<QVideoFrame::PixelFormat> Capturador::supportedPixelFormats(QAbstractVideoBuffer::HandleType handleType) const  {
		if (handleType == QAbstractVideoBuffer::NoHandle) {
			return QList<QVideoFrame::PixelFormat>()
					<< QVideoFrame::Format_RGB32
					<< QVideoFrame::Format_ARGB32;
		} else {
			return QList<QVideoFrame::PixelFormat>();
		}
	}


**El constructor**

.. code-block:: c++
   :caption: Read Hacker News on a budget
   :url: http://news.ycombinator.com
   :title: Hacker News

	Capturador::Capturador(QObject *parent) : QAbstractVideoSurface(parent)  {

	}

**Podemos ahora llevar las imágenes de la cámara como textura a OpenGL**

.. code-block:: c++

	class Visual : public Ogl  {
		Q_OBJECT
	public:
		Visual();
		void iniciarCamara();

	protected:
		void initializeGL();
		void resizeGL(int ancho, int alto);
		void paintGL();

	private:
		Capturador * capturador;
		QCamera * camera;

		void cargarTexturas();
		void cargarTexturaCamara();

		unsigned char *texturaCielo;
		unsigned char *texturaMuro;
		GLuint idTextura[2];

		unsigned char *texturaCamara;
		GLuint idTexturaCamara[1];
	};


	void Visual::iniciarCamara()  {
		capturador = new Capturador;

		QList<QCameraInfo> cameras = QCameraInfo::availableCameras();

		for (int i=0 ; i<cameras.size() ; i++)  {
			qDebug() << cameras.at(i).description();

			if (cameras.at(i).description().contains("Truevision", Qt::CaseInsensitive))  {
				camera = new QCamera(cameras.at(i));
				camera->setViewfinder(capturador);
				camera->start(); // to start the viewfinder
			}
		}

		glGenTextures(1, idTexturaCamara);
	}


	void Visual::cargarTexturaCamara()  {

		QVideoFrame frameActual = capturador->getFrameActual();
		texturaCamara = frameActual.bits();

		glBindTexture(GL_TEXTURE_2D, idTexturaCamara[0]);  // Activamos idTextura.
		glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR); 
		glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR); 

		glTexImage2D(GL_TEXTURE_2D, 0, 3, frameActual.width(), frameActual.height(), 0, GL_BGRA, GL_UNSIGNED_BYTE, texturaCamara);
	}

**Ejercicio 1:**

- Usar el ejercicio del primer parcial
- Colgar de la tapia del fondo un monitor LCD mostrando las imágenes de la cámara

**Ejercicio 2:**

- En el medio del escenario tapia-piso-cielo dibujar un cubo girando
- Pegar la textura de la cámara en cada lado del cubo



	






