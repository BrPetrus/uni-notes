# Funkcionalne vypocetne paradigma
program = vyraz + def. funkcii
vypocet = uprava vyrazu do jednoduhscieho tvaru
vysledok = hodnota

# Typy
typ = oznacenie pre mnozinu vsetkych hodnot danej kvality

cisla (Integer, Int, Float, Rational)
znaky (Char, String=[Char])
Bool (True a False)

Monomorfne typy: Bool -> Bool

Polymorfne typy = miesto konk. typu dam typovu premennu, kt. sa instancuje pri
pouziti funkcie: [a] -> Int

Kvalifikovane typy: Eq a => a -> Bool

Typove triedy obmedz. typ. premenne
Integral - celociselne
Num - numericke
Ord - usporiadatelne
Eq - porovnatelne

## Funkcne typy
Ak su $\alpha$  a $\beta$ typy, tak $\alpha$ -> $\beta$ je typ $\forall$ funkcii s
par. typu $\alpha$ a funkc. hod. $\beta$.

## Hodnotove konstuktory
[] :: [a]
(:) :: a -> [a] -> [a]

## Typove konstruktory
data nazovtypu = Hodnotovy konstruktor

## Odvodzovanie typov
TODO

# Redukcny krok
= uprava vyrazu, v ktorom sa nejaky jeho podvyraz nahradi zjednodusenym
podvyrazom

Upravovany vyraz ma tvar aplikacie funkcie na argumenty, upraveny vyraz ma tvar
pravej strany definicie tejto fukcie do ktorej su za formalne parametre dosadene
skutocne parametre.

# Redukcna strategia
= prepis, ktory urcuje, ktory podvyraz sa bude upravovat v nasledujucom
redukcnom kroku.

## Striktna redukcna strategia
nieco pod. ako ked riesime matematiku.

Pri uprave aplikacie F X najskor uplne upravime argument X a az potom upravujeme
vyraz F. Az nakoniec upravime cely vyraz F X. => Pri uprave teda postupujeme
zvnutra.

Pr.:
cube (3+5) ~> cube 8 ~> 8 * 8 * 8 ~> 64 * 8 ~> 512

## Normalna redukcna strategia
Postupne upravujem vyraz zvonka => Upravovanym podvyrazom je cely vyraz; ak uz
nejde takto upravit aplikaciu F X, upravime najskor F, pokial to nestaci k tomu,
aby sme mohli upravit F X, upravujeme ciatocny vyraz X, ale iba do tej miery,
aby sme mohli upravit vyraz F X.

Pr.:
cube (3+5) ~> (3+5)*(3+5)*(3+5) ~> 8 * (3+5) * (3+5) ~> 64 * (3+5) ~> 64 * 8 ~>
512

Tuto strategiu pouziva Haskell, ale hovori sa o **lenivom vyhodnoteni**, pretoze
sa vyhodnocuje len to, co je potrebne k dalsiemu vypoctu (False \and Hocico je
False, 0 * hocico = 0).

## Leniva redukcna strategia
To je normalna redukcna strategia, ale pamata si hodnoty upravenych podvyrazov a
ziadny s opakovanym vyskytom nevyhodnucujeme viac ako raz.

Vyuziva referencnu transparentnost. Nejde aplikovat na vyrazi s
vedlajsim efektom.

cube (3+5) ~> (3+5) * (3+5) * (3+5) ~> 8 * 8 * 8 ~> 64 * 8 ~> 512

Toto ide v Haskelli docielit, ak je rovnaky podvyraz vo vyraze zavedeny pomocou
lokalnej definicie.

## eta-redukcia
= vynechanie parametru
`daco x = daco_ine x` <=> `daco = dacoine`
pointwise ~> pointfree

## Referencna transparentnost
Vysledok vyhodnotenia vyrazu nezavisi na kontexte, v ktorom sa vyhodnocuje. Moze
mat vedlajsi efekt ale nesmie ovplyvnit vysledok. Haskell je ref.
transparentny!

## Vplyv zvolenej redukcnej strategie
Church-Rosserova věta 
Vysledna hodnota ukonceneho vypoctu vyrazu nezalezi na redukcnej strategii:
pokud vypocet skonci, je jeho vysledok vzdy rovnaky.

O perpetualite
Ak pre nejaky vyraz M existuje redukcna strategia, s pouzitim ktorej sa uprava
vyrazu M zacykli, tak sa tento vypocet zacykli i s pouzitim striktnej redukcnej
strategie.

O normalizacii
Ak pre nejaky vyraz M existuje redukcna strategia, s pouzitim ktorej sa uprava
vtrazu M nezacykli, tak sa tento vypocet nezacykli ani s pouzitim normalnej
redukcnej strategie.

# Funkcie vyssich radov
Funkcia je povazovana za funkciu vyssieho radu, pokial aspon jeden z jej
argumentov alebo vysledok, je znovu funkcia.

Kazdu funkciu s aspon 2 parametrami ide chapat ako funkciu vyssieho radu. Tie
potom mozeme ciastocne aplikovat.

Má-li funkce více formálních argumentů, částená aplikace probíhá vždy od
parametru nejvíce vlevo. (Pokud nepoužijeme funkci flip :: (a -> b -> c) -> b ->
a -> c)

Pokud chceme zabránit částečné apliakce, použijeme n-tici (funkce curry,
uncurry).

map, filter
operátorová sekce
akumulační seznamové funkce (foldl, foldr), pomocí kterých můžeme aplikovat pinární operaci na všechny prvky senamu navzájem; foldl (+) 0 [x_1, ..., x_n] = x_1 + ... + x_n
tvorba bezparametrové definice funkce využívá funkce flip, curry, uncurry, (.) a operátorové sekce; f x = (not . odd) x (definice s parametrem) vs. f = not . odd (bezparametrová definice)

# Lambda funkcie
V zasade ide o anonymne funkcie. Obvykle potrebujeme nejakej funkcii predat
funckciu, o ktoru inde v kode nestojime.
\x -> 2 * x  -- anonymna unarna funkcia


# other Notes
if daco then vyraz1 else vyraz 2
vyraz1 a vyraz2 musi mat rovnakt typ!!

# Q sort
```haskell
qSort :: Ord a => [a] -> [a]
qSort [] = []
qSort (p:s) = qSort [ x | x<-s, x<p ]
              ++ [p] ++
              qSort [ x | x<-s, x>p ]
```

# Factorial
factorial 0 = 1
factorial n = n * factorial (n-1)

# Fibonnaci
fibs = 0 : 1 : zipWith (+) fibs (tail fibs)
