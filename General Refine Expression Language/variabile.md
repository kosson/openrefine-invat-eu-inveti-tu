# Variabile

Variabilele sunt parte componentă a expresiilor GREL.

|numele variabilei|ce reprezintă?|
|:--|:--|
|`value`|Reprezintă valoarea din celula din rândul curent de pe coloana de bază; valoarea sa poate fi și `null`|
|`row`|Reprezintă rândul asupra căruia se fac operațiuni. Această variabilă are drept valoare un obiect a căror proprietăți sunt celulele de pe rând.|
|`row.record`|Reprezintă valoarea uneia sau a mai multor rânduri care au fost grupate împreună pentru a constitui o înregistrare unitară: `record`. Această valoare este un obiect.|
|`cells`|Reprezintă toate celulele de pe rândul de lucru. Numele coloanelor vor fi cheile proprietăților|
|`cell`|Reprezintă o celulă pe care se aplică operațiunile. Acestă valoare este un obiect.|
|`cell.recon`|Informație privind reconcilierea făcută asupra celulei ce a venit de la un serviciu de reconciliere sau de la un furnizor care oferă reconciliere pentru date.|
|`rowIndex`|Valoarea indexului pentru rândul curent pe care se operează|
|`columnName`|Numele coloanei pentru celula în care operezi curent. Este returnată ca șir de caractere|

## Rândul - `row`

Un rând sau o linie din fișier care este reprezentat ca un obiect. Proprietățile acestui obiect pot fi accesate cu operatorul punct sau cu paranteze pătrate la fel ca sintaxa de acces a proprietăților din obiectele JavaScript: `row.index` sa `row.["index"]`.

### `row.index`

Este indexul rândului care va fi prelucrat. Indexul rândurilor în General Refine Expression Language pornește de la valoarea 0.

### `row.cells`

Are drept valoare toate celulele rândului.

### `row.columnNames`

Oferă toate numele coloanelor rândului de lucru într-un array în care fiecare element este valoarea fiecărei celule din rând. O coloană poate fi apelată individual printr-o expresie de tipul `row.columnNames[2]`.

### `row.starred`

Are valoare boolean (true/false) și indică dacă rândului i s-a activat o steluță sau nu.

### `row.flagged`

Are valoare boolean (true/false) și indică dacă rândului i s-a activat fanionul sau nu.

### `row.record`

Este un obiect de tip `record` care, de fapt, este reprezentarea întregului rând.

## Celulele - `cells`

Acesta este un obiect care poate fi accesat și ca `row.cells`. Proprietățile sale sunt tot atâtea câte coloane există iar numele cheilor sunt numele coloanelor.

Accesarea unei singure proprietăți, precum în cazul: `cells.NrInventar`, are drept efect returnarea unui obiect cu toate proprietățile aferente celulei adresate din rândul de lucru.
Aici fii foarte atent pentru că în cazul în care denumirea coloanei este formată din mai multe cuvinte delimitate de spații, metoda pentru a accesa proprietatea va fi cea cu paranteze drepte: `cells["Nr Inventar"]`.

În cazul în care dorești să accesezi valoarea celulei aferente coloanei, vei folosi proprietatea `value` apelată prin sintaxă cu punct: `cells["Nr Inventar"].value`.

Când dorești să editezi toate valorile de pe o anumită coloană, pur și simplu introduci numele coloanei între ghilimele: "Nr Inventar". În acest moment, toate celulele de pe coloana aleasă vor fi afectate de orice transformare faci. Acest lucru poate fi realizat și prin folosirea instrumentului grafic `Facets` din interfața grafică.

## Celulă - `cell`

Obiectul `cell` are doar două proprietăți:

- `cell.value`, care oferă valoarea din respectiva celulă. Acestă valoare la rândul ei poate fi o valoare boolean, un șir de caractere, valoarea `null` sau o eroare.
- `cell.recon` este un obiect care conține rezultatul obținut din reconcilierea datelor pentru acea celulă.

## Reconciliere - `recon`

