# Ansible Execution Environment auf UBI 10

Dieser Ordner enthält die Build-Definition und die Release-Dokumentation für ein Ansible Execution Environment auf Basis von Red Hat Universal Base Image 10. Das Container-Image bündelt Python 3.12, Ansible Core 2.21.6, Ansible Runner und die Abhängigkeiten für den Einsatz mit VMware, Kubernetes und Ansible Automation Controller.

## Build-Dateien

| Datei | Inhalt |
| --- | --- |
| [execution-environment.yml](execution-environment.yml) | Ansible-Builder-Definition für Basis-Image, Laufzeit, Abhängigkeiten und zusätzliche Build-Schritte. |
| [requirements.yml](requirements.yml) | Ansible-Collections, die beim Build installiert werden. |
| [requirements.txt](requirements.txt) | Zusätzlich benötigte Python-Pakete. |
| [bindep.txt](bindep.txt) | Systempakete, die über DNF installiert werden. |
| [files/ansible.cfg](files/ansible.cfg) | Konfiguration der Collection-Quellen Red Hat Automation Hub und Ansible Galaxy. Den Automation-Hub-Token erhält der Build über die Umgebungsvariable `ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_TOKEN`. |

Die [Build-Anleitung im Repository-Stamm](../README.md) enthält die Befehle zum Bauen, Starten und Scannen des Images. Führe die Build-Befehle aus diesem Unterordner aus.

## Release-Dokumentation

[releases/1.0.2/](releases/1.0.2/) dokumentiert den Stand des gebauten Images. Dort findest du Informationen zum Basis-Image und den Build-Werkzeugen, die Image-Historie und -Metadaten, Listen der installierten Collections, Python- und RPM-Pakete, die Prüfsumme des OCI-Archivs sowie Software-Stücklisten (SBOMs) in CycloneDX und SPDX. Die dort abgelegten Versionslisten dokumentieren den Build-Stand. Der Build liest seine Abhängigkeiten aus den oben verlinkten Build-Dateien.

## Sicherheitsprüfung und lokale Artefakte

[.trivyignore.yaml](.trivyignore.yaml) enthält Ausnahmen für Trivy-Schwachstellenfunde. [templates/html.tpl](templates/html.tpl) ist die Vorlage für HTML-Scanberichte.

Ansible Builder erzeugt den Build-Kontext unter `context/`. Exportierte Container-Images liegen lokal als TAR-Dateien vor. Trivy-Berichte liegen als `report-*.json` oder im Ordner `reports/` vor. Git ignoriert diese generierten Dateien und Archive, einschließlich exportierter OCI-Archive unter `releases/`.
