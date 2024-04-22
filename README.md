GARDEN DATABASE
```sql
CREATE TABLE gama(
  id_gama VARCHAR(50) PRIMARY KEY,
  descripcion_texto TEXT,
  descripcion_html TEXT,
  imagen VARCHAR(256)
  );
  
INSERT INTO gama (id_gama, descripcion_texto, descripcion_html, imagen) VALUES
('G001', 'Descripción de gama 1', '<p>Descripción en HTML de gama 1</p>', 'imagen1.jpg'),
('G002', 'Descripción de gama 2', '<p>Descripción en HTML de gama 2</p>', 'imagen2.jpg'),
('G003', 'Ornamentales', '<p>Descripción en HTML de gama Ornamentales</p>', 'imagen3.jpg'),
('G004', 'Frutales', '<p>Descripción en HTML de gama Frutales</p>', 'imagen4.jpg');

CREATE TABLE dimensiones(
  id_dimensiones INT(10) PRIMARY KEY,
  ancho DECIMAL (18,5) NOT NULL,
  alto DECIMAL (18,5) NOT NULL,
  largo DECIMAL (18,5)NOT NULL
  );
  
INSERT INTO dimensiones (id_dimensiones, ancho, alto, largo) VALUES
(1, 5.25, 7.10, 15.80),
(2, 6.00, 8.25, 18.00);

CREATE TABLE proveedor(
  id_proveedor INT(10) PRIMARY KEY,
  nombre VARCHAR(50) NOT NULL,
  apellido1 VARCHAR(50)NOT NULL,
  apellido2 VARCHAR(50),
  telefono INT(10)
  );

CREATE TABLE stock(
  id_stock INT(10) PRIMARY KEY,
  cantidad_producto INT(10) NOT NULL
  );

INSERT INTO stock (id_stock, cantidad_producto) VALUES
(1,25),
(2,30),
(3,150),
(4, 50);

INSERT INTO proveedor (id_proveedor, nombre, apellido1, apellido2, telefono) VALUES
(1, 'Juan', 'Pérez', 'López', 987654321),
(2, 'Ana', 'García', 'Mendoza', 123456789);
  
CREATE TABLE producto(
  codigo_producto INT(10) PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  gama_producto VARCHAR(50) NOT NULL,
  dimensiones_producto INT(10),
  descripcion text,
  id_proveedor_producto INT(10), 
  cantidad_stock INT(10) NOT NULL,
  precio_venta DECIMAL(15,2) NOT NULL,
  precio_proveedor DECIMAL(15,2),
	CONSTRAINT FK_gama FOREIGN KEY (gama_producto) REFERENCES gama(id_gama),
	CONSTRAINT FK_dimensiones FOREIGN KEY (dimensiones_producto) REFERENCES dimensiones(id_dimensiones),
	CONSTRAINT FK_proveedor FOREIGN KEY (id_proveedor_producto) REFERENCES proveedor(id_proveedor),
	CONSTRAINT FK_stock FOREIGN KEY (cantidad_stock) REFERENCES stock(id_stock)
  );
  
INSERT INTO producto (codigo_producto, nombre, gama_producto, dimensiones_producto, descripcion, id_proveedor_producto, cantidad_stock, precio_venta, precio_proveedor) VALUES
(1001, 'Producto 1', 'G001', 1, 'Descripción de producto 1', 1, 1, 99.99, 50.00),
(1002, 'Producto 2', 'G002', 2, 'Descripción de producto 2', 2, 2, 119.99, 55.00),
(1003, 'Producto 3', 'G003', 1, 'Descripción de producto 3', 1, 3, 129.99, 70.00),
(1004, 'Producto 4', 'G004', 2, 'Descripción de producto 4', 2, 4, 89.99, 40.00);

CREATE TABLE tipo_tel(
  id_tipo_tel INT(10) PRIMARY KEY,
  nombre_tipo_tel VARCHAR(50) NOT NULL,
  descripcion_tipo_tel TEXT
  );
  
INSERT INTO tipo_tel (id_tipo_tel, nombre_tipo_tel) VALUES
(1, 'Teléfono fijo'),
(2, 'Teléfono móvil');

CREATE TABLE telefono(
  id_telefono INT(10)PRIMARY KEY,
  id_tipo INT(10) NOT NULL,
  numero INT(10) NOT NULL,
  prefijo VARCHAR(5),
  CONSTRAINT FK_tipo_tel FOREIGN KEY (id_tipo) REFERENCES tipo_tel(id_tipo_tel)
  );
  
INSERT INTO telefono (id_telefono, id_tipo, numero, prefijo) VALUES
(1, 1, 111111111, '+34'),
(2, 2, 222222222, '+34');
  
CREATE TABLE contacto(
  id_contacto INT(10) PRIMARY KEY,
  nombre_contacto VARCHAR(50) NOT NULL,
  apellido1_contacto VARCHAR(50) NOT NULL,
  apellido2_contacto VARCHAR(50),
  telefono_contacto INT(10) NOT NULL,
  CONSTRAINT FK_telefono_contacto FOREIGN KEY (telefono_contacto) REFERENCES telefono(id_telefono)
  );

CREATE TABLE fax(
  id_fax INT(10)PRIMARY KEY,
  codigo_fax VARCHAR(50) NOT NULL
  );
  
CREATE TABLE calle(
  id_calle INT(10) PRIMARY KEY,
  calle VARCHAR(100) NOT NULL
  );

CREATE TABLE barrio(
  id_barrio INT(10) PRIMARY KEY,
  barrio VARCHAR(100) NOT NULL
  );
  
CREATE TABLE direccion(
  id_direccion INT(10)PRIMARY KEY,
  id_calle_direccion INT(10) NOT NULL,
  id_barrio_direccion INT(10) NOT NULL,
  vivienda VARCHAR(50),
  CONSTRAINT FK_calle FOREIGN KEY (id_calle_direccion) REFERENCES calle(id_calle), 
  CONSTRAINT FK_barrio FOREIGN KEY (id_barrio_direccion) REFERENCES barrio(id_barrio)
  );

CREATE TABLE ciudad(
  id_ciudad INT(10) PRIMARY KEY,
  ciudad VARCHAR(100) NOT NULL
  );
  
CREATE TABLE region(
  id_region INT(10) PRIMARY KEY,
  region VARCHAR(100) NOT NULL
  );
  
CREATE TABLE pais(
  id_pais INT(10) PRIMARY KEY,
  pais VARCHAR(100) NOT NULL
  );
  
CREATE TABLE codigo_postal(
  id_codigo_postal INT(10) PRIMARY KEY,
  codigo_postal VARCHAR(50) NOT NULL,
  lugar_perteneciente VARCHAR(50)
  );

CREATE TABLE cliente(
  id_cliente INT(10) PRIMARY KEY,
  nombre_cliente VARCHAR(50) NOT NULL,
  apellido1_cliente VARCHAR(50) NOT NULL,
  apellido2_cliente VARCHAR(50),
  id_contacto_cliente INT(10),
  telefono_cliente INT(10) NOT NULL,
  id_fax_cliente INT(10) NOT NULL,
  id_direccion1_cliente INT(10) NOT NULL,
  id_direccion2_cliente INT(10),
  id_ciudad_cliente INT(10) NOT NULL,
  id_region_cliente INT(10) NOT NULL,
  id_pais_cliente INT(10) NOT NULL,
  codigo_postal_cliente INT(10),
  codigo_empleado_rep_ventas INT(10),
  limite_credito DECIMAL(15,2),
  CONSTRAINT FK_contacto FOREIGN KEY (id_contacto_cliente) REFERENCES contacto(id_contacto),
  CONSTRAINT FK_telefono_cliente FOREIGN KEY (telefono_cliente) REFERENCES telefono(id_telefono),
  CONSTRAINT FK_fax_cliente FOREIGN KEY (id_fax_cliente) REFERENCES fax(id_fax),
  CONSTRAINT FK_direccion1_cliente FOREIGN KEY (id_direccion1_cliente) REFERENCES direccion(id_direccion),
  CONSTRAINT FK_direccion2_cliente FOREIGN KEY (id_direccion2_cliente) REFERENCES direccion(id_direccion),
  CONSTRAINT FK_ciudad_cliente FOREIGN KEY (id_ciudad_cliente) REFERENCES ciudad(id_ciudad),
  CONSTRAINT FK_region_cliente FOREIGN KEY (id_region_cliente) REFERENCES region(id_region),
  CONSTRAINT FK_pais_cliente FOREIGN KEY (id_pais_cliente) REFERENCES pais(id_pais),
  CONSTRAINT FK_codigo_postal_cliente FOREIGN KEY (codigo_postal_cliente) REFERENCES codigo_postal(id_codigo_postal)
  );
  
CREATE TABLE estado(
  id_estado INT(10) PRIMARY KEY,
  estado VARCHAR(50) NOT NULL
  );

CREATE TABLE pedido(
  codigo_pedido INT(11) PRIMARY KEY,
  fecha_pedido DATE NOT NULL,
  fecha_esperada DATE NOT NULL,
  fecha_entrega DATE, 
  estado_pedido INT(10) NOT NULL,
  comentarios TEXT,
  codigo_cliente_pedido INT(10) NOT NULL,
  CONSTRAINT FK_estado FOREIGN KEY (estado_pedido) REFERENCES estado(id_estado),
  CONSTRAINT FK_cliente_pedido FOREIGN KEY (codigo_cliente_pedido) REFERENCES cliente(id_cliente)
  );
  
CREATE TABLE detalle_pedido(
  codigo_pedido_detalle INT(10) NOT NULL,
  codigo_producto_pedido INT(10) NOT NULL,
  cantidad INT(11) NOT NULL,
  precio_unidad DECIMAL(15,2) NOT NULL,
  numero_linea SMALLINT(6) NOT NULL,
  PRIMARY KEY (codigo_pedido_detalle, codigo_producto_pedido),
  CONSTRAINT FK_producto_pedido FOREIGN KEY (codigo_producto_pedido) REFERENCES producto(codigo_producto),
  CONSTRAINT FK_codigo_pedido FOREIGN KEY (codigo_pedido_detalle) REFERENCES pedido(codigo_pedido)
  );

CREATE TABLE forma_pago(
  id_forma_pago INT(10) PRIMARY KEY,
  nombre_forma_pago VARCHAR(50) NOT NULL,
  descripcion_forma_pago TEXT
  );
  
CREATE TABLE transaccion(
  id_transaccion VARCHAR(50),
  cod_cliente INT(10) NOT NULL,
  forma_pago_trasaccion INT(10) NOT NULL,
  fecha_transaccion DATE,
  total_transaccion DECIMAL (15,2),
  CONSTRAINT FK_forma_pago FOREIGN KEY (forma_pago_trasaccion) REFERENCES forma_pago(id_forma_pago)
  );

CREATE TABLE oficina(
  codigo_oficina VARCHAR(10) PRIMARY KEY,
  id_ciudad_oficina INT(10) NOT NULL,
  id_region_oficina INT(10) NOT NULL,
  id_pais_oficina INT(10),
  id_codigo_postal_oficina INT(10) NOT NULL,
  id_telefono_oficina INT(10) NOT NULL,
  id_direccion_oficina1 INT(10) NOT NULL,
  id_direccion_oficina2 INT(10)
  );
  
CREATE TABLE empleado(
  id_empleado INT(11) PRIMARY KEY,
  nombre_empleado VARCHAR(50) NOT NULL,
  apellido1_empleado VARCHAR(50) NOT NULL,
  apellido2_empleado VARCHAR(50),
  extension VARCHAR(10) NOT NULL,
  email_empleado VARCHAR(100) NOT NULL,
  codigo_oficina_empleado VARCHAR(10) NOT NULL,
  codigo_jefe INT(11),
  puesto VARCHAR(50),
  CONSTRAINT FK_oficina_empleado FOREIGN KEY (codigo_oficina_empleado) REFERENCES oficina(codigo_oficina)
  );
  

INSERT INTO contacto (id_contacto, nombre_contacto, apellido1_contacto, apellido2_contacto, telefono_contacto) VALUES
(1, 'Pedro', 'González', 'Rodríguez', 1), 
(2, 'Laura', 'Martínez', 'Hernández', 2);

INSERT INTO fax (id_fax, codigo_fax) VALUES
(1, 'F001'),
(2, 'F002');

INSERT INTO calle (id_calle, calle) VALUES
(1, 'Calle Principal'),
(2, 'Calle Secundaria');

INSERT INTO barrio (id_barrio, barrio) VALUES
(1, 'Barrio Norte'),
(2, 'Barrio Sur');

INSERT INTO direccion (id_direccion, id_calle_direccion, id_barrio_direccion, vivienda) VALUES
(1, 1, 1, 'Casa 1'),
(2, 2, 2, 'Casa 2');

INSERT INTO ciudad (id_ciudad, ciudad) VALUES
(1, 'Ciudad A'),
(2, 'Ciudad B'),
(3, 'Madrid'),
(4, 'Fuenlabrada');

INSERT INTO region (id_region, region) VALUES
(1, 'Región 1'),
(2, 'Región 2');

INSERT INTO pais (id_pais, pais) VALUES
(1, 'País A'),
(2, 'País B'),
(3, 'España');

INSERT INTO codigo_postal (id_codigo_postal, codigo_postal, lugar_perteneciente) VALUES
(1, '12345', 'Lugar A'),
(2, '54321', 'Lugar B');

INSERT INTO cliente (id_cliente, nombre_cliente, apellido1_cliente, apellido2_cliente, id_contacto_cliente, telefono_cliente, id_fax_cliente, id_direccion1_cliente, id_direccion2_cliente, id_ciudad_cliente, id_region_cliente, id_pais_cliente, codigo_postal_cliente, codigo_empleado_rep_ventas, limite_credito) VALUES
(1, 'Cliente 1', 'Apellido1', 'Apellido2', 1, 1, 1, 1, NULL, 1, 1, 1, 1, NULL, 10000),
(2, 'Cliente 2', 'Apellido3', 'Apellido4', 2, 2, 2, 2, NULL, 3, 2, 2, 2, 11, 15000),
(3, 'Cliente 3', 'Apellido5', 'Apellido6', NULL, 1, 1, 1, NULL, 3, 1, 3, NULL, 30, NULL),
(4, 'Cliente 4', 'Apellido7', 'Apellido8', NULL, 2, 2, 2, NULL, 2, 2, 2, NULL, NULL, NULL);

INSERT INTO estado (id_estado, estado) VALUES
(1, 'En progreso'),
(2, 'Rechazado');

INSERT INTO pedido (codigo_pedido, fecha_pedido, fecha_esperada, fecha_entrega, estado_pedido, comentarios, codigo_cliente_pedido) VALUES
(1001, '2024-01-01', '2024-01-10','2024-01-07' , 1, 'Primer pedido', 1),
(1002, '2009-02-01', '2009-02-10', '2009-02-24', 2, 'Segundo pedido', 2),
(1003, '2024-03-01', '2024-03-10', '2024-03-09', 1, NULL, 3);

INSERT INTO detalle_pedido (codigo_pedido_detalle, codigo_producto_pedido, cantidad, precio_unidad, numero_linea) VALUES
(1001, 1001, 5, 99.99, 1),
(1002, 1002, 3, 119.99, 2),
(1003, 1003, 15, 129.99, 1);

INSERT INTO forma_pago (id_forma_pago, nombre_forma_pago, descripcion_forma_pago) VALUES
(1, 'Tarjeta de Crédito', 'Pago con tarjeta de crédito'),
(2, 'Transferencia Bancaria', 'Pago mediante transferencia bancaria'),
(3, 'Paypal', 'Pago mediante Paypal');

INSERT INTO transaccion (id_transaccion, cod_cliente, forma_pago_trasaccion, fecha_transaccion, total_transaccion) VALUES
('T001', 1, 3, '2008-01-05', 499.95),
('T002', 2, 2, '2008-02-15', 359.97),
('T003', 3, 3, '2008-03-05', 1949.85),
('T004', 3, 1, '2009-04-10', 649.95);

INSERT INTO oficina (codigo_oficina, id_ciudad_oficina, id_region_oficina, id_pais_oficina, id_codigo_postal_oficina, id_telefono_oficina, id_direccion_oficina1, id_direccion_oficina2) VALUES
('O001', 1, 1, 1, 1, 1, 1, NULL),
('O002', 2, 2, 2, 2, 2, 2, NULL),
('O003', 3, 2, 3, 3, 2, 1, NULL);

INSERT INTO empleado (id_empleado, nombre_empleado, apellido1_empleado, apellido2_empleado, extension, email_empleado, codigo_oficina_empleado, codigo_jefe, puesto) VALUES
(1, 'Empleado 1', 'Apellido Empleado 1', 'Apellido Empleado 2', '123', 'empleado1@example.com', 'O001', NULL, 'Vendedor'),
(2, 'Empleado 2', 'Apellido Empleado 3', 'Apellido Empleado 4', '456', 'empleado2@example.com', 'O002', NULL, 'Gerente'),
(3, 'Empleado 3', 'Apellido Empleado 5', 'Apellido Empleado 6', '789', 'empleado3@example.com', 'O003', 2, 'Representante de Ventas'),
(4, 'Empleado 4', 'Apellido Empleado 7', 'Apellido Empleado 8', '101', 'empleado4@example.com', 'O001', 1, 'Gerente'),
(5, 'Empleado 5', 'Apellido Empleado 9', 'Apellido Empleado 10', '112', 'empleado5@example.com', 'O002', 2, 'Representante de Ventas');

```

