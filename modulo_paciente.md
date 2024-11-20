Módulo de Pacientes

Descripción
El Módulo de Pacientes gestiona la información de las pacientes que asisten a la clínica. Incluye datos personales, antecedentes médicos y ginecológicos, resultados de exámenes físicos y ginecológicos, así como el plan de seguimiento. Este módulo permite a la doctora llevar un registro completo y detallado de cada paciente.

Tablas del Módulo
1. Tablas de Apoyo
estado_civil: Clasifica el estado civil de las pacientes (Soltera, Casada, Divorciada, etc.).
cantidad_sangrado_menstrual: Especifica la cantidad de sangrado menstrual (Escaso, Moderado, Abundante).
orientacion_sexual: Define la orientación sexual de las pacientes (Heterosexual, Homosexual, Bisexual, Otro).
numero_parejas_sexuales: Clasifica el número de parejas sexuales (Menos de tres, Tres, Más de tres).

2. Tablas Principales
pacientes: Registra la información personal y de contacto de las pacientes.
antecedentes_patologicos: Registra los antecedentes patológicos personales importantes de la paciente.
antecedentes_heredofamiliares: Registra los antecedentes heredofamiliares relevantes de la paciente.
antecedentes_quirurgicos: Almacena información sobre cirugías previas de la paciente.
ciclo_menstrual: Registra detalles sobre el ciclo menstrual de la paciente.
actividad_sexual: Almacena información sobre la actividad sexual de la paciente.
historial_obstetrio: Registra el historial obstétrico de la paciente, incluyendo embarazos y partos.
examenes_preventivos: Almacena las fechas de exámenes preventivos realizados por la paciente.
motivo_consulta: Registra el motivo de consulta de la paciente.
examen_fisico: Almacena los resultados del examen físico general realizado a la paciente.
examen_ginecologico: Registra los hallazgos del examen ginecológico de la paciente.
resumen_lista_problemas: Contiene un resumen y lista de problemas identificados en la paciente.
plan_seguimiento: Almacena el plan de seguimiento y tratamiento para la paciente.

Tablas
1. Tablas de Apoyo

Tabla estado_civil
CREATE TABLE estado_civil (
    id_estado_civil SERIAL PRIMARY KEY,
    nombre_estado_civil VARCHAR(50) UNIQUE NOT NULL
);

Tabla cantidad_sangrado_menstrual
CREATE TABLE cantidad_sangrado_menstrual (
    id_cantidad_sangrado SERIAL PRIMARY KEY,
    nombre_cantidad_sangrado VARCHAR(20) UNIQUE NOT NULL
);

Tabla orientacion_sexual
CREATE TABLE orientacion_sexual (
    id_orientacion_sexual SERIAL PRIMARY KEY,
    nombre_orientacion_sexual VARCHAR(50) UNIQUE NOT NULL
);

Tabla numero_parejas_sexuales
CREATE TABLE numero_parejas_sexuales (
    id_numero_parejas SERIAL PRIMARY KEY,
    descripcion_numero_parejas VARCHAR(50) UNIQUE NOT NULL
);

2. Tablas Principales
Tabla pacientes
CREATE TABLE pacientes (
    id_paciente SERIAL PRIMARY KEY,
    nombre_paciente VARCHAR(50) NOT NULL,
    primer_apellido_paciente VARCHAR(50) NOT NULL,
    segundo_apellido_paciente VARCHAR(50),
    fecha_registro_paciente TIMESTAMP DEFAULT NOW(),
    fecha_nacimiento_paciente DATE NOT NULL,
    identificacion_paciente VARCHAR(20) NOT NULL UNIQUE,
    id_estado_civil INTEGER REFERENCES estado_civil(id_estado_civil) ON DELETE SET NULL,
    ocupacion_paciente VARCHAR(100),
    lugar_residencia_paciente VARCHAR(255),
    numero_celular_paciente VARCHAR(20),
    correo_electronico_paciente VARCHAR(100),
    activo_paciente BOOLEAN DEFAULT TRUE
);

