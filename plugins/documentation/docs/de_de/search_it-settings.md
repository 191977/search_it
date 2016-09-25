#Konfiguration

- [Wartung](#wartung)
- [Einstellungen](#einstellungen)
    - [Suchmodus](#einstellungen-suchmodus)
    - [Suchergebnis](#einstellungen-suchergebnis)
    - [Quelle](#einstellungen-quelle)
- [Plaintext](#plaintext)
- [Search Highlighter](#search_highlighter)

<a name="wartung"></a>
# Wartung

Um die Suche in Betrieb zu nehmen, sollten zun�chst alle gew�nschten Einstellungen vorgenommen werden und anschlie�end der Suchindex eingerichtet werden.

> Tipp: Die hier definierten Sucheinstellungen k�nnen auch direkt an der `search_it`-Klasse vorgenommen bzw. �berschrieben werden, um mehrere Suchen auf einer Seite umzusetzen.

Aktion | Erl�uterung
------ | ------
Index vollst�ndig erstellen | Die Index-Tabelle wird gel�scht und neu aufgebaut.
Index schrittweise erstellen | F�hrt die Indexierung in mehreren Schritten aus, um die Skriptlaufzeit (max_execution_time) gering zu halten. Artikel und Medien werden einzeln indexiert, Datenbankeintr�ge in 100er-Schriten.
Suchcache l�schen | Wenn eine Neuindexierung nicht erforderlich ist, kann auch ausschlie�lich der Cache gel�scht werden.
Keyword-Index leeren | L�scht alle Keywords, die bei der Indexierung oder �ber Suchanfragen gesammelt wurden. Da diese Keywords z. B. f�r die �hnlichkeitssuche gebraucht werden, sollten diese nur in Ausnahmef�llen gel�scht werden.
Statistik l�schen | Setzt die Statistik zur�ck. Die Anzahl aller gesuchten Begriffe wird auf 0 gesetzt.

<a name="einstellungen"></a>
# Einstellungen

<a name="einstellungen-suchmodus"></a>
## Suchmodus

Dies sind die Standard-Einstellungen f�r jede Suche. 

> Tipp: In den erweiterten Beispielen wird erkl�rt, wie das Suchobjekt mit eigenen Parametern �berschrieben werden kann. So lassen sich mehrere Suchen in einer Website realisieren, bspw. eine Produktsuche oder eine Mitarbeiter-Suche.

### Suchmodi

#### Logischer Suchmodus

Wenn mehr als ein Begriff in das Suchfeld eingegeben wird, ...

Option | Erl�uterung
------ | ------
`Konjunktive Suche (AND)` | ... m�ssen beide Begriffe im Treffer vorkommen.
`Disjunktive Suche (OR)` | ... gen�gt einer von beiden Begriffen.

#### Textmodus

Der Textmodus besagt, welche Inhalte f�r die Suche auf einer Seite verarbeitet werden.

Option | Erl�uterung
------ | ------
Durchsuche Text ohne HTML-Tags (Plain) | durchsuche ausschlie�lich Text (empfohlen)
Durchsuche Text mit HTML-Tags (HTML) | durchsucht auch HTML-Code und Attribute
Durchsuche beides (HTML und Plain) |

> Tipp: In der Datenbank `rex_search_it_index` werden die indexierten Varianten `plaintext` und `unchangedtext` abgelegt.

#### �hnlichkeitssuche

Bei der �hnlichkeitssuche werden �hnliche Begriffe dem gesuchten Begriff zugeordnet. Gleichklingende bekommen dabei einen gleichen Code. Beispiele hierf�r sind:
* Tippfehler: `Standard` vs. `Standart`
* Verwechslungen: `Maier` vs. `Meyer`

Option | Erl�uterung
------ | ------
Deaktivieren | Es werden nur exakte Treffer angezeigt.
Soundex | Gleichklingende W�rter f�hren zu einem Treffer. Verwendet den [Soundex-Algorithmus](https://de.wikipedia.org/wiki/Soundex). 
Metaphone | Gleichklingende W�rter f�hren zu einem Treffer. [Metaphone](https://de.wikipedia.org/wiki/Metaphone) eignet sich f�r englische Begriffe.
K�lner Phonetik | Gleichklingende W�rter f�hren zu einem Treffer. [K�lner Phonetik](https://de.wikipedia.org/wiki/K%C3%B6lner_Phonetik) eignet sich f�r deutsche Begriffe.
Alle | �berpr�ft Soundex, Metaphone und K�lner Phonetik nach Treffern.
Die �hnlichkeitssuche auch dann durchf�hren, wenn Ergebnisse vorhanden sind? | Ausschalten, um die �hnlichkeitssuche nur dann zu aktivieren, wenn kein Suchergebnis gefunden wurde.

#### MySQL-Suchmodus

Option | Erl�uterung
------ | ------
LIKE | findet auch Teilw�rter, z.B. `Boot` in `Hausboot`, ist jedoch langsamer.
MATCH AGAINST  | findet nur ganze W�rter, ist daf�r schneller.

> Tipp: Obwohl die genauere Suche mit MATCH AGAINST weniger Suchergebnisse pr�sentiert, wird der Einsatz dieser Methode empfohlen, da die Suche dadurch beschleunigt wird. Das Manko der genaueren Suche - wenn man es denn so empfindet - kann �ber die �hnlichkeitssuche ausgeglichen werden.

### Indexierung

Bei der Indexierung durchsucht Search it alle in den Einstellungen angegebenen Orte (Artikel, Datenbank, Medienpool) und erstellt einen Suchindex-Cache. 

#### Art und Weise

Legt fest, wie Artikel indexiert werden.

Option | Erl�uterung
------ | ------
Indexierung der Artikel �ber eine HTTP-GET-Anfrage | indexiert Artikel so, als wenn Sie �ber das Frontend abgerufen werden.
Indexierung der Artikel �ber den Redaxo-Cache (ohne Template, nur der Artikel) | indexiert den Artikel so, wie er in __todo__ 
Indexierung der Artikel �ber den Redaxo-Cache (mit Template, liefert das gleiche Ergebnis wie per HTTP-GET-Anfrage) | indexiert die vollst�ndige Seite.
Offline-Artikel indexieren | indexiert auch Artikel, die in der Struktur als `offline` markiert wurden.
Artikel (ADD, EDIT, DELETE) automatisch (de)indexieren | indexiert automatisch neue Artikel, reindexiert bearbeitete Artikel und deindexiert Artikel, die gel�scht wurden.
Extension Point `"OUTPUT_FILTER"` aufrufen | Ruft den OUTPUT_FILTER auf, bspw., wenn das SPROG-Addon benutzt wurde und die Einstellung `Indexierung der Artikel` �ber den Redaxo-Cache erfolgt. __todo__ ***stimmt das?***

<a name="einstellungen-suchergebnis"></a>
## Suchergebnis

Bei der Ausgabe der Suchergebnisse k�nnen Standard-Einstellungen gesetzt werden:

### Erscheinungsbild des Highlight-Texts

Der Highlight-Text zeigt den gefundenen Suchbegriff als Teaser im Kontext. Der gefundene Suchbegriff kann im Highlight-Text ausgezeichnet werden, um ihn optisch zu formatieren, z.B. bei der Suche nach dem Begriff `Geld`:

```
<p>... wie viel <strong class="search_it-keyword">Geld</strong> l�sst sich damit verdienen? Erfahren ....</p>
```

Option | Erl�uterung
------ | ------
Start-Tag | Tag vor dem gefundenen Suchbegriff, z.B. `<strong class="search_it-keyword">`
End-Tag | Tag nach dem gefunden Suchbegriff, z.B. `</strong>`
Maximale Trefferanzahl | Wenn der gefundene Suchbegriff mehr als 1x im Highlight-Text erscheint: Gibt an, wie oft der Treffer angezeigt wird.
Maximale Zeichenanzahl f�r Teaser | Anzahl der Zeichen, mit denen das Suchergebnis angeteasert wird.
Maximale Zeichenanzahl um hervorgehobene Suchbegriffe herum | Anazhl der Zeichen, mit denen der gefundene Suchbegriff umgeben wird.

### Hervorhebung

Langer Rede kurzer Sinn: Die Hervorhebung wird bei der Auswahl in einer Vorschau dargestellt und k�nnte dort nicht besser erkl�rt werden als hier ;)

Option | Erl�uterung
------ | ------
Ab Anfang des Satzes, in dem mindestens einer der Suchbegriffe auftaucht | 
Ab Anfang des Absatzes, in dem mindestens einer der Suchbegriffe auftaucht | 
Alle gefundenen Suchbegriffe werden mit den sie umgebenden W�rtern dargestellt | 
F�r jeden gefundenen Suchbegriff wird genau eine Textstelle wiedergegeben | 
Als Teaser, in dem eventuell vorkommende Suchebgriffe hervorgehoben sind | 
Als Array mit allen Suchbegriffen und Textstellen | 
Beispieltext mit Sucheingabe |

### W�rter, Kategorien und Artikel von der Suche ausschlie�en

Schlie�t Begriffe, Artikel und Kategorien standardm��ig von der Suche aus. 

> Hinweis: Diese Einstellungen betreffen nur die Suchergebnisse und k�nnen in der `search_it`-Klasse �berschrieben werden. Begriffe, Kategorien und Artikel werden trotzdem bei der Indexierung ber�cksichtigt. __todo__ ***Stimmt das?***

Option | Erl�uterung
------ | ------
W�rter (kommaseperiert) | Begriffe, die von der Suche ausgeschlossen werden.
Artikel | Artikel (`rex_article`-IDs), die von der Suche ausgeschlossen werden.
Kategorien | Kategorien (`rex_category`-IDs), die von der Suche ausgeschlossen werden.

> Tipp: Der Artikel des Suchergebnis sollte von der Suche ausgeschlossen werden.

<a name="einstellungen-quelle"></a>
## Quelle

Hier werden Datenquellen f�r die Indexierung zus�tzlich zu den Redaxo-Artikeln definiert, z. B. Datenbanktabellen, der Medienpool sowie externe Verzeichnisse.

### Datenbankspalten in die Suche einschlie�en

Hier k�nnen DB-Spalten ausgew�hlt werden, die auch durchsucht werden sollen. Hierf�r bietet sich zus�tzliche Addon-Felder an, z. B. `rex_article.yrewrite_description` oder Daten, die �ber das Addon `yform` erstellt werden.

> Tipp: Die Indexierung sollte neben den gew�nschten Inhaltsfeldern auch das `id`-Feld / den Primary Key des Datensatzes indizieren sowie alle Felder, die bei der Ausgabe ber�cksichtigt werden sollen, bspw. Bilder, Teaser o.�.

### Dateisuche

Die Dateisuche durchsucht angegebene Dateien nach Begriffen. Bei PDFs, deren Inhalt als Text vorliegt, wird eine Volltextsuche im PDF erm�glicht. 

Option | Erl�uterung
------ | ------
Dateiendungen (frei lassen f�r beliebige Dateien) | Kommagetrennte Angabe von Dateien, die in der Medienpool-Indexierung
Medienpool indexieren | Gibt an, ob die Tabelle `rex_media` zur Medienpool-Suche indexiert wird.
Verzeichnistiefe | Gibt an, bis zu welcher Tiefe Dateien in den ausgew�hlten Verzeichnissen indexiert werden sollen.
Folgende Ordner in die Suche einschlie�en | Externe Ordner innerhalb der Redaxo-Installation werden indexiert.
Unterordner ausw�hlen |

<a name="plaintext"></a>
# Plaintext

Option | Erl�uterung
------ | ------
CSS-Selektoren (komma-separiert) | __todo__
Regul�re Ausdr�cke | __todo__
Textile parsen | __todo__
HTML-Tags entfernen | __todo__
Standard-Plaintext-Konvertierung durchf�hren | __todo__

<a name="search_highlighter"></a>
# Search Highlighter

Option | Erl�uterung
------ | ------
Tag um die Suchbegriffe | __todo__
Class | __todo__
inline CSS | __todo__
Stil CSS einbinden | __todo__
Stil (CSS) | __todo__
Eigener Stil | __todo__