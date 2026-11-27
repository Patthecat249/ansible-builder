# Ansible Execution Environment für UBI 10 · Release 1.0.2

Dieser Ordner dokumentiert den Stand des gebauten Container-Images. Die Versionslisten erfassen die tatsächlich installierten Abhängigkeiten. Die Build-Definition und ihre Eingabedateien sind im [übergeordneten Ordner](../../README.md) beschrieben.

| Datei | Aufgabe und Inhalt |
| --- | --- |
| `ansible-ee-ubi10-1.0.2.oci.tar` | Export des fertigen Container-Images im OCI-Format. Enthält den Image-Index, das Manifest, die Konfiguration und die Dateisystem-Layer. Das Archiv liegt lokal vor und wird von Git ignoriert. |
| [image.sha256](image.sha256) | SHA-256-Prüfsumme und Dateipfad des OCI-Archivs. Damit lässt sich prüfen, ob das Archiv unverändert vorliegt. |
| [base-image.txt](base-image.txt) | Zwei Referenzen auf das UBI-10-Basis-Image mit SHA-256-Digests. Die Digests identifizieren konkrete Inhalte, auch wenn sich der Tag `latest` später ändert. |
| [build-toolchain.txt](build-toolchain.txt) | Protokoll der Build-Umgebung mit Build-Zeitpunkt, Rocky Linux als Host-Betriebssystem, Architektur und Versionen von Ansible Builder, Podman, Buildah, Python und Trivy. |
| [image-history.txt](image-history.txt) | Image-Historie in Tabellenform. Zeigt die aufgezeichneten Build-Anweisungen, die zugehörigen Image-IDs, Zeitangaben und Layer-Größen. |
| [image-inspect.json](image-inspect.json) | Detaillierte Metadaten zum fertigen Image, unter anderem Image-ID, Tags, Architektur, Layer, Labels und Laufzeitkonfiguration mit Benutzer, Umgebungsvariablen und Startbefehl. |
| [os-release.txt](os-release.txt) | Betriebssysteminformationen aus dem Container, darunter Name, Versionsnummer und Kennung. Der Export weist Red Hat Enterprise Linux 10.3 aus. |
| [collections.json](collections.json) | JSON-Inventar der sieben installierten Ansible-Collections. Ordnet die Collections ihrem Installationspfad zu und enthält die jeweiligen Versionen. |
| [requirements-lock.yml](requirements-lock.yml) | Dieselben sieben Collections als Ansible-Galaxy-Anforderungsliste mit exakten Versionen. |
| [python-packages.json](python-packages.json) | Detailliertes Inventar der 78 installierten Python-Pakete. Enthält neben Namen und Versionen auch Paketmetadaten, Abhängigkeitsangaben sowie Informationen zur Python-Umgebung und zu pip. |
| [requirements-lock.txt](requirements-lock.txt) | Kompakte Liste der 78 installierten Python-Pakete im Format `Paket==Version`. |
| [rpm-packages-lock.txt](rpm-packages-lock.txt) | Inventar der 254 installierten RPM-Pakete mit Name, Epoch, Version, Release und Architektur. |
| [rpm-repositories.txt](rpm-repositories.txt) | Liste der RPM-Repository-IDs und Anzeigenamen für UBI 10 BaseOS, AppStream und CodeReady Builder. |
| [sbom.cyclonedx.json](sbom.cyclonedx.json) | Software-Stückliste im CycloneDX-Format mit 368 Komponenten, deren Metadaten und Abhängigkeitsbeziehungen. |
| [sbom.spdx.json](sbom.spdx.json) | Software-Stückliste im SPDX-Format mit 369 Paketeinträgen einschließlich des Images sowie Paketbeziehungen und Lizenzinformationen, soweit erfasst. |
| `README.md` | Diese Übersicht über die Release-Dateien und ihre Verwendung. |

## Archiv prüfen

Wenn das OCI-Archiv lokal vorliegt, führe diesen Befehl im Ordner `ansible-ee-ubi10/` aus. Der Pfad in der Prüfsummendatei geht von diesem Ordner aus.

```bash
sha256sum -c releases/1.0.2/image.sha256
```