Consultas sobre una tabla

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

```sql
select o.codigo_oficina, c.ciudad
from oficina as o, ciudad as c
where o.id_ciudad_oficina = c.id_ciudad
+----------------+----------+
| codigo_oficina | ciudad   |
+----------------+----------+
| O001           | Ciudad A |
| O002           | Ciudad B |
| O003           | Madrid   |
+----------------+----------+
```

1. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

```sql
SELECT ciudad.ciudad, telefono.numero
FROM oficina
JOIN ciudad ON oficina.id_ciudad_oficina = ciudad.id_ciudad
JOIN pais ON oficina.id_pais_oficina = pais.id_pais
JOIN telefono ON oficina.id_telefono_oficina = telefono.id_telefono
WHERE pais.pais = 'España';
+--------+-----------+
| ciudad | numero    |
+--------+-----------+
| Madrid | 222222222 |
+--------+-----------+
```

1. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo
jefe tiene un código de jefe igual a 7.

```sql
SELECT nombre_empleado, apellido1_empleado, apellido2_empleado, email_empleado
FROM empleado
WHERE codigo_jefe = 7;
+-----------------+---------------------+----------------------+-----------------------+
| nombre_empleado | apellido1_empleado  | apellido2_empleado   | email_empleado        |
+-----------------+---------------------+----------------------+-----------------------+
| Empleado 1      | Apellido Empleado 1 | Apellido Empleado 2  | empleado1@example.com |
| Empleado 5      | Apellido Empleado 9 | Apellido Empleado 10 | empleado5@example.com |
+-----------------+---------------------+----------------------+-----------------------+
```

1. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la
empresa.

```sql
SELECT puesto,nombre_empleado, apellido1_empleado, apellido2_empleado, email_empleado
FROM empleado
WHERE codigo_jefe IS NULL;
+---------+-----------------+---------------------+---------------------+-----------------------+
| puesto  | nombre_empleado | apellido1_empleado  | apellido2_empleado  | email_empleado        |
+---------+-----------------+---------------------+---------------------+-----------------------+
| Gerente | Empleado 2      | Apellido Empleado 3 | Apellido Empleado 4 | empleado2@example.com |
+---------+-----------------+---------------------+---------------------+-----------------------+
```

1. Devuelve un listado con el nombre, apellidos y puesto de aquellos
empleados que no sean representantes de ventas.

```sql
SELECT nombre_empleado, apellido1_empleado, apellido2_empleado, puesto
FROM empleado
WHERE puesto != 'Representante de Ventas';
+-----------------+---------------------+---------------------+----------+
| nombre_empleado | apellido1_empleado  | apellido2_empleado  | puesto   |
+-----------------+---------------------+---------------------+----------+
| Empleado 1      | Apellido Empleado 1 | Apellido Empleado 2 | Vendedor |
| Empleado 2      | Apellido Empleado 3 | Apellido Empleado 4 | Gerente  |
| Empleado 4      | Apellido Empleado 7 | Apellido Empleado 8 | Gerente  |
+-----------------+---------------------+---------------------+----------+
```

