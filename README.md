# Zeichenprüfer

Ein Werkzeug, das unsichtbare Unicode-Zeichen, geschützte Leerzeichen und typografische
Sonderformen aus Texten entfernt — und vorher anzeigt, was gefunden wurde.

**Live: [zeichenpruefer.de](https://zeichenpruefer.de)**

---

## Warum der Quelltext offen liegt

Auf jeder Seite steht: *„Läuft komplett im Browser · nichts wird gesendet."*

Das ist eine Behauptung, und Behauptungen über Datenschutz sollte man nicht glauben müssen.
Dieses Repository macht sie überprüfbar. Der Beleg ist schnell geführt:

```bash
grep -riE "fetch|XMLHttpRequest|sendBeacon|websocket|<script src|<link .*stylesheet" --include="*.html" .
```

Der Befehl gibt nichts aus. Es gibt keinen Server-Aufruf, keine externe Skripteinbindung,
kein Analysewerkzeug, keine Schriftart von einem fremden Anbieter. Die gesamte Verarbeitung
passiert per JavaScript im Browser des Nutzers.

Die Seite funktioniert deshalb auch offline: Datei speichern, Netzwerk trennen, weiterarbeiten.

## Was das Werkzeug entfernt

| Gruppe | Beispiele | Verhalten |
|---|---|---|
| Unsichtbare Steuerzeichen | `U+200B` ZWSP, `U+FEFF` BOM, `U+00AD` weiches Trennzeichen, `U+FE0F` Variationsselektor, Bidi-Steuerzeichen, Tag-Zeichen `U+E0000–E007F` | werden entfernt |
| Sonderleerzeichen | `U+00A0` geschützt, `U+202F` schmal geschützt, `U+2000–U+200A`, `U+3000` | werden zu einem normalen Leerzeichen |
| Gedankenstriche, Auslassungspunkte | `U+2014` —, `U+2013` –, `U+2212` −, `U+2026` … | werden zu `-` bzw. `...` |
| Typografische Anführungszeichen | `„ " ' ' « »` | optional, standardmäßig **aus** |
| Leerraum | Leerzeichen am Zeilenende, Mehrfach-Leerzeichen, überzählige Leerzeilen | werden aufgeräumt |

Jede Gruppe ist einzeln abschaltbar. Die Analyse-Ansicht markiert jede Fundstelle im
Originaltext mit ihrem Unicode-Codepoint.

## Was es ausdrücklich nicht kann

Es entfernt **keine KI-Wasserzeichen**. Das Wasserzeichen, das Anthropic seit August 2026
in Claude-Texte einbettet, ist ein statistisches Muster in der Wortwahl selbst — kein Zeichen,
das man suchen oder löschen könnte. Kein zeichenbasiertes Werkzeug kann das leisten, dieses
eingeschlossen. Ausführlich: [zeichenpruefer.de/claude-wasserzeichen](https://zeichenpruefer.de/claude-wasserzeichen/)

Es macht Texte auch nicht „unerkennbar". Es räumt Formatierung auf, nicht Schreibstil.

## Aufbau

```
index.html                        Werkzeug mit Analyse-Ansicht und Regelschaltern
unicode-zeichen/                  Nachschlagewerk je Codepoint
ki-texte-erkennen/                Erkennungsverfahren im Vergleich, mit Fehlalarm-Rechner
claude-wasserzeichen/             Was das Claude-Wasserzeichen ist und was nicht
chatgpt-wasserzeichen/            Was in ChatGPT-Texten steckt
geschuetztes-leerzeichen-word/    Anleitung für Word
geschuetztes-leerzeichen-excel/   Anleitung für Excel
csv-unsichtbare-zeichen/          BOM und Kodierungsprobleme bei CSV
404.html                          Fehlerseite
_headers                          Sicherheits- und Cache-Header (Netlify)
_redirects                        Weiterleitungen auf die Hauptdomain (Netlify)
robots.txt, sitemap.xml           Suchmaschinen
og-*.png                          Vorschaubilder fürs Teilen
```

Kein Build-Schritt, keine Abhängigkeiten, kein Paketmanager. Jede Seite ist eine einzelne
HTML-Datei mit eingebettetem CSS und JavaScript.

## Lokal ausprobieren

```bash
git clone https://github.com/Zeichenpruefer/zeichenpruefer.git
```

Danach `index.html` im Browser öffnen. Kein Server nötig.

## Hinweis für Beiträge

Unsichtbare Zeichen im Quelltext bitte **immer** als Escape-Sequenz schreiben (`\u200B`),
niemals als literales Zeichen. Editoren und Zwischenablagen normalisieren sie sonst
stillschweigend weg — bei einem Projekt, das genau diese Zeichen behandelt, ist das eine
besonders unangenehme Fehlerquelle.

Fehler und fehlende Zeichen gern als Issue melden.

## Lizenz

MIT — siehe [LICENSE](LICENSE).
