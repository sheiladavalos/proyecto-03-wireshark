# Proyecto 03: Análisis de tráfico de red con Wireshark

## 1. Introducción

El presente proyecto tiene como objetivo realizar un análisis básico de tráfico de red utilizando Wireshark, una herramienta ampliamente utilizada en ciberseguridad para capturar, inspeccionar y analizar paquetes que circulan por una red.

La actividad se desarrolló en un entorno controlado mediante una máquina virtual Kali Linux ejecutada en VirtualBox. Se generó tráfico HTTP para observar la comunicación entre un equipo cliente y un servidor web, identificando información relevante como direcciones IP, métodos HTTP y códigos de respuesta.

Este ejercicio permite aplicar conocimientos fundamentales de monitoreo de redes y análisis defensivo, competencias importantes para desempeñarse en áreas SOC (Security Operations Center) y Blue Team.

## 2. Objetivos

- Capturar tráfico de red mediante Wireshark.
- Identificar solicitudes y respuestas HTTP.
- Reconocer direcciones IP de origen y destino.
- Interpretar métodos HTTP y códigos de estado.
- Conservar evidencia digital mediante archivos PCAPNG.
- Verificar la integridad de la captura utilizando SHA-256.

## 3. Entorno de laboratorio

- Sistema operativo: Kali Linux.
- Plataforma de virtualización: VirtualBox.
- Interfaz de red: eth0.
- Dirección IP local: 10.0.2.15.
- Herramienta de análisis: Wireshark 4.6.6.
- Archivo de captura: evidencias/captura_http.pcapng.

## 4. Procedimiento realizado

Se inició Wireshark y se seleccionó la interfaz de red eth0 para comenzar la captura de paquetes.

Posteriormente, se generó tráfico HTTP mediante una solicitud realizada desde la terminal de Kali Linux utilizando el comando:

`curl -I --max-time 10 http://example.com`

Se obtuvo una respuesta HTTP satisfactoria con código 200 OK.

Una vez finalizada la captura, se aplicaron filtros de visualización para localizar los paquetes relacionados con HTTP:

- `http`
- `http.response`

Estos filtros permitieron identificar la solicitud y la respuesta del servidor.

## 5. Resultados obtenidos

Durante el análisis se identificaron los siguientes datos:

| Parámetro | Resultado |
|---|---|
| Método HTTP | HEAD |
| Host | example.com |
| Código de respuesta | 200 OK |
| IP de origen | 10.0.2.15 |
| IP de destino | 104.20.23.154 |

### 5.1. Análisis del método HTTP

El método HTTP HEAD permite solicitar los encabezados de respuesta de un recurso web sin descargar su contenido.
 En esta práctica se utilizó para consultar el dominio example.com y verificar la respuesta del servidor. La captura permitió identificar una solicitud HEAD y su correspondiente respuesta HTTP 200 OK.

### 5.2. Análisis de direcciones IP

La dirección 10.0.2.15 corresponde a la máquina virtual Kali Linux, mientras que 104.20.23.154 corresponde a la dirección de destino observada en el paquete seleccionado.

La identificación de las direcciones de origen y destino permite comprender el sentido de la comunicación y reconstruir el intercambio de información entre los equipos.

En los paquetes de respuesta, las direcciones de origen y destino normalmente aparecen invertidas.

### 5.3. Análisis del código HTTP 200 OK
El código 200 OK indica que el servidor procesó satisfactoriamente la solicitud HTTP observada.

Este resultado confirma que se recibió una respuesta exitosa para la petición realizada.

Sin embargo, no demuestra por sí solo que toda la comunicación sea segura ni que el sistema esté libre de amenazas.

### 5.4. Correlación de paquetes mediante Wireshark y TShark

Mediante el análisis de la captura con Wireshark y la herramienta TShark, se identificó una solicitud HTTP HEAD (paquete 122), enviada desde la dirección IP 10.0.2.15 hacia 104.20.23.154. Posteriormente, se observó una respuesta HTTP 200 OK (paquete 124), correspondiente al mismo flujo TCP. Esto confirma que el servidor recibió y respondió correctamente a la solicitud.

Para verificar la correlación entre ambos paquetes, se utilizó el siguiente comando:

 tshark -r ../evidencias/captura_http.pcapng -Y 'frame.number == 122 || frame.number == 124'

## 6. Interpretación desde SOC / Blue Team

Desde una perspectiva de monitoreo defensivo, el tráfico observado es compatible con una comunicación web normal.

La identificación del método HTTP, el dominio solicitado y las direcciones IP permite establecer una referencia del comportamiento de la comunicación.

Un analista SOC podría ampliar la investigación mediante:

- Revisión de conexiones hacia destinos desconocidos.
- Identificación de solicitudes HTTP inusuales.
- Detección de conexiones repetitivas o sospechosas.
- Análisis de consultas DNS.
- Correlación con registros del sistema operativo y eventos de seguridad.

Con los datos disponibles no se identificaron indicadores suficientes para clasificar esta comunicación como maliciosa.

## 7. Integridad de la evidencia digital

La captura se almacenó en formato PCAPNG dentro de la carpeta evidencias.

Se calculó su huella digital mediante SHA-256 utilizando el comando:

`sha256sum evidencias/captura_http.pcapng`

Hash obtenido:

`0347ed0f5276e2a36de36f4cb062be9827057e714f68d7935905858146531773`

Posteriormente, se registró el valor en el archivo `analisis/hashes.sha256` y se verificó mediante:

`sha256sum -c analisis/hashes.sha256`

Resultado obtenido:

`evidencias/captura_http.pcapng: OK`

La verificación confirma que el archivo coincide con el hash registrado.


## 8. Consideraciones de seguridad

El protocolo HTTP no cifra por sí mismo el contenido de las comunicaciones. Por este motivo, determinados datos podrían quedar expuestos a terceros capaces de observar el tráfico.

Como medida preventiva, se recomienda utilizar HTTPS para proteger la confidencialidad e integridad de las comunicaciones y revisar las conexiones de red para detectar comportamientos anómalos.

## 9. Conclusión

La práctica permitió adquirir experiencia inicial en captura y análisis de tráfico de red utilizando Wireshark.

Se identificaron solicitudes HTTP, direcciones IP de origen y destino y una respuesta exitosa del servidor mediante el código 200 OK.

Además, se aplicaron procedimientos básicos de preservación de evidencia digital mediante la generación y verificación de hashes SHA-256.

Este proyecto representa una práctica introductoria de análisis de redes, aplicable a tareas de monitoreo, detección y respuesta ante incidentes en entornos SOC y Blue Team.

