## Zadanie 1
```sql
select * from zespoly;
```

## Zadanie 2
```sql
select * from pracownicy;
```

## Zadanie 3
```sql
select nazwisko, placa_pod*12 as roczna_placa from pracownicy order by nazwisko asc;
```

## Zadanie 4
```sql
select nazwisko, etat, placa_pod + coalesce(placa_dod, 0) 
as miesieczne_zarobki from pracownicy order by miesieczne_zarobki desc;
```

## Zadanie 5
```sql
select * from zespoly order by nazwa asc;
```

## Zadanie 6
```sql
select unique nazwa from etaty;
```

## Zadanie 7
```sql
select * from pracownicy where etat = 'ASYSTENT' order by nazwisko asc;
```

## Zadanie 8
```sql
select * from pracownicy where id_zesp in(40, 30) order by placa_pod desc;
```

## Zadanie 9
```sql
select * from pracownicy where placa_pod between 300 and 800 order by nazwisko asc;
```

## Zadanie 10
```sql
select nazwisko, etat, id_zesp from pracownicy 
where nazwisko like '%SKI' order by nazwisko asc;
```

## Zadanie 11
```sql
select id_prac, id_szefa, nazwisko, placa_pod from pracownicy 
where placa_pod > 1000 and id_szefa is not NULL;
```

## Zadanie 12
```sql
select nazwisko, id_zesp from pracownicy 
where id_zesp = 20 and (nazwisko like 'M%' or nazwisko like '%SKI') 
order by nazwisko asc;
```

## Zadanie 13
```sql
select nazwisko, id_zesp from pracownicy 
where id_zesp = 20 and (nazwisko like 'M%' or nazwisko like '%SKI') 
order by nazwisko asc;
```

## Zadanie 14
```sql
select nazwisko, etat, placa_pod, placa_dod from pracownicy 
where placa_pod + coalesce(placa_dod, 0) > 1000 order by etat asc, nazwisko asc;
```

## Zadanie 15
```sql
select nazwisko || ' pracuje od ' || zatrudniony || ' i zarabia ' || placa_pod 
as profesorowie from pracownicy where etat = 'PROFESOR' order by placa_pod desc;
```
