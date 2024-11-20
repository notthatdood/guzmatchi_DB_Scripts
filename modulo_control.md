Módulo de Control Subsecuente

Descripción
El Módulo de Control Subsecuente permite gestionar las citas de seguimiento de las pacientes después de su primera consulta. Facilita la programación de citas futuras, el registro de detalles de cada consulta, el manejo de pagos y el envío de recordatorios a las pacientes.

Tablas del Módulo
1. Tablas de Apoyo
categorias_servicio: Clasifica los servicios en categorías específicas como "Consulta ginecología" y "Consulta obstetricia".
servicios: Registra los servicios específicos que ofrece la clínica, asociados a una categoría.
estados_cita: Define los posibles estados de una cita (Programada, Completada, Cancelada, No Asistió).
formas_pago: Especifica las formas de pago aceptadas (Efectivo, Tarjeta, SINPE).
medios_recordatorio: Define los medios por los cuales se envían recordatorios (SMS, Email, Llamada Telefónica, WhatsApp).
estados_recordatorio: Especifica los posibles estados de un recordatorio (Pendiente, Enviado, Confirmado, Cancelado).

2. Tablas Principales
controles_subsecuentes: Registra los controles subsecuentes (citas) de las pacientes, incluyendo detalles como fecha, servicio, monto y estado.
recordatorios: Gestiona los recordatorios enviados a las pacientes sobre sus citas, incluyendo detalles como fecha, medio y estado.

Tablas
1. Tablas de Apoyo

Tabla categorias_servicio
CREATE TABLE categorias_servicio (
    id_categoria_servicio SERIAL PRIMARY KEY,
    nombre_categoria_servicio VARCHAR(50) UNIQUE NOT NULL
);

Tabla servicios
CREATE TABLE servicios (
    id_servicio SERIAL PRIMARY KEY,
    id_categoria_servicio INTEGER REFERENCES categorias_servicio(id_categoria_servicio) ON DELETE SET NULL,
    nombre_servicio VARCHAR(100) NOT NULL,
    UNIQUE(id_categoria_servicio, nombre_servicio)
);

Tabla estados_cita
CREATE TABLE estados_cita (
    id_estado_cita SERIAL PRIMARY KEY,
    nombre_estado_cita VARCHAR(50) UNIQUE NOT NULL
);

Tabla formas_pago
CREATE TABLE formas_pago (
    id_forma_pago SERIAL PRIMARY KEY,
    nombre_forma_pago VARCHAR(50) UNIQUE NOT NULL
);

Tabla medios_recordatorio
CREATE TABLE medios_recordatorio (
    id_medio_recordatorio SERIAL PRIMARY KEY,
    nombre_medio_recordatorio VARCHAR(50) UNIQUE NOT NULL
);

Tabla estados_recordatorio
CREATE TABLE estados_recordatorio (
    id_estado_recordatorio SERIAL PRIMARY KEY,
    nombre_estado_recordatorio VARCHAR(50) UNIQUE NOT NULL
);

2. Tablas Principales

Tabla controles_subsecuentes
CREATE TABLE controles_subsecuentes (
    id_control_subsecuente SERIAL PRIMARY KEY,
    id_paciente INTEGER REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    id_doctor INTEGER REFERENCES usuarios(id_usuario) ON DELETE SET NULL,
    id_servicio INTEGER REFERENCES servicios(id_servicio) ON DELETE SET NULL,
    fecha_hora_control TIMESTAMP NOT NULL,
    motivo_consulta_control TEXT,
    observaciones_control TEXT,
    indicaciones_control TEXT,
    monto_control DECIMAL(10,2),
    id_forma_pago INTEGER REFERENCES formas_pago(id_forma_pago) ON DELETE SET NULL,
    id_estado_cita INTEGER REFERENCES estados_cita(id_estado_cita) ON DELETE SET NULL,
    fecha_creacion_control TIMESTAMP DEFAULT NOW()
);

