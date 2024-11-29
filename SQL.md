# INTRO a SQL

## TIPOS DE DATO

**Numéricos**

INT ...... -2.147.483.648 hasta 2.147.483.647
FLOAT .... Decimales de baja precisión
DECIMAL .. Decimales de precisión exacta

**Texto**

VARCHAR(N) ... Cadenas de longitud máxima N
CHAR(N) ...... Cadena de longitud EXACTA N
TEXT ......... Cadena de texto ilimitada (cuidado!)

**Boleanos**

BOOLEAN ..... (0/1, TRUE/FALSE)

**Fecha/Hora**

DATE ...... Fecha (yyyy-mm-dd)
TIME ...... Hora (hh:mm:ss)
DATETIME .. Fecha y hora
TIMESTAMP . Marca de tiempo (número de milisegundos transcurridos desde 01/01/1970)

## Consulta de datos

```sql
SELECT col1, col2....
  FROM tabla;

SELECT *
  FROM tabla;

-- Filtrar:
SELECT *
  FROM tabla
 WHERE nombre = 'Tony';

-- Filtrar eliminando repetidos
SELECT DISTINCT ciudad
  FROM tabla
 WHERE nombre = 'Tony';

-- Ordenación
SELECT *
  FROM productos
 ORDER BY precio DESC; --  (ASC)

SELECT *
  FROM productos
 ORDER BY precio DESC
 LIMIT 10;

```

## DML - Manipulación de datos

```sql
-- agregar un registro
INSERT INTO usuarios (id, nombre, username, password)
VALUES (6, 'Tony', 'tonygb', 'supersegura');

-- Modificar datos
UPDATE usuarios
   SET nombre = 'Pedro'

--  WHERE id = 5
 WHERE nombre = 'Tony';

-- Eliminar
DELETE FROM usuarios
 WHERE username = 'tonygb';

-- Vaciar completamente una tabla
TRUNCATE TABLE usuarios;
```

## DDL - Creación y modificación de tablas

```sql
CREATE TABLE productos (
    id INT PRIMARY KEY,
    nombre VARCHAR(100),
    precio DECIMAL(4,2)
);

-- Agregar una columnma a una tabla existente
ALTER TABLE productos ADD categoria VARCHAR(25);

-- Modificar una columna
ALTER TABLE productos MODIFY precio INT;

-- Eliminar una columna
ALTER TABLE productos DROP COLUMN categoria;

-- Eliminar tabla
DROP TABLE productos;
```

## Relaciones

### Inner join

Se seleccionan los registros que tienen el valor igual en los campos de la unión.

```sql
SELECT ciudades.nombre as ciudad, paises.nombre as pais
 FROM ciudades
INNER JOIN paises
   ON ciudades.id_pais = paises.id
```

### Full outer join

```sql
SELECT ciudades.nombre as ciudad, paises.nombre as pais
 FROM ciudades
 FULL OUTER JOIN paises
   ON ciudades.id_pais = paises.id
```

### Left join, Right join

```sql
--- todas las ciudades tengan o no país pero no los paises que no tengan ciudad
SELECT ciudades.nombre as ciudad, paises.nombre as pais
 FROM ciudades
 LEFT JOIN paises
   ON ciudades.id_pais = paises.id
```

```sql
--- todos los paises tengan o no ciudades pero no las ciudades que no tengan país
SELECT ciudades.nombre as ciudad, paises.nombre as pais
 FROM ciudades
 LEFT JOIN paises
   ON ciudades.id_pais = paises.id
```

## Algunos operadores

```sql
...WHERE email IS NULL
...WHERE email IS NOT NULL
...WHERE pais IN ('España', 'Portugal')
...WHERE edad BETWEEN 3 AND 10
```

## Funciones de agregación (de dominio agrwegado)

```sql
-- contar todos los productos
SELECT COUNT(*) as total_productos
  FROM productos;

-- sumar el precio de todos los productos
SELECT SUM(precio) as total_precios
  FROM productos;

-- calcular el valor de mi stock en almacén
SELECT SUM(precio * stock) as valor_inversion
  FROM productos;

-- precio medio de mis productos
SELECT AVG(precio) as precio_medio
  FROM productos;

-- el precio del producto más caro
SELECT MAX(precio) as producto_mas_caro
  FROM productos;

-- el precio del producto más barato
SELECT MIN(precio) as producto_mas_barato
  FROM productos;

-- combinados
SELECT MIN(precio) as mas_barato, MAX(precio) as mas_caro, AVG(precio) as precio_medio
  FROM productos;

-- combinados
SELECT MIN(precio) as mas_barato, MAX(precio) as mas_caro, AVG(precio) as precio_medio
  FROM productos
 WHERE categoria = 'hardware';

-- agrupar funciones agrupadas
-- combinados
SELECT categoria, MAX(precio) as mas_caro, SUM(precio*stock) as valor
  FROM productos
 GROUP BY categoria;
```