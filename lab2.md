## Zadanie 1
```sql
select nazwisko, SUBSTR(etat, 1, 2) || id_prac as kod from pracownicy;
```

## Zadanie 2
```sql
select nazwisko, translate(nazwisko, 'KLM', 'XXX') as wojna_literom from pracownicy;
```

## Zadanie 3
```sql
select nazwisko from pracownicy where substr(nazwisko, 1, length(nazwisko)/2) like '%L%';
```

## Zadanie 4
```sql
select nazwisko, round(placa_pod*1.15, 0) as podwyzka from pracownicy;
```

## Zadanie 5
```sql
select nazwisko, placa_pod, placa_pod*0.2 as inwestycja, 0.2 * placa_pod*power(1.1, 10) as kapital, 0.2 * placa_pod*power(1.1, 10) - placa_pod*0.2 as zysk from pracownicy;
```

## Zadanie 6
```sql
select nazwisko, zatrudniony, extract(year from(date '2000-01-01' - zatrudniony) year to month) as staz_w_2000 from pracownicy;
```

## Zadanie 7
```sql
select nazwisko, to_char(zatrudniony, ('fmMONTH'||', '||'DD YYYY')) as data_zatrudnienia from pracownicy where id_zesp = 20;
```

## Zadanie 8
```sql
select to_char(sysdate, 'day') as dzis from dual;
```

## Zadanie 9
```sql
select nazwa, adres, case adres when 'PIOTROWO 3A' then 'Nowe Miasto' when 'STRZELECKA 14' then 'Stare Miasto' when 'MIELZYNSKIEGO 30' then 'Stare Miasto' when 'WLODKOWICA 16' then 'Grunwald' end as dzielnica from zespoly;
```

## Zadanie 10
```sql
select nazwisko, placa_pod, case when placa_pod < 480 then 'ponizej 480' when placa_pod = 480 then 'dokladnie 480' when placa_pod > 480 then 'powyzej 480' end as prog from pracownicy order by placa_pod desc;
```