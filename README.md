Laboratorio 02 - Docker Compose

Profesor, en este laboratorio configuré un entorno completo utilizando Docker Compose para desplegar una API en Node.js y una base de datos PostgreSQL de forma aislada y persistente.

Explicación de lo que realicé en el proyecto:

- 3 copias de la API con build local: Cloné el código de la API dentro de la carpeta api/ y configuré el docker-compose.yml para levantar 3 instancias distintas (api1, api2, api3) corriendo en los puertos 3001, 3000 y 3002.
- Configuración de Base de Datos: Agregué el servicio de PostgreSQL (postgres:latest) asignándole el nombre de contenedor some-postgres y mapeando el puerto 5432.
- Uso de Variables de Entorno: Creé un archivo .env local con las credenciales de la base de datos (POSTGRES_DB, POSTGRES_USER, POSTGRES_PASSWORD, POSTGRES_PORT). El archivo docker-compose.yml consume estas variables para no dejar contraseñas expuestas en el código.
- Uso de Volúmenes: Definí un volumen administrado llamado postgres_data para respaldar la carpeta /var/lib/postgresql/data. Con esto, los datos de PostgreSQL no se pierden si el contenedor se detiene.
- Uso de .gitignore: Agregué el archivo .gitignore al inicio del proyecto para evitar subir el archivo .env con mis credenciales sensibles y los archivos de logs al repositorio.
- Conventional Commits: Realicé los cambios en Git utilizando el estándar de mensajes como chore:, feat: y docs:.

Comandos para desplegar el proyecto:

Para levantar los contenedores y construir las imágenes locales:
docker compose up -d --build

Para verificar que las 3 APIs y la base de datos están corriendo correctamente:
docker compose ps

Para detener y limpiar todos los servicios:
docker compose down

Preguntas Teóricas:

Tipos de redes en Docker:

- Bridge: Es la red por defecto. La usé en este proyecto para que las 3 réplicas de la API y la base de datos PostgreSQL se puedan comunicar entre sí mediante IPs internas.
- Host: Le quita el aislamiento de red al contenedor y hace que utilice directamente la red y puertos de la computadora anfitriona.
- Overlay: Permite conectar contenedores que se ejecutan en diferentes máquinas físicas o nodos dentro de un clúster (Docker Swarm).
- Macvlan: Le asigna una dirección MAC física al contenedor para que la red lo reconozca como si fuera un dispositivo físico independiente.
- None: Desactiva completamente la interfaz de red del contenedor, dejándolo totalmente aislado.

Tipos de volúmenes en Docker:

- Named Volumes: Son volúmenes gestionados por Docker. Fue el que utilicé para la base de datos (postgres_data), ya que es la mejor opción para garantizar la persistencia de datos.
- Bind Mounts: Mapean una carpeta o archivo específico de mi computadora dentro del contenedor. Si cambio algo en mi máquina, se actualiza en el contenedor al instante.
- tmpfs Mounts: Guardan la información temporalmente en la memoria RAM del sistema anfitrión. Cuando el contenedor se apaga, todo lo guardado ahí se borra.