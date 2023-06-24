# BD-P4-CASO-1
Inserte nuevas Personas con los siguientes datos:
< kf@gmail.com, kf, Katerin Falcon >
< mgh@gmail.com, mgh,Rosa González>
< rgh@gmail.com, rlh,Rosa López>

INSERT INTO persona VALUES
('kf@gmail.com','kf','Katerin Falcon');
INSERT INTO persona VALUES
('mgh@gmail.com','mgh','Rosa Gonzales');
INSERT INTO persona VALUES
('rgh@gmail.com','rlh','Rosa Lopez');

Inserte el nuevo curso con los siguientes datos: 
< Ruby, 40>

INSERT INTO curso VALUES
('Ruby',40);

Inserte los temas para el curso con los siguientes datos:
< Ruby, Estructura de Datos>
< Ruby, Estructura de Control>

INSERT INTO tema VALUES
('Ruby','Estructura de Datos');
INSERT INTO tema VALUES
('Ruby','Estructura de Control');

Inserte la nueva tupla para DICTA con los siguientes datos: 
< kf@gmail.com, Ruby>

INSERT INTO dicta VALUES
('kf@gmail.com','Ruby');

Modifique la carga horaria del curso Ruby con el valor 60

update curso SET ch=60 where nom='Ruby';

Elimine el curso Ruby.

delete from curso where nom='Ruby';

CONSULTAS
1*) select correo, nombre from persona
2*) select nom from curso
3*) select nom from curso where ch > (select max(ch) from curso natural join (select nom from dicta where correo='pedroibañez@yahoo.com.ar') A)
4*) select * from persona where nombre like '%Rosa%'
5*) select nom from curso where ch > (select ch from curso where nom='Kotlin I')
6*) SELECT * from curso where ch > 40
7*) FORMA 1*) SELECT * from curso where ch >= 40 and ch <= 45
    FORMA 2*) SELECT * from curso where ch between 40 and 45
8*) select nom as nombreCurso,ch,tema from (select * from curso natural join tema) A
9*) select distinct correo,nombre from persona natural join dicta
10*) select * from (select nom, correo from curso natural join dicta) A natural join persona
11*) select A.nom,A.ch,persona.correo,persona.nomu,persona.nombre from (select curso.nom,curso.ch,inscripcion.correo from curso LEFT OUTER JOIN inscripcion on curso.nom=inscripcion.nom) A LEFT OUTER JOIN persona on A.correo=persona.correo 
12*) select * from persona natural join (select correo from dicta where nom='Python I') A
13*) select * from persona natural join (select correo from dicta where nom='Python II') A
14*) select correo,nombre from persona natural join ((select correo from dicta where nom='Python I') union (select correo from dicta where nom='Python II')) A
15*) (select correo from dicta where nom='Python I') intersect (select correo from dicta where nom='Python II')
16*) select * from persona natural join (select correo from inscripcion natural join ((select correod as correo from inscripcion) union (select correo from dicta)) A) B
17*) select * from persona natural join (select correo from inscripcion where nom='Kotlin I') A
18*) select * from persona natural join (select correo from inscripcion where nom='Kotlin II') A
19*) (select correo from inscripcion where nom='Kotlin I') intersect (select correo from inscripcion where nom='Kotlin II')
20*) select * from persona natural join ((select correo from inscripcion where nom='Python I' and nota >= 4) intersect (select correo from inscripcion where nom='Python II' and nota >= 4)) A
21*) select distinct correo from ((select correo,nom from inscripcion) A cross join (select correo as CO, nom as NOO from inscripcion) B) C where correo=co and nom!=noo
     OTRA FORMA EPICA CON GROUP BY(MUY IMPORTANTE)*) select correo from inscripcion group by correo having count(correo) > 1
22*) select * from persona natural join (select distinct correo from ((select nom,correo from curso natural join dicta where ch < 30) A cross join (select nom as NOO,correo as CO from curso natural join dicta where ch < 30) B) C where correo=CO and nom!=NOO) D
     OTRA FORMA EPICA CON GROUP BY(MUY IMPORTANTE)*) select * from (select correo from dicta natural join curso where ch < 30 group by 
     correo having count(correo) > 1) A natural join persona
23*) select  correo,CO from ((select correo,nom from inscripcion) A cross join (select correo as CO, nom as NOO from inscripcion) B) C where correo!=CO and nom=NOO
     OTRA FORMA CON INNER JOIN PERO UNA SOLA COLUMNA*) select distinct i1.correo
     from inscripcion i1 inner join inscripcion i2 on i1.correo != i2.correo and i1.nom = i2.nom
24*) select  correo,CO from ((select correo,nom,correod from inscripcion) A cross join (select correo as CO, nom as NOO, correod as COD from inscripcion) B) C where correo!=CO and nom=NOO and correod!=COD
     OTRA FORMA CON INNER JOIN PERO UNA SOLA COLUMNA*) select distinct i1.correo
     from inscripcion i1 inner join inscripcion i2 on i1.correo != i2.correo and i1.nom = i2.nom and i1.correod != i2.correod
25*) insert into inscripcion values ('anagarcia32@gmail.com','Kotlin I','pedroibañez@yahoo.com.ar', 10),
							   ('anagarcia32@gmail.com', 'Kotlin II', 'pedroibañez@yahoo.com.ar',8),
							   ('anagarcia32@gmail.com','Javascript I', 'pedroibañez@yahoo.com.ar', 7),
							   ('anagarcia32@gmail.com', 'Javascript II', 'pedroibañez@yahoo.com.ar', 7),
							   ('anagarcia32@gmail.com','Ruby', 'pedroibañez@yahoo.com.ar',9);
     select *
from persona
where not exists(select *
				from curso
				where not exists(select *
								from inscripcion
								where inscripcion.nom=curso.nom and inscripcion.correo=persona.correo))
26*) select *
from persona
where not exists(select * 
				 from curso natural join dicta A where correo='pedroibañez@yahoo.com.ar' and
				 not exists(select *
							from inscripcion
							where inscripcion.correo=persona.correo and inscripcion.nom=A.nom))        
