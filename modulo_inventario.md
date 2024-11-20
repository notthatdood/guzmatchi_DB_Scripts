Módulo de Inventario

Descripción
El Módulo de Inventario permite gestionar de manera eficiente los suministros y materiales utilizados en la clínica. Facilita el control de entradas y salidas de inventario, mantiene actualizados los niveles de stock y ayuda a prever necesidades futuras, asegurando que siempre haya suficientes suministros para atender a los pacientes.

Tablas del Módulo
1. Tablas de Apoyo
categorias_suministro: Clasifica los suministros en categorías como Consultorio, Oficina, Limpieza, etc.
tipos_suministro: Define el tipo de suministro según su naturaleza (Consumible sin estado, Consumible con estado, Muestra/Regalía).
unidad_medida: Especifica las unidades de medida utilizadas (Unidades, Botellas, Cajas, Rollos, etc.).
tipos_entidad: Clasifica las entidades en Proveedor, Empresa (Muestra/Regalía) y Compra Directa.
motivo_entrada: Define los motivos por los cuales ingresan suministros (Compra, Muestra/Regalía).
motivo_salida: Define los motivos por los cuales salen suministros (Uso, Ajuste).

2. Tablas Principales
suministros: Registra información de cada suministro disponible en la clínica.
entidades: Almacena información de proveedores, empresas de muestras y lugares de compra directa.
suministro_entidad: Relaciona los suministros con las entidades que los proveen, indicando si es el proveedor favorito.
entradas_suministro: Registra las entradas de suministros al inventario, incluyendo detalles como cantidad, motivo y proveedor.
salidas_suministro: Registra las salidas de suministros del inventario, incluyendo detalles como cantidad, motivo y justificación.

Tablas
1. Tablas de Apoyo

Tabla categorias_suministro
CREATE TABLE categorias_suministro (
    id_categoria_suministro SERIAL PRIMARY KEY,
    nombre_categoria_suministro VARCHAR(50) UNIQUE NOT NULL
);

Tabla tipos_suministro
CREATE TABLE tipos_suministro (
    id_tipo_suministro SERIAL PRIMARY KEY,
    nombre_tipo_suministro VARCHAR(50) UNIQUE NOT NULL
);

Tabla unidad_medida
CREATE TABLE unidad_medida (
    id_unidad_medida SERIAL PRIMARY KEY,
    nombre_unidad VARCHAR(50) UNIQUE NOT NULL
);

Tabla tipos_entidad
CREATE TABLE tipos_entidad (
    id_tipo_entidad SERIAL PRIMARY KEY,
    nombre_tipo_entidad VARCHAR(50) UNIQUE NOT NULL
);

Tabla motivo_entrada
CREATE TABLE motivo_entrada (
    id_motivo_entrada SERIAL PRIMARY KEY,
    nombre_motivo_entrada VARCHAR(50) UNIQUE NOT NULL
);

Tabla motivo_salida
CREATE TABLE motivo_salida (
    id_motivo_salida SERIAL PRIMARY KEY,
    nombre_motivo_salida VARCHAR(50) UNIQUE NOT NULL
);

2. Tablas Principales

Tabla suministros
CREATE TABLE suministros (
    id_suministro SERIAL PRIMARY KEY,
    nombre_suministro VARCHAR(100) NOT NULL,
    id_categoria_suministro INTEGER REFERENCES categorias_suministro(id_categoria_suministro) ON DELETE SET NULL,
    id_tipo_suministro INTEGER REFERENCES tipos_suministro(id_tipo_suministro) ON DELETE SET NULL,
    id_unidad_medida INTEGER REFERENCES unidad_medida(id_unidad_medida) ON DELETE SET NULL,
    cantidad_disponible NUMERIC(10,2) DEFAULT 0,
    stock_minimo NUMERIC(10,2) DEFAULT 0,
    fecha_ultima_actualizacion TIMESTAMP DEFAULT NOW(),
    nota_suministro TEXT
);

Tabla entidades
CREATE TABLE entidades (
    id_entidad SERIAL PRIMARY KEY,
    nombre_entidad VARCHAR(100) NOT NULL,
    id_tipo_entidad INTEGER REFERENCES tipos_entidad(id_tipo_entidad) ON DELETE SET NULL,
    telefono_entidad VARCHAR(20),
    email_entidad VARCHAR(100),
    direccion_entidad TEXT,
    nota_entidad TEXT
);

Tabla suministro_entidad
CREATE TABLE suministro_entidad (
    id_suministro_entidad SERIAL PRIMARY KEY,
    id_suministro INTEGER REFERENCES suministros(id_suministro) ON DELETE CASCADE,
    id_entidad INTEGER REFERENCES entidades(id_entidad) ON DELETE CASCADE,
    entidad_favorita BOOLEAN DEFAULT FALSE
);

