# HTML-Berichte mit Trivy

[html.tpl](html.tpl) ist eine Go-Template-Datei für die HTML-Ausgabe von Trivy. Sie gruppiert die Scanergebnisse nach Ergebnistyp und hebt Schweregrade farblich hervor. Für Schwachstellen zeigt sie Paketname, Kennung, Schweregrad, installierte Version, Version mit Fehlerbehebung (falls verfügbar) und Referenzlinks. Für Fehlkonfigurationen zeigt sie Prüfkennung, Titel, Schweregrad, Meldung und einen weiterführenden Link.

Das Template stellt die Ergebnisse dar, die Trivy ihm übergibt. Es enthält Ausgaben für Schwachstellen und Fehlkonfigurationen. Funde der Secret- und Lizenzscanner werden darin nicht angezeigt.

## Container-Archiv scannen

Führe die Befehle mit installiertem Trivy aus dem Ordner `ansible-ee-ubi10/` aus. Das Beispiel verwendet das lokale Container-Archiv für Version 1.0.2. Passe den Pfad bei einem anderen Archiv an.

```bash
mkdir -p reports
trivy image --input ansible-ee-ubi10-1.0.2.tar \
  --scanners vuln,misconfig \
  --ignorefile .trivyignore.yaml \
  --format template --template '@templates/html.tpl' \
  --output reports/vulnerability-report-1.0.2.html
```

Das `@` weist Trivy an, das Template aus der angegebenen Datei zu laden. Mit `--ignorefile` berücksichtigt der Scan die [Ausnahmen des Projekts](../.trivyignore.yaml). Öffne anschließend `reports/vulnerability-report-1.0.2.html` im Browser. Git ignoriert den Ordner `reports/`.

## Vorhandenen JSON-Bericht umwandeln

Wenn bereits ein Trivy-JSON-Bericht vorliegt, kannst du ihn ohne erneuten Scan umwandeln. Der HTML-Bericht übernimmt die im JSON enthaltenen Funde.

```bash
mkdir -p reports
trivy convert --format template --template '@templates/html.tpl' \
  --output reports/vulnerability-report-from-json.html \
  report-full.json
```
