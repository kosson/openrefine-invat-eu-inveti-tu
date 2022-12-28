---
version: 3.6
---

# Funcții GREL

Există câteva prescurtări de care trebuie ținut seama atunci când va fi consultată această documentație. Prescurtările indică tipul de date folosit.

- `s` pentru string;
- `b` pentru boolean;
- `n` pentru număr;
- `d` pentru date calendaristice;
- `a` pentru array;
- `p` pentru un șablon regex;
- `o` pentru obiect (adică orice tip de date care pot include `null` și `error`);

Dacă o funcție poate prelucra mai multe tipuri de date sau returnează mai multe tipuri de date, acestea vor fi menționate precum în `s or a` sau `o` care este un obiect, asta însemnând mai multe tipuri de date (string, boolean, date, number, etc.). Mai sunt menționate niște prescurtări cum ar fi `sub` pentru substring sau `sep` pentru caractere folosite drept separatoare. Pentru argumentele care sunt opționale, va fi menționat `optional`. Acolo unde funcția acceptă drept argument un șir de caractere sau un șablon regex, vei pune între ghilimele duble. Dacă dorești să folosești notația specifică pentru un regex, vei poziționa șablonul între forward slashes.

## Funcții boolean

### and(b1, b2, ...)

### or(b1, b2, ...)

### not(b)

### xor(b1, b2, ...)

## Funcții aplicate pe șiruri de caractere

### length(s)

Returnează numărul caracterelor din șirul pasat ca `s`.

### toString(o, string format (optional))

Ia orice valoare (string, number, date, boolean, error, null) și returnează o variantă string. Poți converti numere la șiruri ș.a.m.d. Dacă aplici funcția pe o celulă a cărei valoare este `null`, vei obține `"null"`.

### startsWith(s, sub)

### endsWith(s, sub)

### contains(s, sub or p)

### toLowercase(s)

### toUppercase(s)

### toTitlecase(s)

### trim(s)

### strip(s)

### chomp(s, sep)

### substring(s, n from, n to (optional))

### slice(s, n from, n to (optional))

### get(s, n from, n to (optional))

### indexOf(s, sub)

### lastIndexOf(s, sub)

### replace(s, s or p find, s replace)

### replaceChars(s, s find, s replace)

### replaceEach(s, a find, a replace)

### find(s, sub or p)

### match(s, p)

### toNumber(s)

### split(s, s or p sep, b preserveTokens (optional))

### splitByLengths(s, n1, n2, ...)

### smartSplit(s, s or p sep (optional))

### splitByCharType(s)

### partition(s, s or p fragment, b omitFragment (optional))

### rpartition(s, s or p fragment, b omitFragment (optional))

### diff(s1, s2, s timeUnit (optional))

### escape(s, s mode)

### unescape(s, s mode)

### encode(s, s encoding)

### decode(s, s encoding)

### md5(o)

### sha1(o)

### phonetic(s, s encoding)

### reinterpret(s, s encoderTarget, s encoderSource)

### fingerprint(s)

### ngram(s, n)

### ngramFingerprint(s, n)

### unicode(s)

### unicodeType(s)

### detectLanguage(s) 


## Funcții aplicate pe JSON, HTML sau XML

### jsonize(o)

### parseHtml(s)

### parseXml(s)

### select(element, s)

### htmlAttr(s, element)

### xmlAttr(s, element)

### htmlText(element)

### xmlText(element)

### wholeText(element)

### innerHtml(element)

### innerXml(element)

### ownText(element)

### parent(element)

### parseUri(s)

## Funcții aplicate pe array-uri

### length(a)

### slice(a, n from, n to (optional))

### get(a, n from, n to (optional))

### inArray(a, s)

### reverse(a)

### sort(a)

### sum(a)

### join(a, sep)

### uniques(a)

## Funcții aplicate pe date calendaristice

### now()

### toDate(o, b monthFirst, s format1, s format2, ...)

### diff(d1, d2, s timeUnit)

### inc(d, n, s timeUnit)

### datePart(d, s timeUnit)

### timeSinceUnixEpochToDate(duration, scale)

## Funcții aplicate pe expresii matematice

### abs(n)

### acos(n)

### asin(n)

### atan(n)

### atan2(n1, n2)

### ceil(n)

### combin(n1, n2)

### cos(n)

### cosh(n)

### degrees(n)

### even(n)

### exp(n)

### fact(n)

### factn(n1, n2)

### floor(n)

### gcd(n1, n2)

### lcm(n1, n2)

### ln(n)

### log(n)

### max(n1, n2)

### min(n1, n2)

### mod(n1, n2)

### multinomial(n1, n2 …(optional))

### odd(n)

### pow(n1, n2)

### quotient(n1, n2)

### radians(n)

### random(n lowerBound, n upperBound)

### round(n)

### sin(n)

### sinh(n)

### sum(a)

### tan(n)

### tanh(n)

## Diverse alte funcții

### type(o)

### facetCount(choiceValue, s facetExpression, s columnName)

### hasField(o, s name)

### coalesce(o1, o2, o3, ...)

### cross(cell, s projectName (optional), s columnName (optional))

## Resurse

- [GREL functions | openrefine.org/docs](https://openrefine.org/docs/manual/grelfunctions)
- [Recipes | github.com/OpenRefine/OpenRefine/](https://github.com/OpenRefine/OpenRefine/wiki/Recipes)