Tabla entradas_suministro
CREATE TABLE entradas_suministro (
    id_entrada_suministro SERIAL PRIMARY KEY,
    id_suministro INTEGER REFERENCES suministros(id_suministro) ON DELETE CASCADE,
    cantidad_entrada NUMERIC(10,2) NOT NULL,
    fecha_entrada TIMESTAMP DEFAULT NOW(),
    id_motivo_entrada INTEGER REFERENCES motivo_entrada(id_motivo_entrada) ON DELETE SET NULL,
    id_entidad INTEGER REFERENCES entidades(id_entidad) ON DELETE SET NULL,
    justificacion_entrada TEXT,
    observacion_entrada TEXT
);

Tabla salidas_suministro
CREATE TABLE salidas_suministro (
    id_salida_suministro SERIAL PRIMARY KEY,
    id_suministro INTEGER REFERENCES suministros(id_suministro) ON DELETE CASCADE,
    cantidad_salida NUMERIC(10,2) NOT NULL,
    fecha_salida TIMESTAMP DEFAULT NOW(),
    id_motivo_salida INTEGER REFERENCES motivo_salida(id_motivo_salida) ON DELETE SET NULL,
    justificacion_salida TEXT,
    observacion_salida TEXT
);


Ejemplos de Población de Datos
1. Población de Tablas de Apoyo

Tabla categorias_suministro
INSERT INTO categorias_suministro (nombre_categoria_suministro) VALUES
('Consultorio'),
('Oficina'),
('Limpieza');

Tabla tipos_suministro
INSERT INTO tipos_suministro (nombre_tipo_suministro) VALUES
('Consumible sin estado'),
('Consumible con estado'),
('Muestra/Regalía');

Tabla unidad_medida
INSERT INTO unidad_medida (nombre_unidad) VALUES
('Unidades'),
('Botellas'),
('Cajas'),
('Rollos')
('Galones')
('Bolsas')
('Frascos');

Tabla tipos_entidad
INSERT INTO tipos_entidad (nombre_tipo_entidad) VALUES
('Proveedor'),
('Muestra/Regalía'),
('Compra Directa');

Tabla motivo_entrada
INSERT INTO motivo_entrada (nombre_motivo_entrada) VALUES
('Compra'),
('Muestra/Regalía');

Tabla motivo_salida
INSERT INTO motivo_salida (nombre_motivo_salida) VALUES
('Uso'),
('Ajuste');

2. Registro de Entidades
Tabla entidades

Registro de un proveedor:
INSERT INTO entidades (
    nombre_entidad,
    id_tipo_entidad,
    telefono_entidad,
    email_entidad,
    direccion_entidad
)
VALUES (
    'Proveedor XYZ',
    1,  -- Proveedor
    '2222-3333',
    'contacto@proveedorxyz.com',
    'Avenida Central, Edificio 5'
);

Registro de una empresa de muestras:
INSERT INTO entidades (
    nombre_entidad,
    id_tipo_entidad,
    telefono_entidad,
    email_entidad,
    direccion_entidad
)
VALUES (
    'Laboratorios ABC',
    2,  -- (Muestra/Regalía)
    '4444-5555',
    'info@lababc.com',
    'Calle 10, Zona Industrial'
);

Registro de un lugar de compra directa:
INSERT INTO entidades (
    nombre_entidad,
    id_tipo_entidad
)
VALUES (
    'Supermercado La Favorita',
    3  -- Compra Directa
);

3. Registro de Suministros
Tabla suministros

Registrar un nuevo suministro:
INSERT INTO suministros (
    nombre_suministro,
    id_categoria_suministro,
    id_tipo_suministro,
    id_unidad_medida,
    stock_minimo
)
VALUES (
    'Cloro',
    3,  -- Limpieza
    1,  -- Consumible sin estado
    2,  -- Botellas
    5   -- Stock mínimo
);

4. Relacionar Suministros con Entidades
Tabla suministro_entidad

Relacionar el suministro con un proveedor:
INSERT INTO suministro_entidad (
    id_suministro,
    id_entidad,
    entidad_favorita
)
VALUES (
    1,  -- ID del suministro 'Cloro'
    1,  -- ID de 'Proveedor XYZ'
    TRUE -- Es el proveedor favorito de cloro
);

5. Registrar Entrada de Suministro
Tabla entradas_suministro

Entrada por compra a proveedor:
INSERT INTO entradas_suministro (
    id_suministro,
    cantidad_entrada,
    id_motivo_entrada,
    id_entidad,
    justificacion_entrada
)
VALUES (
    1,     -- ID del suministro 'Cloro'
    20,    -- Cantidad de entrada
    1,     -- Motivo 'Compra'
    1,     -- ID de 'Proveedor XYZ'
    'Compra mensual de cloro para limpieza general'
);


