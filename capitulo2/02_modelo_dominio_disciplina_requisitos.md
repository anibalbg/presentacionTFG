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

* **Sesión de captura**: representa la activación del sistema por parte de un jugador en un hoyo determinado. Esta sesión permite asociar correctamente los clips generados al usuario y al contexto de juego.

* **Evento detectado**: representa el golpe identificado por el sistema. No todos los movimientos del jugador generan un clip; el sistema debe diferenciar entre movimientos previos, swings de práctica y golpes válidos.

* **Clip**: representa el resultado final del proceso. Es el vídeo generado automáticamente tras detectar un golpe válido y puede ser consultado posteriormente desde la aplicación móvil.

## Diagrama de clases del dominio


![Diagrama de clases del dominio](../documentacion/imagenes/clases.svg)


## Diagrama de objetos

![Diagrama de objetos](../documentacion/imagenes/objetos.svg)


## Diagrama de estados

![Diagrama de estados](../documentacion/imagenes/estados.svg)


# Disciplina de Requisitos

La disciplina de requisitos permite definir qué debe hacer el sistema, quién interactúa con él y qué funcionalidades principales deben estar disponibles para cada tipo de usuario.

En este proyecto, la especificación de requisitos se centra en el flujo completo del sistema: autenticación del usuario, activación del contexto de captura mediante código QR, detección automática del swing, generación de clips, consulta del contenido generado y funciones de administración.

## Requisitos del sistema


## Requisitos funcionales

No estan todos los requisitos del sistema

| Código | Requisito funcional                                                                                                                  |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| RF-01  | El sistema deberá permitir identificar al usuario antes de asociarle cualquier contenido generado.                                   |
| RF-02  | El sistema deberá permitir activar una sesión de captura mediante el escaneo de un código QR asociado a un hoyo.                     |
| RF-03  | El sistema deberá vincular cada sesión de captura al usuario autenticado, al campo y al hoyo identificados.                          |
| RF-04  | El sistema deberá habilitar para la sesión activa únicamente las cámaras asociadas al contexto identificado por el QR.               |
| RF-05  | El sistema deberá analizar automáticamente el vídeo captado en la zona de salida para identificar la realización de un swing válido. |
| RF-06  | El sistema deberá diferenciar entre movimientos previos, swings de práctica y swing de salida válido.                                |
| RF-07  | El sistema deberá validar el swing combinando movimiento corporal y detección visual de la bola.                                     |
| RF-08  | El sistema deberá generar automáticamente un clip de vídeo a partir del swing validado.                                              |
| RF-09  | El sistema deberá incorporar en el clip las vistas configuradas para el hoyo.                                                        |
| RF-10  | El sistema deberá procesar los vídeos en un formato compatible con la aplicación móvil.                                              |
| RF-11  | El sistema deberá almacenar cada clip conservando su relación con usuario, campo y hoyo.                                             |
| RF-12  | El sistema deberá permitir consultar los clips generados.                                                                            |
| RF-13  | El sistema deberá permitir reproducir los clips disponibles.                                                                         |
| RF-14  | El sistema deberá permitir marcar y desmarcar clips como favoritos.                                                                  |
| RF-15  | El sistema deberá aplicar una política de conservación temporal a los clips no favoritos.                                            |
| RF-16  | El sistema deberá permitir consultar estadísticas personales del jugador.                                                            |
| RF-17  | El sistema deberá permitir a los administradores consultar usuarios, clips y estadísticas globales.                                  |
| RF-18  | El sistema deberá permitir a los administradores realizar operaciones básicas de gestión sobre usuarios y clips.                     |


## Actores


![Diagrama Jerarquía de actores](../documentacion/imagenes/jerarquiaActores.svg)

En el sistema se identifican dos actores principales: el **Jugador** y el **Administrador**.

El **Jugador** es el usuario principal del sistema. Su función es iniciar sesión, activar el contexto de captura mediante el escaneo del código QR, consultar sus clips generados, reproducirlos, marcarlos como favoritos y consultar sus estadísticas personales.

El **Administrador** es el usuario encargado de supervisar el funcionamiento general del sistema. Dispone de permisos para consultar clips registrados, consultar usuarios, gestionar usuarios, gestionar clips y acceder a estadísticas globales.

Ambos actores comparten la condición de usuarios autenticados, pero se diferencian por las funcionalidades disponibles y por el nivel de permisos dentro de la aplicación.

## Casos de uso por actor


![Diagrama de casos de uso](../documentacion/imagenes/diagramaCasosDeUso.svg)

Los casos de uso identificados representan las funcionalidades principales del sistema desde el punto de vista de los actores.


## Relación entre casos de uso y requisitos funcionales

