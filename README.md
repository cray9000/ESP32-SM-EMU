# ESP32-SM-EMU

ESP32-SM-EMU ist eine ESP32-basierte Smart-Meter-Emulator-Firmware für Fronius-nahe bzw. SunSpec-orientierte Integrationsszenarien.

Das Projekt übernimmt Messwerte aus konfigurierbaren Eingangsquellen, verarbeitet sie intern und stellt sie in kompatibler Form für nachgelagerte Systeme bereit. Der aktuelle Schwerpunkt liegt auf **Modbus-RTU** oder **MQTT** als Eingangsquelle, **Modbus-TCP** als primärer Ausgangsschnittstelle sowie einer integrierten **Weboberfläche** für Konfiguration, Diagnose und Wartung.

---

## Features

- ESP32-basierter Smart-Meter-Emulator
- Konfigurierbare Eingangsquelle
  - **Modbus-RTU**
  - **MQTT-IN**
- Primäre Ausgangsschnittstelle
  - **Modbus-TCP**
- Weboberfläche für Konfiguration und Diagnose
- JSON-Import und JSON-Export von Konfigurationsprofilen
- Laden von Standardwerten über die Weboberfläche
- Laden gespeicherter NVS-Werte über die Weboberfläche
- Persistente Konfigurationsspeicherung im **NVS**
- **OTA-Firmware-Update**
- UART / RS485 Tuning und Diagnose
- Laufzeit-Status- und Diagnoseendpunkte
- Task-basierte Firmware-Architektur auf ESP32

---

## Einsatzbereiche

ESP32-SM-EMU eignet sich insbesondere für:

- Emulation eines Smart Meters für Test- und Integrationszwecke
- Bridging von **Modbus-RTU** nach **Modbus-TCP**
- Einspeisung von Messwerten aus **MQTT** in Modbus-basierte Systeme
- Labor- und Entwicklungsumgebungen rund um ESP32, Home Assistant und Fronius-nahe Setups
- Diagnose und Tuning von **RS485/UART**-Kommunikation auf ESP32-Systemen

---

## Projektstatus

Das Projekt ist aktiv in Entwicklung.

Der funktionale Schwerpunkt liegt aktuell auf:

- webbasierter Konfiguration
- Modbus-RTU- und MQTT-basierter Datenerfassung
- Modbus-TCP-Bereitstellung
- JSON-Import/Export von Parametern
- OTA-Update
- UART / RS485 Tuning und Diagnose

Einzelne Bereiche werden laufend erweitert oder verfeinert. Für produktive Einsätze sollten deshalb immer der konkrete Firmwarestand und die tatsächlich implementierten Funktionen geprüft werden.

---

## Architekturüberblick

ESP32-SM-EMU trennt die Aufgaben logisch in mehrere Bereiche:

### Input Layer
Liest Werte aus einer konfigurierbaren Quelle, aktuell insbesondere:

- Modbus-RTU
- MQTT-IN

### Verarbeitung / Mapping
Validiert, skaliert und verwaltet Messwerte intern.

### Output Layer
Stellt Werte für andere Systeme bereit, aktuell mit Fokus auf:

- Modbus-TCP

### Management Layer
Weboberfläche, Statusseiten, JSON-Import/Export, OTA, NVS-Verwaltung und Diagnose.

---

## Weboberfläche

Die Weboberfläche dient als zentrale Verwaltungs- und Diagnoseinstanz.

Typische Funktionen:

- Netzwerk- und Gerätekonfiguration
- Auswahl und Konfiguration der Eingangsquelle
- Modbus-RTU-Parameter konfigurieren
- MQTT-IN-Parameter konfigurieren
- JSON-Export von Konfigurationen
- JSON-Import von Konfigurationen
- Laden von Standardkonfigurationen
- Laden gespeicherter NVS-Werte
- UART / RS485 Tuning
- Status- und Diagnoseanzeigen
- OTA-Firmware-Upload

Je nach Firmwarestand können einzelne Menüpunkte und Bezeichnungen leicht variieren.

---

## Unterstützte Datenquellen

### Modbus-RTU

Der RTU-Eingang ist für RS485-basierte Feldgeräte vorgesehen.

Typische konfigurierbare Parameter:

- Baudrate
- Parität
- Stopbits
- Slave-ID / Unit-ID
- Timeout
- Registerdefinitionen / Profile

Zusätzlich existieren Diagnose- und Tuning-Funktionen für UART/RS485, um Kommunikationsprobleme robuster analysieren zu können.

### MQTT-IN

MQTT-IN erlaubt die Übernahme externer Messwerte aus MQTT-Topics.

Damit lassen sich Werte aus Automatisierungssystemen, Middleware oder anderen Datensammlern einspeisen.

Typische Parameter:

- MQTT-Server
- Port
- Benutzername / Passwort
- Topics
- Mapping der Eingangswerte

---

## Ausgangsschnittstelle

### Modbus-TCP

Modbus-TCP ist aktuell die primäre Ausgangsschnittstelle des Projekts.