Un obiect care reprezintă un rezultat al concilierii de date are următoarele proprietăți, care descriu și oferă valori rezultate direct din procesul de reconciliere.

### Verdictul reconcilierii: `recon.judgment`

Această proprietate este un șir de caractere care poate fi una dintre următoarele variante: "matched", "new" și "none".

### O combinare reușită: `recon.matched`

Este o valoare boolean, care indică prin true/false dacă verdictul reconcilierii este "matched".

### Candidatul indicat pentru potrivire cu valoarea celulei: `recon.match`

Poate avea valoarea `null`, dacă nu s-a găsit niciun candidat pentru o potrivire cu valoarea celulei sau chiar valoarea candidatului în cazul în care există o potrivire.

În cazul în care există o potrivire, aceasta este accesibilă ca un obiect iar acest obiect are următoarele proprietăți la rândul său:

- `.id`
- `.name`
- `.type`

### Cel mai bun candidat din câți există - `recon.best`

Această proprietate poate avea valoarea `null` în cazul în care nu există sau un obiect reprezentând cea mai bună potrivire posibilă cu valaorea din celulă care s-a putut face.

Acest obiect rezultate, dacă există poate avea la rândul său următoarele proprietăți:

- `.id`
- `.name`
- `.type`
- `.score`

### Obiectul candidatului expune caracteristicile sale prin `recon.features`

Acesta este un obiect care reprezintă potrivirea pentru valoarea celulei și are următoarele proprietăți:

- `.typeMatch` (valoare boolean)
- `.nameMatch` (valoare boolean)
- `.nameLevenshtein` (o valoare numerică care indică cât de aproape este candidatul de cea mai bună variantă care merge cu valoarea din celulă. Un număr mai mare indică o distanță mai mare).
- `.nameWordDistance` (valoarea numerică a distanței de potrivire cu valoarea celulei).

### Un obiect care include trei candidați posibili: `recon.candidates`

Acest obiect asemănător cu array-urile are următoarele proprietăți:

- `.id`
- `.name`
- `.type`
- `.score`

Documentația oferă următorul exemplu de unificare a conținutului celor trei candidați într-unul singur: `forEach(cell.recon.candidates,v,v.id).join(",")`.

Să presupunem că dorești să accesezi cel mai bun candidat pentru valoarea unei celule. Ai următoarele expresii echivalente de lucru:

- `recon.best.id`
- `cell.recon.best.id`
- `row.cells["nume coloană"].recon.best.id`

## Înregistrare - `record`

Acesta este un obiect care cuprinde unu sau mai multe rânduri, care sunt grupate împreună.

Un exemplu de grupare:

|index|autor|titlu|an|
|:-|:-|:-|:-|
|1|Gib Mihăiescu|Grandiflora|1928|
|2||Rusoaica|1933|
|3||Donna Alba|1935|
|4|Mateiu Caragiale|Craii de Curtea-Veche|1928|
|5||Remember|1921|

Accesarea membrilor acestui obiect se poate face prin intermediul celor două sintaxe posibile: cea cu punct și cu paranteze pătrate: `record.autor` sau `record["titlu"]`.

Să vedem ce putem accesa ca valori apelând pproprietățile unui obiect `record`. Considerând exemplul oferit mai sus putem accesa următoarele informații.

### Indexul înregistrării - `record index`

O înregistrare are structurate intern informațiile după un index care pornește de la 0. Folosind `record index`, vom putea afla la ce poziție din index se face prelucrarea datelor.

### Celulele înregistrării - `record.cells`

Este un array cu toate celulele. Pentru înregistrarea de mai sus `row.record.cells.titlu.value` aplicată pe al doilea rând (primul rând este cel al numelor de coloană, de regulă), va returna un array cu [`"Grandiflora", "Rusoaica", "Donna Alba"]`.

### Indexul primului rând - `record.fromRowIndex`

Folosind această proprietate afli valoarea primului index al înregistrării.

### Indexului ultimului rând al înregistrării - `record.toRowindex`

Afli valoarea indexului ultimului element din înregistrare.