Tabla antecedentes_patologicos
CREATE TABLE antecedentes_patologicos (
    id_antecedente_patologico SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    epi BOOLEAN DEFAULT FALSE,
    hepatitis BOOLEAN DEFAULT FALSE,
    hta BOOLEAN DEFAULT FALSE,
    diabetes BOOLEAN DEFAULT FALSE
);

Tabla antecedentes_heredofamiliares
CREATE TABLE antecedentes_heredofamiliares (
    id_antecedente_heredofamiliar SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    diabetes_heredofamiliar BOOLEAN DEFAULT FALSE,
    cardiacos_heredofamiliar BOOLEAN DEFAULT FALSE,
    hta_heredofamiliar BOOLEAN DEFAULT FALSE,
    cancer_heredofamiliar BOOLEAN DEFAULT FALSE,
    cancer_mama_heredofamiliar BOOLEAN DEFAULT FALSE,
    enfermedades_mentales_heredofamiliar BOOLEAN DEFAULT FALSE
);

Tabla antecedentes_quirurgicos
CREATE TABLE antecedentes_quirurgicos (
    id_antecedente_quirurgico SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    cirugias_abdominales_antecedente TEXT
);

Tabla ciclo_menstrual
CREATE TABLE ciclo_menstrual (
    id_ciclo_menstrual SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    edad_menarca_ciclo INTEGER,
    edad_ultimo_ciclo_sin_sangrado INTEGER,
    ritmo_menstrual_periodico BOOLEAN,
    id_cantidad_sangrado INTEGER REFERENCES cantidad_sangrado_menstrual(id_cantidad_sangrado) ON DELETE SET NULL,
    dismenorrea BOOLEAN,
    fecha_ultima_menstruacion DATE
);

Tabla actividad_sexual
CREATE TABLE actividad_sexual (
    id_actividad_sexual SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    edad_primera_relacion_sexual INTEGER,
    id_orientacion_sexual INTEGER REFERENCES orientacion_sexual(id_orientacion_sexual) ON DELETE SET NULL,
    id_numero_parejas INTEGER REFERENCES numero_parejas_sexuales(id_numero_parejas) ON DELETE SET NULL,
    factores_riesgo_sexual TEXT,
    dispareunia BOOLEAN,
    sangrado_sexual BOOLEAN,
    libido BOOLEAN,
    orgasmo BOOLEAN,
    lubricacion BOOLEAN,
    metodos_anticonceptivos_tiempo TEXT
);

Tabla historial_obstetrico
CREATE TABLE historial_obstetrico (
    id_historial_obstetrico SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    numero_gestaciones INTEGER,
    numero_partos INTEGER,
    numero_abortos INTEGER,
    numero_total_embarazos INTEGER,
    numero_total_partos INTEGER,
    numero_puerperios INTEGER,
    lactancia BOOLEAN,
    fecha_ultimo_parto DATE
);

Tabla examenes_preventivos
CREATE TABLE examenes_preventivos (
    id_examen_preventivo SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    fecha_ultimo_papanicolaou DATE,
    fecha_ultima_mamografia DATE,
    fecha_ultima_densitometria DATE,
    fecha_ultimo_ultrasonido_pelvico DATE
);

Tabla motivo_consulta
CREATE TABLE motivo_consulta (
    id_motivo_consulta SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    motivo_consulta TEXT
);

Tabla examen_fisico
CREATE TABLE examen_fisico (
    id_examen_fisico SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    presion_arterial VARCHAR(20),
    frecuencia_pulso INTEGER,
    frecuencia_cardiaca INTEGER,
    frecuencia_respiratoria INTEGER,
    peso DECIMAL(5,2),
    altura DECIMAL(5,2),
    indice_masa_corporal DECIMAL(5,2),
    cabeza_cuello_examen TEXT,
    torax_examen TEXT,
    mamas_examen TEXT,
    abdomen_examen TEXT,
    fosas_iliacas_examen TEXT
);

Tabla examen_ginecologico
CREATE TABLE examen_ginecologico (
    id_examen_ginecologico SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    vulva_examen TEXT,
    cuello_uterino_examen TEXT,
    bulbo_uretral_skene_examen TEXT,
    utero_examen TEXT,
    perine_examen TEXT,
    vagina_examen TEXT,
    colposcopia_examen TEXT,
    anexos_uterinos_examen TEXT
);

