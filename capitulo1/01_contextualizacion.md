
# Capítulo 1 - Contextualización del proyecto

## 1. Introducción

El presente Trabajo de Fin de Grado se centra en el diseño e implementación de un sistema automatizado para la captura de golpes de golf mediante visión por computador y una arquitectura distribuida.

La idea principal del proyecto surge de una limitación habitual en la práctica del golf: cuando un jugador quiere revisar sus golpes, normalmente necesita grabarse manualmente, colocar un dispositivo móvil, pedir ayuda a otra persona o interrumpir la sesión para preparar la cámara. Esto provoca que muchos golpes no se registren, que las grabaciones sean incompletas o que solo exista una vista parcial del golpe.

Frente a esta situación, el sistema propuesto busca automatizar la captura del golpe mediante cámaras instaladas en el entorno de juego. El usuario únicamente debe iniciar sesión en la aplicación móvil y escanear el código QR asociado al hoyo. A partir de ese momento, el sistema activa el contexto de captura correspondiente y las cámaras registran el golpe de forma automática.

El objetivo del proyecto no es desarrollar una solución comercial definitiva, sino construir un prototipo funcional que demuestre la viabilidad técnica del sistema: detectar el swing, generar clips de vídeo, asociarlos al usuario y al hoyo, almacenarlos y permitir su consulta posterior desde una aplicación móvil.

---

## 1.1 Contexto del proyecto

En los últimos años, las tecnologías digitales aplicadas al deporte han adquirido una gran importancia. La visión por computador, el análisis de vídeo y los sistemas de detección automática permiten registrar acciones deportivas, analizarlas y ofrecer información útil al usuario sin necesidad de intervención manual constante.

En el caso del golf, el análisis del golpe resulta especialmente relevante. El jugador no solo necesita observar su movimiento técnico, sino también el comportamiento de la bola después del impacto. Sin embargo, las grabaciones tradicionales suelen depender de una única cámara o de un teléfono móvil colocado manualmente, lo que limita la información disponible.

Además, en muchos casos el jugador no dispone de una grabación completa del golpe. Puede ocurrir que el vídeo empiece tarde, que el móvil no esté bien colocado, que no se aprecie la salida de la bola o que no se capture el resultado del golpe. Esto reduce la utilidad del vídeo como herramienta de revisión.

Por este motivo, el proyecto plantea una solución basada en cámaras fijas, visión por computador y procesamiento de vídeo. La finalidad es registrar automáticamente el golpe desde distintos puntos de vista y generar un clip final asociado al usuario y al contexto del hoyo.

---

## 1.2 Problema identificado

El proceso tradicional de grabación de golpes de golf presenta varias limitaciones:

* Dependencia de la intervención manual del jugador o de otra persona.
* Interrupción de la práctica para preparar la grabación.
* Captura desde un único punto de vista.
* Falta de registro visual del resultado del golpe.
* Dificultad para asociar automáticamente cada vídeo al jugador, al campo y al hoyo.
* Ausencia de un sistema integrado para consultar posteriormente los clips generados.

Estas limitaciones provocan que el registro de golpes no sea sistemático ni cómodo para el usuario. Además, el contenido obtenido puede resultar incompleto, poco homogéneo o difícil de organizar.

En consecuencia, se identifica la necesidad de un sistema capaz de registrar golpes de forma automática, mantener la asociación entre usuario, hoyo y clip, y permitir la consulta posterior del contenido desde una aplicación móvil.

---

## 1.3 Solución propuesta

La solución propuesta consiste en un sistema automatizado de captura de golpes de golf basado en tres elementos principales:

1. **Aplicación móvil**, utilizada por el jugador para iniciar sesión, escanear el código QR del hoyo, consultar clips, reproducir vídeos y marcar favoritos.

2. **Backend**, encargado de gestionar usuarios, cámaras, sesiones, clips, permisos, estadísticas y almacenamiento de la información.

3. **Módulo de visión por computador**, responsable de analizar el vídeo de las cámaras, detectar el swing válido y generar los segmentos de vídeo correspondientes.

El funcionamiento general del sistema es el siguiente:

1. El jugador inicia sesión en la aplicación móvil.
2. Escanea el código QR asociado al hoyo.
3. El backend identifica el campo, el hoyo y las cámaras configuradas.
4. Las cámaras quedan asociadas a la sesión activa del usuario.
5. La cámara del tee detecta el swing válido.
6. Las cámaras complementarias registran sus segmentos correspondientes.
7. El backend recibe los vídeos, los procesa y genera el clip final.
8. El usuario puede consultar el clip desde la aplicación móvil.

De esta forma, el sistema elimina la necesidad de que el jugador coloque manualmente una cámara o inicie la grabación antes de cada golpe.

---

## 2. Marco tecnológico

El sistema desarrollado se apoya en varias tecnologías principales:

### Visión por computador

La visión por computador permite analizar imágenes y vídeo para identificar elementos relevantes dentro de una escena. En este proyecto se utiliza para detectar el swing del jugador y validar el golpe a partir de información visual.

### YOLO

YOLO se emplea como modelo de detección de objetos en tiempo real. Su función principal dentro del sistema es apoyar la detección de la bola de golf y contribuir a la validación del golpe.

### MediaPipe

