# Capítulo 5 - Validación y conclusiones

## Introducción

Este capítulo recoge la validación del prototipo desarrollado y las conclusiones finales del Trabajo de Fin de Grado.

El objetivo principal del proyecto era diseñar e implementar un sistema automatizado capaz de detectar golpes de golf mediante visión por computador, generar clips de vídeo y permitir su consulta posterior desde una aplicación móvil.

---

## 1. Validación del prototipo

La validación se realizó sobre un conjunto de pruebas formado por 50 golpes en condiciones reales o representativas. Se comprobaron los aspectos principales del sistema: detección del swing, generación del clip, asociación con usuario y hoyo, funcionamiento multicámara y reproducción desde la aplicación móvil.

| Aspecto evaluado              | Resultado obtenido                                                                    | Cumplimiento |
| ----------------------------- | ------------------------------------------------------------------------------------- | ------------ |
| Detección del swing           | 45 de 50 golpes detectados correctamente, un 90 %.                                    | Cumple       |
| Generación de clips           | Los 45 golpes detectados generaron un clip válido.                                    | Cumple       |
| Clip multicámara              | Los clips aceptados incluyeron las vistas configuradas.                               | Cumple       |
| Asociación usuario-campo-hoyo | Los clips quedaron asociados al contexto correcto.                                    | Cumple       |
| Consulta en la app            | Los clips generados pudieron reproducirse desde la aplicación móvil.                  | Cumple       |
| Flujo completo                | El sistema completó login, QR, captura, procesamiento, almacenamiento y reproducción. | Cumple       |

Estos resultados permiten afirmar que el prototipo cumple los criterios funcionales definidos dentro del alcance del trabajo.

---

## 2. Cumplimiento de objetivos

El objetivo general se considera cumplido, ya que el sistema desarrollado permite capturar vídeo mediante cámaras, detectar el swing, generar clips y ponerlos a disposición del usuario desde la aplicación móvil.

| Objetivo                                     | Resultado                                             |
| -------------------------------------------- | ----------------------------------------------------- |
| Detectar automáticamente el swing            | Cumplido mediante el módulo de visión por computador. |
| Generar clips sin intervención manual        | Cumplido tras la detección del golpe válido.          |
| Incorporar un sistema multicámara            | Cumplido con cámaras configuradas para el hoyo.       |
| Registrar visualmente el resultado del golpe | Cumplido dentro del alcance del prototipo.            |
| Validar el flujo completo                    | Cumplido mediante pruebas funcionales.                |

El trabajo no se limita a un algoritmo aislado, sino que integra aplicación móvil, backend, base de datos, cámaras, procesamiento de vídeo y almacenamiento multimedia.

---

## 3. Conclusiones

El proyecto demuestra que es viable automatizar la captura de golpes de golf utilizando visión por computador y una arquitectura modular.

La solución reduce la intervención manual del jugador, ya que este solo debe iniciar sesión y escanear el QR del hoyo. A partir de ese momento, el sistema se encarga de asociar el contexto de captura, analizar el vídeo, detectar el swing, generar el clip y permitir su consulta posterior.

Uno de los resultados más importantes es la integración de tecnologías diferentes dentro de un mismo flujo funcional: Expo/React Native para la aplicación móvil, Django REST Framework para el backend, PostgreSQL para la persistencia, cámaras IP para la captura y herramientas como YOLO, MediaPipe, OpenCV y FFmpeg para el procesamiento de vídeo.

---

## 4. Limitaciones

Aunque el prototipo cumple los objetivos planteados, existen algunas limitaciones:

| Limitación                        | Descripción                                                                                                        |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Prototipo no comercial            | La solución valida la viabilidad técnica, pero no representa una instalación definitiva.                           |
| Entorno de pruebas controlado     | Las pruebas se realizaron en condiciones reales o representativas, pero no en una instalación permanente.          |
| Dependencia de las cámaras        | El resultado depende de la posición, estabilidad y calidad de las cámaras.                                         |
| Condiciones visuales              | La iluminación, distancia y fondo pueden afectar a la detección.                                                   |
| Sin seguimiento completo de bola  | El sistema registra visualmente la bola cuando entra en el campo de visión, pero no calcula su trayectoria exacta. |
| Sin análisis biomecánico avanzado | El sistema detecta el golpe y genera clips, pero no evalúa técnicamente la calidad del swing.                      |

Estas limitaciones delimitan el alcance del prototipo y sirven como punto de partida para futuras mejoras.

---

## 5. Líneas futuras

Como futuras líneas de actuación se plantean:

| Línea futura            | Descripción                                                                     |
| ----------------------- | ------------------------------------------------------------------------------- |
| Despliegue físico real  | Instalar el sistema de forma estable en un campo de golf.                       |
| Optimización de IA      | Mejorar la detección en condiciones variables de luz, distancia y fondo.        |
| Procesamiento asíncrono | Separar tareas pesadas del backend para mejorar rendimiento.                    |
| Almacenamiento externo  | Utilizar servicios como S3 o similares para escalar el almacenamiento de clips. |
| Métricas avanzadas      | Añadir datos como velocidad, ángulo o duración del swing.                       |
| Mejora de la app móvil  | Incorporar filtros, búsqueda avanzada, comparación de clips o notificaciones.   |

---

## 6. Cierre final

El Trabajo de Fin de Grado ha permitido desarrollar un prototipo funcional que demuestra la viabilidad de un sistema automatizado de captura de golpes de golf mediante visión por computador.

La solución propuesta responde a una problemática clara: la grabación manual de golpes resulta incómoda, incompleta y poco sistemática. Frente a ello, el sistema desarrollado permite activar la captura mediante QR, procesar el vídeo de cámaras físicas, generar clips automáticamente y consultarlos desde una aplicación móvil.

En conclusión, el proyecto demuestra que la visión por computador puede aplicarse de forma práctica al ámbito deportivo para mejorar la captura de eventos, reducir la intervención manual y ofrecer al usuario una experiencia más automatizada y completa.

