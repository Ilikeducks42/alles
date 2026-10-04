# Tilt Glass — Spielen durch Neigen

Sammlung endloser Spiele, gesteuert ausschließlich durch Geräte-Neigung. Kein Build, keine Dependencies, kein Terminal nötig.

## Schnellstart (ein Gerät)

1. `tilt-glass/index.html` im Browser öffnen (Doppelklick genügt).
2. Auf dem Handy: **Sensor erlauben** (nur iOS), Gerät in neutrale Haltung bringen, **Neigung kalibrieren**.
3. Pfeil → Spiel wählen → neigen. Fertig.
4. Desktop ohne Sensor: Maus bewegen oder Pfeiltasten simulieren die Neigung.

Alle Einstellungen und Bestwerte bleiben in `localStorage` gespeichert.

## Modus 2: Hauptgerät + Handy als Controller (ultra einfach)

Kein Terminal, kein lokaler Server, keine IP-Adressen, kein langes Abtippen:

1. **Controller aufs Handy bringen (eine Datei genügt):** Schicke die Datei `controller-standalone.html` aufs Handy (WhatsApp, USB, E-Mail — sie enthält alles, kein Ordner nötig). Dort im Browser öffnen.
   - Erscheint oben eine **rote Box**, ist die Datei unvollständig — dann erneut die Standalone-Datei übertragen.
2. Hauptgerät (PC): `index.html` → **Externer Controller** → **Code erzeugen** (4-stellig, z. B. 4821).
3. Beide Geräte ins **gleiche WLAN**.
4. Auf dem Controller **denselben Code** eintippen → **Verbinden**. Beide zeigen grün: „Verbunden — jetzt neigen“.

Unter dem Code-Feld läuft ein kleines **Diagnose-Protokoll** mit (z. B. „Vermittlung ok, Code aktiv“ → „Code gefunden“ → „Direktverbindung steht“). Bleibt es hängen, steht dort auch die Ursache.

Technik: Die Code-Vermittlung läuft einmalig über einen öffentlichen Rendezvous-Punkt (Internet nötig). Die Neigung selbst läuft danach direkt Peer-to-Peer per WebRTC (ca. 40 Pakete/s). Klappt die direkte Verbindung nicht (z. B. Gast-WLAN mit Geräte-Isolation), wird automatisch ein Relay-Server benutzt. Ganz ohne Internet geht es per „Code kopieren (Fallback)“.

## Wenn es nicht klappt

| Anzeige | Bedeutung / Lösung |
|---|---|
| Rote Box auf dem Controller | Falsche/unvollständige Datei → `controller-standalone.html` neu übertragen |
| „Vermittlung nicht erreichbar“ | Handy hat kein Internet (WLAN ohne Zugang?) |
| „Code nicht gefunden“ | Zahlendreher, oder Code auf dem PC neu erzeugen. Das Angebot wird beim Vermittler hinterlegt — ein späterer Einstieg reicht also. Zusätzlich prüfen: Steht im Protokoll auf **beiden** Geräten in (Klammern) derselbe Server? |
| „Direktverbindung blockiert“ | WLAN isoliert Geräte (Gast-WLAN!) → normales Heim-WLAN nutzen |
| Datei `controller.html` statt Standalone | funktioniert nur mit dem `js`-Ordner daneben |

Die Ein-Datei-Versionen werden aus den Quellen erzeugt (`node build-standalone.js`, nur für Entwickler nötig).

## Spiele (alle echtes 3D, eigene Engine, keine Libraries)

- **Apex Drive** — 3D-Rennen mit Kurven und Hügeln, Verkehr, 4 Tageszonen (Tag, Wüste, Sunset, Nacht mit Scheinwerfern). Dicht auffahren gibt Combo-Bonus.
- **Skybound** — 3D-Flug durch Ringe (Combo-System), rote Barrieren meiden, türkise Tore geben Boost.
- **Vortex** — 3D-Röhrenflug durch grüne Tore, roten Blockern ausweichen. Die Röhre windet sich immer enger.
- **Slackline** — 3D-Balance auf schwebendem Band mit Münzen, Böen-Warnung und Kanten-Alarm.
- **Starfall** — 3D-Asteroidengürtel mit rotierenden Felsen, Energie-Orbs und Schild-Pickup (rettet einmal).

Dazu: synthetisierte Sound-Effekte (WebAudio, keine Dateien), Partikel, Screen-Shake, Nebel und dynamische Kamera. Bestwerte bleiben pro Spiel lokal gespeichert.

## Dateien

```
tilt-glass/
  index-standalone.html            Start, Auswahl, Host, Spiele (fürs erstgerät)
  controller-standalone.html  Controller als EINE Datei (fürs zweitgerät)
```

## Steuerung

Keine Buttons im Spiel. Nur links / rechts neigen (alle Spiele) plus vor / zurück (Sky: Höhe, Runner: vertikal). Kalibrierung speichert den Nullpunkt, Tiefpass + Deadzone verhindern Zittern, Hoch/Querformat wird kompensiert.

## Hinweise

- iOS verlangt für den Neigungssensor eine explizite Freigabe — dafür gibt es den Button **Sensor erlauben**.
- Falls das Auto-Pairing einmal scheitert (z. B. restriktives Gast-WLAN): Code neu erzeugen oder den Offline-Fallback per Kopieren nutzen.