| Caso de uso                                       | Requisitos funcionales relacionados |
| ------------------------------------------------- | ----------------------------------- |
| CU-01 Login                                       | RF-01                               |
| CU-02 Escanear código QR del hoyo                 | RF-02, RF-03, RF-04                 |
| CU-03 Consultar clips generados                   | RF-11, RF-12, RF-13                 |
| CU-04 Marcar/Desmarcar clip como favorito         | RF-14, RF-15                        |
| CU-05 Consultar estadísticas personales           | RF-16                               |
| CU-06 Gestionar clips registrados                 | RF-17, RF-18                        |
| CU-07 Consultar usuarios del sistema              | RF-17                               |
| CU-08 Gestionar usuarios del sistema              | RF-18                               |
| CU-09 Consultar estadísticas globales del sistema | RF-17                               |

Además de estos casos de uso visibles para los actores, existen procesos internos del sistema relacionados con la detección del swing, la validación del evento, el procesamiento de vídeo y la generación del clip final. Estos procesos dan soporte principalmente a los requisitos RF-05, RF-06, RF-07, RF-08, RF-09 y RF-10.

## Priorización de casos de uso

| Código | Caso de uso                                 | Actor principal         | Prioridad | Justificación                                                                   |
| ------ | ------------------------------------------- | ----------------------- | --------- | ------------------------------------------------------------------------------- |
| CU-01  | Login                                       | Jugador / Administrador | Alta      | Es necesario para identificar al usuario y aplicar permisos.                    |
| CU-02  | Escanear código QR del hoyo                 | Jugador                 | Alta      | Activa el contexto de captura y permite asociar clips al usuario, campo y hoyo. |
| CU-03  | Consultar clips generados                   | Jugador / Administrador | Alta      | Permite acceder al resultado principal del sistema: los clips generados.        |
| CU-04  | Marcar/Desmarcar clip como favorito         | Jugador / Administrador | Media     | Permite conservar clips relevantes y aplicar la política de expiración.         |
| CU-05  | Consultar estadísticas personales           | Jugador                 | Media     | Aporta información adicional sobre la actividad del usuario.                    |
| CU-06  | Gestionar clips registrados                 | Administrador           | Media     | Facilita la supervisión y mantenimiento del contenido almacenado.               |
| CU-07  | Consultar usuarios del sistema              | Administrador           | Media     | Permite supervisar las cuentas registradas.                                     |
| CU-08  | Gestionar usuarios del sistema              | Administrador           | Media     | Permite mantener y administrar las cuentas de usuario.                          |
| CU-09  | Consultar estadísticas globales del sistema | Administrador           | Media     | Ofrece una visión general de la actividad del sistema.                          |

Los casos de uso de prioridad alta corresponden al flujo esencial del sistema: acceso, activación del contexto de captura y consulta de clips. Los casos de prioridad media complementan este flujo mediante funciones de conservación, estadísticas y administración.

## Detalle de casos de uso

Voy a detallar los casos de uso mas importantes


### CU-02 - Escanear código QR del hoyo


![Caso de uso Escanear QR](../recursos/diagramas/EscanearQR.png) 

**Actor principal:** Jugador.

**Descripción:**
Permite que el jugador escanee el código QR asociado a una cámara del hoyo. A partir de ese identificador, el backend recupera el campo, el hoyo y las cámaras correspondientes, activando el contexto de captura.

**Precondición:**
El jugador debe haber iniciado sesión correctamente.

**Resultado esperado:**
El sistema crea una sesión activa y asocia el usuario al campo, hoyo y cámaras configuradas.

**Criterios de aceptación:**

* El sistema reconoce el identificador leído desde el QR.
* El sistema valida que la cámara existe.
* El sistema obtiene campo, hoyo y cámaras asociadas.
* El sistema activa la sesión de captura.
* Los clips generados posteriormente quedan asociados al usuario y al contexto correcto.

---

### CU-03 - Consultar clips generados


![Caso de uso Consultar clips](../recursos/diagramas/ConsultarClips.png)

**Actor principal:** Jugador / Administrador.

**Descripción:**
Permite consultar los clips generados por el sistema. El jugador accede únicamente a sus propios clips, mientras que el administrador puede consultar clips registrados en el sistema según sus permisos.

**Precondición:**
El usuario debe haber iniciado sesión.

**Resultado esperado:**
El sistema muestra una lista de clips disponibles con información básica como campo, hoyo, fecha y estado de favorito.

**Criterios de aceptación:**

* El jugador solo puede ver sus propios clips.
* El administrador puede consultar clips del sistema.
* Cada clip muestra información básica de contexto.
* Los clips disponibles pueden reproducirse desde la aplicación móvil.

