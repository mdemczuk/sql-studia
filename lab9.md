## Zadanie 1
```sql
INSERT INTO pracownicy (id_prac, nazwisko, etat, id_szefa, zatrudniony, placa_pod, placa_dod, id_zesp)
SELECT 250, 'KOWALSKI', 'ASYSTENT', NULL, '2015-01-13', 1500, NULL, 10 FROM DUAL
UNION ALL
SELECT 260, 'ADAMSKI', 'ASYSTENT', NULL, '2014-09-10', 1500, NULL, 10 FROM DUAL
UNION ALL
SELECT 270, 'NOWAK', 'ADIUNKT', NULL, '1990-05-01', 2050, 540, 20 FROM DUAL;

SELECT 	* 
FROM 	pracownicy 
WHERE 	id_prac IN (250, 260, 270);
```

## Zadanie 2
```sql
UPDATE 	pracownicy
SET 	placa_pod = placa_pod * 1.1, 
		placa_dod = NVL2(placa_dod, placa_dod * 1.2, 100)
WHERE 	id_prac IN (250, 260, 270);

SELECT 	* 
FROM	pracownicy 
WHERE 	id_prac IN (250, 260, 270);
```

## Zadanie 3
```sql
INSERT INTO zespoly (id_zesp, nazwa, adres)
VALUES	(60, 'BAZY DANYCH', 'PIOTROWO 2');
SELECT 	* 
FROM 	zespoly 
WHERE 	id_zesp = 60;
```

## Zadanie 4
```sql
UPDATE 	pracownicy
SET 	id_zesp = 
(
	SELECT id_zesp
	FROM zespoly
	WHERE nazwa = 'BAZY DANYCH'
)
WHERE 	id_prac IN (250, 260, 270);

SELECT	*
FROM 	pracownicy 
WHERE 	id_zesp = 60;
```

## Zadanie 5
```sql
UPDATE 	pracownicy
SET 	id_szefa = 
(
	SELECT id_prac 
	FROM pracownicy 
	WHERE nazwisko = 'MORZY'
)
WHERE 	id_zesp = 60;

SELECT 	* 
FROM	pracownicy 
WHERE	id_szefa = 
(
	SELECT 	id_prac
	FROM 	pracownicy 
	WHERE 	nazwisko = 'MORZY'
);
```

## Zadanie 6
```sql
DELETE 
FROM	zespoly
WHERE 	nazwa = 'BAZY DANYCH';
--nie
--ORA-02292: naruszono więzy spójności 
--(BIO130528.FK_ID_ZESP) - znaleziono rekord podrzędny
```

## Zadanie 7
```sql
DELETE 
FROM 	pracownicy
WHERE 	id_zesp = 
(
	SELECT id_zesp 
	FROM zespoly
	WHERE nazwa = 'BAZY DANYCH'
);

DELETE 
FROM	zespoly
WHERE 	nazwa = 'BAZY DANYCH';
```

## Zadanie 8
```sql
SELECT 	nazwisko, 
		placa_pod, 
		(
			SELECT 0.1 * AVG(placa_pod) 
			FROM pracownicy x 
			WHERE x.id_zesp = y.id_zesp
		) 
		AS podwyzka 

FROM	pracownicy y 
ORDER BY 	nazwisko;
```

## Zadanie 9
```sql
UPDATE 	pracownicy y
SET 	placa_pod = placa_pod + 
(
	SELECT 	0.1 * AVG(placa_pod) 
	FROM	pracownicy x 
	WHERE	x.id_zesp = y.id_zesp
);

SELECT 	nazwisko,
 		placa_pod 
FROM 	pracownicy
ORDER BY 	nazwisko;
```

## Zadanie 10
```sql
SELECT 	* 
FROM 	pracownicy
WHERE 	placa_pod =
(
	SELECT 	MIN(placa_pod) 
	FROM 	pracownicy
);
```

## Zadanie 11
```sql
UPDATE	pracownicy
SELECT 	placa_pod = ROUND((SELECT AVG(praca_pod) FROM pracownicy),2)
WHERE 	placa_pod =
	(
		SELECT	 MIN(placa_pod) 
		FROM	 pracownicy
	);
```

## Zadanie 12
```sql
SELECT 	nazwisko, 
		placa_dod
FROM 	pracownicy
WHERE 	id_zesp = 20;

UPDATE	 pracownicy
SET 	placa_dod = 
	(
		SELECT 	AVG(placa_pod)
		FROM 	pracownicy
		WHERE 	id_szefa = 
		(
			SELECT	id_prac 
			FROM	pracownicy 
			WHERE 	nazwisko = 'MORZY'
		)
	)
WHERE 	id_zesp = 20;


SELECT	nazwisko,
		placa_dod
FROM 	pracownicy
WHERE	id_zesp = 20;
```

## Zadanie 13
```sql
UPDATE 
	(
		SELECT	nazwa,
				placa_pod 
		FROM 	pracownicy 
		JOIN 	zespoly 
			USING 	(id_zesp)
		WHERE 	nazwa = 'SYSTEMY ROZPROSZONE'
	)
SET		placa_pod = placa_pod * 1.25;
```

## Zadanie 14
```sql
SELECT 	* 
FROM
	(
		SELECT 	p.nazwisko AS pracownik,
		 		s.nazwisko AS szef
		FROM 	pracownicy p 
		JOIN 	pracownicy s
			ON p.id_szefa = s.id_prac
	)
WHERE 	szef = 'MORZY';

DELETE
FROM
	(
		SELECT 	p.nazwisko AS pracownik,
				s.nazwisko AS szef
		FROM 	pracownicy p 
		JOIN 	pracownicy s
			ON 	p.id_szefa = s.id_prac
	)
WHERE 	szef = 'MORZY';
```

## Zadanie 15
```sql
SELECT 	* 
FROM	pracownicy
```

## Zadanie 16
```sql
CREATE SEQUENCE prac_seq START WITH 300 INCREMENT BY 10;
```

## Zadanie 17
```sql
INSERT INTO pracownicy (id_prac, nazwisko, etat, placa_pod)
VALUES 	(prac_seq.NEXTVAL,'Trabczynski', 'STAZYSTA', 1000);

SELECT	*
FROM	pracownicy 
WHERE 	nazwisko = 'Trabczynski';
```

## Zadanie 18
```sql
UPDATE 	pracownicy
SET 	placa_dod = prac_seq.CURRVAL
WHERE 	nazwisko = 'Trabczynski';

SELECT 	* 
FROM 	pracownicy
WHERE 	nazwisko = 'Trabczynski';
```

## Zadanie 19
```sql
DELETE 
FROM 	pracownicy
WHERE 	nazwisko = 'Trabczynski';
```

## Zadanie 20
```sql
CREATE SEQUENCE mala_seq START WITH 1 INCREMENT BY 1 MAXVALUE 10;
SELECT 	mala_seq.NEXTVAL 
FROM 	DUAL;
--ORA-02287: w tym miejscu numer sekwencji jest niedozwolony
```

## Zadanie 21
```sql
DROP SEQUENCE 	mala_seq;
```