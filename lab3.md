## Zadanie 1
```sql
SELECT MIN(placa_pod) AS minimum, MAX(placa_pod) AS maksimum, (MAX(placa_pod)- MIN(placa_pod)) AS roznica FROM pracownicy;
```

## Zadanie 2
```sql
SELECT etat, AVG(placa_pod) AS srednia FROM pracownicy GROUP BY etat;
```

## Zadanie 3
```sql
SELECT COUNT(etat) AS profesorowie FROM pracownicy WHERE etat = 'PROFESOR';
```

## Zadanie 4
```sql
SELECT id_zesp, SUM(placa_pod)+SUM(placa_dod) AS placa FROM pracownicy GROUP BY id_zesp ORDER BY id_zesp;
```

## Zadanie 5
```sql
SELECT MAX(SUM(placa_pod)+SUM(placa_dod))AS maks_sum_placa FROM pracownicy GROUP BY id_zesp;
```

## Zadanie 6
```sql
SELECT id_szefa, MIN(placa_pod) AS minimalna FROM pracownicy WHERE id_szefa IS NOT NULL GROUP BY id_szefa ORDER BY minimalna DESC;
```

## Zadanie 7
```sql
SELECT id_zesp, COUNT(*) AS ilu_pracuje FROM pracownicy GROUP BY id_zesp ORDER BY ilu_pracuje DESC;
```

## Zadanie 8
```sql
SELECT id_zesp, COUNT(*) AS ilu_pracuje FROM pracownicy GROUP BY id_zesp HAVING COUNT(*) > 3 ORDER BY ilu_pracuje DESC;
```

## Zadanie 9
```sql
SELECT id_prac FROM pracownicy WHERE id_prac IN (SELECT id_prac FROM pracownicy GROUP BY id_prac HAVING COUNT(*) > 1);
```

## Zadanie 10
```sql
SELECT etat, AVG(placa_pod) AS srednia, COUNT(*) AS liczba FROM pracownicy WHERE zatrudniony < DATE '1990-01-01' GROUP BY etat ORDER BY etat;
```

## Zadanie 11
```sql
SELECT id_zesp, etat,  FLOOR(AVG(coalesce(placa_pod+placa_dod, placa_pod, placa_dod, 0)))AS srednia, FLOOR(MAX(coalesce(placa_pod+placa_dod, placa_pod, placa_dod, 0)))AS maksymalna FROM pracownicy WHERE etat = 'PROFESOR' OR etat = 'ASYSTENT' GROUP BY id_zesp, etat ORDER BY id_zesp, etat;
```

## Zadanie 12
```sql
SELECT EXTRACT(YEAR FROM zatrudniony) AS rok, COUNT(*) AS ilu_pracownikow FROM pracownicy GROUP BY EXTRACT(YEAR FROM zatrudniony) HAVING COUNT(*) > 0 ORDER BY rok;
```

## Zadanie 13
```sql
SELECT LENGTH(nazwisko) AS ile_liter, COUNT(*) AS w_ilu_nazwiskach FROM pracownicy GROUP BY LENGTH(nazwisko) HAVING COUNT(*) > 0 ORDER BY ile_liter;
```

## Zadanie 14
```sql
SELECT COUNT(*) AS ile_nazwisk_z_A FROM pracownicy WHERE INSTR(nazwisko,'a') > 0 OR INSTR(nazwisko,'A') > 0;
```

## Zadanie 15
```sql
SELECT distinct
	(SELECT COUNT(*) FROM pracownicy WHERE INSTR(nazwisko,'a') > 0 OR 
		INSTR(nazwisko,'A') > 0) AS ile_nazwisk_z_A,
	(SELECT COUNT(*) FROM pracownicy WHERE INSTR(nazwisko,'e') > 0 OR 
		INSTR(nazwisko,'E') > 0) AS ile_nazwisk_z_E
FROM pracownicy;
```

## Zadanie 16
```sql
SELECT id_zesp, SUM(placa_pod) AS suma_plac,
	LISTAGG(nazwisko||':'||placa_pod,';')
	WITHIN GROUP (ORDER BY nazwisko) AS pracownicy FROM pracownicy GROUP BY id_zesp ORDER BY id_zesp;
```