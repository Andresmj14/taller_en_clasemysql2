# Funciones SQL – Casos de Uso

## Caso 1: Cálculo de Bonificaciones de Empleados

```sql
DELIMITER //

CREATE FUNCTION calcular_bonificacion(salario DECIMAL(10,2)) 
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    DECLARE bonificacion DECIMAL(10,2);

    IF salario < 2000 THEN
        SET bonificacion = salario * 0.10;
    ELSEIF salario >= 200 AND salario <= 5000 THEN
        SET bonificacion = salario * 0.07;
    ELSE
        SET bonificacion = salario * 0.05;
    END IF;

    RETURN bonificacion;
END //

DELIMITER ;

SELECT nombre, calcular_bonificacion(salario) AS bonificacion
FROM empleados;
```

---

## Caso 2: Cálculo de Edad de Clientes

```sql
DELIMITER //

CREATE FUNCTION calcular_edad(fecha_nacimiento DATE) 
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN TIMESTAMPDIFF(YEAR, fecha_nacimiento, CURDATE());
END //

DELIMITER ;

SELECT nombre, calcular_edad(fecha_nacimiento) AS edad 
FROM clientes;
```

---

## Caso 3: Formatear Números de Teléfono

```sql
CREATE TABLE telefonos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    telefonos VARCHAR(50)
);

INSERT INTO telefonos(nombre, telefonos) 
VALUES ('Andres', '3014848690');

DELIMITER //

CREATE FUNCTION formatear_telefono(telefonos_usuario VARCHAR(50)) 
RETURNS VARCHAR(50)
DETERMINISTIC
BEGIN
    DECLARE formateo VARCHAR(50);

    SET formateo = CONCAT(
        '(', SUBSTRING(telefonos_usuario, 1, 3), ') ',
        SUBSTRING(telefonos_usuario, 4, 3), '-',
        SUBSTRING(telefonos_usuario, 7, 4)
    );

    RETURN formateo;
END //

DELIMITER ;

SELECT nombre, formatear_telefono(telefonos) AS formateado
FROM telefonos;
```

---

## Caso 4: Clasificación de Productos por Precio

```sql
DELIMITER //

CREATE FUNCTION clasificar_precio(precio DECIMAL(10,2)) 
RETURNS VARCHAR(50)
DETERMINISTIC
BEGIN
    DECLARE precios VARCHAR(50);

    IF precio < 50 THEN
        SET precios = 'BAJO';
    ELSEIF precio >= 50 AND precio <= 200 THEN
        SET precios = 'MEDIO';
    ELSE
        SET precios = 'ALTO';
    END IF;

    RETURN precios;
END //

DELIMITER ;

SELECT descripcion_p, clasificar_precio(precios) AS clasificacion
FROM productos;
```
