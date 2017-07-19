# API pentru serviciile de reconciliere

Este un serviciu web.
Acest serviciu primește un fragment de text care este valoarea dintr-o celulă și câteva detalii suplimentare. Ceea ce returnează este o listă ierarhizată cu potențiale potriviri, care se numesc oficial „candidați”. Trebuie menționat faptul că ceea ce se returnează de către serviciul de reconciliere, nu este neapărat textul exact al celulei sau a fragmentului de text pentru care a fost solicitată o potrivire externă. Din acest comportament vine și ideea de reconciliere a datelor, în fapt, atingerea celei mai bune soluții, a celei mai apropiate formule ca și conținut cu cel de la care s-a cerut concilierea. Un exemplu rapid ar fi „ciclopul” pentru care am putea avea următoarele variante de conciliere: „ciclopul” ca și ființă mitică, dar și „Ciclop” care este numele parcării din centrul Bucureștiului.

Entitățile care sunt alăturate sursei pentru care se caută potriviri, sunt, de regulă purtătoarele unor identificatori bine stabiliți, care aparțin unor domenii rezervate pentru care există reguli de alcătuire și atribuire a identificatorilor.

Grija principală a serviciului de reconciliere este aceea de a găsi candidații cei mai buni în baze de date accesate în extern. Poți să-ți imaginezi această activitate ca aceea a unor detectivi care pe o tablă de plută leagă fire roșii pornind din centru de la un mobil investigat către alte artefacte care sunt tot atâtea piste. mergând pe această analogie, noi în centrul tablei putem avea:

- un fragment de text;
- un anumit tip de entitate, care poate ajuta mult la restrângerea cu mai multă acuratețe la entitățile de interes. OpenRefine nu definește aceste tipuri, dar serviciul poate face acest lucru;
- opțional, o listă de proprietăți cu valorile aferente folosite pentru a rafina căutarea. În cazul unei baze de date cu monografii, autorul în combinație cu data publicării pot constitui indicii foarte importante pentru o țintire eficientă a informațiilor referitor la o anumită operă. Aceste informații vor fi trimise serviciului de reconciliere. Și în acest caz, OpenRefine nu definește ce este aceea o proprietate, acesta căzând în sarcina serviciului de reconciliere.

Serviciile de reconciliere lucrează cu date formatate folosind JSON (JavaScript Object Notation), iar API-urile sunt accesate prin apeluri REST care folosesc protocolul HTTP. Am menționat API-ul Wikidata care este conform cerințelor de reconciliere cu OpenRefine. Să-i facem o mică anatomie.

- Avem un URL de bază: https://tools.wmflabs.org/openrefine-wikidata/en/api pe care-l putem accesa direct și vor fi returnate câteva informații deja despre natura acestui API, dar scopul este de a-l trata ca pe o poartă de schimb între OpenRefine și seturile de date ale Wikidata.
- În continuare se poate opta pentru o completare interactivă cu informații pe care ți le va solicita OpenRefine.
- Poți opta și pentru previzualizarea datelor aduse de serviciul de reconciliere.
