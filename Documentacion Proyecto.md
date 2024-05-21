<a name="top"></a>

# Guía de Inicio Rápido General

***

## Introducción

Bienvenido a **GeoCardioApp**, un sistema de telemonitoreo georreferenciado diseñado para monitorear las variables vitales y la ubicación de una persona en tiempo real y de forma histórica. El sistema está compuesto por varios componentes integrados que trabajan juntos para proporcionar monitoreo.

***

### Componentes del Sistema

   1. [**Página Web**](#configuración-del-proyecto-react)
      -  **Descripción**: Una interfaz web desarrollada en React que permite a los usuarios observar las variables emuladas de un brazalete (RF-V48) y la ubicación de la persona monitoreada.
      -  **Funciones**: Visualización de datos históricos y en tiempo real de las variables censadas y la localización geográfFica.

   2. [**Servidor HTTPS**](#configuración-del-servidor-https)
      -  **Descripción**: Un servidor seguro que sirve la página web y maneja la comunicación entre el cliente (navegador web) y la base de datos.
      -  **Funciones**: Gestión de solicitudes del cliente, almacenamiento y recuperación de datos en la base de datos.

   3. [**Servidor TCP Personalizado**](#configuración-del-servidor-tcp-personalizado)
      -  **Descripción**: Un servidor especializado en manejar la comunicación básica con el brazalete geolocalizador ReachFar (RF-V48).
      -  **Funciones**: Recepción y petición de las variables censadas por el brazalete, como la ubicación y otras variables vitales.

   4. [**Aplicación Android**](#configuración-de-la-aplicación-android)
      -  **Descripción**: Una aplicación móvil desarrollada en Flutter que emula el envío de información de las variables censadas por el brazalete RF-V48.
      -  **Funciones**: Envío de datos al servidor TCP personalizado para que estos sean almacenados en la base de datos.

   5. [**Base de Datos**](#configuración-de-la-base-de-datos)
      - **Descripción:** Almacena todas las variables censadas y la información de geolocalización, tanto en tiempo real como históricamente.
      - **Estructura:** La estructura de las tablas está definida en el código, y se encarga de gestionar y almacenar los datos recibidos desde los servidores.
      - **Funciones:** Proveer una infraestructura de almacenamiento robusta y eficiente para el acceso rápido y seguro a los datos.


### Flujo de Datos

   1. La **aplicación Android** emula y envía datos del brazalete al **servidor TCP personalizado**.
   2. El **servidor TCP personalizado** recibe y procesa estos datos, luego los almacena en la **base de datos**.
   3. El **servidor HTTPS** sirve la **página web**, permitiendo a los usuarios acceder y visualizar los datos almacenados, tanto en tiempo real como históricos.

GeoCardioApp está diseñado para proporcionar una solución robusta y eficiente para el telemonitoreo de pacientes, facilitando el acceso a información vital.

## Requisitos Previos
   - Node.js
   - Flutter SDK
   - Android SDK
   - Base de datos Postgresql

## Componentes del proyecto
Componentes del Proyecto

La carpeta del proyecto incluye los siguientes directorios y archivos:

   - **/webfront** - Contiene el código fuente de la página web desarrollada en React.
   - **/webback** - Contiene el código del servidor HTTPS.
   - **/GeoCardioProtocol** - Contiene el código del servidor TCP personalizado.
   - **/GCAPP** - Contiene el código fuente de la aplicación Android desarrollada en Flutter.
   - **/database** - Contiene los scripts de inicialización y configuración de la base de datos.


***

## Instalación y Configuración Global

***

   ### Configuración de la Base de Datos

   1. **Instalar PostgreSQL** (si no lo tienes instalado):

      - En Linux:

         ```bash
         sudo apt-get update
         sudo apt-get install postgresql postgresql-contrib
         ```
      - En macOS:

         ```bash
         brew install postgresql
         ```

      - En Windows:
         Descarga e instala PostgreSQL desde postgresql.org.

   2. **Configurar la base de datos:**

      - Crear un nuevo usuario y una base de datos:

         ```sql
         CREATE USER GeoCardio WITH PASSWORD 'CONTRASEÑA';
         CREATE DATABASE GPRS;
         GRANT ALL PRIVILEGES ON DATABASE GPRS TO GeoCardio;
         ```

***

   ### Configuración del proyecto React

   1. **Instalar dependencias**:

      ```bash
      cd ./webfront
      npm install
      ```
   *(en caso de tener error de dependencias, eliminar la carpeta de node_modules y volver a realizar `npm install`)*

   3. **Ejecutar el script de react**:
      - Para ejecutar la aplicacipón en entornos de prueba:

         ```bash
         sudo npm start
         ```
      - Para construir el proyecto para entornos de producción:
         ```bash
         npm build
         ```

***

   ### Configuración del Servidor HTTPS

   1. **Instalar dependencias**:

      ```bash
      cd ./webback
      npm install
      ```
   

      #### Configuración del archivo `.env`

      - El archivo debe tener el siguiente contenido:

         ```env
         PORT=80
         ACCESS_TOKEN_SECRET=<ACCESS_TOKEN_SECRET>
         REFRESH_TOKEN_SECRET=<REFRESH_TOKEN_SECRET>
         SESSION_SECRET=<SESSION_SECRET>
         ```
      - Genera valores para **<ACCESS_TOKEN_SECRET>**, **<REFRESH_TOKEN_SECRET>**, y **<SESSION_SECRET>**. Puedes usar una herramienta en línea para generar UUIDs como [UUID Generator](https://www.uuidgenerator.net/) o generar valores aleatorios desde la terminal. Por ejemplo, en Linux o macOS:

         ```bash
         uuidgen
         ```
      
      - Reemplaza **<ACCESS_TOKEN_SECRET>**, **<REFRESH_TOKEN_SECRET>**, y **<SESSION_SECRET>** con los valores generados.

      - Aquí tienes un ejemplo de cómo debería verse tu archivo .env después de configurarlo:
         ```env
         PORT=80
         ACCESS_TOKEN_SECRET=36d94693-c44c-42ce-925a-a8e388fc3a36
         REFRESH_TOKEN_SECRET=7a5393d9-b7a4-402d-8cb5-4757fb2498f7
         SESSION_SECRET=7a5832d9-b7a4922d-8cb5-4757fy2498f7
         ```
      
      #### Configuración del archivo `config.json`

         1. Abre el archivo `config.json` ubicado en la carpeta correspondiente del proyecto.

         2. Reemplaza los valores existentes con los que correspondan a tu configuración de base de datos. Asegúrate de proporcionar el **nombre de usuario, la contraseña, el nombre de la base de datos, la dirección del host y el puerto correctos**.

         3. Guarda los cambios en el archivo `config.json`.

   3. **Ejecutar el servidor HTTPS**:
      ```bash
      sudo node app.js
      ```

***

   ### Configuración del Servidor TCP Personalizado
   
   1. **Instalar dependencias**:

      ```bash
      cd ./GeoCardioProtocol
      npm install
      ```
   2. **Configurar el servidor**:

      #### Configuración del archivo `config.json`

         1. Abre el archivo `config.json` ubicado en la carpeta correspondiente del proyecto.

         2. Reemplaza los valores existentes con los que correspondan a tu configuración de base de datos. Asegúrate de proporcionar el **nombre de usuario, la contraseña, el nombre de la base de datos, la dirección del host y el puerto correctos**.

         3. Guarda los cambios en el archivo `config.json`.

   3. Ejecutar el servidor TCP:
      ```bash
      sudo node index.js
      ```

***

### Configuración de la Aplicación Android

   - **Configurar Flutter**:
      Sigue las instrucciones de instalación de [Flutter](flutter.dev)

   - **Configurar Android SDK**:
        Asegúrate de tener el Android SDK configurado. Puedes usar [Android Studio](https://developer.android.com/studio) para manejar el SDK.

   - Instalar dependencias y ejecutar la aplicación:
      ```bash
      cd ./GCAPP
      flutter run
      ```