# MYSQL
 
 #  Configuración de MySQL en Docker

---

##   Crear un Contenedor MySQL

Ejecuta el siguiente comando para iniciar un servidor MySQL en Docker:
###  Descargar e instalar Docker
- Descargar e instalar Docker desde [https://www.docker.com/](https://www.docker.com/).
- Verificar la instalación con:
  ```bash
  docker --version
  ```

### 2 Descargar la imagen de MySQL
- Ejecutar el siguiente comando para obtener la imagen oficial de MySQL:
  ```bash
  docker pull mysql:latest
  ```

###  Crear y ejecutar el contenedor de MySQL
- Ejecutar el siguiente comando para crear y ejecutar el contenedor:
  ```bash
  docker run --name servidor-mysql \
    -e MYSQL_ROOT_PASSWORD=mipassword \
    -e MYSQL_DATABASE=mi_basedatos \
    -e MYSQL_USER=usuario \
    -e MYSQL_PASSWORD=clave_usuario \
    -p 3306:3306 \
    -v mysql_data:/var/lib/mysql \
    -d mysql:latest
  ```

###  Verificar que el contenedor esté corriendo
- Comprobar el estado del contenedor con:
  ```bash
  docker ps
  ```
![image](https://github.com/user-attachments/assets/175b159f-5871-4b50-b4c4-16df4c4d391b)


###  Acceder a MySQL dentro del contenedor
- Conectarse al servidor MySQL dentro del contenedor:
  ```bash
  docker exec -it servidor-mysql mysql -u root -p
  ```
- Ingresar la contraseña configurada (`mipassword`).

![alt text](image-1.png)


- Seleccionar la base de datos:
  ```sql
  USE mi_basedatos;
  ```

```bash
docker run --mi-mysql mysql \
  -e MYSQL_ROOT_PASSWORD=josep123 \
  -e MYSQL_DATABASE=mi_basedatos \
  -e MYSQL_USER=usuario \
  -e MYSQL_PASSWORD=clave_usuario \
  -p 3306:3306 \
  -v mysql_data:/var/lib/mysql \
  -d mysql:latest
```
en caso de tener un mysql se cambia a 3307 porque ya esta en uso el puerto 3306

###  Explicación:
- Crea un contenedor `servidor-mysql`.
- Define una base de datos `mi_basedatos`.
- Crea un usuario `usuario` con contraseña `clave_usuario`.
- Expone el puerto `3306`.
- Usa un volumen persistente `mysql_data` para los datos.

en caso de no tener una tabla, debemos crear una con el siguente codigo:
CREATE DATABASE mi_basedatos;
USE mi_basedatos;

![alt text](image-2.png)

---

##  2. Verificar que el Contenedor está Corriendo

```bash
docker ps
```
Si el contenedor aparece en la lista, significa que está funcionando correctamente.

---

##  3. Conectarse a MySQL en el Contenedor

Ejecuta:

```bash
docker exec -it servidor-mysql mysql -u root -p
```

Ingresa la contraseña: `mipassword`.

Para seleccionar la base de datos:

```sql
USE mi_basedatos;
```

---

## 4. Crear la Tabla `Cliente`

Ejecuta el siguiente SQL:

```sql
CREATE TABLE Cliente (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    correo VARCHAR(150) UNIQUE NOT NULL,
    telefono VARCHAR(15),
    fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

![alt text](image-3.png)
---

## 5. Insertar Datos en la Tabla

```sql
INSERT INTO Cliente (nombre, correo, telefono) VALUES 
('Juan Pérez', 'juan@example.com', '123456789'),
('Ana Gómez', 'ana@example.com', '987654321');
```

Para verificar los datos:

```sql
SELECT * FROM Cliente;
```

![alt text](image-4.png)

---

##  6. Modificar la Tabla `Cliente`

### Agregar una Nueva Columna `direccion`:
```sql
ALTER TABLE Cliente ADD direccion VARCHAR(200);
```

### Actualizar la Nueva Columna:
```sql
UPDATE Cliente SET direccion = 'Avenida Central 456' WHERE id = 1;
```

### Insertar un Nuevo Cliente con `direccion`:
```sql
INSERT INTO Cliente (nombre, correo, telefono, direccion) VALUES 
('María Fernández', 'maria@example.com', '987654321', 'Boulevard Norte 789');
```



---

##  7. Detener y Eliminar el Contenedor (Opcional)

Si necesitas detener el servidor:
```bash
docker stop servidor-mysql
```

Para eliminarlo junto con los datos:
```bash
docker rm servidor-mysql
```
```bash
docker volume rm mysql_data
```

---

##  ¡Listo!
Ahora tienes un servidor MySQL funcionando en Docker con una base de datos y una tabla `Cliente`. 🎉
