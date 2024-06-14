![Header Image](assets/header.jpg)
# Proyecto de Creación de Base de Datos Relacional

## Descripción

Este proyecto consiste en la creación de una base de datos relacional para gestionar información de alumnos y docentes de una escuela de bootcamps. A partir de unos datos de entrada sin normalizar, se ha diseñado una estructura normalizada y escalable de base de datos que soporte la gestión académica y administrativa de una institución educativa de estas caracterísitcas.

## Objetivos

- ✔️ **Normalización de Datos:** para eliminar la redundancia y garantizar la integridad de los datos.
- ✔️ **Modelo Entidad-Relación (E/R):** que represente la estructura de la base de datos normalizada.
- ✔️ **Modelo Lógico de la Base de Datos:** Con base en el modelo E/R, desarrollar un modelo lógico.
- ✔️ **Creación de la Base de Datos:** Utilizando el sistema de gestión de bases de datos de PostgreSQL.
- ❌ **Alojar en algún servidor** vuestras bases de datos para poder acceder desde aplicaciones de terceros.

## Asunciones

- Los alumnos pueden cursar diferentes bootcamps en diferentes promociones y modalidades; no al mismo tiempo.
- Los docentes pueden impartir el mismo bootcamp en diferentes promociones y modalidades.
- La modalidad de los alumnos se deja por completar ya que no se nos ha proporcionado el dato. Interesante para escalabilidad.
- Las docentes Rosalva Ayuso y Angelica Corral estan asociadas a programas sin alumnos, y por tanto sin promocion asociada; no los incluimos hasta tener los datos completos.

## Modelo Entidad-Relación (E/R)

![assets/Modelo_Entidad_Relacion.png](assets/Modelo_Entidad_Relacion.png)

## Modelo Lógico de la Base de Datos

![assets/Modelo_Logico.png](assets/Modelo_Logico.png)

## Estructura de la Base de Datos

La base de datos está compuesta por las siguientes tablas:

- **alumnos**: Almacena información sobre los alumnos.
- **docentes**: Almacena información sobre los docentes.
- **programa_alumnos**: Relaciona los alumnos con los programas en los que están inscritos.
- **programa_docentes**: Relaciona los docentes con los programas en los que enseñan.
- **programas**: Almacena información sobre los programas académicos.
- **promociones**: Almacena información sobre las promociones de los programas.
- **notas**: Almacena las notas de los alumnos en los distintos programas.

## Descripción de las Tablas

### alumnos

- **alumnoid**: Identificador único del alumno (integer).
- **nombre**: Nombre del alumno (text).
- **email**: Dirección de correo electrónico del alumno (text).

### docentes

- **docenteid**: Identificador único del docente (integer).
- **nombre**: Nombre del docente (text).
- **rol**: Rol del docente en la institución (text).

### programa_alumnos

- **programaal_id**: Identificador único de cada programa cursado por cada alumno (integer).
- **alumnoid**: Identificador del alumno (integer).
- **programaid**: Identificador del programa (integer).
- **modalidad**: Modalidad del programa (text).

### programa_docentes

- **programado_id**: Identificador único de cada programa cursado por cada docente (integer).
- **docenteid**: Identificador del docente (integer).
- **programaid**: Identificador del programa (integer).
- **modalidad**: Modalidad del programa (text).

### programas

- **programaid**: Identificador único del programa [vertical - sede - promocion] (integer).
- **verticalid**: Identificador del área vertical del programa (integer).
- **promocionid**: Identificador de la promoción del programa (integer).
- **sede**: Sede del programa (text).

### promociones

- **promocionid**: Identificador único de la promoción (integer).
- **mes**: Mes de inicio de la promoción (text).
- **fechainicio**: Fecha de inicio de la promoción (text).

### notas

- **programaal_id**: Identificador de la relación programa-alumno (integer).
- **verticalid**: Identificador del área vertical de la nota (integer).
- **proyectoid**: Identificador del proyecto asociado a la nota (integer).
- **nota**: Nota obtenida (text).

## Queries test
#### Consultar todas las notas:
```sql
SELECT al.alumnoid, al.nombre, v.nombrevertical, n.proyecto_hlf, n.proyecto_eda, n.proyecto_bbdd, n.proyecto_deployment,
n.proyecto_webdev, n.proyecto_frontend, n.proyecto_backend, n.proyecto_react, n.proyecto_fullstack
FROM alumnos al
INNER JOIN programa_alumnos pral ON pral.alumnoid = al.alumnoid
INNER JOIN programas pr ON pr.programaid = pral.programaid
INNER JOIN vertical v ON v.nombrevertical = pr.vertical
INNER JOIN notas n ON n.programaal_id = pral.programaal_id;
```

#### Consultar las notas de un programa específico:
```sql
SELECT al.alumnoid, al.nombre, v.nombrevertical, n.proyecto_hlf, n.proyecto_eda, n.proyecto_bbdd, n.proyecto_deployment,
n.proyecto_webdev, n.proyecto_frontend, n.proyecto_backend, n.proyecto_react, n.proyecto_fullstack
FROM alumnos al
INNER JOIN programa_alumnos pral ON pral.alumnoid = al.alumnoid
INNER JOIN programas pr ON pr.programaid = pral.programaid
INNER JOIN vertical v ON v.nombrevertical = pr.vertical
INNER JOIN notas n ON n.programaal_id = pral.programaal_id
WHERE v.nombrevertical = 'Data Science' AND n.proyecto_hlf IS NOT NULL
```

#### Consultar la vertical que imparte cada docente:
```sql
SELECT d.docenteid, d.nombre, d.rol, v.nombrevertical
FROM docentes d
INNER JOIN programa_docentes prd ON prd.docenteid = d.docenteid
INNER JOIN programas pro ON pro.programaid = prd.programaid
INNER JOIN promociones pr ON pr.promocionid = pro.promocionid
INNER JOIN vertical v ON v.nombrevertical = pro.vertical
```

#### Consultar todos los docentes que imparte una vertical específica:
```sql
SELECT d.docenteid, d.nombre, d.rol, v.nombrevertical
FROM docentes d
INNER JOIN programa_docentes prd ON prd.docenteid = d.docenteid
INNER JOIN programas pro ON pro.programaid = prd.programaid
INNER JOIN promociones pr ON pr.promocionid = pro.promocionid
INNER JOIN vertical v ON v.nombrevertical = pro.vertical
WHERE pro.vertical = 'Full Stack'
```