Darüber können aufbereitete oder emulierte Messwerte von nachgelagerten Systemen gelesen werden, zum Beispiel in Integrations-, Labor- oder Testumgebungen.

---

## Konfigurationsspeicherung

ESP32-SM-EMU speichert Konfigurationswerte persistent im **NVS** des ESP32.

Dazu gehören je nach Bereich unter anderem:

- Netzwerkeinstellungen
- Eingangsquellen-Konfiguration
- Modbus-RTU-Parameter
- MQTT-IN-Parameter
- weitere Laufzeit- und Systemparameter

Über die Weboberfläche können in mehreren Bereichen sowohl:

- **Standardwerte**
- als auch **gespeicherte NVS-Werte**

geladen und in die Oberfläche übernommen werden.

---

## JSON-Import / JSON-Export

Konfigurationsbereiche können als JSON exportiert und wieder importiert werden.

Das ist besonders hilfreich für:

- Backups
- reproduzierbare Setups
- Austausch von Profilen
- schnelles Umstellen zwischen Testkonfigurationen

Typische Anwendungsfälle:

- Export der MQTT-IN-Konfiguration
- Export der Modbus-RTU-Konfiguration
- Wiederherstellung eines bekannten Stands nach Änderungen
- Verteilung definierter Parameterprofile auf mehrere Geräte

---

## OTA-Updates

Die Firmware unterstützt **OTA-Updates** über die Weboberfläche.

Damit kann neue Firmware komfortabel eingespielt werden, ohne das Gerät jedes Mal lokal per USB neu flashen zu müssen.

Je nach Firmwarestand können zusätzlich Statusinformationen rund um OTA, Partitionen oder Rollback-relevante Zustände vorhanden sein oder weiter ausgebaut werden.

---

## UART / RS485 Tuning

Für RTU-basierte Installationen enthält das Projekt spezielle Funktionen zur Analyse und Optimierung der seriellen Kommunikation.

Ziele:

- robustere Kommunikation mit RS485-Geräten
- bessere Diagnose bei Timeout-, CRC- oder Ausnahmefehlern
- vereinfachtes Testen unterschiedlicher UART-Parameter
- transparente Statusanzeige für Feldtests

Diese Funktionen sind besonders nützlich, wenn ein Zielgerät nicht sauber dokumentiert ist oder sich Kommunikationsparameter in der Praxis als kritisch erweisen.

---

## Hardware

Typische Zielplattform:

- **LilyGO / T-CAN485 Board**

ESP32-SM-EMU ist insbesondere für das **LilyGO T-CAN485** geeignet.  
Das Board basiert auf einem **ESP32** und integriert bereits **1× RS485** sowie **1× CAN** direkt auf der Hardware. Zusätzlich sind laut Hersteller unter anderem **5–12 V Versorgung**, **WS2812 RGB-LED** und **TF-/MicroSD-Unterstützung** vorhanden.

Für Modbus-RTU / RS485 ist das Board daher eine naheliegende Zielplattform für dieses Projekt.

### Relevante RS485-Pins des LilyGO T-CAN485

- **TX:** IO22
- **RX:** IO21
- **CALLBACK:** IO17
- **EN:** IO9

### Weitere relevante Pins

- **WS2812 DATA:** IO4
- **Booster EN:** IO16
- **SD MISO:** IO2
- **SD MOSI:** IO15
- **SD SCLK:** IO14
- **SD CS:** IO13

### Referenzen

- Offizielles Board-Repository: [Xinyuan-LilyGO/T-CAN485](https://github.com/Xinyuan-LilyGO/T-CAN485)

> Hinweis: Die im Projekt tatsächlich verwendete Pinbelegung im Sketch hat Vorrang, falls sie von der Herstellerdokumentation oder von Beispielprojekten abweicht.

---

## Software / Build-Umgebung

Typischerweise:

- Arduino IDE mit ESP32-Core  
oder
- kompatible ESP32-/Arduino-basierte Toolchain

Bei Verwendung des **LilyGO T-CAN485** kann in der Arduino IDE typischerweise **ESP32 Dev Module** als Board-Profil verwendet werden, sofern im konkreten Projektstand nichts Abweichendes dokumentiert ist.

Je nach verwendetem Quellstand werden zusätzlich Bibliotheken für folgende Bereiche benötigt:

- WLAN
- Webserver
- MQTT
- JSON
- NVS / Preferences
- OTA
- Modbus / serielle Kommunikation

Da sich Bibliotheksabhängigkeiten im Projektstand ändern können, sollten die tatsächlich eingebundenen Includes im Sketch bzw. in den zugehörigen Quelldateien als maßgeblich betrachtet werden.

---

## Empfohlene Projektstruktur

```text
ESP32-SM-EMU/
├── ESP32-SM-EMU.ino
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── screenshots/
│   └── wiring/
├── profiles/
│   ├── modbus-rtu/
│   └── mqtt-in/
└── examples/
    ├── modbus-rtu/
    └── mqtt-in/