1. Devuelve un listado con el nombre de los todos los clientes españoles.

```sql
SELECT cliente.nombre_cliente
FROM cliente
JOIN pais ON cliente.id_pais_cliente = pais.id_pais
WHERE pais.pais = 'España';
+----------------+
| nombre_cliente |
+----------------+
| Cliente 3      |
+----------------+
```

1. Devuelve un listado con los distintos estados por los que puede pasar un
pedido.

```sql
SELECT estado
FROM estado;
+-------------+
| estado      |
+-------------+
| En progreso |
| Rechazado   |
+-------------+
```

1. Devuelve un listado con el código de cliente de aquellos clientes que
realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar
aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:
• Utilizando la función YEAR de MySQL.

```sql
SELECT DISTINCT cod_cliente
FROM transaccion
WHERE YEAR(fecha_transaccion) = 2008;
+-------------+
| cod_cliente |
+-------------+
|           1 |
|           2 |
+-------------+
```

• Utilizando la función DATE_FORMAT de MySQL.

```sql
SELECT DISTINCT cod_cliente
FROM transaccion
WHERE DATE_FORMAT(fecha_transaccion, '%Y') = '2008';
+-------------+
| cod_cliente |
+-------------+
|           1 |
|           2 |
+-------------+
```

• Sin utilizar ninguna de las funciones anteriores.

