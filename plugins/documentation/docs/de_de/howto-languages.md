# Wie kann ich sprachabh�ngig suchen?

Search it sucht per Standard in allen Sprachen. Um sprachabh�ngige Suchen zu erlauben, muss der `search_it`-Klasse die Sprach-ID der Sprache, in der gesucht werden soll, �bergeben werden.

*Such-Formular*

```
<input type="hidden" name="clang" value="REX_CLANG_ID" /> 
```

*Suchergebnis-Ausgabe*

```
$search_it = new search_it(REX_CLANG_ID);
```