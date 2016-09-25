# Suchergebnisse nur aus bestimmten Kategorien anzeigen

Der `search_it`-Klasse kann mitgeteilt werden, auf welche Kategorien und Artikel die Ausgabe der Suchergebnisse beschr�nkt wird. Diese Artikel und Kategorien m�ssen zuvor indexiert worden sein.

```
$search_it = new search_it();
$search_it->searchInCategories(array(5,6,13));
$result = $search_it->search(rex_request('search', 'string'));
```