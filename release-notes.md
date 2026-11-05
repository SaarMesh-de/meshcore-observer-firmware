Offizielle MeshCore-Repeater-Firmware **$VERSION** ([Upstream-Tag `$TAG`](https://github.com/meshcore-dev/MeshCore/releases/tag/$TAG), Commit `$COMMIT`), unverändert gebaut mit **einer** zusätzlichen Option: `-D MESH_PACKET_LOGGING=1`.

Damit gibt der Repeater jedes empfangene und gesendete Paket über USB-Seriell aus, sodass [meshcoretomqtt](https://github.com/Cisien/meshcoretomqtt) es an MQTT/Analyzer weiterleiten kann (Observer).

| Datei | Wofür |
|---|---|
| `*_repeater-…-saarmesh-*.bin` | ESP32: **Update** ohne Datenverlust, an Adresse `0x10000` |
| `*_repeater-…-saarmesh-*-merged.bin` | ESP32: **Neuinstallation** an `0x0` (löscht alles!) |
| `*_repeater-…-saarmesh-*.uf2` | nRF52: per Drag & Drop ins Bootloader-Laufwerk |
| `*_repeater-…-saarmesh-*.zip` | nRF52: DFU-Paket (z. B. adafruit-nrfutil / nRF Connect) |

Anleitung, Prüfung der Dateien und Hinweise: siehe [README](https://github.com/SaarMesh-de/meshcore-observer-firmware#readme).

⚠️ Nach einer **Neuinstallation** ist das Admin-Passwort `password` – bitte sofort ändern (`password <neu>`).
