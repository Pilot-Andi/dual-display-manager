# Dual Display Manager for Home Assistant

**Version 1.0**

Automatische TV-/Projektor-Verriegelung für Home Assistant mit kompatiblen Denon- und Marantz-AV-Receivern.

Der Dual Display Manager steuert Heimkino-Setups, bei denen ein Fernseher und ein Projektor am selben AV-Receiver verwendet werden. Er sorgt automatisch dafür, dass der richtige Monitor-Ausgang des AV-Receivers aktiv ist und verhindert, dass TV und Projektor unbeabsichtigt gleichzeitig eingeschaltet bleiben.

## Features

- Automatische Verriegelung zwischen TV und Projektor
- Automatische Umschaltung zwischen zwei AVR-Monitor-Ausgängen
- Unterstützung für Denon- und kompatible Marantz-AV-Receiver
- Projektor-Priorität bei nahezu gleichzeitigem Einschalten
- Schutz vor verspäteten TV-Statusmeldungen
- Optionale Prüfung aktiver Zuspieler
- Beliebig viele Zuspieler auswählbar
- Unterstützt z. B. Apple TV, MagentaTV, Nvidia Shield und Fire TV
- Optionaler TV-Start über Wake-on-LAN
- Alternativer TV-Start über `media_player.turn_on`
- TV-Autostart kann vollständig deaktiviert werden
- Frei konfigurierbare Umschalt- und Prüfverzögerungen
- Einrichtung vollständig über die Home-Assistant-Blueprint-Oberfläche
- Keine festen Entity-IDs erforderlich

## Typisches Einsatzszenario

Ein Heimkino verwendet beispielsweise:

**Monitor 1:** Projektor  
**Monitor 2:** Fernseher

Wird der Projektor eingeschaltet, schaltet der AV-Receiver automatisch auf den Projektor-Ausgang und der Fernseher wird ausgeschaltet.

Wird stattdessen der Fernseher eingeschaltet, prüft der Dual Display Manager zunächst, ob der Projektor gerade gestartet wurde. Dadurch wird verhindert, dass eine verspätete Statusmeldung des Fernsehers einen gerade gestarteten Projektor sofort wieder ausschaltet.

Wird der Projektor ausgeschaltet, kann automatisch zurück auf den Fernseher gewechselt werden.

## Zuspielerprüfung

Die optionale Zuspielerprüfung ist standardmäßig aktiviert.

Die Automation reagiert dadurch nur, wenn der AV-Receiver eingeschaltet und mindestens ein definierter Zuspieler aktiv ist.

Beispiele: Apple TV 4K, MagentaTV, Nvidia Shield, Fire TV oder andere Home-Assistant-`media_player`-Entitäten.

Diese zusätzliche Prüfung hilft, unnötige Umschaltungen durch unerwartete oder verspätete Gerätestatusmeldungen zu vermeiden.

Die Zuspielerprüfung kann bei Bedarf deaktiviert werden.

## Projektor-Priorität

Viele Smart-TVs und Home-Assistant-Integrationen melden Statusänderungen nicht exakt gleichzeitig. Dadurch kann beispielsweise folgende Situation entstehen:

1. Der Projektor wird eingeschaltet.
2. HDMI-CEC oder ein angeschlossener Zuspieler aktiviert gleichzeitig den Fernseher.
3. Der Fernseher meldet seinen Status etwas später an Home Assistant.
4. Eine einfache Verriegelungsautomation würde nun möglicherweise den Projektor wieder ausschalten.

Der Dual Display Manager erkennt deshalb, ob der Projektor erst vor wenigen Sekunden gestartet wurde. Ein frisch gestarteter Projektor erhält Vorrang. Der Zeitraum kann in den erweiterten Einstellungen angepasst werden.

## TV automatisch einschalten

Beim Ausschalten des Projektors stehen drei Methoden zur Verfügung.

### Wake-on-LAN

Der Fernseher wird über seine MAC-Adresse gestartet. Diese Methode eignet sich besonders für Fernseher, die sich über ihre Home-Assistant-Integration im ausgeschalteten Zustand nicht zuverlässig starten lassen.

