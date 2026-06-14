# Capítulo 4 - Descripción de la solución

## Introducción

Este capítulo presenta la solución desarrollada desde una perspectiva práctica, mostrando cómo se materializa el análisis y diseño anterior en un prototipo funcional.

El sistema implementado permite que un jugador active el contexto de captura de un hoyo mediante el escaneo de un código QR. A partir de esa activación, las cámaras asociadas al hoyo registran el entorno de juego, el módulo de visión por computador analiza el vídeo, detecta el swing válido y el backend almacena el clip generado para que pueda ser consultado desde la aplicación móvil.


---

## Tecnologías utilizadas

La solución se ha implementado utilizando tecnologías diferenciadas para cada parte del sistema.

| Parte del sistema      | Tecnología utilizada                                 |
| ---------------------- | ---------------------------------------------------- |
| Aplicación móvil       | Expo / React Native                                  |
| Backend                | Django REST Framework                                |
| Base de datos          | PostgreSQL                                           |
| Procesamiento de vídeo | Python, OpenCV y FFmpeg                              |
| Detección de objetos   | YOLO                                                 |
| Estimación de pose     | MediaPipe                                            |
| Captura de vídeo       | Cámaras IP mediante RTSP                             |
| Autenticación          | JWT                                                  |
| Reproducción de vídeo  | Streaming HTTP con soporte para peticiones parciales |

La combinación de estas tecnologías permite cubrir el flujo completo: activación del usuario, captura del entorno, detección del golpe, generación del clip, almacenamiento y consulta desde la aplicación móvil.

---

## Estructura general del proyecto

El proyecto se organiza en tres bloques principales:

```text
Golf_proyect/
│
├── golf_mobile/      # Aplicación móvil
├── golf_backend/     # Backend, API, base de datos y gestión de clips
└── golf/             # Módulo de IA y procesamiento de vídeo
```

Cada bloque tiene una responsabilidad concreta. La aplicación móvil se centra en la interacción con el usuario, el backend actúa como núcleo de coordinación y el módulo de IA se encarga de analizar el vídeo procedente de las cámaras.

Esta separación facilita el mantenimiento del sistema y permite que cada parte pueda evolucionar de forma independiente.

---

## Funcionamiento general del sistema

El flujo principal de funcionamiento es el siguiente:

| Paso | Descripción                                                           |
| ---- | --------------------------------------------------------------------- |
| 1    | El usuario inicia sesión en la aplicación móvil.                      |
| 2    | El jugador escanea el código QR asociado al hoyo.                     |
| 3    | El backend identifica la cámara, el campo y el hoyo correspondientes. |
| 4    | Se activa la sesión de captura para el usuario.                       |
| 5    | Las cámaras registran el entorno de juego.                            |
| 6    | El módulo de IA analiza el vídeo y detecta el swing válido.           |
| 7    | El sistema genera el clip correspondiente al golpe.                   |
| 8    | El backend almacena el clip y sus metadatos.                          |
| 9    | El usuario consulta y reproduce el clip desde la aplicación móvil.    |
| 10   | El clip puede marcarse como favorito o quedar sujeto a expiración.    |

Este flujo permite automatizar la captura del golpe sin que el jugador tenga que colocar el móvil, iniciar manualmente una grabación o interrumpir la práctica deportiva.


---


## Alcance del prototipo

El prototipo desarrollado demuestra la viabilidad técnica del sistema, pero no debe interpretarse como una instalación comercial definitiva.

Dentro del alcance del prototipo se incluye:

| Incluido                    | Descripción                                                       |
| --------------------------- | ----------------------------------------------------------------- |
| Captura con cámaras físicas | El sistema puede recibir vídeo de cámaras IP mediante RTSP.       |
| Activación mediante QR      | El usuario activa el contexto de captura desde la app móvil.      |
| Detección automática        | El sistema analiza el vídeo y detecta el swing válido.            |
| Generación de clips         | Se generan vídeos asociados al usuario y al hoyo.                 |
| Consulta desde la app       | El jugador puede consultar y reproducir sus clips.                |
| Administración              | El administrador puede supervisar usuarios, clips y estadísticas. |

Quedan fuera del alcance del prototipo:

| Fuera del alcance                                        | Motivo                                                                                  |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Instalación comercial completa                           | El trabajo se centra en validar el prototipo.                                           |
| Seguimiento exacto de la trayectoria completa de la bola | Se registra visualmente el resultado cuando las cámaras lo permiten.                    |
| Infraestructura cloud definitiva                         | La arquitectura permite evolucionar, pero el prototipo se valida en entorno controlado. |
| Análisis biomecánico profesional del swing               | El sistema detecta el evento, no realiza análisis técnico avanzado del movimiento.      |

---
## Demostración del prototipo
[Contenido](documentacion/contenido/)
