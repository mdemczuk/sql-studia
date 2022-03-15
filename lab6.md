## Zadanie 1
```sql
SELECT nazwisko, etat, id_zesp
FROM pracownicy
WHERE id_zesp =
	(SELECT id_zesp
	FROM pracownicy
	WHERE nazwisko = 'BRZEZINSKI')
ORDER BY nazwisko;
```

## Zadanie 2
```sql
SELECT nazwisko, etat, nazwa
FROM pracownicy p INNER JOIN zespoly z ON p.id_zesp = z.id_zesp
WHERE p.id_zesp =
	(SELECT id_zesp
	FROM pracownicy
	WHERE nazwisko = 'BRZEZINSKI')
ORDER BY nazwisko;
```

## Zadanie 3
```sql
SELECT nazwisko, etat, TO_CHAR(zatrudniony, 'YYYY/MM/DD') AS zatrudniony
FROM pracownicy
WHERE zatrudniony =
	(SELECT MIN(zatrudniony)
	FROM pracownicy
	WHERE etat = 'PROFESOR');
```

## Zadanie 4
```sql
SELECT nazwisko, TO_CHAR(zatrudniony, 'YYYY/MM/DD') AS zatrudniony, id_zesp
FROM pracownicy
WHERE zatrudniony IN
	(SELECT MAX(zatrudniony)
	FROM pracownicy
	GROUP BY id_zesp)
ORDER BY zatrudniony;
```

## Zadanie 5
```sql
SELECT *
FROM zespoly
WHERE id_zesp NOT IN
	(SELECT id_zesp
	FROM pracownicy
	GROUP BY id_zesp);
```

## Zadanie 6
```sql
SELECT nazwisko
FROM pracownicy
WHERE etat = 'PROFESOR' AND
id_prac NOT IN
	(SELECT id_szefa
	FROM pracownicy
	WHERE etat = 'STAZYSTA');
```

## Zadanie 7
```sql
SELECT id_zesp, SUM(placa_pod) AS suma_plac
FROM pracownicy
GROUP BY id_zesp
HAVING SUM(placa_pod) =
	(SELECT MAX(SUM(placa_pod))
	FROM pracownicy
	GROUP BY id_zesp);
```

## Zadanie 8
```sql
SELECT nazwa, SUM(placa_pod) AS suma_plac
FROM pracownicy p RIGHT OUTER JOIN zespoly z ON p.id_zesp = z.id_zesp
GROUP BY nazwa
HAVING SUM(placa_pod) =
	(SELECT MAX(SUM(placa_pod))
	FROM pracownicy
	GROUP BY id_zesp);
```

## Zadanie 9
```sql
SELECT nazwa, COUNT(*) AS ilu_pracowników
FROM zespoly z INNER JOIN pracownicy p ON z.id_zesp = p.id_zesp
GROUP BY nazwa
HAVING COUNT(*) >
	(SELECT COUNT(*)
	FROM zespoly z INNER JOIN pracownicy p ON z.id_zesp = p.id_zesp
	GROUP BY nazwa
	HAVING nazwa = 'ADMINISTRACJA')
ORDER BY nazwa;
```

## Zadanie 10
```sql
SELECT etat
FROM pracownicy
GROUP BY etat
HAVING COUNT(*) > ANY
	(SELECT COUNT(*)
	FROM zespoly z INNER JOIN pracownicy p ON z.id_zesp = p.id_zesp
	GROUP BY nazwa
	HAVING nazwa = 'ADMINISTRACJA')
ORDER BY etat;
```

## Zadanie 11
```sql
SELECT etat,
	LISTAGG(nazwisko, ',')
	WITHIN GROUP (ORDER BY nazwisko) AS pracownicy
FROM pracownicy
GROUP BY etat
HAVING COUNT(*) > ANY
	(SELECT COUNT(*)
	FROM zespoly z INNER JOIN pracownicy p ON z.id_zesp = p.id_zesp
	GROUP BY nazwa
	HAVING nazwa = 'ADMINISTRACJA')
ORDER BY etat;
```

## Zadanie 12
```sql
SELECT p.nazwisko AS pracownik, s.nazwisko AS szef
FROM pracownicy p LEFT OUTER JOIN pracownicy s ON p.id_szefa = s.id_prac
WHERE s.placa_pod - p.placa_pod =
	(SELECT MIN(s.placa_pod - p.placa_pod)
	FROM pracownicy p LEFT OUTER JOIN pracownicy s ON p.id_szefa = s.id_prac);
```