## Ingeniero Oscar Javier García Chacón
### Prueba SQL - Neoris


#### Base de datos

Se tiene la siguiente base de evaluación de estudiantes y se tiene los puntajes de los examenes de varios alumnos en varias materias.

Diseño de la base:

1. Estudiante ( id_estudiante, nombre, apellido, nivel )
Nota: Nivel = Nivel que esta cursando el estudiante
2. Materia(id_materia, descripcion,nivel);
Nota: Nivel = Nivel en el que se imparte la materia
3. Examen ( id_estudiante, id_materia, nota, fecha )
Nota: Fecha = Fecha de evaluación

<img src="https://user-images.githubusercontent.com/90927855/232962900-96268340-2a49-43dc-bfea-58809ccc5e20.png" width="250" height="250">

### Libreria a utilizar
#### sqlite3


### Creacion de Base de datos en Python

>conn = sqlite3.connect('test.db')
>print("Base de datos abierta con exito");
>
>conn.execute('''drop table if exists Estudiante;''')
>conn.execute('''drop table if exists Materia;''')
>conn.execute('''drop table if exists Examen;''')
>
>conn.execute("create table Estudiante(id_estudiante int, nombre text, apellido text, nivel int);")
>conn.execute("create table Materia(id_materia int, descripcion text, nivel int);")
>conn.execute("create table Examen(id_estudiante int, id_materia int, nota int, fecha date);")
>
>conn.execute("insert into Estudiante(id_estudiante,nombre,apellido,nivel)values(101, 'Jorge', 'Campos',	3);")
>conn.execute("insert into Estudiante(id_estudiante,nombre,apellido,nivel)values(102, 'Valeria', 'Suarez',	3);")
>conn.execute("insert into Estudiante(id_estudiante,nombre,apellido,nivel)values(103, 'Cristina', 'Cabezas',	4);")
>conn.execute("insert into Estudiante(id_estudiante,nombre,apellido,nivel)values(104, 'Carlos', 'Rojas',	4);")
>conn.execute("insert into Estudiante(id_estudiante,nombre,apellido,nivel)values(105, 'Vicky', 'Torres',	5);")
>conn.execute("insert into Estudiante(id_estudiante,nombre,apellido,nivel)values(106, 'Tatiana', 'Gonzales',	5);")
>
>conn.execute("insert into Materia (id_materia,descripcion, nivel) values (	201	,'Matematicas',	3	);")
>conn.execute("insert into Materia (id_materia,descripcion, nivel) values (	202	,'Calculo Diferencial',	4	);")
>conn.execute("insert into Materia (id_materia,descripcion, nivel) values (	203	,'Fisica General',	5	);")
>conn.execute("insert into Materia (id_materia,descripcion, nivel) values (	204	,'Geometria Analitica',	3	);")
>conn.execute("insert into Materia (id_materia,descripcion, nivel) values (	205	,'Calculo Integral',	4	);")
>conn.execute("insert into Materia (id_materia,descripcion, nivel) values (	206	,'Matematicas Discretas',	5	);")
>
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	101	,	201	,	6	,	'28/03/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	102	,	201	,	4	,	'04/05/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	103	,	202	,	7	,	'28/11/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	104	,	202	,	8	,	'08/06/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	105	,	203	,	8	,	'04/11/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	106	,	203	,	9	,	'10/09/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	101	,	204	,	9	,	'01/07/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	102	,	204	,	8	,	'29/10/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	103	,	205	,	5	,	'22/10/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	104	,	205	,	4	,	'05/07/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	105	,	206	,	7	,	'30/01/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	106	,	206	,	9	,	'01/04/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	101	,	204	,	6	,	'24/03/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	103	,	206	,	9	,	'23/06/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	106	,	203	,	5	,	'08/10/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	104	,	205	,	4	,	'23/01/2022');")
>conn.execute("insert into Examen ( id_estudiante,id_materia,nota,fecha) values (	102	,	202	,	5	,	'11/06/2022');")
>
>conn.commit()
>print("Valores insertados con éxito");


#### En el siguiente paso se adapta la fecha a formato aceptado por sqlite, esto para realizar las consultas solicitadas.

>conn = sqlite3.connect('test.db')
>
>cursor = conn.execute('''
>
>UPDATE Examen SET fecha = REPLACE(substr(fecha, 7, 4) || '-' || substr(fecha, 4, 2) || '-' || substr(fecha, 1, 2), '/', '-');
>
>''')
>
>conn.commit()
>
>conn.close()

### SQL: Pregunta 1: Obtener los nombres de los alumnos que aprobaron, las materias y la nota, ordenado por la fecha del examen. Se aprueba la materia con una nota superior o igual a 7.

#### Para responder a esta pregunta, se realiza un join de la tabal Examen con los campos requeridos de las tablas Materia y Estudiante; posteriormente seleccionamos solo los estudiantes que lograron una nota mayor o igual a 7.