### `media_player.turn_on`

Home Assistant verwendet direkt den Einschaltbefehl der ausgewählten TV-Integration.

### Kein automatischer Start

Der AV-Receiver wechselt zurück auf den TV-Ausgang, der Fernseher wird jedoch nicht automatisch eingeschaltet.

## Voraussetzungen

- Home Assistant 2024.6 oder neuer
- Ein unterstützter Denon- oder Marantz-AV-Receiver mit zwei Monitor-Ausgängen
- Die Denon-AVR-Integration mit Unterstützung für `denonavr.get_command`
- TV und Projektor als `media_player` in Home Assistant
- Die Wake-on-LAN-Integration, wenn Wake-on-LAN als Startmethode gewählt wird

## AVR-Ausgänge

Standardmäßig verwendet der Blueprint:

| Display | Befehl |
| --- | --- |
| Projektor | `/goform/formiPhoneAppDirect.xml?VSMONI1` |
| TV | `/goform/formiPhoneAppDirect.xml?VSMONI2` |

Die Befehle können innerhalb des Blueprints angepasst werden. Dadurch können Installationen berücksichtigt werden, bei denen TV und Projektor an anderen Monitor-Ausgängen angeschlossen sind.

## Installation

Nach dem Kauf erhältst du die Blueprint-Datei `dual_display_manager_v1.yaml` über den vorgesehenen Verkaufs- bzw. Installationsweg. Die kommerzielle Blueprint-Datei ist bewusst nicht Bestandteil dieses öffentlichen Repositorys.

Nach dem Import erscheint **Dual Display Manager - Denon / Marantz v1.0** in der Blueprint-Auswahl von Home Assistant. Anschließend müssen lediglich die eigenen Geräte ausgewählt werden; Änderungen am YAML-Code sind nicht erforderlich.

## Konfiguration

Für die Grundkonfiguration werden TV, Projektor, AV-Receiver und optional ein oder mehrere Zuspieler gewählt. Projektor- und TV-Ausgang sind standardmäßig Monitor 1 beziehungsweise Monitor 2. Als TV-Einschaltmethode stehen Wake-on-LAN, `media_player.turn_on` oder Deaktivierung zur Verfügung.

## Erweiterte Einstellungen

Für die meisten Installationen können die Standardwerte verwendet werden:

- TV-Prüfverzögerung: 2 Sekunden
- Projektor-Prioritätszeit: 5 Sekunden
- Umschaltverzögerung: 1 Sekunde
- TV-Startverzögerung: 2 Sekunden

Diese Werte ermöglichen die Anpassung an Geräte mit unterschiedlich schnellen Statusmeldungen oder Einschaltzeiten.

## Kompatibilität

Version 1.0 wurde für Heimkino-Installationen mit Home Assistant und Denon-/Marantz-AV-Receivern entwickelt. Da Hersteller, Integrationen, HDMI-CEC und Gerätezustände unterschiedlich reagieren können, kann nicht garantiert werden, dass jedes Geräte-Modell ohne Anpassung identisch arbeitet.

## Datenschutz

Der Blueprint wird ohne installationsspezifische Geräteinformationen verteilt. Lokale Entity-IDs, MAC-Adressen und andere Einstellungen werden ausschließlich in der jeweiligen Home-Assistant-Installation des Anwenders konfiguriert.

## Support und Updates

Fehlerberichte und Verbesserungsvorschläge können über dieses GitHub-Repository eingereicht werden. Kleinere Verbesserungen und Kompatibilitätserweiterungen erscheinen innerhalb der Version-1-Reihe; größere Funktionsänderungen können zukünftig als Version 2 veröffentlicht werden.

## Hinweis

Dieses Projekt ist ein unabhängiges Home-Assistant-Produkt und steht in keiner Verbindung zu Home Assistant, Denon, Marantz, Apple, LG, Hisense oder anderen genannten Herstellern und Marken. Alle genannten Marken und Produktnamen sind Eigentum ihrer jeweiligen Rechteinhaber.

---

© 2026 Andreas Steppan. All rights reserved.
