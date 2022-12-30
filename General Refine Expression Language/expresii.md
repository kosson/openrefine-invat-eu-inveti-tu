----
version: 3.6
---
# Expresii

General Refine Expression Language este limbajul prin care comunici intențiile de modelare a datelor pachetului OpenRefine. GREL este proiectat să semene cu JavaScript. Majoritatea funcțiilor interne din JavaScript vor funcționa fără probleme în GREL. Unele sintaxe seamănă mai degrabă cu Python.

În GREL (General Refine Expression Language), expresiile sunt folosite pentru a transforma și pentru a crea date pe baza celor existente.

O expresie va fi aplicată tuturor rândurilor din set pentru o anumită valoare poziționată pe o anumită coloană.

## Valorile

Valorile sunt conținutul celulelor intr-un rând. Aceste valori pot fi accesate repetitiv fiecare dintre ele pentru întreg setul de date. Acest lucru se va face creându-se o expresie de prelucrare pentru valoare. Valoare (`value`) este ca un identificator pentru o variabilă, care se va modifica pe măsură ce se va parcurge întreg setul de date analizându-se rând după rând.

Să presupunem că avem următorul set:

|index|autor|titlu|an|
|:-|:-|:-|:-|
|1|Gib Mihăiescu|Grandiflora|1928|
|2|Mateiu Caragiale|Craii de Curtea-Veche|1928|

Acest set a fost constituit anterior. Poți să-l reconstuiești simplu deschizând LibreOffice Calc, introduci datele și salvezi ca CSV. Apoi deschizi OpenRefine și faci un proiect încărcând setul.

În OpenRefine arată astfel, după ce ai creat proiectul.

![](Set2GibSiMateiu.png)

Folosind Transform (Edit->Transform) putem modela conținutul celulelor de pe o coloană folosindu-ne de o expresie, care în cazul nostru aplică două funcții consecutive: `split`, care tratează valoarea unei celule ca pe un șir de caractere care poate fi modelat după unele dintre ele așa cum este spațiul și apoi `join`, care recompune textul folosing un caracter menționat.

![](Coloana-EditCells-Transform-Gib-Mihaiu.png)

Trebuie să completez aici cu câteva informații importante. Un șir de caractere asupra căruia i se aplică o funcție `split` va fi „spart” după caracterul sau secvența de caractere menționată ca argument al funcției split într-un array (valori indexate într-o structură de date container care se numește array și care le ține precum un șirag de mărgele). Dacă în cazul `Gib Mihăiescu` aveam șirul unitar, ca valoare șir în sine, imediat după aplicarea funcției split(" "), care menționează spațiul ca fiind caracterul după care se va face fragmentarea, va rezulta o structură de date indexată (array): `["Gib", "Mihăiescu"]`.

![](Autor-EditCells-Transform-split-join.png)

Te vei întreba care este avantajul? Acesta este că folosindu-te de array, ai acces la fiecare cuvânt în parte din șir apelându-l după indexul alocat automat începând cu 0. Cool, nu?

Am lămurit care-i treaba cu array-ul. Trebuie să completez că `join(", ")` este o funcție care face operațiunea inversă. Adică preia un array și unește într-un șir de caractere toate valorile din array, pe care le aranjează într-o succesiune delimitată de un caracter menționat ca argument al funcției între ghilimele.

Hai să facem uz de avantajele pe care le expune lucrul cu un array și să extragem doar un fragment din șirul de caractere pe care-l avem ca valoare într-o celulă: `value.split(" ")[0]`.

Rezultatul este că obținem doar un singur cuvânt reducând astfel un șir întreg doar la un singur fragment util.

## Cunoștințe de bază

Am menționat faptul că OpenRefine folosește o sintaxă care seamănă cu cea a JavaScript-ului. Cei care cunosc JavaScript și ceva Python, nu vor avea probleme să înțeleagă expresiile folosite pentru a modela datele. Pentru cei care nu cunosc programare, nu aveți a vă teme de nimic. Voi încerca să explic noțiunile cu care lucrăm și tot ceea ce vom face va fi bogat ilustrat pentru a compensa în mod util.

Pentru avea cel mai bun start, vom arunca o privire către valorile pe care le vom manipula.

## Controale

GREL oferă mecanisme de control pentru a realiza condiții sau looping, dar spre deosebire de funcții, argumentele lor nu sunt evaluate înainte de a fi rulată respectivul control. Un control poate decide care parte a codului poate să o execute și poate să modifice valorile cu care se leagă la mediu.

### if(e, eTrue, eFalse)

Expresia `e` este evaluată la o valoare. Dacă valoarea este `true`, va fi evaluată expresia de la `eTrue` iar rezultatul va fi valoarea întregii expresii `if()`. În caz contrar, este evaluată expresia `eFalse` iar acel rezultat va fi valoarea.