Tabla resumen_lista_problemas
CREATE TABLE resumen_lista_problemas (
    id_resumen SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    resumen_problemas TEXT
);

Tabla plan_seguimiento
CREATE TABLE plan_seguimiento (
    id_plan_seguimiento SERIAL PRIMARY KEY,
    id_paciente INTEGER NOT NULL UNIQUE REFERENCES pacientes(id_paciente) ON DELETE CASCADE,
    plan_seguimiento TEXT
);

Ejemplos de Población de Datos
1. Población de Tablas de Apoyo

Tabla estado_civil
INSERT INTO estado_civil (nombre_estado_civil) VALUES
('Soltera'),
('Casada'),
('Divorciada'),
('Viuda'),
('Unión Libre');


Tabla cantidad_sangrado_menstrual
INSERT INTO cantidad_sangrado_menstrual (nombre_cantidad_sangrado) VALUES
('Escaso'),
('Moderado'),
('Abundante');

Tabla orientacion_sexual
INSERT INTO orientacion_sexual (nombre_orientacion_sexual) VALUES
('Heterosexual'),
('Homosexual'),
('Bisexual'),
('Pansexual'),
('Asexual'),
('Otro');

Tabla numero_parejas_sexuales
INSERT INTO numero_parejas_sexuales (descripcion_numero_parejas) VALUES
('Menos de tres'),
('Tres'),
('Más de tres');

2. Registro de una Paciente y su Información

Caso de Uso: Registro de la paciente María Pérez Rodríguez

Tabla pacientes

Registrar a la paciente:
INSERT INTO pacientes (
    nombre_paciente,
    primer_apellido_paciente,
    segundo_apellido_paciente,
    fecha_nacimiento_paciente,
    identificacion_paciente,
    id_estado_civil,
    ocupacion_paciente,
    lugar_residencia_paciente,
    numero_celular_paciente,
    correo_electronico_paciente
)
VALUES (
    'María',
    'Pérez',
    'Rodríguez',
    '1985-07-15',
    '102030405',
    2,  -- Casada
    'Ingeniera Civil',
    'San José, Costa Rica',
    '8888-7777',
    'maria.perez@example.com'
);

Obtenemos el id_paciente generado (por ejemplo, 1).

Tabla antecedentes_patologicos

Registrar antecedentes patológicos:
INSERT INTO antecedentes_patologicos (
    id_paciente,
    epi,
    hepatitis,
    hta,
    diabetes
)
VALUES (
    1,
    FALSE,
    FALSE,
    FALSE,
    FALSE
);

Tabla antecedentes_heredofamiliares

Registrar antecedentes heredofamiliares:
INSERT INTO antecedentes_heredofamiliares (
    id_paciente,
    diabetes_heredofamiliar,
    cardiacos_heredofamiliar,
    hta_heredofamiliar,
    cancer_heredofamiliar,
    cancer_mama_heredofamiliar,
    enfermedades_mentales_heredofamiliar
)
VALUES (
    1,
    TRUE,
    FALSE,
    TRUE,
    FALSE,
    FALSE,
    FALSE
);

Tabla antecedentes_quirurgicos

Registrar antecedentes quirúrgicos:
INSERT INTO antecedentes_quirurgicos (
    id_paciente,
    cirugias_abdominales_antecedente
)
VALUES (
    1,
    'Colecistectomía en 2010'
);

Tabla ciclo_menstrual

Registrar información del ciclo menstrual:
INSERT INTO ciclo_menstrual (
    id_paciente,
    edad_menarca_ciclo,
    ritmo_menstrual_periodico,
    id_cantidad_sangrado,
    dismenorrea,
    fecha_ultima_menstruacion
)
VALUES (
    1,
    13,
    TRUE,
    2,  -- Moderado
    FALSE,
    '2023-08-01'
);

Tabla actividad_sexual

