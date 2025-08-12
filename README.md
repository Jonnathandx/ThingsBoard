# ThingsBoard CE + PostgreSQL con Docker Compose

Este proyecto despliega una instancia de **ThingsBoard Community Edition** junto con una base de datos **PostgreSQL** usando Docker Compose.  
Incluye configuración para **TLS/SSL**, puertos configurables y persistencia de datos.

---

## 📂 Estructura del proyecto

```bash
.
├── docker-compose.yml
├── 🛠️ .env
├── 📁 data/ # Datos persistentes de PostgreSQL
└── 📁 ssl/ # Certificados SSL para ThingsBoard
```

## ⚙️ Variables de entorno

Crea un archivo `.env` en el mismo directorio con el siguiente contenido:

```env
# PostgreSQL Credential
POSTGRES_DB=thingsboard
POSTGRES_PASSWORD=postgres
POSTGRES_PORT=5432

# ThingsBoard Credentials
HTTP_PORT=8080
RPC_PORT=7070
MQTT_PORT=1883
MQTTS_PORT=8883
COAP_PORT_INI=5683
COAP_PORT_FIN=5688
```
## 📑 Servicios

1. PostgreSQL
   - Imagen: `postgres:17.5`
   - Base de datos persistente en: `./data`
   - Zona horaria: `America/Guayaquil`
   - Puerto expuesto solo dentro del contenedor
2. ThingsBoard CE
   - Imagen: `thingsboard/tb-node:4.1.0`
   - Conectado a: `PostgreSQL`
   - Zona horaria: `America/Guayaquil`
   - Configuración para habilitar: `SSL/TLS`
   - Certificados ubicados en: `./ssl`
   - Puertos expuestos:
     - HTTP
     - RPC
     - MQTT
     - MQTT con TLS
     - COAP

## 📜 Certificados SSL

Genera el certificado PEM mediante los comandos:
```bash
openssl genrsa -out server.key 2048
openssl req -new -x509 -key server.key -out server.crt -days 365
cat server.key server.crt > server.pem
```
Coloca tu certificado en `./ssl/`

## ▶️ Ejecución
1. Antes de iniciar ThingsBoard, se debe iniciar el esquema de la base de datos y cargar los activos integrados ejecutando:
```bash
docker compose run --rm -e INSTALL_TB=true -e LOAD_DEMO=true thingsboard-ce
```

2. Construir y levantar los contenedores
```bash
docker compose up -d
```
3. Verificar logs
```bash
docker logs -f thingsboard
```
4. Acceder a la interfaz web: https://{your-host-ip}:8080
5. Se debe usar las siguientes credenciales predeterminadas:
   - **System Administrator**: sysadmin@thingsboard.org / sysadmin
   - **Tenant Administrator**: tenant@thingsboard.org / tenant
   - **Customer User**: customer@thingsboard.org / customer

## 📌 Notas

- Asegúrate de que los puertos no estén ocupados por otros servicios.
- Si usas un firewall, habilita los puertos HTTP, MQTT y COAP.
- La conexión a PostgreSQL se realiza vía variable `SPRING_DATASOURCE_URL` en ThingsBoard.
