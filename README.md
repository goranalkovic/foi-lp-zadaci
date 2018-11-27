# foi-lp-zadaci
Rješeni primjeri zadataka iz Logičkog programiranja 2018.

Naveden je svaki zadatak uz opis, primjere i rješenje.

# Kolokvij 1

## Zadatak 1

Bez korištenja ugrađenih predikata za rad s listama implementirajte predikat `pomnozi/3` koji će elemente zadane liste brojeva pomnoziti s proizvoljnim parametrom.

### Primjeri
```
| ?- pomnozi( [1,2,3], L, 2 ).

L = [2,4,6];

no
```
```
| ?- pomnozi( [1,2,3], L, 3 ).

L = [3,6,9];

no
```
```
| ?- pomnozi( [3,2,1], L, 3 ).

L = [9,6,3];

no
```

### Rješenje

```Prolog
pomnozi([], [], _).
pomnozi([G|R], [G1|R1], Br) :-
    pomnozi(R, R1, Br),
    G1 is G * Br.
```

## Zadatak 2

Bez korištenja ugrađenih predikata implementirajte predikat `zbroji_liste/3` koji će za dvije zadane liste jednake duljine u treću upisati zbrojeve njihovih elemenata.

### Primjeri

```
| ?- zbroji_liste( [1,2,3], [4,5,6], L ).

L = [5,7,9];

no
```
```
| ?- zbroji_liste( [1,2,3,4], [3,2,1], L ).

no
```

### Rješenje

```Prolog
zbroji_liste([], [], []).
zbroji_liste([G|R], [G1|R1], [G2|R2]) :-
    zbroji_liste(R, R1, R2),
    G2 is G + G1.
```

## Zadatak 3

Bez korištenja ugrađenih predikata, implementirajte predikat `manji_od/3` koji će iz zadane liste i proizvoljnog broja konstruirati listu koja će se sastojati samo od onih elemenata iz zadane liste koji su manji od zadanog broja.

### Primjeri

```
| ?- manji_od( [ 1, 2, 3, 4, 2, 3, 4, 5 ], 3, X ).

X = [1,2,2];

no
```
```
| ?- manji_od( [ 1, 2, 3, 4, 2, 3, 4, 5 ], 4, X ).

X = [1,2,3,2,3];

no
```
```
| ?- manji_od( [ 1, 2, 3, 4, 2, 3, 4, 5 ], 1, X ).

X = [];

no
```

### Rješenje

```Prolog
manji_od([], _, []).
manji_od([G|R], Br, Rez) :-
    (
        G < Br 
        -> Rez = [G|R1]
        ;  Rez = R1
    ),
    manji_od(R, Br, R1).
```

## Zadatak 4

Pomoću lista implementirajte predikat `dan_u_tjednu/2` koji će za zadani broj vratiti odgovarajući dan u tjednu i obrnuto.

### Primjeri

```
| ?- dan_u_tjednu( 2, X ).

X = uto;

no
```
```
| ?- dan_u_tjednu( Y, cet ).

Y = 4;

no
```

### Rješenje

```Prolog
TODO
```

## Zadatak 5

Bez korištenja ugrađenih predikata potrebno je implementirati predikat `podlista/2` koji će uspjevati ako je prvi parametar podlista drugog parametra.

### Primjeri

```
| ?- podlista( [1,2,3], [1,2,3,4,5] ).

yes
```
```
| ?- podlista( X, [1,2,3] ).

X = [];

X = [1];

X = [1,2];

X = [1,2,3];

no
```
```
| ?- podlista( [1,2,4], [1,2,3,4] ).

no
```

### Rješenje

```Prolog
podlista([],L).
podlista([G|R1], [G|R2]) :- 
	podlista(R1,R2).
```

## Zadatak 6

Bez korištenja ugrađenih predikata implementirajte predikat `pomnozi/2` čiji je prvi argument lista brojeva (potrebno je provjeriti je li riječ o brojevima), a drugi umnožak brojeva u listi.

### Primjeri

```
| ?- pomnozi([a,1,2],X).

no
```
```
| ?- pomnozi([4,3,2],X).

X = 24;

no
```
```
| ?- pomnozi([1.25,44],X).

X = 55.0000;

no
```

### Rješenje

```Prolog
pomnozi([], 1).
pomnozi([H|T], Rez) :- 
    number(H),    
    pomnozi(T, Rez1), 
    Rez is H * Rez1.
```

## Zadatak 7

Neka je zadan sljedeći logički program:

```
ivek voli barica.
barica voli joza.
barica voli ivek.
joza voli stefica.
stefica voli joza.

ljubavni_par( X, Y ) :-
  X voli Y,
  Y voli X.
```

Implementirajte odgovarajući operator `voli` kako bi program radio, te predikat `parovi/1` koji će vraćati listu svih ljubavnih parova na sljedeći način:

```
| ?- parovi(L).

L = [par(barica,ivek),par(ivek,barica),par(joza,stefica),par(stefica,joza)];

no
```

### Rješenje

```Prolog
:- op(500, xfx, voli).

ivek voli barica.
barica voli joza.
barica voli ivek.
joza voli stefica.
stefica voli joza.

ljubavni_par( X, Y ) :-
    X voli Y,
    Y voli X.

parovi(L) :-
    setof(par(X, Y), ljubavni_par(X, Y), L).
```

## Zadatak 8

Potrebno je implementirati operatore `vminus` i `vjednako` te predikate za oduzimanje vektora dimenzije 5.

### Primjer

```
| ?- [1,2,3,4,5] vminus [5,4,3,2,1] vjednako Rezultat.

Rezultat = [-4,-2,0,2,4];

no
```

### Rješenje

```Prolog
:- op(500, yfx, vminus).
:- op(600, xfx, vjednako).

oduzmi([T1], [T2], [T3]) :- 
    T3 is T1 - T2.

oduzmi([H1|T1], [H2|T2], [H3|T3]) :- 
    oduzmi(T1, T2, T3), 
    H3 is H1 - H2.

X vminus Y vjednako Z :- oduzmi(X, Y, Z).
```