De exemplu:
- `if("internationalization".length() > 10, "șir de caractere mare", "șir de caractere mic")` va returna valoarea `șir de caractere mare`;
- `if(mod(37, 2) == 0, "par", "impar")` va returna `impar`.

In exemplu în care este simulat un `switch case`:

```grel
if(value == 'Place', 'http://www.example.com/Location',

    if(value == 'Person', 'http://www.example.com/Agent',

    if(value == 'Book', 'http://www.example.com/Publication',

null)))
```

### with(e1, variable v, e2)

Evaluează expresia `e1` apoi atribuie valoarea lui `variable v` și apoi evaluează expresia de la `e2` pentru care returnează rezultatul.

De exemplu:

- `with("european union".split(" "), a, a.length())` pentru care returnează `2`;
- `with("european union".split(" "), a, forEach(a, v, v.length()))` pentru care returnează `[8, 5]`;
- `with("european union".split(" "), a, forEach(a, v, v.length()).sum() / a.length())` pentru care returnează `6.5`.

### filter(e1, v, e test)

Evaluează expresia `e1` la un array pe care îl va considera date de lucru. Apoi, pentru fiecare element va trimite valoarea în `v`, va evalua expresia `e test` care trebuie să returneze un `boolean` la final. Dacă valoarea este `true`, va reține (face push) valoarea lui `v` în array-ul care rezultă.

De exemplu:

- `filter([ 3, 4, 8, 7, 9 ], v, mod(v, 2) == 1)` returnează array-ul `[ 3, 7, 9 ]`.

### forEach(e1, v, e2)

Evaluează expresia `e1` la un array. Apoi, pentru fiecare element al array-ului, va trimite valoarea lui `v`. Se va continua prin evaluarea expresiei `e2` și se va trimite rezultatul (face push) în array-ul care este rezultatul. În cazul în care `e1` este un obiect JSON, `forEach` va itera peste cheile sale. 

Exemplu:

- `forEach([ 3, 4, 8, 7, 9 ], v, mod(v, 2))` returnează array-ul `[ 1, 0, 0, 1, 1 ]`.


### forEachIndex(e1, i, v, e2)

Mai întâi este evaluată expresia `e1` care va fi redusă la un array. Apoi, pentru fiecare element, valoarea indexului său va fi trimisă în variabila `i`, iar valoarea în variabila `v`. Apoi, este evaluată expresia `e2` și la final rezultatul este adăugat în array-ul rezultat (se face push).

De exemplu:

- `forEachIndex([ "anne", "ben", "cindy" ], i, v, (i + 1) + ". " + v).join(", ")` a cărui rezultat este array-ul transformat în șir prin aplicarea lui `join`: `1. anne, 2. ben, 3. cindy`.

### forRange(n from, n to, n step, v, e)

Funcția iterează valoarea care este dată lui `v` începând cu `n from` în pași de dimensiunea specificată la `n step` cu limită setată (*până la*) prin valoarea lui `n to`. La fiecare iterație va evalua expresia `e` și va introduce rezultatul  în array-ul care se formează ca valoare returnată.

### forNonBlank(e, v, eNonBlank, eBlank)

Evaluează expresia de la `e`. Dacă nu este o valoare blank, `forNonBlank()` va trimite valoarea în variabila `v`, va urma evaluarea expresiei `eNonBlank` și va returna rezultatul. În caz contrar, va fi evaluată expresia de la `eBlank` și va returna acel rezultat.

Spre deosebire de alte funcții GREL care încep cu *for* aceasta nu este iterativă. Este doar o sintaxă prescurtată pentru a ajunge la același rezultat prin utilizarea funcției `isNonBlank()` într-o expresie `if`.


### isBlank(e), isNonBlank(e), isNull(e), isNotNull(e), isNumeric(e), isError(e)

Evaluează expresia `e` și returnează un boolean. Adu-ți mereu aminte că acestea sunt controale și nu pot fi utilizare ca metode; `e.isX()` nu va funcționa.

De exemplu:

- `isBlank("abc")` returnează `false`;
- `isNonBlank("abc")` returnează `true`;
- `isNull("abc")` returnează `false`;
- `isNotNull("abc")` returnează `true`;
- `isNumeric(2)` returnează `true`;
- `isError(1)` returnează `false`;
- `isError("abc")` returnează `false`;
- `isError(1 / 0)` returnează `true`

## Resurse

- Video
	- https://www.youtube.com/watch?v=wGVtycv3SS0
	
- [General Refine Expression Language](https://openrefine.org/docs/manual/grel)
- [Library Carpentry: OpenRefine](https://librarycarpentry.org/lc-open-refine/)