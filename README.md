# SQLite

#Funcionamiento de cada topico de SQLite3

# Update:
Es una de las opciones de SQLite que se encarga de modificar un grupo de valores de una tabla que se encuentra en una base de datos en específica.
La especificidad de la tabla recae en que debe ser nombrada, en un principio, por el método update.

update usuarios set clave='starbucks';

También podemos actualizar varios campos en una sola instrucción:

update usuarios set nombre='francisco', clave='pacho'
where nombre='francisco';

# Schema:
Es un conjunto de objetos creado dentro de la misma base de datos para organizar la información mediante tablas, procedimientos, índices, funciones, etc. Esta clasificación se conoce como esquema dentro de la base de datos, y recibe de manera predeterminada el nombre de database owner o dbo.

CREATE TABLE sqlite_schema(
  type text,
  name text,
  tbl_name text,
  rootpage integer,
  sql text
);

# Type:
La columna sqlite_schema.type será una de las siguientes cadenas de texto: 'table', 'index', 'view' o 'trigger' según el tipo de objeto definido. La cadena 'table' se usa tanto para tablas ordinarias como virtuales .

# Name:
La columna sqlite_schema.name contendrá el nombre del objeto. ( Las restricciones UNIQUE y PRIMARY KEY en las tablas hacen que SQLite cree índices internos con nombres de la forma "sqlite_autoindex_TABLE_N", donde TABLE se reemplaza por el nombre de la tabla que contiene la restricción y N es un número entero que comienza con 1 y aumenta en uno con cada restricción vista en la definición de la tabla.

# tbl_name:
La columna sqlite_schema.tbl_name contiene el nombre de una tabla o vista a la que está asociado el objeto. Para una tabla o vista, la columna tbl_name es una copia de la columna de nombre. Para un índice, tbl_name es el nombre de la tabla que está indexada.

# rootpage:
La columna sqlite_schema.rootpage almacena el número de página de la página del árbol b raíz para tablas e índices. Para las filas que definen vistas, activadores y tablas virtuales, la columna de la página raíz es 0 o NULL.

# sql text:
La columna sqlite_schema.sql almacena texto SQL que describe el objeto. Este texto SQL es una instrucción CREATE TABLE , CREATE VIRTUAL TABLE , CREATE INDEX , CREATE VIEW o CREATE TRIGGER que, si se evalúa con el archivo de base de datos cuando es la base de datos principal de una conexión de base de datos , recrearía el objeto.


# Date and Time functions:
En SQLite se nos permite el uso de 6 funciones de fecha y hora, son las siguientes:
1. date(time-value, modifier, modifier, ...)
2. time(time-value, modifier, modifier, ...)
3. datetime(time-value, modifier, modifier, ...)
4. julianday(time-value, modifier, modifier, ...)
5. unixepoch(time-value, modifier, modifier, ...)
6. strftime(format, time-value, modifier, modifier, ...)

Las seis funciones de fecha y hora toman un valor de hora opcional como argumento, seguido de cero o más modificadores. La función strftime() también toma una cadena de formato como primer argumento.

Los valores de fecha y hora se pueden almacenar como:

1. Texto en un subconjunto del formato ISO-8601.
2. Números que representan el día juliano.
3. Números que representan el número de segundos desde (o antes) 1970-01-01 00:00:00 UTC (la marca de tiempo de Unix).
------------------------------------------------------------------------------
# Primary key constraint:
Para crear una primary key, primero debemos crear una tabla para que en ella se guarde la llave primaria, normalmente se guarda como entero y en algunos casos, como un autoincrementable:

CREATE TABLE COMPANY(
   ID INT PRIMARY KEY,
   NAME TEXT NOT NULL,
   AGE INT NOT NULL UNIQUE,
   ADDRESS CHAR(25),
   SALARY REAL,       
);
Se puede observar que en la creacion de la tabla "COMPANY", creamos la llave primaria quien la contiene el campo ID.


# Not null constraint:
En SQLite, la restricción Not Null se usa para indicar que la columna no permitirá almacenar valores NULL.

En general, en SQLite, las columnas predeterminadas permitirán valores NULL en caso de que tenga un requisito como no permitir valores NULL en la columna, entonces necesitamos agregar la restricción NOT NULL en la columna.

Una vez que agregamos la restricción NOT NULL en una columna requerida y si intentamos insertar o actualizar un valor NULL en una fila nueva o existente, arrojará un error debido a la violación de la restricción.

Sintaxis:

CREATE TABLE tablename (
column1 INTEGER,
column2 TEXT NOT NULL,
column3 TEXT NOT NULL
);

# Unique constraint:
Garantiza que todos los valores de una columna o un grupo de columnas sean distintos entre sí o únicos.

Para definir una restricción ÚNICA, utilice la palabra clave ÚNICA seguida de una o más columnas.

Puede definir una restricción ÚNICA a nivel de columna o de tabla. Solo a nivel de tabla, puede definir una restricción ÚNICA en varias columnas.

Sintaxis para añadir el unique constraint para una columna:
CREATE TABLE table_name(
    ...,
    column_name type UNIQUE,
    ...
);

Para una tabla:
CREATE TABLE table_name(
    ...,
    UNIQUE(column_name)
);

Para múltiples columnas:
CREATE TABLE table_name(
    ...,
    UNIQUE(column_name1,column_name2,...)
);

Ejemplo:
CREATE TABLE contacts(
    contact_id INTEGER PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    email TEXT NOT NULL UNIQUE
);
INSERT INTO contacts(first_name,last_name,email)
VALUES ('John','Doe','john.doe@gmail.com');

# Default constraint:
se utiliza para definir valores predeterminados para una columna. En general, en la restricción predeterminada de SQLite, se insertará el valor predeterminado en una columna en caso de que el valor de la columna sea nulo o esté vacío.

Podemos añadir una restricción predeterminada en la columna mientras creamos una nueva tabla usando Crear instrucción o modificando/alterando la tabla usando la instrucción ALTER. Ejemplo:

CREATE TABLE BOOK

(Book_id INTEGER PRIMARY KEY,

Book_name TEXT NOT NULL,

Price INTEGER DEFAULT 100);


# Check constraint:
La restricción CHECK habilita una condición para verificar el valor que se ingresa en un registro. Si la condición se evalúa como falsa, el registro viola la restricción y no se ingresa en la tabla.

Ejemplo:
El siguiente SQL crea una nueva tabla llamada CLIENTES y agrega cinco columnas. Aquí, agregamos un CHEQUE con la columna EDAD, para que no pueda tener ningún CLIENTE menor de 18 años −

CREATE TABLE COMPANY(
   ID   INT       PRIMARY KEY     ,
   NAME          TEXT     NOT NULL,
   AGE  INT              NOT NULL UNIQUE,
   ADDRESS  CHAR (25) ,
   SALARY   REAL  ,       
   
);
Si la tabla EMPRESA ya se ha creado, entonces para agregar una restricción CHECK a la columna EDAD, escribiría una declaración similar a la siguiente:

ALTER TABLE COMPANY
   MODIFY AGE INT NOT NULL CHECK (AGE >= 18 );


# Alter table:
Este comando nos permite modificar una tabla existente sin realizar un volcado completo y recargar los datos. Puede cambiar el nombre de una tabla usando la declaración ALTER TABLE y se pueden agregar columnas adicionales en una tabla existente usando la declaración ALTER TABLE.

No hay otra operación compatible con el comando ALTER TABLE en SQLite, excepto cambiar el nombre de una tabla y agregar una columna en una tabla existente.

Sintaxis:
ALTER TABLE database_name.table_name RENAME TO new_table_name;

# Delete:
Se utiliza para eliminar los registros existentes de una tabla. Puede usar la cláusula WHERE con la consulta DELETE para eliminar las filas seleccionadas; de lo contrario, se eliminarían todos los registros.

Sintaxis:
DELETE FROM table_name
WHERE [condition];

# Drop:
La instrucción DROP TABLE se usa para eliminar una definición de tabla y todos los datos, índices, activadores, restricciones y especificaciones de permisos asociados para esa tabla.

Debe tener cuidado al usar este comando porque una vez que se elimina una tabla, toda la información disponible en la tabla también se perderá para siempre.

Sintaxis:
DROP TABLE database_name.table_name;

# Backup:
El shell de la línea de comandos de SQLite proporciona el comando .backup dot que le permite realizar copias de seguridad de una base de datos de forma rápida y sencilla.

Para usar este comando, proporcione el nombre de la base de datos que desea respaldar y un nombre de archivo para el archivo de respaldo.
Ejemplo:
.backup Store Store_backup.db

# Restore:
El comando .restore realiza una copia de bajo nivel de un archivo de base de datos en una base de datos abierta o adjunta. Si no se proporciona el nombre de la base de datos, se completará la base de datos principal. Este comando se usa con frecuencia para llenar una base de datos en memoria activa desde un archivo de base de datos. Es seguro ejecutar este comando en una base de datos de origen activa (aunque es posible que no funcione).

Sintaxis:
.restore [database_name] filename


