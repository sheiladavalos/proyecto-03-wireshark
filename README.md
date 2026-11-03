# Proyecto 03: Análisis de tráfico de red con Wireshark

## 1. Introducción

Este proyecto tiene como objetivo realizar un análisis básico de tráfico de red utilizando Wireshark, una herramienta ampliamente utilizada en ciberseguridad para capturar, visualizar y examinar paquetes que circulan por una red.

La práctica se desarrolló en un entorno de laboratorio controlado mediante Kali Linux ejecutado en una máquina virtual VirtualBox.

## 2. Objetivos

- Capturar tráfico de red utilizando Wireshark.
- Identificar solicitudes y respuestas HTTP.
- Reconocer el método HTTP HEAD.
- Analizar direcciones IP de origen y destino.
- Interpretar el código de respuesta HTTP 200 OK.
- Preservar la integridad de la evidencia mediante SHA-256.

## 3. Entorno y herramientas utilizadas

- Sistema operativo: Kali Linux.
- Virtualización: Oracle VirtualBox.
- Herramienta de análisis: Wireshark 4.6.6.
- Herramienta complementaria: TShark.
- Protocolo analizado: HTTP.
- Sitio de prueba: example.com.

## 4. Procedimiento realizado

Se inició una captura de tráfico en la interfaz de red de Kali Linux mediante Wireshark.

Posteriormente, se generó una solicitud HTTP utilizando el siguiente comando:

`curl -I --max-time 10 http://example.com`

El parámetro `-I` permite realizar una solicitud HTTP HEAD, que obtiene los encabezados de respuesta sin descargar el contenido del recurso.

Se guardó la captura en formato PCAPNG para su posterior análisis.

## 5. Resultados obtenidos

Durante el análisis se identificaron los siguientes datos:

| Parámetro | Resultado |
|---|---|
| Método HTTP | HEAD |
| Host | example.com |
| Código de respuesta | 200 OK |
| IP de origen | 10.0.2.15 |
| IP de destino | 104.20.23.154 |

Se verificó mediante TShark que la solicitud HEAD quedó registrada en el paquete 122 y que la respuesta HTTP 200 OK se registró en el paquete 124. Ambos pertenecen al mismo flujo TCP.

## 6. Análisis de seguridad

El análisis de tráfico permite identificar los protocolos utilizados, los equipos que se comunican y las respuestas de los servidores.

Desde una perspectiva SOC / Blue Team, esta actividad resulta útil para comprender el comportamiento de las comunicaciones de red y reconocer posibles anomalías.

También permite observar que HTTP transmite información sin cifrado. Por ese motivo, para comunicaciones reales que involucren información sensible se recomienda utilizar HTTPS.

## 7. Integridad de la evidencia

Se utilizó SHA-256 para calcular y verificar la integridad del archivo de captura PCAPNG.

La comprobación se realizó mediante:

`sha256sum -c analisis/hashes.sha256`

Resultado obtenido:

`evidencias/captura_http.pcapng: OK`

## 8. Evidencias

La carpeta `evidencias/` contiene:

- `captura_http.pcapng`: captura original del tráfico.
- `wireshark_solicitud_http.png`: evidencia visual de la solicitud HEAD.
- `wireshark_respuesta_200.png`: evidencia visual de la respuesta HTTP 200 OK.

El informe técnico completo se encuentra en `analisis/informe_wireshark.md`.

## 9. Conclusión

La práctica permitió adquirir experiencia inicial en captura y análisis de tráfico de red mediante Wireshark y TShark.

Se identificó una solicitud HTTP HEAD y su correspondiente respuesta satisfactoria 200 OK, verificando las direcciones IP involucradas y preservando la integridad de la captura mediante SHA-256.

Este ejercicio constituye una primera aproximación a tareas de monitoreo y análisis de comunicaciones que pueden formar parte del trabajo de un analista SOC.