>conn = sqlite3.connect('test.db')
>
>cursor = conn.execute('''
>
>SELECT r.id_estudiante, r.id_materia, r.nota, r.fecha, e.id_estudiante, e.nombre, e.apellido, e.nivel, m.descripcion
>
>FROM Examen AS r 
>
>JOIN Materia AS m ON r.id_materia = m.id_materia
>
>JOIN Estudiante AS e ON r.id_estudiante = e.id_estudiante
>
>WHERE nota >=7
>
>ORDER BY fecha ASC;
>
>''')
>for row in cursor:
>>print(row)
>
>conn.close()

### SQL: Pregunta 2: Obtener la mejor nota de cada nivel, por materia, incluir apellido, nombre del estudiante y materia.

### La respuesta a este punto se realiza utilizando la funcion MAX en el campo nota de la tabla Examen y realizando un JOIN con los campos requeridos de las tablas Estudiante y Materia
### Teniendo en cuenta que hay campo nivel en la tabla Estudiante y en la tabla Materia, se realizan dos soluciones, una bajo el nivel de Materia y la otra en Estudiante


#### Ejercicio # 2.1

>conn = sqlite3.connect('test.db')
>
>cursor = conn.execute('''
>
>SELECT e.nivel, r.id_materia, MAX(r.nota) AS nota_maxima, m.descripcion, e.nombre 
>
>FROM Examen r
>
>JOIN Materia m ON r.id_materia = m.id_materia
>
>JOIN Estudiante e ON r.id_estudiante = e.id_estudiante
>
>GROUP BY e.nivel, m.descripcion;
>''')
>
>for row in cursor:
>>print(row)
>
>conn.close()

#### Ejercicio # 2.2
>conn = sqlite3.connect('test.db')
>
>cursor = conn.execute('''
>
>SELECT m.nivel, r.id_materia, MAX(r.nota) AS nota_maxima, m.descripcion, e.nombre 
>
>FROM Examen r
>
>JOIN Materia m ON r.id_materia = m.id_materia
>
>JOIN Estudiante e ON r.id_estudiante = e.id_estudiante
>
>GROUP BY m.nivel, m.descripcion;
>''')
>
>for row in cursor:
>>print(row)
>
>conn.close()

### SQL: Pregunta 3: Obtener los estudiantes que hayan rendido mas de un examen de alguna materia siempre y cuando la segunda vez se haya obtenido una mejor nota, incluir la nota, el apellido y nombre del estudiante, la materia y el nivel.

>conn = sqlite3.connect('test.db')
>
>cursor = conn.execute('''
>
>SELECT e.apellido, e.nombre, m.descripcion AS materia, m.nivel, r.nota
>
>FROM examen r
>
>INNER JOIN estudiante e ON r.id_estudiante = e.id_estudiante
>
>INNER JOIN materia m ON r.id_materia = m.id_materia
>
>WHERE r.nota > (SELECT MIN(r2.nota) FROM examen r2 WHERE r2.id_materia = r.id_materia AND r2.id_estudiante = r.id_estudiante);
>
>''')
>for row in cursor:
>>print(row)
>
>conn.close()


### SQL: Pregunta 4: Obtener los apellidos y nombres de los estudiantes que han rendido exámenes de materias de un nivel diferente al que están cursando, incluir la materia y la nota obtenida, ordenar alfabéticamente por el apellido del estudiante.

### La respuesta a este punto se da por un JOIN entre la tabla Examen y los campos requeridos de Estudiante y Materia; posteriormente se activa una condiciones para determina cuales estudiantes estan presentado examenes de un nivel diferente al que estan.

>conn = sqlite3.connect('test.db')
>
>cursor = conn.execute('''
>
>SELECT e.apellido, e.nombre, m.descripcion AS materia, r.nota
>FROM examen r
>
>INNER JOIN estudiante e ON r.id_estudiante = e.id_estudiante
>
>INNER JOIN materia m ON r.id_materia = m.id_materia
>
>WHERE m.nivel <> e.nivel
>
>ORDER BY e.apellido ASC;
>
>''')
>for row in cursor:
>>
>>print(row)
>
>conn.close()

### SQL: Pregunta 5: Obtener los promedios totales por materia, en los casos de estudiantes que rindieron mas de un examen en alguna materia, considerar la mejor nota obtenida.

### La respuesta a este punto se da utilizando la funcion AVG para encontrar la mejor nota, pero con una subconsulta para seleccionar la mejor de los estudiantes uqe presentaro mas de una vez el examen.

>conn = sqlite3.connect('test.db')
>
>cursor = conn.execute('''
>
>SELECT r.id_materia, m.descripcion AS materia, AVG(r.mejor_nota) AS promedio_total
>
>FROM Materia m
>
>INNER JOIN (
>>
>>SELECT id_materia, id_estudiante, MAX(nota) AS mejor_nota
>>
>>FROM Examen
>>
>>GROUP BY id_materia, id_estudiante
>>
>) AS r ON r.id_materia = m.id_materia
>
>GROUP BY m.descripcion
>
>ORDER BY r.id_materia ASC;
>
>''')
>for row in cursor:
>>print(row)
>
>conn.close()

