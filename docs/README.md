# Proyecto: Base de Datos para Control de Ventas en un Centro Recreacional

## Descripción del Proyecto

Este proyecto implementa una base de datos relacional en **MySQL** para gestionar el control de ventas de un centro recreacional. La base de datos permite almacenar información sobre clientes, empleados, productos, ventas, y detalles de las ventas realizadas, facilitando el seguimiento y análisis de las actividades de ventas dentro del centro.

Se han utilizado los principales conceptos de bases de datos relacionales, como la creación de tablas con relaciones de claves primarias y foráneas, manipulación de datos (DML), y consultas avanzadas para generar informes y métricas de rendimiento.

## Estructura de la Base de Datos

La base de datos cuenta con las siguientes tablas:
1. **Clientes**: Información de los clientes registrados.
2. **Empleados**: Datos de los empleados.
3. **Productos**: Detalles de los productos y servicios disponibles.
4. **Ventas**: Registros de las ventas realizadas, conectando clientes y empleados.
5. **DetallesVenta**: Información sobre los productos vendidos en cada venta.

## Requisitos

Antes de comenzar, asegúrate de tener lo siguiente:
- **MySQL** instalado en tu sistema.
- **SQLyog** o cualquier otro cliente MySQL para ejecutar las consultas SQL.
- Acceso a un servidor MySQL o MySQL local instalado.

## Restaurar la Base de Datos

### Paso 1: Crear la base de datos y las tablas

1. Abre **SQLyog** o cualquier cliente MySQL de tu elección.
2. Copia y pega el siguiente código SQL para crear la base de datos, las tablas y poblarlas con datos de ejemplo.

```sql
CREATE DATABASE IF NOT EXISTS CentroRecreacional;
USE CentroRecreacional;

-- Tabla Clientes
CREATE TABLE Clientes (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    email VARCHAR(150),
    telefono VARCHAR(20),
    fecha_registro DATE DEFAULT CURRENT_DATE
);

-- Tabla Empleados
CREATE TABLE Empleados (
    id_empleado INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    cargo VARCHAR(50),
    salario DECIMAL(10, 2),
    fecha_contratacion DATE DEFAULT CURRENT_DATE
);

-- Tabla Productos
CREATE TABLE Productos (
    id_producto INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    precio DECIMAL(10, 2) NOT NULL,
    stock INT NOT NULL
);

-- Tabla Ventas
CREATE TABLE Ventas (
    id_venta INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT,
    id_empleado INT,
    fecha_venta DATETIME DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10, 2),
    FOREIGN KEY (id_cliente) REFERENCES Clientes(id_cliente),
    FOREIGN KEY (id_empleado) REFERENCES Empleados(id_empleado)
);

-- Tabla DetallesVenta
CREATE TABLE DetallesVenta (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_venta INT,
    id_producto INT,
    cantidad INT NOT NULL,
    precio_unitario DECIMAL(10, 2),
    subtotal DECIMAL(10, 2),
    FOREIGN KEY (id_venta) REFERENCES Ventas(id_venta),
    FOREIGN KEY (id_producto) REFERENCES Productos(id_producto)
);

-- Insertar Clientes
INSERT INTO Clientes (nombre, apellido, email, telefono) VALUES
('Juan', 'Pérez', 'juan.perez@email.com', '123456789'),
('Ana', 'González', 'ana.gonzalez@email.com', '987654321'),
('Pedro', 'Ramírez', 'pedro.ramirez@email.com', '555123456'),
('Laura', 'Torres', 'laura.torres@email.com', '444987654');

-- Insertar Empleados
INSERT INTO Empleados (nombre, apellido, cargo, salario) VALUES
('Luis', 'Martínez', 'Cajero', 1500.00),
('María', 'López', 'Vendedora', 1300.00),
('Carlos', 'Gómez', 'Supervisor', 2000.00),
('Sofía', 'Reyes', 'Recepcionista', 1200.00);

-- Insertar Productos
INSERT INTO Productos (nombre, descripcion, precio, stock) VALUES
('Entrada General', 'Entrada al parque recreacional', 10.00, 100),
('Alquiler de Kayak', 'Alquiler de kayak por 1 hora', 20.00, 50),
('Bebida', 'Bebida refrescante', 3.00, 200),
('Alquiler de Bicicleta', 'Alquiler de bicicleta por 1 hora', 15.00, 30),
('Comida', 'Comida rápida', 7.50, 150);

-- Insertar Ventas
INSERT INTO Ventas (id_cliente, id_empleado, total) VALUES
(1, 1, 30.00),
(2, 2, 13.00),
(3, 3, 40.00),
(4, 1, 25.00);

-- Insertar Detalles de Venta
INSERT INTO DetallesVenta (id_venta, id_producto, cantidad, precio_unitario, subtotal) VALUES
(1, 1, 2, 10.00, 20.00),
(1, 3, 2, 3.00, 6.00),
(1, 5, 1, 7.50, 7.50),
(2, 1, 1, 10.00, 10.00),
(2, 3, 1, 3.00, 3.00),
(3, 2, 2, 20.00, 40.00),
(4, 4, 1, 15.00, 15.00),
(4, 3, 2, 3.00, 6.00),
(4, 5, 1, 7.50, 7.50);