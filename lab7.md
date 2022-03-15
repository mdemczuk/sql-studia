## Zadanie 1
```sql
SELECT 	id_zesp,
		nazwa,
		adres
FROM 	zespoly z
WHERE 	(z.id_zesp) NOT IN
(
	SELECT 	p.id_zesp 
	FROM 	pracownicy p
	WHERE	p.id_zesp = z.id_zesp
);
```

## Zadanie 2
```sql
SELECT	nazwisko,
		placa_pod, 
		etat
FROM	pracownicy p
WHERE	placa_pod > 
(	
	SELECT 	AVG(placa_pod) 
	FROM 	pracownicy 
	WHERE	etat = p.etat
);
```

## Zadanie 3
```sql
SELECT 	nazwisko, placa_pod
FROM	pracownicy p
WHERE 	placa_pod >=
(
	SELECT 	placa_pod * 0.75
	FROM 	pracownicy 
	WHERE 	id_prac = p.id_szefa
);
```

## Zadanie 4
```sql
SELECT 	nazwisko
FROM 	pracownicy p
WHERE 	etat = 'PROFESOR' 
	AND	p.id_prac IN 
	(
		SELECT id_szefa 
		FROM pracownicy 
		WHERE etat NOT IN 'STAZYSTA'
	)
	AND p.id_prac NOT IN 
	(
		SELECT id_szefa
		FROM pracownicy
		WHERE etat IN 'STAZYSTA'
	);
```

LUB

```sql
SELECT nazwisko
FROM pracownicy p
WHERE EXISTS 
(
	SELECT 	* 
	FROM 	pracownicy
	WHERE id_szefa = p.id_prac 
		AND etat NOT IN 'STAZYSTA'
) 
	AND NOT EXISTS
	(
		SELECT 	*
		FROM 	pracownicy
		WHERE 	id_szefa = p.id_prac 
			AND etat IN 'STAZYSTA'
	) 
	AND etat = 'PROFESOR'
```

## Zadanie 5
```sql
SELECT	nazwa, 
		maks_suma_plac
FROM 
(
	(
		SELECT MAX(SUM(placa_pod)) AS maks_suma_plac
		FROM pracownicy
		GROUP BY id_zesp 
	)
	INNER JOIN
	(
		SELECT id_zesp, SUM(placa_pod) AS suma_plac
		FROM pracownicy
		GROUP BY id_zesp
	)
	ON maks_suma_plac = suma_plac
) 
NATURAL INNER JOIN zespoly;
```

## Zadanie 6
```sql
SELECT 	nazwisko,
		placa_pod
FROM 	pracownicy
WHERE
(
	placa_pod IN
  		(
  			SELECT 	placa_pod 
  			FROM 	pracownicy
        	ORDER BY 	placa_pod DESC
        	FETCH FIRST 3 ROWS ONLY
        )
);
```

## Zadanie 7
```sql
SELECT	EXTRACT(YEAR FROM zatrudniony) AS rok,
		COUNT(id_prac) AS LICZBA
FROM 	pracownicy 
GROUP BY 	EXTRACT(YEAR FROM zatrudniony)
ORDER BY 	liczba DESC
```

## Zadanie 8
```sql
SELECT	EXTRACT(YEAR FROM zatrudniony) AS rok,
		COUNT(id_prac) AS liczba
FROM 	pracownicy
GROUP BY 	EXTRACT(YEAR FROM zatrudniony)
HAVING 
	(
		SELECT MAX(COUNT(id_prac)) 
		FROM pracownicy 
		GROUP BY EXTRACT(YEAR FROM zatrudniony)
	)
		= COUNT(id_prac);
```

## Zadanie 9
SELECT
```sql
SELECT	nazwisko,
		placa_pod,
		(
			placa_pod - (SELECT AVG(placa_pod)
			FROM pracownicy
			GROUP BY id_zesp
			HAVING id_zesp = p.id_zesp)
		) 	
		AS roznica		
FROM pracownicy p
ORDER BY nazwisko;
```
FROM
```sql
SELECT	p.nazwisko,
		p.placa_pod,
		(p.placa_pod - srednia) AS roznica
FROM 	pracownicy p 
	JOIN 
	(
		SELECT 	id_zesp,
				AVG(placa_pod) AS srednia
		FROM 	pracownicy
		GROUP BY	 id_zesp
	) s
		ON 	s.id_zesp = p.id_zesp
ORDER BY 	p.nazwisko;
```

## Zadanie 10
SELECT
```sql
SELECT 	nazwisko, 
		placa_pod,
	(
		placa_pod - (SELECT AVG(placa_pod)
		FROM pracownicy
		GROUP BY id_zesp
		HAVING id_zesp = p.id_zesp)
	)
		AS roznica
FROM 	pracownicy p
WHERE 
(
	placa_pod - (SELECT AVG(placa_pod)
	FROM pracownicy
	GROUP BY id_zesp
	HAVING id_zesp = p.id_zesp)
)	> 0

ORDER BY	nazwisko;
```
FROM
```sql
SELECT	p.nazwisko,
		p.placa_pod, 
		(p.placa_pod - srednia) AS roznica
FROM pracownicy p 
JOIN 	
(	
	SELECT 	id_zesp,
			AVG(placa_pod) AS srednia
	FROM	pracownicy
	GROUP BY 	id_zesp
) s
	ON	s.id_zesp = p.id_zesp
WHERE	p.placa_pod - srednia > 0
ORDER BY 	p.nazwisko;
```

## Zadanie 11
```sql
SELECT	p.nazwisko,
       	(
       	SELECT COUNT(*)
		FROM pracownicy
		GROUP BY id_szefa
		HAVING p.id_prac = id_szefa
		) 		
		AS podwladni
FROM pracownicy p 
	JOIN zespoly z 
	ON p.id_zesp = z.id_zesp
WHERE etat = 'PROFESOR' 
	AND adres = 'PIOTROWO 3A'
ORDER BY podwladni DESC;
```

## Zadanie 12
```sql
SELECT 	nazwa,
		srednia_w_zespole,
		srednia_ogolna,
		CASE
			WHEN srednia_w_zespole IS NULL THEN '???'
			WHEN srednia_w_zespole >= srednia_ogolna THEN ':)'
			ELSE ':('
		END 
		AS nastroje
FROM zespoly z 
	LEFT OUTER JOIN 
	(	
		SELECT 	id_zesp, 
				AVG(placa_pod) AS srednia_w_zespole
		FROM pracownicy
		GROUP BY id_zesp
	) s
	ON z.id_zesp = s.id_zesp
CROSS JOIN 
(
	SELECT ROUND(AVG(placa_pod), 2) AS srednia_ogolna
	FROM pracownicy
)
ORDER BY nazwa;
```

## Zadanie 13
```sql
SELECT *
FROM etaty e
ORDER BY 
(
	SELECT COUNT(*)
	FROM pracownicy p
	WHERE e.nazwa = p.etat
	GROUP BY p.etat	
) 
DESC,e.nazwa;
```