Tabla recordatorios
CREATE TABLE recordatorios (
    id_recordatorio SERIAL PRIMARY KEY,
    id_paciente INTEGER REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    id_control_subsecuente INTEGER REFERENCES controles_subsecuentes(id_control_subsecuente) ON DELETE SET NULL,
    fecha_hora_recordatorio TIMESTAMP NOT NULL,
    id_medio_recordatorio INTEGER REFERENCES medios_recordatorio(id_medio_recordatorio) ON DELETE SET NULL,
    id_estado_recordatorio INTEGER REFERENCES estados_recordatorio(id_estado_recordatorio) ON DELETE SET NULL,
    comentario_recordatorio TEXT,
    fecha_creacion_recordatorio TIMESTAMP DEFAULT NOW()
);

Ejemplos de Población de Datos
1. Población de Tablas de Apoyo

Tabla categorias_servicio
INSERT INTO categorias_servicio (nombre_categoria_servicio) VALUES
('Consulta ginecología'),
('Consulta obstetricia');

Tabla servicios
Servicios para "Consulta ginecología" (id_categoria_servicio = 1):
INSERT INTO servicios (id_categoria_servicio, nombre_servicio) VALUES
(1, 'Atención, mantenimiento y prevención de la salud reproductiva'),
(1, 'Atención de la menopausia y climaterio'),
(1, 'Atención de patología cervical'),
(1, 'Anticoncepción asistida'),
(1, 'Ginecología general, estudios colposcópicos');

Servicios para "Consulta obstetricia" (id_categoria_servicio = 2):
INSERT INTO servicios (id_categoria_servicio, nombre_servicio) VALUES
(2, 'Control prenatal'),
(2, 'Ultrasonido obstétrico');

Tabla estados_cita
INSERT INTO estados_cita (nombre_estado_cita) VALUES
('Programada'),
('Completada'),
('Cancelada'),
('No Asistió');

Tabla formas_pago
INSERT INTO formas_pago (nombre_forma_pago) VALUES
('Efectivo'),
('Tarjeta'),
('SINPE');

Tabla medios_recordatorio
INSERT INTO medios_recordatorio (nombre_medio_recordatorio) VALUES
('SMS'),
('Email'),
('Llamada Telefónica'),
('WhatsApp');

Tabla estados_recordatorio
INSERT INTO estados_recordatorio (nombre_estado_recordatorio) VALUES
('Pendiente'),
('Confirmado'),
('Cancelado');

2. Caso de Uso: Programación y Registro de un Control Subsecuente

Paciente: María Pérez Rodríguez (id_paciente = 1)
Doctor: Ana Guzmán Hidalgo (id_usuario = 1)

Paso 1: Programación de la Cita Futura
Tabla controles_subsecuentes

Programar una cita para dentro de dos semanas:
INSERT INTO controles_subsecuentes (
    id_paciente,
    id_doctor,
    id_servicio,
    fecha_hora_control,
    id_estado_cita
)
VALUES (
    1,  -- ID de la paciente María Pérez
    1,  -- ID de la doctora Ana Guzmán
    3,  -- Servicio 'Atención de patología cervical' (id_servicio = 3)
    '2023-09-15 10:00:00',
    1   -- Estado 'Programada'
);

Obtenemos el id_control_subsecuente generado (por ejemplo, 1).

Paso 2: Envío de Recordatorio
Tabla recordatorios

Enviamos un recordatorio un día antes de la cita:
INSERT INTO recordatorios (
    id_paciente,
    id_control_subsecuente,
    fecha_hora_recordatorio,
    id_medio_recordatorio,
    id_estado_recordatorio,
    comentario_recordatorio
)
VALUES (
    1,  -- ID de la paciente
    1,  -- ID del control subsecuente
    '2023-09-14 09:00:00',
    4,  -- Medio 'WhatsApp' (id_medio_recordatorio = 4)
    2,  -- Estado 'Enviado' (id_estado_recordatorio = 2)
    'Recordatorio de su cita para atención de patología cervical mañana a las 10:00 AM.'
);

Paso 3: Realización del Control y Registro de Detalles

Después de la consulta, la doctora registra los detalles:
UPDATE controles_subsecuentes
SET
    motivo_consulta_control = 'Seguimiento de dolor pélvico',
    observaciones_control = 'Mejora significativa, se observa reducción de síntomas.',
    indicaciones_control = 'Continuar con el tratamiento y volver en un mes.',
    monto_control = 30000.00,  -- Monto en colones
    id_forma_pago = 1,         -- Forma de pago 'Efectivo'
    id_estado_cita = 2         -- Estado 'Completada'
WHERE
    id_control_subsecuente = 1;