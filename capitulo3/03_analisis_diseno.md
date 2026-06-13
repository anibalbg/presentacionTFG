# Capítulo 3 - Análisis y diseño

## Introducción

Una vez definidos el modelo del dominio, los requisitos funcionales, los actores y los casos de uso principales, el siguiente paso consiste en transformar esa especificación en una estructura más cercana a la implementación.

El análisis y diseño del sistema permite identificar los componentes principales de la solución, sus responsabilidades, la forma en que se comunican entre sí y la organización interna necesaria para desarrollar el prototipo.

En este capítulo se presentan los elementos principales del diseño del sistema:

* Arquitectura general.
* Análisis y diseño de casos de uso.
* Diagramas de secuencia de diseño.
* Clases modelo, vista y controladoras.
* Organización por paquetes.
* Modelo lógico de base de datos.
* Modelo de despliegue.
* Trazabilidad entre requisitos, casos de uso y diseño.

---

## Arquitectura del sistema

El sistema propuesto presenta una arquitectura distribuida, formada por varios componentes físicos y lógicos que colaboran entre sí para realizar el flujo completo de captura, procesamiento, almacenamiento y consulta de clips.

Los componentes principales son:

| Componente                | Responsabilidad                                                                                                                      |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Aplicación móvil          | Permite al usuario iniciar sesión, escanear el QR, consultar clips, reproducir vídeos, gestionar favoritos y acceder a estadísticas. |
| Backend                   | Coordina la autenticación, usuarios, sesiones, cámaras, clips, permisos, estadísticas y almacenamiento.                              |
| Módulo de procesamiento   | Analiza el vídeo capturado, detecta el swing válido y genera el contenido audiovisual correspondiente.                               |
| Cámaras IP                | Capturan el entorno de juego desde diferentes puntos de vista.                                                                       |
| Base de datos             | Almacena la información estructurada del sistema.                                                                                    |
| Almacenamiento multimedia | Conserva los ficheros de vídeo generados para su reproducción posterior.                                                             |


[Diagrama Arquitectura del sistema](../documentacion/imagenes/arquitecturalDelSistema.svg) |

La arquitectura separa las responsabilidades del sistema en capas diferenciadas. La aplicación móvil actúa como interfaz de usuario, el backend concentra la lógica de negocio, el módulo de procesamiento ejecuta la detección y generación de clips, y la base de datos junto con el almacenamiento multimedia conservan la información necesaria.

Esta separación permite que el sistema sea más mantenible y facilite futuras ampliaciones, como añadir nuevas cámaras, nuevos hoyos o nuevos mecanismos de almacenamiento.

---

## Capas principales de la arquitectura

La solución se organiza en varias capas funcionales:

### Capa de presentación

Está formada por la aplicación móvil. Su responsabilidad es mostrar información al usuario y recoger sus acciones, como iniciar sesión, escanear el QR, consultar clips o marcar favoritos.

### Capa de servicios

Está formada por el backend. Es el punto central de coordinación del sistema. Gestiona usuarios, autenticación, permisos, sesiones de captura, cámaras, clips, favoritos y estadísticas.

### Capa de procesamiento

Contiene los servicios encargados de analizar el vídeo procedente de las cámaras. En esta capa se detecta el swing, se valida el evento deportivo y se genera el clip correspondiente.

### Capa de captura

Está formada por las cámaras instaladas en el entorno de juego. Estas cámaras capturan el vídeo desde distintas perspectivas y proporcionan la entrada visual al sistema.

### Capa de persistencia

Incluye la base de datos y el almacenamiento multimedia. La base de datos almacena información estructurada, mientras que los vídeos generados se conservan como ficheros multimedia.

---

## Análisis de casos de uso

A partir de los casos de uso definidos en el capítulo anterior, se analizan las responsabilidades internas necesarias para llevarlos a cabo.

Los casos de uso pueden agruparse en cinco bloques funcionales:

