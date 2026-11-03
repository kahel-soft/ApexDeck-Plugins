# ApexDeck-Plugins

Verteilung der Plugins für ApexDeck, die Steuerung für das Ulanzi D200. Dieses Repo enthält keinen Quellcode, sondern nur den Plugin-Katalog und die fertigen Plugin-Pakete.

## Aufbau

- `index.json` – der Katalog (`schema_version`, `plugins`). Pro Plugin: `id`, `name`, `version`, `description`, `author`, `min_app_version`, `min_python`, `download_url`, `sha256`, `size`.
- Releases – pro Plugin-Version ein Release mit dem Tag `<id>-v<version>` (zum Beispiel `obs-studio-v1.1.0`) und genau einer Datei `<id>-<version>.zip`. Die ZIP enthält das Plugin samt fertig gebündelter Abhängigkeiten im Ordner `lib/`.

Die App liest `index.json` vom Branch `main` und lädt die ZIP über die `download_url` aus dem Eintrag. Sie prüft Größe und SHA-256 gegen den Index, bevor sie installiert.

## Veröffentlichen

Gebaut wird im App-Repo mit `scripts/build_plugin_release.py`. Das Skript führt selbst nichts aus, es druckt den Release-Befehl nur aus.

1. ZIP bauen und den Index-Eintrag in der lokalen `index.json` erneuern (noch nichts veröffentlicht; Beispiel, Pfade anpassen):

   ```
   python scripts/build_plugin_release.py plugins/obs-studio --index ../ApexDeck-Plugins/index.json --out dist/plugins
   ```

2. Zuerst das Release mit der ZIP anlegen (der Befehl wird vom Skript ausgegeben):

   ```
   gh release create obs-studio-v1.1.0 dist/plugins/obs-studio-1.1.0.zip --repo kahel-soft/ApexDeck-Plugins --title "OBS Studio 1.1.0" --notes ""
   ```

3. Erst danach die geänderte `index.json` in diesem Repo committen und nach `main` pushen.

Reihenfolge beachten: Das Release muss existieren, bevor die App den neuen Index-Eintrag nutzt, sonst läuft der Download ins Leere. Der Index darf nie auf eine ZIP zeigen, die noch nicht herunterladbar ist; deshalb zuerst das Release, dann der Index.

## Hinweis

Plugins sind Programme: Die App startet sie auf dem Rechner der Nutzerin oder des Nutzers. Deshalb liegen hier ausschließlich eigene, geprüfte Plugins. Fremde Plugins werden nicht aufgenommen.