Registrar información sobre actividad sexual:
INSERT INTO actividad_sexual (
    id_paciente,
    edad_primera_relacion_sexual,
    id_orientacion_sexual,
    id_numero_parejas,
    factores_riesgo_sexual,
    dispareunia,
    sangrado_sexual,
    libido,
    orgasmo,
    lubricacion,
    metodos_anticonceptivos_tiempo
)
VALUES (
    1,
    20,
    1,  -- Heterosexual
    1,  -- Menos de tres
    'Ninguno',
    FALSE,
    FALSE,
    TRUE,
    TRUE,
    TRUE,
    'Píldora anticonceptiva desde hace 5 años'
);

Tabla historial_obstetrico

Registrar historial obstétrico:
INSERT INTO historial_obstetrico (
    id_paciente,
    numero_gestaciones,
    numero_partos,
    numero_abortos,
    numero_total_embarazos,
    numero_total_partos,
    numero_puerperios,
    lactancia,
    fecha_ultimo_parto
)
VALUES (
    1,
    2,
    2,
    0,
    2,
    2,
    2,
    TRUE,
    '2018-05-10'
);

Tabla examenes_preventivos

Registrar exámenes preventivos:
INSERT INTO examenes_preventivos (
    id_paciente,
    fecha_ultimo_papanicolaou,
    fecha_ultima_mamografia,
    fecha_ultima_densitometria,
    fecha_ultimo_ultrasonido_pelvico
)
VALUES (
    1,
    '2022-07-20',
    '2023-01-15',
    NULL,
    '2023-06-30'
);

Tabla motivo_consulta

Registrar motivo de consulta:
INSERT INTO motivo_consulta (
    id_paciente,
    motivo_consulta
)
VALUES (
    1,
    'Dolor pélvico recurrente'
);

Tabla examen_fisico

Registrar examen físico:
INSERT INTO examen_fisico (
    id_paciente,
    presion_arterial,
    frecuencia_pulso,
    frecuencia_cardiaca,
    frecuencia_respiratoria,
    peso,
    altura,
    indice_masa_corporal,
    cabeza_cuello_examen,
    torax_examen,
    mamas_examen,
    abdomen_examen,
    fosas_iliacas_examen
)
VALUES (
    1,
    '120/80',
    72,
    72,
    16,
    65.0,
    1.65,
    23.88,
    'Sin alteraciones',
    'Normal',
    'Sin masas ni dolor',
    'Blando, no doloroso',
    'Sin hallazgos'
);

Tabla examen_ginecologico

Registrar examen ginecológico:
INSERT INTO examen_ginecologico (
    id_paciente,
    vulva_examen,
    cuello_uterino_examen,
    bulbo_uretral_skene_examen,
    utero_examen,
    perine_examen,
    vagina_examen,
    colposcopia_examen,
    anexos_uterinos_examen
)
VALUES (
    1,
    'Normal',
    'Sin lesiones',
    'Sin alteraciones',
    'Tamaño y posición normales',
    'Integridad completa',
    'Mucosa normal',
    'Sin hallazgos patológicos',
    'Sin masas palpables'
);

Tabla resumen_lista_problemas

Registrar resumen y lista de problemas:
INSERT INTO resumen_lista_problemas (
    id_paciente,
    resumen_problemas
)
VALUES (
    1,
    'Paciente con dolor pélvico recurrente, se sospecha de endometriosis.'
);

Tabla plan_seguimiento

Registrar plan de seguimiento:
INSERT INTO plan_seguimiento (
    id_paciente,
    plan_seguimiento
)
VALUES (
    1,
    'Se solicita ultrasonido transvaginal y se programa cita de seguimiento en dos semanas.'
);

3. Funciones, Vistas e Índices
Índices y Restricciones en el Módulo de Pacientes

Crear Índices para Mejorar el Rendimiento
CREATE INDEX idx_pacientes_identificacion ON pacientes(identificacion_paciente);
CREATE INDEX idx_pacientes_nombre ON pacientes(nombre_paciente, primer_apellido_paciente, segundo_apellido_paciente);

Agregar Restricciones CHECK
ALTER TABLE pacientes ADD CHECK (fecha_nacimiento_paciente <= CURRENT_DATE);
ALTER TABLE ciclo_menstrual ADD CHECK (edad_menarca_ciclo BETWEEN 8 AND 18);
ALTER TABLE examen_fisico ADD CHECK (peso >= 0);
ALTER TABLE examen_fisico ADD CHECK (altura >= 0);