6. Registrar Salida de Suministro
Tabla salidas_suministro

Salida por uso en la clínica:
INSERT INTO salidas_suministro (
    id_suministro,
    cantidad_salida,
    id_motivo_salida,
    justificacion_salida
)
VALUES (
    1,     -- ID del suministro 'Cloro'
    5,     -- Cantidad de salida
    1,     -- Motivo 'Uso'
    'Limpieza semanal de áreas comunes'
);

7.Funciones, Vistas e Índices

1. Vista de Alertas de Bajo Stock en el Módulo de Inventario
CREATE OR REPLACE VIEW alertas_bajo_stock AS
SELECT
    s.id_suministro,
    s.nombre_suministro,
    s.cantidad_disponible,
    s.stock_minimo
FROM
    suministros s
WHERE
    s.cantidad_disponible <= s.stock_minimo;

1. Función y Trigger para Entradas de Suministro

Función actualizar_stock_entrada
Esta función se ejecuta cada vez que se inserta un nuevo registro en entradas_suministro. Su objetivo es sumar la cantidad de entrada al cantidad_disponible del suministro correspondiente.

CREATE OR REPLACE FUNCTION actualizar_stock_entrada()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE suministros
    SET cantidad_disponible = cantidad_disponible + NEW.cantidad_entrada,
        fecha_ultima_actualizacion = NOW()
    WHERE id_suministro = NEW.id_suministro;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

Trigger trg_actualizar_stock_entrada
Este trigger se asocia con la tabla entradas_suministro y se ejecuta después de insertar un nuevo registro.

CREATE TRIGGER trg_actualizar_stock_entrada
AFTER INSERT ON entradas_suministro
FOR EACH ROW
EXECUTE FUNCTION actualizar_stock_entrada();

Cómo funciona:
Cuando se inserta un nuevo registro en entradas_suministro, el trigger trg_actualizar_stock_entrada se activa.
El trigger llama a la función actualizar_stock_entrada.
La función actualiza el cantidad_disponible en la tabla suministros, sumando NEW.cantidad_entrada al valor actual.
También actualiza fecha_ultima_actualizacion a la fecha y hora actuales.

3. Función y Trigger para Salidas de Suministro

Función actualizar_stock_salida
Esta función se ejecuta cada vez que se inserta un nuevo registro en salidas_suministro. Su objetivo es restar la cantidad de salida del cantidad_disponible del suministro correspondiente.

CREATE OR REPLACE FUNCTION actualizar_stock_salida()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE suministros
    SET cantidad_disponible = cantidad_disponible - NEW.cantidad_salida,
        fecha_ultima_actualizacion = NOW()
    WHERE id_suministro = NEW.id_suministro;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

Trigger trg_actualizar_stock_salida
Este trigger se asocia con la tabla salidas_suministro y se ejecuta después de insertar un nuevo registro.

CREATE TRIGGER trg_actualizar_stock_salida
AFTER INSERT ON salidas_suministro
FOR EACH ROW
EXECUTE FUNCTION actualizar_stock_salida();

Cómo funciona:
Cuando se inserta un nuevo registro en salidas_suministro, el trigger trg_actualizar_stock_salida se activa.
El trigger llama a la función actualizar_stock_salida.
La función actualiza el cantidad_disponible en la tabla suministros, restando NEW.cantidad_salida del valor actual.
También actualiza fecha_ultima_actualizacion a la fecha y hora actuales.


4. Consulta de Movimientos de Suministro
Ejemplo de consulta para obtener los movimientos de un suministro específico en un rango de fechas:
SELECT
    s.nombre_suministro,
    e.cantidad_entrada,
    e.fecha_entrada,
    me.nombre_motivo_entrada AS motivo_entrada,
    e.justificacion_entrada,
    e.observacion_entrada,
    sa.cantidad_salida,
    sa.fecha_salida,
    ms.nombre_motivo_salida AS motivo_salida,
    sa.justificacion_salida,
    sa.observacion_salida
FROM
    suministros s
LEFT JOIN entradas_suministro e ON s.id_suministro = e.id_suministro
LEFT JOIN motivo_entrada me ON e.id_motivo_entrada = me.id_motivo_entrada
LEFT JOIN salidas_suministro sa ON s.id_suministro = sa.id_suministro
LEFT JOIN motivo_salida ms ON sa.id_motivo_salida = ms.id_motivo_salida
WHERE
    s.nombre_suministro = 'Cloro' AND
    (e.fecha_entrada BETWEEN '2023-08-01' AND '2023-08-31' OR
     sa.fecha_salida BETWEEN '2023-08-01' AND '2023-08-31');

