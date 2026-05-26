# SQL Server usando Docker
## Descripción
Crear una base de datos en `SQL Server` usando `Docker` y gestionarla con `SQL Server Management Studio (SSMS)` es un proceso rápido: levantas el contenedor, te conectas a él desde `SSMS` y creas la base de datos gráficamente o mediante código.

## Pasos
### <ins>Paso 1</ins>: Iniciar el contenedor de `SQL Server` en `Docker`
1. Abre tu terminal (`PowerShell`, `CMD` o `Bash`).
2. Ejecuta el siguiente comando para descargar e iniciar la última versión de `SQL Server`.
    - Asegúrate de cambiar la contraseña, que para efectos del ejemplo es `TuPasswordFuerte123!`, por otra.
    ```Bash
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<password>" -e "MSSQL_PID=Developer" -p 1433:1433 --name sql_server_container --hostname sql_server_container -d mcr.microsoft.com/mssql/server:2025-latest
    ```

### <ins>Paso 2</ins>: Conectarte con `SQL Server Management Studio (SSMS)`
1. Abre `SQL Server Management Studio`.
2. En la ventana de conexión, ingresa los siguientes datos:
    - **<ins>`Server name`</ins>:** Ingresa `localhost` o `127.0.0.1`. Si usaste un puerto dinámico al `1433`, usa el formato `localhost,puerto` (ej. `localhost,1433`).
    - **<ins>`Authentication`</ins>:** `SQL Server Authentication`.
    - **<ins>`Login`</ins>:** `sa`.
    - **<ins>`Password`</ins>:** La contraseña que definiste en el comando de `Docker` (ej. `TuPasswordFuerte123!`).
3. Haz clic en `Connect`.

### <ins>Paso 3</ins>: Crear la base de datos
Una vez dentro, tienes dos métodos para crearla:

#### <ins>Método A</ins>: Usando la interfaz gráfica (`GUI`)
1. En el panel izquierdo (`Object Explorer`), haz clic derecho sobre la carpeta `Databases`.
2. Selecciona `New Database...`.
3. En el campo `Database name`, escribe el nombre que desees para tu base de datos.
4. Haz clic en `OK`.

#### <ins>Método B</ins>: Usando una consulta `T-SQL`
1. Haz clic en el botón `New Query` en la barra de herramientas superior.
2. Escribe y ejecuta el siguiente código:
   ```SQL
   CREATE DATABASE MiBaseDeDatosDocker;
   GO
   ```
3. Haz clic en `Execute` (o presiona `F5`).

### **<ins>(Opcional) Paso 4</ins>:** Listar contenedores creados
Para saber cuántos contenedores tienes en `Docker`, puedes seguir 2 caminos: La del terminal o aplicación de escritorio.
#### Método terminal
En la terminal deberás ejecutar uno de los siguientes comandos:
* Para ver solo los contenedores activos: `docker ps`.
* Para ver todos los contenedores (incluyendo los que están detenidos o apagados): `docker ps -a`.
* Para obtener únicamente el número total (contando todos): `docker ps -a -q | wc -l`.
#### Método aplicación de escritorio
Si prefieres una interfaz gráfica, abre `Docker Desktop` y ve a la sección `Containers` en el menú lateral. Allí verás el listado completo y un contador visible en la parte superior de la pantalla.

## Definiciones
### <ins>Paso 1</ins>: Iniciar el contenedor de `SQL Server` en `Docker`
* **<ins>`MSSQL_SA_PASSWORD`</ins>:** Define la contraseña del usuario administrador (`sa`).
* `MSSQL_PID`: Es una variable de entorno que define la edición de `SQL Server` o el `ID` de producto que se ejecutará en el contenedor. Determina las características, los límites de recursos y el tipo de licencia (de pago o gratuita) que utilizará tu instancia.
    * **<ins>`Developer` (predeterminada)</ins>:** Otorga todas las características premium de la edición `Enterprise`, pero únicamente para desarrollo y pruebas (sin licencia para producción). Si no declaras la variable, el contenedor asume esta opción.
    * **<ins>`Express`</ins>:** Edición gratuita y ligera, ideal para producción a pequeña escala con limitaciones de memoria y CPU.
    * **<ins>`Standard` o `Enterprise`</ins>:** Ediciones comerciales completas que requieren el ingreso de una clave de producto (`Product Key`) de licenciamiento por
* **<ins>`-p 1433:1433`</ins>:** Mapea el puerto del contenedor al de tu máquina local, permitiendo que `SSMS` lo "vea".

## Más información
1. [Microsoft SQL Server desde Docker Tutorial](https://www.youtube.com/watch?v=uHz9xOiaBbw).
2. [Microsoft SQL Server - Ubuntu based images](https://hub.docker.com/r/microsoft/mssql-server).
