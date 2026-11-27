# Getting-Started

Der Ordner baut und dokumentiert ein **Ansible Execution Environment auf Basis von Red Hat UBI 10**. Das Ergebnis ist ein Container-Image, in dem Ansible-Playbooks mit einer einheitlichen Laufzeit und den benötigten Bibliotheken ausgeführt werden können, etwa für VMware, Kubernetes und Ansible Automation Controller. Im Ordner liegen die Bauanleitung, der generierte Build-Kontext, fertige Images und die zugehörigen Prüfberichte. Eigene Playbooks, Inventare und eine CI-Pipeline sind im Ordner nicht vorhanden.

## Wie das Image entsteht

Die zentrale Datei ist [execution-environment.yml](/root/git/ansible-builder/execution-environment.yml). Sie verwendet das v3-Format von Ansible Builder und definiert UBI 10, Python 3.12, `ansible-core==2.21.6` und Ansible Runner. Das fertige Image startet standardmäßig mit Bash, läuft unter UID `1000` und verwendet `/runner` als Arbeitsverzeichnis.

| Bereich | Inhalt |
| --- | --- |
| [README.md](/root/git/ansible-builder/README.md) | Befehle zum Erzeugen des Build-Kontexts, zum Bauen und Starten mit Podman und zum Scannen mit Trivy. Die Beispiele verwenden überwiegend `1.0.0` und `1.0.1`. |
| [requirements.yml](/root/git/ansible-builder/requirements.yml) und [ansible.cfg](/root/git/ansible-builder/files/ansible.cfg) | Fünf direkt angeforderte Collections. `ansible.controller` ist auf `4.5.36` festgelegt und kommt aus Red Hat Automation Hub. Die übrigen decken VMware, Kubernetes, POSIX und allgemeine Ansible-Module ab. |
| [requirements.txt](/root/git/ansible-builder/requirements.txt) und [bindep.txt](/root/git/ansible-builder/bindep.txt) | Python-Pakete wie `pyvmomi`, `awxkit` und `kubernetes` sowie Systemwerkzeuge wie Git, SSH, `jq` und `rsync`. |
| [context/](/root/git/ansible-builder/context) | Generierter vierstufiger Build für Basis, Galaxy-Download, Abhängigkeitsauflösung und finales Image. Die Hilfsskripte prüfen Ansible, lesen Abhängigkeiten aus den Collections aus, installieren Pakete und richten beim Start die Laufzeitumgebung ein. Die vier kopierten Eingabedateien stimmen bytegenau mit ihren Vorlagen überein. |

## Was gebaut und geprüft wurde

- **Drei Image-Versionen.** Die drei TAR-Dateien im Hauptordner enthalten laut Manifest `1.0.0`, `1.0.1` und `1.0.2`. Das zusätzliche OCI-Archiv unter `releases/1.0.2/` hat dieselbe Image-Konfigurations-ID wie das TAR von `1.0.2`. Seine hinterlegte SHA-256-Prüfsumme stimmt. [Release-Ordner](/root/git/ansible-builder/releases/1.0.2)

- **Release `1.0.2` ist ausführlich dokumentiert.** Erfasst sind sieben installierte Collections, 78 Python-Pakete und 254 RPMs sowie Image-Historie, Basisimage-Digests, Build-Werkzeuge und zwei Software-Stücklisten in CycloneDX und SPDX. Ein erneuter Build kann trotzdem andere Paketstände ergeben. Die Build-Datei verwendet `ubi:latest`, führt `dnf update` aus und bindet die mitgelieferten Versionslisten nicht ein. [Build-Definition und Release-Dateien](/root/git/ansible-builder/execution-environment.yml)

- **Die Trivy-Berichte für `1.0.2` weichen voneinander ab.** Die beiden JSON-Berichte enthalten vier Python-Funde. Drei betreffen `setuptools@70.3.0`, das laut `.trivyignore.yaml` in pip eingebettet ist. Die separat installierte Version ist laut Paketbestand `84.2.1`. Der vierte Fund betrifft `awxkit 24.6.1` mit Schweregrad `MEDIUM`. Der spätere HTML-Bericht zeigt null Funde, obwohl die Ignore-Datei nur die drei `setuptools`-Einträge aufführt. Warum `awxkit` dort fehlt, geht aus den Dateien nicht hervor. [JSON-Bericht](/root/git/ansible-builder/report-full.json) · [Ignore-Datei](/root/git/ansible-builder/.trivyignore.yaml) · [HTML-Bericht](/root/git/ansible-builder/reports/vulnerability-report-1.0.2.html)

- **Die README verwendet `token-automationhub` als Zugangsdatenquelle für Red Hat Automation Hub.** Die Datei hat Rechte `0600`. Ihren Inhalt habe ich bewusst nicht gelesen.

Ich habe 45 Dateien erfasst und die Containerarchive anhand ihrer Metadaten geprüft. Ich habe kein Image neu gebaut oder gestartet und keinen neuen Trivy-Scan ausgeführt. Der Ordner belegt rund 2,6 GB und enthält keine Git-Metadaten.
