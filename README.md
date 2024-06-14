![Header Image](assets/header.jpg)
# Proyecto de Creación de Base de Datos Relacional

## Descripción

Este proyecto consiste en la creación de una base de datos relacional para gestionar información de alumnos y docentes de una escuela de bootcamps. A partir de unos datos de entrada sin normalizar, se ha diseñado una estructura normalizada y escalable de base de datos que soporte la gestión académica y administrativa de una institución educativa de estas caracterísitcas.

## Objetivos
- ✔️ **Normalización de Datos:** Realizar una normalización adecuada para eliminar la redundancia y garantizar la integridad de los datos.
- ✔️ **Modelo Entidad-Relación (E/R):** Diseñar un modelo E/R que represente la estructura de la base de datos normalizada.
- ✔️ **Modelo Lógico de la Base de Datos:** Con base en el modelo E/R, desarrollar un modelo lógico de la base de datos.
- ✔️ **Creación de la Base de Datos:** Utilizando el sistema de gestión de bases de datos de PostgreSQL.
- ❌ **Alojar en algún servidor** vuestras bases de datos para poder acceder desde aplicaciones de terceros.

## Asunciones
- Los alumnos pueden cursar diferentes bootcamps en diferentes promociones y modalidades; no al mismo tiempo.
- Los docentes pueden impartir el mismo bootcamp en diferentes promociones y modalidades.
- La modalidad de los alumnos se deja por completar ya que no se nos ha proporcionado el dato. Interesante para escalabilidad.
- Las docentes Rosalva Ayuso y Angelica Corral estan asociadas a programas sin alumnos, y por tanto sin promocion asociada; no los incluimos hasta tener los datos completos.

## Modelo Entidad-Relación (E/R)
![Modelo_Entidad_Relacion](assets/Modelo_Entidad_Relacion.png)

## Modelo Lógico de la Base de Datos
![Modelo_Logico](assets/Modelo_Logico.png)

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

### programa_alumnos
- **programaal_id**: Identificador único de la relación programa-alumno (integer).
- **alumnoid**: Identificador del alumno (integer).
- **programaid**: Identificador del programa (integer).
- **modalidad**: Modalidad del programa (text).

### programa_docentes
- **programado_id**: Identificador único de la relación programa-docente (integer).
- **docenteid**: Identificador del docente (integer).
- **programaid**: Identificador del programa (integer).
- **modalidad**: Modalidad del programa (text).




## Queries test
conectar tabla alumnos con programas
```sql
SELECT al.nombre, al.email, pr.sede, pral.programaid FROM alumnos al INNER JOIN programa_alumnos pral ON pral.alumnoid = al.alumnoid INNER JOIN programas pr ON pr.programaid = pral.programaid;
```




    SELECT al.nombre, al.email, pr.sede, pral.programaid FROM alumnos al INNER JOIN programa_alumnos pral ON pral.alumnoid = al.alumnoid INNER JOIN programas pr ON pr.programaid = pral.programaid;