```sql
SELECT DISTINCT cod_cliente
FROM transaccion
WHERE fecha_transaccion BETWEEN '2008-01-01' AND '2008-12-31';
+-------------+
| cod_cliente |
+-------------+
|           1 |
|           2 |
+-------------+
```

1. Devuelve un listado con el código de pedido, código de cliente, fecha
esperada y fecha de entrega de los pedidos que no han sido entregados a
tiempo.

```sql
SELECT codigo_pedido, codigo_cliente_pedido, fecha_esperada, fecha_entrega
FROM pedido
WHERE fecha_entrega > fecha_esperada;
+---------------+-----------------------+----------------+---------------+
| codigo_pedido | codigo_cliente_pedido | fecha_esperada | fecha_entrega |
+---------------+-----------------------+----------------+---------------+
|          1002 |                     2 | 2024-02-10     | 2024-02-24    |
+---------------+-----------------------+----------------+---------------+
```

1. Devuelve un listado con el código de pedido, código de cliente, fecha
esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al
menos dos días antes de la fecha esperada.
• Utilizando la función ADDDATE de MySQL.

```sql
SELECT codigo_pedido, codigo_cliente_pedido, fecha_esperada, fecha_entrega
FROM pedido
WHERE fecha_entrega <= ADDDATE(fecha_esperada, INTERVAL -2 DAY);
+---------------+-----------------------+----------------+---------------+
| codigo_pedido | codigo_cliente_pedido | fecha_esperada | fecha_entrega |
+---------------+-----------------------+----------------+---------------+
|          1001 |                     1 | 2024-01-10     | 2024-01-07    |
+---------------+-----------------------+----------------+---------------+
```

