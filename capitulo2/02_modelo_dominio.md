# Capítulo 2 - Modelo del dominio y disciplina de requisitos

# Modelo de Dominio

El modelo del dominio permite representar los conceptos principales que forman parte del sistema y las relaciones existentes entre ellos. En este proyecto, el dominio se corresponde con un sistema automatizado de captura y gestión de golpes de golf mediante visión por computador.

El objetivo de este modelo no es describir todavía la implementación técnica del sistema, sino identificar los elementos relevantes del problema: el jugador, el campo, el hoyo, las cámaras, la sesión de captura, el evento detectado y el clip generado.

A partir de este modelo se establece una base conceptual común que sirve para definir posteriormente los requisitos, los casos de uso y el diseño del sistema.

## Glosario

| Término                | Descripción                                                                                                              |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Usuario                | Persona que utiliza el sistema. Puede actuar como jugador o como administrador según sus permisos.                       |
| Jugador                | Usuario principal del sistema. Escanea el código QR del hoyo, activa la captura y consulta los clips generados.          |
| Administrador          | Usuario con permisos ampliados para consultar usuarios, gestionar clips y acceder a estadísticas globales.               |
| Campo                  | Instalación deportiva donde se desarrolla la actividad de golf. Permite contextualizar los hoyos y los clips generados.  |
| Hoyo                   | Unidad de juego dentro de un campo de golf. Cada clip queda asociado al hoyo donde se ha producido el golpe.             |
| Cámara                 | Dispositivo instalado en el entorno de juego para capturar vídeo. Puede tener distintos roles, como TEE, BACK o GREEN.   |
| Código QR              | Identificador visual que permite activar el contexto de captura de un campo y hoyo concreto.                             |
| Sesión de captura      | Periodo en el que un usuario activa el sistema en un hoyo determinado para registrar sus golpes.                         |
| Evento detectado       | Acción identificada automáticamente por el sistema, normalmente asociada a un swing válido.                              |
| Swing                  | Movimiento realizado por el jugador para ejecutar el golpe de golf. Es el evento principal que debe detectar el sistema. |
| Clip                   | Vídeo generado automáticamente a partir de un golpe detectado y asociado a un usuario, campo y hoyo.                     |
| Clip favorito          | Clip marcado por el usuario para conservarlo y evitar su eliminación automática.                                         |
| Procesamiento de vídeo | Conjunto de operaciones aplicadas al vídeo capturado, como detección, recorte, validación, conversión y almacenamiento.  |
| Estadísticas           | Información calculada a partir de los clips y usuarios registrados, como número de clips generados o favoritos.          |

## Conceptos principales del dominio

Los conceptos principales identificados en el dominio del sistema son los siguientes:

* **Usuario**: representa a la persona que interactúa con el sistema. Puede utilizar la aplicación para activar la captura, consultar clips o realizar tareas de administración.

* **Campo**: representa el espacio deportivo en el que se instala el sistema. Sirve para organizar los hoyos y asociar los clips generados a un contexto físico concreto.

* **Hoyo**: representa la zona específica del campo donde se produce el golpe. Cada hoyo puede tener cámaras asociadas.

* **Cámara**: representa el dispositivo físico encargado de capturar vídeo. En el prototipo se contemplan cámaras con roles diferenciados: cámara lateral del tee, cámara posterior y cámara orientada al green.

* **Sesión de captura**: representa la activación del sistema por parte de un jugador en un hoyo determinado. Esta sesión permite asociar correctamente los clips generados al usuario y al contexto de juego.

* **Evento detectado**: representa el golpe identificado por el sistema. No todos los movimientos del jugador generan un clip; el sistema debe diferenciar entre movimientos previos, swings de práctica y golpes válidos.

* **Clip**: representa el resultado final del proceso. Es el vídeo generado automáticamente tras detectar un golpe válido y puede ser consultado posteriormente desde la aplicación móvil.

## Diagrama de clases del dominio

| Diagrama                                                                    | Código fuente                                                                  |
| --------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| ![Diagrama de clases del dominio](../recursos/diagramas/diagramaClases.png) | [Ver código del diagrama de clases](../recursos/diagramas/diagramaClases.puml) |

El diagrama de clases del dominio representa las entidades principales del sistema y sus relaciones. En él se muestra cómo un usuario puede activar sesiones de captura, cómo estas sesiones se asocian a un campo y un hoyo, y cómo los eventos detectados pueden dar lugar a la generación de clips.

El modelo también incorpora la cámara como elemento del entorno físico asociado a un hoyo. Aunque el código QR interviene en el proceso de activación, no se considera la entidad central del dominio, sino un mecanismo utilizado para identificar el contexto de captura.

De esta forma, el sistema puede mantener la trazabilidad entre el jugador, el campo, el hoyo, las cámaras y el clip generado.

## Diagrama de objetos

| Diagrama                                                          | Código fuente                                                                    |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| ![Diagrama de objetos](../recursos/diagramas/diagramaObjetos.png) | [Ver código del diagrama de objetos](../recursos/diagramas/diagramaObjetos.puml) |

El diagrama de objetos muestra un ejemplo concreto del funcionamiento del sistema en un momento determinado.

En este ejemplo, un jugador inicia una sesión de captura en un hoyo concreto de un campo. Las cámaras asociadas a ese hoyo quedan vinculadas al contexto activo. Cuando el sistema detecta un swing válido, se genera un evento detectado que posteriormente da lugar a un clip asociado al usuario y al hoyo correspondiente.

Este diagrama ayuda a comprender cómo se materializan las clases del dominio en una situación real de uso.

## Diagrama de estados

| Diagrama                                                          | Código fuente                                                                    |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| ![Diagrama de estados](../recursos/diagramas/diagramaEstados.png) | [Ver código del diagrama de estados](../recursos/diagramas/diagramaEstados.puml) |

El diagrama de estados representa el ciclo de vida del clip dentro del sistema.

Un clip comienza como contenido pendiente o en proceso de generación. Una vez que el sistema valida el golpe y procesa el vídeo, el clip pasa a estar disponible para el usuario. A partir de ese momento, puede ser reproducido desde la aplicación móvil, marcado como favorito o quedar sujeto a la política de expiración.

Si el clip no se marca como favorito, puede pasar a estado expirado y ser eliminado automáticamente por el sistema. En cambio, si el usuario lo marca como favorito, se conserva para futuras consultas.

Este comportamiento permite controlar el almacenamiento del sistema y conservar únicamente los clips considerados relevantes por el usuario.
