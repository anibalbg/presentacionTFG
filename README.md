# Presentación de defensa - Golf Swing Tracker

**Trabajo Final de Grado:** Diseño e implementación de un sistema automatizado de captura de golpes en golf mediante visión por computador y arquitectura distribuida.  
**Autor:** Aníbal Bayas Galindo  
**Formato:** presentación en README para recorrer el repositorio durante la defensa.  
**Criterio seguido:** esta presentación sigue el **modelo lógico y de diseño descrito en el TFG**. Las diferencias concretas del prototipo implementado se reservan para la explicación oral o para la sección final de implementación.

---

## Índice de exposición

1. [Problema y objetivo del sistema](#1-problema-y-objetivo-del-sistema)
2. [Modelo del dominio](#2-modelo-del-dominio)
3. [Requisitos del sistema](#3-requisitos-del-sistema)
4. [Actores y casos de uso](#4-actores-y-casos-de-uso)
5. [Contexto global y evolución del sistema](#5-contexto-global-y-evolución-del-sistema)
6. [Flujos de actividad de los casos de uso principales](#6-flujos-de-actividad-de-los-casos-de-uso-principales)
7. [Diagramas de secuencia](#7-diagramas-de-secuencia)
8. [Modelo MVC de análisis](#8-modelo-mvc-de-análisis)
9. [Prototipos de vistas](#9-prototipos-de-vistas)
10. [Arquitectura del sistema](#10-arquitectura-del-sistema)
11. [Modelo de datos](#11-modelo-de-datos)
12. [Modelo de despliegue](#12-modelo-de-despliegue)
13. [Organización del código](#13-organización-del-código)
14. [Relación entre diseño e implementación](#14-relación-entre-diseño-e-implementación)
15. [Validación del prototipo](#15-validación-del-prototipo)
16. [Cierre](#16-cierre)

---

## 1. Problema y objetivo del sistema

El proyecto parte de una limitación habitual en la práctica del golf: la grabación de golpes suele depender de una acción manual del jugador, de otra persona o de un dispositivo colocado previamente.

| Problema identificado | Consecuencia | Necesidad del sistema |
|---|---|---|
| Grabación manual del golpe | Interrumpe la práctica deportiva | Automatizar la captura |
| Captura desde un único ángulo | Información visual incompleta | Usar varias cámaras |
| Falta de contexto del vídeo | El clip no queda asociado al usuario, campo u hoyo | Gestionar sesión, usuario y hoyo |
| Dificultad para revisar golpes | El usuario no dispone de un histórico claro | Consultar clips y estadísticas |

**Objetivo general:** diseñar e implementar un sistema capaz de detectar automáticamente un golpe de golf, generar un clip de vídeo desde distintas perspectivas y asociarlo al usuario y al hoyo correspondiente.

---

## 2. Modelo del dominio

El modelo del dominio es el punto de partida del proceso de ingeniería. Representa los conceptos del problema antes de hablar de pantallas, base de datos o código.

### 2.1 Conceptos principales

| Concepto | Papel dentro del dominio |
|---|---|
| Usuario | Persona registrada en el sistema. Puede actuar como jugador o administrador. |
| Jugador | Activa la captura mediante QR y consulta sus clips. |
| Administrador | Supervisa usuarios, clips y estadísticas globales. |
| Campo | Instalación deportiva donde se ubican los hoyos. |
| Hoyo | Unidad de juego donde se produce el golpe. |
| Cámara | Dispositivo asociado a un hoyo y a un rol de captura. |
| Código QR | Identificador visual usado para activar el contexto de captura. |
| Sesión de captura | Asociación temporal entre usuario, campo, hoyo y cámaras. |
| Evento detectado | Acción deportiva identificada como swing candidato o válido. |
| Clip | Vídeo generado a partir de un evento válido. |
| Estadística | Información derivada de clips, usuarios y actividad registrada. |

### 2.2 Artefactos del modelo del dominio

| Artefacto | Archivo |
|---|---|
| Modelo del dominio simplificado | [diagramaModeloDominio.puml](<documentacion/modelo del dominio/diagramaModeloDominio.puml>) |
| Diagrama de clases del dominio | [diagramaClases.puml](<documentacion/modelo del dominio/diagramaClases.puml>) |
| Diagrama de objetos | [diagramaObjetos.puml](<documentacion/modelo del dominio/diagramaObjetos.puml>) |
| Diagrama de estados | [diagramaEstados.puml](<documentacion/modelo del dominio/diagramaEstados.puml>) |

### 2.3 Idea central del dominio

El sistema no guarda vídeos de forma aislada. Cada clip tiene trazabilidad desde el usuario hasta el contexto deportivo en el que se genera.

```text
Usuario -> Sesión de captura -> Evento detectado -> Clip
                 |
              Campo / Hoyo / Cámaras
```

---

## 3. Requisitos del sistema

Los requisitos transforman el problema en funcionalidades y restricciones verificables.

### 3.1 Requisitos funcionales agrupados

| Grupo | Requisitos | Finalidad |
|---|---|---|
| Identificación y activación | RF-01 a RF-04 | Autenticar al usuario y activar el contexto del hoyo mediante QR. |
| Detección del evento deportivo | RF-05 a RF-07 | Detectar el swing de salida y diferenciarlo de movimientos previos. |
| Generación del clip | RF-08 a RF-11 | Crear, procesar y almacenar el vídeo resultante. |
| Asociación y trazabilidad | RF-12 a RF-14 | Relacionar usuario, sesión, evento, campo, hoyo y clip. |
| Consulta y reproducción | RF-15 a RF-17 | Permitir que el usuario acceda a sus clips. |
| Conservación y favoritos | RF-18 a RF-22 | Marcar favoritos y aplicar caducidad a clips no conservados. |
| Estadísticas y administración | RF-23 a RF-26 | Consultar estadísticas y gestionar usuarios o clips desde administración. |

### 3.2 Requisitos suplementarios

| Categoría | Aplicación en el diseño |
|---|---|
| Rendimiento | El procesamiento de vídeo se separa del uso normal de la app. |
| Fiabilidad | La detección combina información corporal y visual de la bola. |
| Seguridad | El acceso se controla por autenticación y permisos. |
| Disponibilidad | El sistema se divide en componentes independientes. |
| Mantenibilidad | Se separan captura, IA, backend, persistencia y app móvil. |
| Portabilidad | La arquitectura permite ejecutar componentes en nodos distintos. |
| Usabilidad | El flujo del jugador se reduce a iniciar sesión, escanear QR y consultar clips. |

---

## 4. Actores y casos de uso

Los casos de uso muestran los servicios que el sistema ofrece a los actores externos. El actor siempre es el punto inicial de la interacción.

### 4.1 Actores

| Actor | Responsabilidad |
|---|---|
| Usuario | Generalización de jugador y administrador. |
| Jugador | Activa la captura y consulta su contenido. |
| Administrador | Supervisa el sistema y gestiona recursos. |

### 4.2 Artefactos de casos de uso

| Artefacto | Archivo |
|---|---|
| Jerarquía de actores | [diagramaJerarquiaActores.puml](<documentacion/casos de uso/diagramaJerarquiaActores.puml>) |
| Diagrama de casos de uso | [diagramaCasosDeUso.puml](<documentacion/casos de uso/diagramaCasosDeUso.puml>) |

### 4.3 Casos de uso identificados

| Código | Caso de uso | Actor principal | Prioridad |
|---|---|---|---|
| CU-01 | Login | Jugador / Administrador | Alta |
| CU-02 | Escanear código QR del hoyo | Jugador | Alta |
| CU-03 | Consultar clips generados | Jugador / Administrador | Alta |
| CU-04 | Marcar o desmarcar clip como favorito | Jugador / Administrador | Media |
| CU-05 | Consultar estadísticas personales | Jugador | Media |
| CU-06 | Gestionar clips registrados | Administrador | Media |
| CU-07 | Consultar usuarios del sistema | Administrador | Media |
| CU-08 | Gestionar usuarios del sistema | Administrador | Media |
| CU-09 | Consultar estadísticas globales del sistema | Administrador | Media |

---

## 5. Contexto global y evolución del sistema

El contexto global muestra cómo el sistema cambia de estado a través de los casos de uso. La idea principal es que el usuario no interactúa directamente con la IA ni con las cámaras, sino que activa un contexto que el backend coordina.

### 5.1 Estados principales del sistema

| Estado inicial | Caso de uso que interviene | Estado resultante |
|---|---|---|
| Usuario no autenticado | CU-01 Login | Usuario autenticado |
| Usuario autenticado sin contexto | CU-02 Escanear QR | Sesión de captura activa |
| Sesión activa sin golpe | Detección automática del evento | Evento detectado |
| Evento detectado válido | Generación automática de clip | Clip disponible |
| Clip disponible | CU-03 Consultar clips | Clip consultado o reproducido |
| Clip disponible | CU-04 Marcar favorito | Clip conservado |
| Clips y usuarios registrados | CU-05 / CU-09 Estadísticas | Estadísticas calculadas |
| Clips o usuarios registrados | CU-06 / CU-08 Gestión | Datos actualizados |

### 5.2 Artefactos relacionados

| Artefacto | Archivo |
|---|---|
| Diagrama de casos de uso | [diagramaCasosDeUso.puml](<documentacion/casos de uso/diagramaCasosDeUso.puml>) |
| Diagrama de estados | [diagramaEstados.puml](<documentacion/modelo del dominio/diagramaEstados.puml>) |
| Diagrama de arquitectura del sistema | [diagramaArquitecturaSistema.puml](<documentacion/diagramas generales/diagramaArquitecturaSistema.puml>) |

---

## 6. Flujos de actividad de los casos de uso principales

En esta sección se resume el flujo de éxito de los casos de uso más importantes. Estos flujos explican qué proceso debe completarse para que el caso de uso tenga éxito.

### 6.1 CU-02 - Escanear código QR del hoyo

| Paso | Actividad |
|---|---|
| 1 | El jugador inicia sesión en la aplicación móvil. |
| 2 | El jugador selecciona la opción de escanear QR. |
| 3 | La aplicación lee el identificador del QR. |
| 4 | El backend valida el identificador recibido. |
| 5 | El sistema recupera la cámara asociada al QR. |
| 6 | A partir de la cámara, el sistema obtiene campo, hoyo y cámaras del contexto. |
| 7 | Se crea o actualiza una sesión de captura activa. |
| 8 | El sistema confirma al jugador que el contexto queda activado. |

### 6.2 Generación automática del clip

| Paso | Actividad |
|---|---|
| 1 | Las cámaras asociadas al hoyo capturan vídeo del entorno. |
| 2 | El módulo de visión por computador analiza la secuencia. |
| 3 | El sistema identifica candidatos de swing. |
| 4 | Se valida el evento combinando movimiento corporal y detección visual de la bola. |
| 5 | El sistema descarta movimientos previos o swings de práctica. |
| 6 | El evento válido desencadena la generación del clip. |
| 7 | El vídeo se recorta y se procesa en formato compatible. |
| 8 | El clip se almacena asociado al usuario, campo, hoyo y evento. |
| 9 | El usuario puede consultar el clip desde la aplicación móvil. |

### 6.3 CU-03 - Consultar clips generados

| Paso | Actividad |
|---|---|
| 1 | El usuario autenticado accede a la sección de clips. |
| 2 | La aplicación solicita al backend los clips disponibles. |
| 3 | El backend comprueba permisos según el rol del usuario. |
| 4 | El sistema recupera los clips accesibles. |
| 5 | La aplicación muestra la lista con información básica. |
| 6 | El usuario puede reproducir el clip seleccionado. |

---

## 7. Diagramas de secuencia

Los diagramas de secuencia muestran el orden temporal de interacción entre actor, aplicación móvil, backend, base de datos y servicios internos.

### 7.1 Secuencias de casos de uso

| Caso de uso | Archivo |
|---|---|
| CU-01 Login | [Login.puml](<documentacion/casos de uso/Login.puml>) |
| CU-02 Escanear QR | [EscanearQR.puml](<documentacion/casos de uso/EscanearQR.puml>) |
| CU-03 Consultar clips | [ConsultarClips.puml](<documentacion/casos de uso/ConsultarClips.puml>) |
| CU-04 Marcar / desmarcar favorito | [MarcarDesmarcarFavorito.puml](<documentacion/casos de uso/MarcarDesmarcarFavorito.puml>) |
| CU-05 Consultar estadísticas personales | [ConsultarEstadisticas.puml](<documentacion/casos de uso/ConsultarEstadisticas.puml>) |
| CU-06 Gestionar clips | [GestionarClips.puml](<documentacion/casos de uso/GestionarClips.puml>) |
| CU-07 Consultar usuarios | [ConsultarUsarios.puml](<documentacion/casos de uso/ConsultarUsarios.puml>) |
| CU-08 Gestionar usuarios | [GestionarUsuarios.puml](<documentacion/casos de uso/GestionarUsuarios.puml>) |
| CU-09 Consultar estadísticas globales | [ConsultarEstadísticasGlobales.puml](<documentacion/casos de uso/ConsultarEstadísticasGlobales.puml>) |

### 7.2 Secuencias de diseño seleccionadas

| Flujo de diseño | Archivo | Importancia |
|---|---|---|
| Activación del contexto mediante QR | [diagramaSecuenciaDiseñoQR.puml](<documentacion/secuencias diseño/diagramaSecuenciaDiseñoQR.puml>) | Explica cómo el QR activa usuario, campo, hoyo y cámaras. |
| Generación automática del clip | [diagramaSencuenciaDiseñoClip.puml](<documentacion/secuencias diseño/diagramaSencuenciaDiseñoClip.puml>) | Explica cómo un vídeo capturado se transforma en un clip persistente. |

---

## 8. Modelo MVC de análisis

El análisis MVC separa responsabilidades: la vista presenta y recoge acciones, el controlador coordina el caso de uso, y el modelo representa los conceptos derivados del dominio.

### 8.1 Artefactos MVC

| Artefacto | Archivo |
|---|---|
| Clases vista | [diagramaClasesVista.puml](<documentacion/MVC/diagramaClasesVista.puml>) |
| Clases modelo | [diagramaClasesModelo.puml](<documentacion/MVC/diagramaClasesModelo.puml>) |
| Clases controladoras por caso de uso | [diagramaClasesControladora.puml](<documentacion/MVC/diagramaClasesControladora.puml>) |

### 8.2 Responsabilidades por capa

| Capa | Responsabilidad | Ejemplos del sistema |
|---|---|---|
| Vista | Presentar información y recoger acciones del usuario | VistaLogin, VistaEscaneoQR, VistaListadoClips, VistaEstadisticas |
| Controlador | Coordinar el caso de uso y aplicar validaciones | ControladorLogin, ControladorEscanearQR, ControladorConsultarClips |
| Modelo | Representar entidades y relaciones del dominio | Usuario, Campo, Hoyo, Camara, SesionCaptura, EventoDetectado, Clip |

### 8.3 Trazabilidad MVC por caso de uso

| Caso de uso | Vista principal | Controlador | Modelo implicado |
|---|---|---|---|
| CU-01 Login | VistaLogin | ControladorLogin | Usuario |
| CU-02 Escanear QR | VistaEscaneoQR | ControladorEscanearQR | Usuario, Camara, Campo, Hoyo, SesionCaptura |
| CU-03 Consultar clips | VistaListadoClips | ControladorConsultarClipsGenerados | Usuario, Clip, Hoyo, Campo |
| CU-04 Favorito | VistaDetalleClip | ControladorMarcarClipFavorito | Usuario, Clip |
| CU-05 Estadísticas personales | VistaEstadisticasPersonales | ControladorConsultarEstadisticasPersonales | Usuario, Clip |
| CU-06 Gestionar clips | VistaGestionClips | ControladorGestionarClipsRegistrados | Usuario, Clip |
| CU-07 Consultar usuarios | VistaUsuariosAdministrador | ControladorConsultarUsuariosSistema | Usuario |
| CU-08 Gestionar usuarios | VistaGestionUsuarios | ControladorGestionarUsuariosSistema | Usuario |
| CU-09 Estadísticas globales | VistaEstadisticasGlobales | ControladorConsultarEstadisticasGlobales | Usuario, Clip |

---

## 9. Prototipos de vistas

Los prototipos permiten validar la interacción antes de la implementación. En la defensa se utilizan como puente entre casos de uso, vistas MVC y pantallas reales.

| Caso de uso | Vista prototipada | Función |
|---|---|---|
| CU-01 Login | Inicio de sesión | Introducción de credenciales y acceso al sistema. |
| CU-02 Escanear QR | Escaneo QR | Activación del contexto de captura. |
| CU-03 Consultar clips | Listado de clips | Consulta de vídeos generados. |
| CU-04 Favorito | Detalle de clip | Marcado y desmarcado de favorito. |
| CU-05 Estadísticas personales | Mis estadísticas | Consulta de resumen del jugador. |
| CU-06 Gestionar clips | Administración de clips | Supervisión y eliminación de clips. |
| CU-07 / CU-08 Usuarios | Administración de usuarios | Consulta y gestión de cuentas. |
| CU-09 Estadísticas globales | Estadísticas globales | Resumen general del sistema. |

---

## 10. Arquitectura del sistema

La arquitectura se organiza por responsabilidades. El objetivo es separar la captura, el procesamiento, la gestión del sistema, la persistencia y la presentación.

### 10.1 Artefacto principal

| Artefacto | Archivo |
|---|---|
| Diagrama de arquitectura del sistema | [diagramaArquitecturaSistema.puml](<documentacion/diagramas generales/diagramaArquitecturaSistema.puml>) |

### 10.2 Capas de la solución

| Capa | Responsabilidad |
|---|---|
| Captura | Cámaras instaladas en el campo que registran el entorno de juego. |
| Procesamiento | Detección del swing, validación del evento y generación del clip. |
| Servicios / backend | Gestión de usuarios, sesiones, cámaras, clips, permisos y estadísticas. |
| Persistencia | Base de datos y almacenamiento de archivos multimedia. |
| Presentación | Aplicación móvil para jugador y administrador. |

### 10.3 Flujo arquitectónico principal

| Paso | Componente | Acción |
|---|---|---|
| 1 | App móvil | El usuario inicia sesión. |
| 2 | App móvil | El jugador escanea el QR del hoyo. |
| 3 | Backend | Se asocia usuario, campo, hoyo y cámaras. |
| 4 | Cámaras | Capturan vídeo del entorno. |
| 5 | IA / procesamiento | Se detecta el swing válido. |
| 6 | Backend | Se recibe y almacena el clip generado. |
| 7 | Base de datos | Se guardan metadatos y relaciones. |
| 8 | App móvil | El usuario consulta y reproduce el clip. |

---

## 11. Modelo de datos

El modelo de datos deriva del dominio y permite persistir la información necesaria para mantener la trazabilidad del sistema.

### 11.1 Artefacto principal

| Artefacto | Archivo |
|---|---|
| Diagrama entidad-relación | [diagramaEntidadRelacion.puml](<documentacion/diagramas generales/diagramaEntidadRelacion.puml>) |

### 11.2 Entidades principales del modelo lógico

| Entidad | Información que representa |
|---|---|
| Usuario | Cuenta registrada, credenciales y tipo de usuario. |
| Campo | Instalación deportiva. |
| Hoyo | Unidad de juego perteneciente a un campo. |
| Camara | Dispositivo asociado a un hoyo y a un rol de captura. |
| CodigoQR | Identificador visual que activa el contexto. |
| SesionCaptura | Asociación entre usuario, hoyo y periodo de captura. |
| EventoDetectado | Acción deportiva identificada por el sistema. |
| Clip | Vídeo generado y sus metadatos. |
| Estadistica | Información agregada derivada de la actividad del sistema. |

### 11.3 Relaciones clave

| Relación | Significado |
|---|---|
| Campo 1 - N Hoyo | Un campo contiene varios hoyos. |
| Hoyo 1 - N Camara | Un hoyo puede tener varias cámaras asociadas. |
| Usuario 1 - N SesionCaptura | Un usuario puede activar varias sesiones. |
| SesionCaptura 1 - N EventoDetectado | Una sesión puede producir eventos candidatos o válidos. |
| EventoDetectado 1 - 0..1 Clip | Un evento válido puede generar un clip. |
| Usuario 1 - N Clip | Cada clip pertenece a un usuario. |
| Hoyo 1 - N Clip | Cada clip queda contextualizado en un hoyo. |

---

## 12. Modelo de despliegue

El modelo de despliegue muestra dónde se ejecutan los componentes físicos y lógicos de la solución.

### 12.1 Artefactos generales

| Artefacto | Archivo |
|---|---|
| Diagrama de despliegue | [diagramaDespliegue.puml](<documentacion/diagramas generales/diagramaDespliegue.puml>) |
| Diagrama de paquetes | [diagramaPaquetes.puml](<documentacion/diagramas generales/diagramaPaquetes.puml>) |

### 12.2 Nodos principales

| Nodo | Responsabilidad |
|---|---|
| Cámaras IP | Capturan vídeo del tee, zona posterior o green. |
| Red local | Comunica cámaras, equipo de procesamiento y backend. |
| Equipo de procesamiento | Ejecuta el módulo de visión por computador. |
| Servidor backend | Expone la API y coordina la lógica de negocio. |
| Base de datos | Conserva usuarios, cámaras, sesiones, clips y relaciones. |
| Almacenamiento multimedia | Guarda los archivos de vídeo generados. |
| Dispositivo móvil | Permite al usuario interactuar con el sistema. |

---

## 13. Organización del código

La implementación se divide en tres bloques principales: IA/captura, backend y aplicación móvil.

### 13.1 Módulo de IA y captura

| Archivo | Responsabilidad |
|---|---|
| [swing_capture.py](<golf/src/swing_capture.py>) | Punto de entrada del módulo de captura y procesamiento. |
| [config.py](<golf/src/config.py>) | Construcción de configuración mediante argumentos y variables de entorno. |
| [detector.py](<golf/src/detector.py>) | Detección del swing combinando pose y bola. |
| [pipeline_live.py](<golf/src/pipeline_live.py>) | Captura en vivo, activación por cámara y subida de segmentos. |
| [pipeline_backend.py](<golf/src/pipeline_backend.py>) | Procesamiento offline de vídeos subidos. |
| [video_io.py](<golf/src/video_io.py>) | Escritura de clips y utilidades de vídeo. |
| [uploader.py](<golf/src/uploader.py>) | Subida de clips o segmentos al backend. |

### 13.2 Backend

| Archivo | Responsabilidad |
|---|---|
| [qr.py](<golf_backend/clips/views/qr.py>) | Escaneo QR, activación de sesión y notificación de swing. |
| [cameras.py](<golf_backend/clips/views/cameras.py>) | Estado de cámaras y sesión activa. |
| [upload.py](<golf_backend/clips/views/upload.py>) | Recepción, reparación, procesamiento y almacenamiento de clips. |
| [clips_list.py](<golf_backend/clips/views/clips_list.py>) | Consulta de clips por usuario y permisos. |
| [clips_stream.py](<golf_backend/clips/views/clips_stream.py>) | Streaming de vídeo con control de acceso. |
| [clips_admin.py](<golf_backend/clips/views/clips_admin.py>) | Favoritos y administración de clips. |
| [clips_stats.py](<golf_backend/clips/views/clips_stats.py>) | Estadísticas personales y globales. |
| [validators.py](<golf_backend/clips/views/validators.py>) | Validación de identificadores como camera_uuid. |
| [utils.py](<golf_backend/clips/views/utils.py>) | Utilidades de FFmpeg, parsing y concatenación. |

### 13.3 Aplicación móvil

| Archivo | Responsabilidad |
|---|---|
| [App.js](<golf_mobile/App.js>) | Entrada principal de la app. |
| [AuthContext.js](<golf_mobile/AuthContext.js>) | Gestión del estado de autenticación. |
| [navigation/DrawerNavigator.js](<golf_mobile/navigation/DrawerNavigator.js>) | Navegación principal. |
| [screens/ScanQrScreen.js](<golf_mobile/screens/ScanQrScreen.js>) | Escaneo del QR del hoyo. |
| [screens/HomeScreen.js](<golf_mobile/screens/HomeScreen.js>) | Listado principal de clips del jugador. |
| [screens/StatsScreen.js](<golf_mobile/screens/StatsScreen.js>) | Estadísticas personales. |
| [screens/AdminClipsScreen.js](<golf_mobile/screens/AdminClipsScreen.js>) | Gestión de clips por administrador. |
| [components/ClipCard.js](<golf_mobile/components/ClipCard.js>) | Tarjeta visual de cada clip. |
| [VideoPlayer.js](<golf_mobile/VideoPlayer.js>) | Reproducción de clips. |
| [utils/api.js](<golf_mobile/utils/api.js>) | Comunicación HTTP con el backend. |

---

## 14. Relación entre diseño e implementación

La implementación conserva la separación conceptual planteada en el diseño, aunque algunos elementos del modelo lógico pueden simplificarse en el prototipo.

| Diseño | Implementación |
|---|---|
| Usuario | Modelo de usuario y control de permisos en backend. |
| Campo / Hoyo | Metadatos asociados a cámaras, sesiones y clips. |
| Cámara | Registro con UUID, rol y contexto de captura. |
| Código QR | Mecanismo de entrada que contiene el identificador de cámara. |
| Sesión de captura | Activación temporal del contexto de usuario y hoyo. |
| Evento detectado | Estado lógico del golpe y activación del flujo multicámara. |
| Clip | Registro persistente con archivo de vídeo y metadatos. |
| Estadísticas | Cálculo dinámico a partir de clips y usuarios. |

---

## 15. Validación del prototipo

La validación comprueba que el sistema cumple el flujo funcional definido en los requisitos.

| Aspecto validado | Resultado esperado |
|---|---|
| Inicio de sesión | El usuario accede con permisos adecuados. |
| Escaneo QR | El sistema identifica campo, hoyo y cámaras. |
| Activación de cámaras | Las cámaras quedan vinculadas a la sesión activa. |
| Detección del swing | El sistema identifica el golpe válido. |
| Generación del clip | El vídeo resultante incluye el golpe y queda almacenado. |
| Consulta desde app | El usuario visualiza sus clips. |
| Favoritos | El usuario conserva clips relevantes. |
| Administración | El administrador consulta usuarios, clips y estadísticas. |

---

## 16. Cierre

El proyecto demuestra un proceso completo de ingeniería de software aplicado a un sistema distribuido con visión por computador:

| Fase | Resultado obtenido |
|---|---|
| Dominio | Identificación de entidades del problema deportivo. |
| Requisitos | Definición de funcionalidades y restricciones. |
| Casos de uso | Descripción de interacción entre actores y sistema. |
| MVC | Separación entre vistas, controladores y modelos. |
| Arquitectura | Diseño distribuido por capas y responsabilidades. |
| Datos | Modelo lógico para conservar trazabilidad. |
| Implementación | Prototipo funcional con IA, backend y app móvil. |
| Validación | Comprobación del flujo completo de captura y consulta. |

**Conclusión:** el sistema transforma un golpe de golf real en un clip digital trazable, asociado al usuario y al hoyo, generado automáticamente y disponible para su consulta posterior.