| Bloque funcional                   | Casos de uso incluidos            | Responsabilidad principal                                      |
| ---------------------------------- | --------------------------------- | -------------------------------------------------------------- |
| Acceso y autenticación             | CU-01 Login                       | Validar la identidad del usuario y aplicar permisos.           |
| Activación del contexto de captura | CU-02 Escanear código QR del hoyo | Asociar usuario, campo, hoyo, sesión y cámaras.                |
| Consulta de clips                  | CU-03 Consultar clips generados   | Mostrar al usuario los clips disponibles según sus permisos.   |
| Conservación y favoritos           | CU-04 Marcar/Desmarcar favorito   | Gestionar el estado de favorito y la política de expiración.   |
| Estadísticas y administración      | CU-05 a CU-09                     | Consultar estadísticas y realizar operaciones administrativas. |

Este análisis permite pasar de una descripción externa de los casos de uso a una visión interna del sistema, identificando qué elementos deben intervenir para completar cada operación.

---

## Diseño de casos de uso

Para diseñar los casos de uso se utiliza una separación basada en clases modelo, clases vista y clases controladoras.

| Tipo de clase        | Función                                                                                                  |
| -------------------- | -------------------------------------------------------------------------------------------------------- |
| Clases modelo        | Representan la información principal del sistema.                                                        |
| Clases vista         | Representan la interacción con el usuario.                                                               |
| Clases controladoras | Coordinan la ejecución de cada caso de uso.                                                              |
| Servicios internos   | Encapsulan tareas técnicas como detección, procesamiento, generación de clips o cálculo de estadísticas. |

Esta separación permite reducir el acoplamiento entre interfaz, lógica de negocio y persistencia. Además, facilita que cada parte del sistema pueda evolucionar de forma independiente.

---

## Diagramas de secuencia de diseño

Para representar el comportamiento interno de los flujos más relevantes se han definido dos diagramas de secuencia de diseño.

### Escaneo del código QR


 [Diagrama secuencia QR](../documentacion/imagenes/secuenciaQR.svg) |

Este diagrama representa el proceso de activación del contexto de captura. El jugador escanea el código QR desde la aplicación móvil y el sistema utiliza el identificador recibido para localizar la cámara asociada. A partir de esa cámara se obtiene el campo, el hoyo y el rol de captura correspondiente.

El resultado del flujo es la creación o actualización de una sesión activa, que permite asociar correctamente los clips generados posteriormente al usuario y al contexto de juego.

### Generación automática del clip


[Diagrama secuencia clip](../documentacion/imagenes/secuenciaClip.svg) |

Este diagrama representa el proceso interno de generación de un clip. La cámara proporciona la secuencia de vídeo al módulo de detección, que analiza el movimiento del jugador y la información visual de la bola.

Si el evento detectado se considera válido, el sistema recorta el intervalo relevante, procesa el vídeo, almacena el archivo generado y crea el registro del clip asociado al usuario, al hoyo y al evento detectado.

---

## Análisis de clases

El sistema se organiza en tres grupos principales de clases: modelo, vista y controladoras.

### Clases modelo


[Diagrama clases modelo](../documentacion/imagenes/clasesModelo.svg) |

Las clases modelo representan los conceptos principales sobre los que opera el sistema.

Entre ellas se encuentran:

| Clase           | Responsabilidad                                                                       |
| --------------- | ------------------------------------------------------------------------------------- |
| Usuario         | Representa una cuenta registrada y permite diferenciar entre jugador y administrador. |
| Campo           | Representa el campo de golf donde se instala el sistema.                              |
| Hoyo            | Representa el hoyo concreto en el que se produce la captura.                          |
| Camara          | Representa una cámara registrada y asociada a un hoyo.                                |
| SesionCaptura   | Representa la activación del sistema por parte de un usuario.                         |
| EventoDetectado | Representa un swing válido identificado por el sistema.                               |
| Clip            | Representa el vídeo generado y asociado al usuario y al contexto de juego.            |

Estas clases forman la base conceptual del sistema y permiten mantener la trazabilidad entre usuario, contexto de captura, evento detectado y clip generado.

