# SaarMesh Observer-Firmware für MeshCore

Fertig gebaute **MeshCore-Repeater-Firmware mit Paket-Logging** für den Betrieb als Observer –
also ein Repeater, der per USB an einem Raspberry Pi (o. ä.) hängt und mit
[meshcoretomqtt](https://github.com/Cisien/meshcoretomqtt) alle empfangenen Pakete an MQTT bzw. an
Analyzer wie [live.saarmesh.de](https://live.saarmesh.de) oder letsmesh.net weitergibt.

➡️ **Downloads: [Releases](https://github.com/SaarMesh-de/meshcore-observer-firmware/releases/latest)**

## Was ist anders als bei der offiziellen Firmware?

Genau eine Sache: Die Firmware wird mit `-D MESH_PACKET_LOGGING=1` gebaut. Quellcode, Versionen und
Buildskript sind **unverändert** die des offiziellen Projekts
[meshcore-dev/MeshCore](https://github.com/meshcore-dev/MeshCore). Die Versionskennung lautet z. B.
`v1.18.0-saarmesh-<commit>`, damit man den Observer-Build vom Original unterscheiden kann.

Gebaut wird ausschließlich hier über GitHub Actions ([Workflow](.github/workflows/build.yml)).
Der Workflow prüft täglich, ob es einen neuen offiziellen Repeater-Release gibt, und baut ihn dann automatisch.

## Unterstützte Boards

| Board | Datei beginnt mit | Chip |
|---|---|---|
| Heltec V3 | `Heltec_v3_repeater` | ESP32-S3 |
| Heltec V4 | `heltec_v4_repeater` | ESP32-S3 |
| RAK WisBlock 4631 | `RAK_4631_repeater` | nRF52840 |
| Seeed Xiao ESP32-S3 + Wio-SX1262 | `Xiao_S3_WIO_repeater` | ESP32-S3 |
| Seeed Xiao nRF52840 + Wio-SX1262 | `Xiao_nrf52_repeater` | nRF52840 |
| ProMicro nRF52840 / Faketec | `ProMicro_repeater` | nRF52840 |

Weitere Boards auf Anfrage – meist reicht eine Zeile im Workflow.

## Flashen

### ESP32 (Heltec V3/V4, Xiao S3)

**Update eines bestehenden Repeaters** – Name, Schlüssel und Einstellungen bleiben erhalten:

```bash
esptool --chip esp32s3 -p /dev/ttyUSB0 write-flash 0x10000 Heltec_v3_repeater-v1.18.0-saarmesh-xxxxxxx.bin
```

**Neuinstallation** (löscht alles, neue Identität):

```bash
esptool --chip esp32s3 -p /dev/ttyUSB0 erase-flash
esptool --chip esp32s3 -p /dev/ttyUSB0 write-flash 0x0 Heltec_v3_repeater-v1.18.0-saarmesh-xxxxxxx-merged.bin
```

Läuft auf dem Pi bereits meshcoretomqtt, den Dienst vorher stoppen (`sudo systemctl stop mctomqtt`),
sonst blockiert er die serielle Schnittstelle.

### nRF52 (RAK 4631, Xiao nRF52, ProMicro/Faketec)

Zweimal kurz Reset drücken → es erscheint ein USB-Laufwerk → die `.uf2`-Datei hineinkopieren.
Das Board startet danach automatisch neu; Einstellungen bleiben erhalten.

### Nach dem Flashen

- Bei einer Neuinstallation ist das Admin-Passwort **`password`** → sofort ändern: `password <neues-passwort>`
- Name, Position und Funkparameter (SaarMesh: 869.618 MHz) wie gewohnt per Konsole oder
  [config.meshcore.io](https://config.meshcore.io) setzen.
- Prüfen: `ver` zeigt `v…-saarmesh-…`; meshcoretomqtt zählt RX-Pakete in seinem Log.

## Echtheit prüfen

Jedes Release enthält eine `SHA256SUMS.txt`:

```bash
sha256sum -c SHA256SUMS.txt --ignore-missing
```

Zusätzlich hat jede Datei einen von GitHub signierten **Herkunftsnachweis** (Build Provenance). Damit
lässt sich belegen, dass sie genau von diesem Workflow aus dem offiziellen Quellcode gebaut wurde:

```bash
gh attestation verify Heltec_v3_repeater-v1.18.0-saarmesh-xxxxxxx.bin -R SaarMesh-de/meshcore-observer-firmware
```

## Lizenz

MeshCore steht unter der MIT-Lizenz (© 2025 Scott Powell / rippleradios.com); jedem Release liegt die
Original-Lizenz als `MeshCore-LICENSE.txt` bei. Die Dateien dieses Repos (Workflow, Doku) stehen ebenfalls unter MIT.

Dies ist ein Community-Angebot von [SaarMesh](https://saarmesh.de), kein offizielles MeshCore-Release.
Nutzung auf eigene Verantwortung.
