Módulo de Usuarios

Descripción
El Módulo de Usuarios administra la información de los usuarios que tienen acceso al sistema, incluyendo sus datos personales, credenciales y roles asignados. Esto asegura un control adecuado sobre quién puede acceder y qué acciones puede realizar dentro del sistema.

Tablas del Módulo
1. Tablas de Apoyo
roles_usuario: Define los diferentes roles que pueden tener los usuarios.

2. Tablas Principales
usuarios: Registra la información personal y de acceso de cada usuario.

Tablas
1. Tablas de Apoyo

Tabla roles_usuario
CREATE TABLE roles_usuario (
    id_rol_usuario SERIAL PRIMARY KEY,
    nombre_rol_usuario VARCHAR(50) UNIQUE NOT NULL
);

2. Tablas Principales
Tabla usuarios:
CREATE TABLE usuarios (
    id_usuario SERIAL PRIMARY KEY,
    nombre_usuario VARCHAR(50) NOT NULL,
    primer_apellido_usuario VARCHAR(50) NOT NULL,
    segundo_apellido_usuario VARCHAR(50),
    correo_electronico_usuario VARCHAR(100) UNIQUE NOT NULL,
    contrasena_usuario VARCHAR(255) NOT NULL,
    id_rol_usuario INTEGER REFERENCES roles_usuario(id_rol_usuario) ON DELETE SET NULL,
    activo_usuario BOOLEAN DEFAULT TRUE
);

Ejemplos de Población de Datos
1. Población de Tablas de Apoyo

Tabla roles_usuario
INSERT INTO roles_usuario (nombre_rol_usuario) VALUES
('Administrador'),
('Doctor'),
('Asistente');

2. Registro de Usuarios
Registrar usuarios:

Registrar a usuarios:
-- Registrar a la doctora Ana Guzmán (Administrador)
INSERT INTO usuarios (
    nombre_usuario,
    primer_apellido_usuario,
    segundo_apellido_usuario,
    correo_electronico_usuario,
    contrasena_usuario,
    id_rol_usuario
)
VALUES (
    'Ana',
    'Guzmán',
    'Hidalgo',
    'ana.guzman@clinicagh.com',
    'hashed_password_ana',  -- Reemplazar con la contraseña hasheada
    1  -- Administrador
);

-- Registrar a la asistente Fabiola Guzmán (Asistente)
INSERT INTO usuarios (
    nombre_usuario,
    primer_apellido_usuario,
    segundo_apellido_usuario,
    correo_electronico_usuario,
    contrasena_usuario,
    id_rol_usuario
)
VALUES (
    'Fabiola',
    'Guzmán',
    'Moreno',
    'fabiola.guzman@clinicagh.com',
    'hashed_password_fabiola',  -- Reemplazar con la contraseña hasheada
    3  -- Asistente
);