### Clases vista


[Diagrama clases vista](../documentacion/imagenes/vista.svg) |

Las clases vista representan los puntos de interacción entre el usuario y el sistema. Su responsabilidad es mostrar información y recoger acciones, sin contener la lógica principal de negocio.

Entre las vistas principales se encuentran:

| Vista                       | Función                                               |
| --------------------------- | ----------------------------------------------------- |
| VistaLogin                  | Permite introducir credenciales e iniciar sesión.     |
| VistaEscaneoQR              | Permite escanear el QR del hoyo.                      |
| VistaListadoClips           | Muestra los clips disponibles.                        |
| VistaDetalleClip            | Permite reproducir el clip y gestionar su favorito.   |
| VistaEstadisticasPersonales | Muestra estadísticas del jugador.                     |
| VistaGestionClips           | Permite al administrador consultar y gestionar clips. |
| VistaGestionUsuarios        | Permite al administrador gestionar usuarios.          |
| VistaEstadisticasGlobales   | Muestra estadísticas globales del sistema.            |

### Clases controladoras


 [Diagrama clases controladoras](../documentacion/imagenes/controladora.svg) |

Las clases controladoras coordinan la ejecución de los casos de uso. Actúan como intermediarias entre las vistas, las clases modelo y los servicios internos.

| Controlador                                | Caso de uso asociado                    |
| ------------------------------------------ | --------------------------------------- |
| ControladorLogin                           | CU-01 Login                             |
| ControladorEscanearQR                      | CU-02 Escanear código QR del hoyo       |
| ControladorConsultarClipsGenerados         | CU-03 Consultar clips generados         |
| ControladorMarcarDesmarcarClipFavorito     | CU-04 Marcar/Desmarcar favorito         |
| ControladorConsultarEstadisticasPersonales | CU-05 Consultar estadísticas personales |
| ControladorGestionarClipsRegistrados       | CU-06 Gestionar clips registrados       |
| ControladorConsultarUsuariosSistema        | CU-07 Consultar usuarios del sistema    |
| ControladorGestionarUsuariosSistema        | CU-08 Gestionar usuarios del sistema    |
| ControladorConsultarEstadisticasGlobales   | CU-09 Consultar estadísticas globales   |

Esta organización permite que cada caso de uso tenga una responsabilidad de control claramente identificada.

---

## Servicios internos del sistema

Además de las clases modelo, vista y controladoras, el diseño incorpora servicios internos encargados de tareas técnicas reutilizables.

| Servicio                      | Responsabilidad                                                               |
| ----------------------------- | ----------------------------------------------------------------------------- |
| ServicioDeteccionSwing        | Analiza el vídeo para identificar un swing de salida válido.                  |
| ServicioProcesamientoVideo    | Recorta, valida, convierte y prepara el vídeo generado.                       |
| ServicioGeneracionClip        | Coordina la creación del clip final a partir del evento detectado.            |
| ServicioCalculoEstadisticas   | Calcula estadísticas personales y globales a partir de los datos almacenados. |
| ServicioEliminacionAutomatica | Elimina clips expirados que no están marcados como favoritos.                 |

Estos servicios permiten separar la lógica técnica de procesamiento y mantenimiento de la lógica propia de los casos de uso.

---

## Diseño de paquetes


[Diagrama paquetes](../documentacion/imagenes/paquetes.svg) |

El sistema se organiza en paquetes funcionales para mantener una estructura modular.

| Paquete        | Responsabilidad                                                            |
| -------------- | -------------------------------------------------------------------------- |
| Presentación   | Agrupa las vistas de la aplicación móvil.                                  |
| Autenticación  | Gestiona login, usuario autenticado y permisos.                            |
| Captura        | Agrupa cámara, campo, hoyo y sesión de captura.                            |
| Procesamiento  | Gestiona detección del swing, validación del evento y generación del clip. |
| Clips          | Gestiona consulta, reproducción, favoritos, expiración y eliminación.      |
| Administración | Agrupa funciones específicas del administrador.                            |
| Estadísticas   | Calcula estadísticas personales y globales.                                |
| Persistencia   | Gestiona acceso a base de datos y almacenamiento multimedia.               |

