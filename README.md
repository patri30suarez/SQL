ESTRUCTURA
SELECT
FROM
WHERE
Para que se ejecute una función tiene que terminar en punto y coma
Distingue entre mayúsculas y minúsculas
Hay que respetar los espacios
Para poder usar una coma es \’

SELECT
REPLACE (‘lo que se quiere cambiar’, ‘la parte que se cambia’, ’por lo que se cambia’ )

ROUND (numero, numero de decimales). Se usa para redondear

CONCAT (lo que se quiere escribir) Ejemplo CONCAT  (‘hola mundo’) Resultado hola mundo

DISTINT. Se usa para que no se repita los valores del resultado

COUNT ( ) Es para contar

SUM ( ) Suma 

COALESCE (atributo, valor por defecto)




FROM
JOIN tabla 2 ON clave principal=clave ajena. Se usa para consultar atributos de otras tablas. Si no se usa ON las claves van en WHERE. Se puede cambiar un JOIN por una tabla anidada. Non saca os nulos 

INNERJOIN Tiene el mismo uso que el JOIN solo que no permite eliminar el ON. Tampoco saca os nulos

AS se usa para renombrar atributos

LEFT JOIN Saca los de la izquierda aunque en la derecha haya nulos 

RIGHT JOIN Saca los de la izquierda aunque en la izquierda haya nulos 

WHERE 
ORDER BY ordenar por
	ASC ascendente
	DESC descendente

BETWEEN Entre. Incluye los extremos. No se puede separar los números. Se puede sustituir por >= numero <=numero

LIKE Como 
% Representa a varios caracteres
­_ Representa a un solo carácter

XOR Cuando solo se cumple una de ls opciones nunca varias juntas

LENGTH Cuenta las letras de la palabra 

GROUP BY agrupar según un atributo

HAVING Es para que sume cada subtabla. Ten que ir debajo de GROUP BY 

LIMIT para limitar el numero de soluciones

EJEMPLOS 
    1. Some countries have populations more than three times that of any theirs neighbours (in same continent). Give the countries and the continent.

SELECT name, continent
FROM  world AS outerworld
WHERE population > ALL
	(SELECT population *3
FROM world AS innerworld
WHERE outerworld.continent=innerworld.continent
AND outerworld.name <> innerworld.name
AND population IS NOT NULL);

    2. List the films where ‘Harrison Ford’ has appeared – but not in starring role (note: the ord field of casting gives the position of the actor if ord=1 then this actor is in starring role)

SELECT movie.title
FROM actor JOIN movie ON movie.id=casting.movieid
		   JOIN casting ON actor.id=casting.actorid
WHERE actor.name=’Harrison Ford’
	  AND ord <> 1;

    • Que erro dara si cambiamos FROM actor y eliminamos JOIN movie ON movie.id=casting.movieid
Non recoñece movie.title

    • Fai a mesma consulta sin usar ON 

SELECT movie.title
FROM actor JOIN movie 
		   JOIN casting 
WHERE actor.name=’Harrison Ford’
		  AND ord <> 1
		AND movie.id=casting.movieid
		AND actor.id=casting.actorid;


    • Fai a mesma consulta con solo un join 

SELECT movie.title
FROM actor JOIN movie ON  movie.id=casting.movieid
		   JOIN casting 
WHERE casting.actorid = (SELECT actor.id
FROM actor
WHERE actor.name=’Harrison Ford’);

    1. Esta consulta se ejecuta
SELECT movie.yr
	COUNT (movie.title) AS movies
FROM movie

Non porque o SELECT pide todos los años y a la vez COUNT es un reductor, para solucionar necesitaremos un GROUP BY 

    • Donde se coloca COUNT (movie.title)>2
Al ser un predicado sería en WHERE pero como también tiene un reductor se coloca en HAVING 