• Utilizando la función DATEDIFF de MySQL.

```sql
SELECT codigo_pedido, codigo_cliente_pedido, fecha_esperada, fecha_entrega
FROM pedido
WHERE DATEDIFF(fecha_esperada, fecha_entrega) >= 2;
+---------------+-----------------------+----------------+---------------+
| codigo_pedido | codigo_cliente_pedido | fecha_esperada | fecha_entrega |
+---------------+-----------------------+----------------+---------------+
|          1001 |                     1 | 2024-01-10     | 2024-01-07    |
+---------------+-----------------------+----------------+---------------+
```

• ¿Sería posible resolver esta consulta utilizando el operador de suma + o
resta -?

```sql
SELECT codigo_pedido, codigo_cliente_pedido, fecha_esperada, fecha_entrega
FROM pedido
WHERE fecha_esperada - INTERVAL 2 DAY >= fecha_entrega;
+---------------+-----------------------+----------------+---------------+
| codigo_pedido | codigo_cliente_pedido | fecha_esperada | fecha_entrega |
+---------------+-----------------------+----------------+---------------+
|          1001 |                     1 | 2024-01-10     | 2024-01-07    |
+---------------+-----------------------+----------------+---------------+
```

1. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

```sql
SELECT p.codigo_pedido
FROM pedido as p
WHERE p.fecha_pedido BETWEEN '2009-01-01' AND '2009-12-31'
  AND p.estado_pedido = (
    SELECT id_estado
    FROM estado
    WHERE estado = 'Rechazado'
  );
 +---------------+
| codigo_pedido |
+---------------+
|          1002 |
+---------------+
```

