# Introducere

OpenRefine este un software cu sursă eschisă care are la origini o creație Google care se numea GoogleRefine.

OpenRefine este scris în Java și rulează fără a fi online. Chiar dacă OpenRefine rulează un server la momentul executării servind o pagină de web, datele prelucrate și acest server nu comunică pe Internet. Datele sunt doar pe computerul local.

## Structura datelor

TODO: Completează și fă o anatomie a modalităților de reprezentare a datelor într-un set așa cum le folosește OpenRefine. [\[ILUSTREZĂ!]]

Materialul de lucru de zi cu zi sunt datele agregate în formate tabelare care dispun valori la intersecția unei coloane cu un rând. Seamănă foarte mult modelul de așezare cu un grafic cartezian pentru care aveam o intersecție a lui x cu y într-un punct.

||coloană|
|:-|:-|
|rând|valoare|

Cel mai simplu mod de a reprezenta date este într-o structură tabelară folosind apoi un format de reprezentare a dispunerii datelor care utilizează caractere ce delimitează seriile de date. De exemplu, unul dintre cele mai utilizate formate  în acest moment este Comma Separated Values, pe scurt CSV. Mai pe românește, un CSV este un mod de a pune date într-un fișier text în care fiecare rând este un rând de date având valorile celulelor delimitate prin caracterul virgulă.

```text
numeCol1,numeCol2
10, 20
ceva, altceva
```

Alte fișiere pe care le poate importa OpenRefince sunt menționate chiar la deschiderea paginii de interacțiune. În mare sunt toate cele care oferă o schemă de agregare a datelor după un anumit standard: TSV, CSV, *SV, Excel (.xls and .xlsx), JSON, XML, RDF și XML.

OpenRefine a fost extins și în domeniul Web-ului Semantic, fiind capabil să gestioneze triple RDF prin extensiile sau versiunile derivate ale software-ului.