La organización por paquetes permite reducir el acoplamiento entre componentes y facilita la evolución del sistema. Por ejemplo, sería posible mejorar el módulo de procesamiento de vídeo sin modificar directamente la interfaz móvil o la gestión de usuarios.

---

## Modelo lógico de la base de datos


[Diagrama entidad relacion](../documentacion/imagenes/entidadRelacion.svg) |

El modelo lógico de la base de datos define las entidades persistentes necesarias para soportar el funcionamiento del sistema.

Las entidades principales son:

| Entidad         | Descripción                                                                |
| --------------- | -------------------------------------------------------------------------- |
| Usuario         | Cuenta registrada en el sistema.                                           |
| Campo           | Campo de golf asociado a la instalación.                                   |
| Hoyo            | Hoyo perteneciente a un campo.                                             |
| Camara          | Cámara instalada y asociada a un hoyo.                                     |
| SesionCaptura   | Activación del sistema por parte de un usuario en un contexto determinado. |
| EventoDetectado | Evento válido detectado durante una sesión.                                |
| Clip            | Vídeo generado a partir de un evento detectado.                            |

El código QR no se almacena como entidad independiente, ya que funciona como mecanismo de entrada para identificar una cámara registrada. Las estadísticas tampoco se almacenan como tabla propia, sino que se calculan dinámicamente a partir de usuarios, clips, favoritos, hoyos y fechas.

---

## Modelo físico propuesto

A nivel físico, el prototipo simplifica parte del modelo lógico para adaptarlo a la implementación real.

Las tablas principales son:

| Tabla         | Información almacenada                                                                                 |
| ------------- | ------------------------------------------------------------------------------------------------------ |
| Usuario       | Datos de autenticación, correo, nombre de usuario, tipo de usuario y estado.                           |
| Camara        | Identificador de cámara, nombre, campo, hoyo y estado de activación.                                   |
| SesionCaptura | Usuario, cámara, hoyo, campo, fecha de inicio, fecha de fin y estado.                                  |
| Clip          | Usuario propietario, archivo de vídeo, campo, hoyo, fecha de creación, favorito y fecha de expiración. |

Esta simplificación permite reducir la complejidad del prototipo sin perder la trazabilidad entre usuario, cámara, sesión, hoyo y clip generado.

---

## Modelo de despliegue


[Diagrama despliegue](../documentacion/imagenes/diagramaDespliegue.svg) |

El modelo de despliegue representa la distribución física y lógica de los componentes del sistema.

| Nodo                       | Responsabilidad                                                           |
| -------------------------- | ------------------------------------------------------------------------- |
| Cámaras IP                 | Registran el entorno de juego desde diferentes puntos de vista.           |
| Red local / Switch PoE     | Permite conectar las cámaras y transmitir vídeo al nodo de procesamiento. |
| Nodo de IA o procesamiento | Analiza el vídeo, detecta el swing y genera los clips.                    |
| Servidor backend           | Gestiona usuarios, sesiones, cámaras, clips, permisos y estadísticas.     |
| Base de datos              | Almacena la información estructurada del sistema.                         |
| Almacenamiento multimedia  | Conserva los ficheros de vídeo generados.                                 |
| Aplicación móvil           | Permite al usuario interactuar con el sistema.                            |

Este modelo muestra que la solución está diseñada de forma distribuida, aunque el prototipo pueda ejecutarse de forma simplificada durante las pruebas.

---



Los diagramas de arquitectura, secuencia, clases, paquetes, entidad-relación y despliegue muestran cómo se estructura el sistema antes de pasar a la descripción de la solución implementada. De este modo, este capítulo actúa como puente entre la especificación de requisitos y el prototipo funcional que se presenta en el capítulo siguiente.