1. Devuelve un listado de todos los pedidos que han sido entregados en el
mes de enero de cualquier año.

```sql
SELECT codigo_pedido
FROM pedido
WHERE MONTH(fecha_entrega) = 1;
+---------------+
| codigo_pedido |
+---------------+
|          1001 |
+---------------+
```

1. Devuelve un listado con todos los pagos que se realizaron en el
año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

```sql
SELECT t.id_transaccion
FROM transaccion as t
WHERE t.fecha_transaccion BETWEEN '2008-01-01' AND '2008-12-31' 
  AND t.forma_pago_trasaccion = (
  SELECT f.id_forma_pago
  FROM forma_pago as f
  WHERE f.nombre_forma_pago = 'Paypal'
  )
ORDER BY total_transaccion DESC;
+----------------+
| id_transaccion |
+----------------+
| T003           |
| T001           |
+----------------+
```

1. Devuelve un listado con todas las formas de pago que aparecen en la
tabla pago. Tenga en cuenta que no deben aparecer formas de pago
repetidas.

```sql
SELECT DISTINCT nombre_forma_pago 
FROM forma_pago;
+------------------------+
| nombre_forma_pago      |
+------------------------+
| Tarjeta de Crédito     |
| Transferencia Bancaria |
| Paypal                 |
+------------------------+
```

1. Devuelve un listado con todos los productos que pertenecen a la
gama Ornamentales y que tienen más de 100 unidades en stock. El listado
deberá estar ordenado por su precio de venta, mostrando en primer lugar
los de mayor precio.

```sql
SELECT p.nombre, p.precio_venta
FROM producto as p
WHERE p.gama_producto = (
  SELECT g.id_gama
  FROM gama as g
  WHERE g.descripcion_texto = 'Ornamentales'
  )
  AND p.cantidad_stock = (
  SELECT s.id_stock
  FROM stock as s
  WHERE s.cantidad_producto > 100
  )
ORDER BY precio_venta DESC;
+------------+--------------+
| nombre     | precio_venta |
+------------+--------------+
| Producto 3 |       129.99 |
+------------+--------------+
```

1. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y
cuyo representante de ventas tenga el código de empleado 11 o 30.

```sql
SELECT nombre_cliente
FROM cliente
WHERE id_ciudad_cliente = (SELECT id_ciudad FROM ciudad WHERE ciudad = 'Madrid') 
AND (codigo_empleado_rep_ventas = 11 OR codigo_empleado_rep_ventas = 30);
+----------------+
| nombre_cliente |
+----------------+
| Cliente 2      |
| Cliente 3      |
+----------------+
```
![Diagrama en blanco](https://github.com/dvn495/GARDEN-DATABASE/assets/152308538/755c7971-4cc1-4c4e-aa8a-25810de2f313)