MediaPipe se utiliza para analizar el movimiento corporal del jugador mediante la estimación de pose. Esta información permite detectar cambios en partes del cuerpo relevantes durante el swing, como muñecas, hombros o brazos.

### Procesamiento de vídeo

El procesamiento de vídeo permite recortar los fragmentos relevantes, preparar los clips generados y convertirlos a un formato compatible con la aplicación móvil.

### Arquitectura distribuida

El sistema se plantea como una arquitectura modular formada por aplicación móvil, backend, base de datos, módulo de procesamiento y cámaras IP. Aunque el prototipo puede ejecutarse en un entorno simplificado, la separación lógica de componentes facilita su evolución futura.

---

## 3. Justificación de la propuesta

La solución propuesta se justifica por la necesidad de mejorar la forma en que los jugadores registran y consultan sus golpes.

Las soluciones comerciales existentes suelen estar orientadas al análisis técnico avanzado, pero pueden requerir infraestructuras costosas, sensores adicionales o instalaciones complejas. Por otro lado, las aplicaciones móviles de grabación son más accesibles, pero dependen de la intervención manual del usuario y normalmente ofrecen una única perspectiva del golpe.

El sistema desarrollado busca cubrir un punto intermedio: una solución automatizada, basada en cámaras fijas y visión por computador, capaz de generar contenido visual real del golpe sin obligar al jugador a interrumpir la práctica.

Las principales ventajas de la propuesta son:

* Automatización de la captura.
* Reducción de la intervención manual.
* Asociación automática entre usuario, campo, hoyo y clip.
* Captura desde varios puntos de vista.
* Consulta posterior desde una aplicación móvil.
* Gestión de favoritos y conservación de clips relevantes.
* Posibilidad de ampliación futura a más cámaras, hoyos o estadísticas.

---

## 4. Objetivos

### Objetivo general

Desarrollar un sistema automatizado basado en técnicas de visión por computador capaz de detectar el momento del swing en el golf y generar clips de vídeo del golpe desde diferentes ángulos, incorporando información visual sobre el comportamiento de la bola tras el impacto.

### Objetivos específicos

1. Detectar automáticamente el momento del swing en secuencias de vídeo capturadas en un entorno real o representativo.

2. Generar automáticamente clips de vídeo del golpe sin intervención manual del usuario.

3. Implementar un sistema multicámara que genere clips válidos únicamente cuando estén disponibles las vistas configuradas para el hoyo.

4. Registrar visualmente el resultado del golpe mediante una cámara fija orientada a la zona del green.

5. Validar el funcionamiento global del sistema comprobando la detección del golpe, la generación del clip y la disponibilidad posterior del contenido en la aplicación móvil.

---

## 4.1 Alcance de la propuesta

El proyecto se centra en el desarrollo de un prototipo funcional que permite validar el flujo completo del sistema.

Dentro del alcance se incluyen las siguientes funcionalidades:

* Inicio de sesión de usuarios.
* Escaneo de código QR para activar el contexto de captura.
* Asociación de usuario, campo, hoyo y cámaras.
* Captura de vídeo mediante cámaras físicas.
* Detección automática del swing.
* Generación automática de clips.
* Almacenamiento de clips y metadatos.
* Consulta y reproducción desde la aplicación móvil.
* Marcado y desmarcado de clips favoritos.
* Estadísticas personales y globales.
* Funciones básicas de administración de usuarios y clips.

Quedan fuera del alcance del prototipo:

* Instalación comercial definitiva en un campo de golf.
* Seguimiento avanzado de la trayectoria completa de la bola.
* Estimación automática exacta del punto de caída.
* Análisis biomecánico profesional del swing.
* Integración con sistemas comerciales como TrackMan o Toptracer.
* Despliegue definitivo en una infraestructura cloud escalable.

Por tanto, el prototipo debe entenderse como una validación técnica de la solución propuesta.

---

## 4.2 Metodología

El desarrollo del proyecto se ha llevado a cabo siguiendo un enfoque iterativo e incremental.

Primero se analizó el problema y se definieron los requisitos principales del sistema. Posteriormente, se diseñó la arquitectura general, separando los distintos componentes: aplicación móvil, backend, base de datos, procesamiento de vídeo y cámaras.

A partir de este diseño, se fue implementando el sistema de forma progresiva. En primer lugar, se desarrollaron las funciones básicas de autenticación, gestión de usuarios y clips. Después, se incorporó el escaneo QR y la asociación del contexto de captura. Más adelante, se integró el módulo de visión por computador y el sistema multicámara.

Finalmente, se realizaron pruebas funcionales para validar el flujo completo: escaneo QR, activación de cámaras, detección del swing, generación del clip, almacenamiento y reproducción desde la aplicación móvil.

---

## 4.3 Hipótesis

La hipótesis principal del proyecto es que la aplicación de técnicas de visión por computador y procesamiento de vídeo permite automatizar la captura de golpes de golf, reduciendo la intervención manual del jugador y mejorando la disponibilidad de contenido visual para su revisión posterior.

Bajo esta hipótesis, el sistema desarrollado debe ser capaz de detectar un swing válido, generar un clip asociado al golpe y permitir su consulta desde la aplicación móvil.

Si el prototipo consigue completar correctamente este flujo, se demuestra la viabilidad técnica de una solución automatizada para la captura y gestión de golpes de golf en un entorno real o representativo.
