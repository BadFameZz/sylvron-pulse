# ⚙️ Sylvron Pulse – alles, was du selbst einstellen kannst

Pulse richtet sich **automatisch** ein. Alles hier ist optional. Du brauchst es nur, wenn du etwas anders haben willst.

---

## 1. Ohne Code, direkt in der Karte

| Was | Wo |
|---|---|
| **Kacheln anordnen** | oben rechts **LAYOUT ⊞** → Kacheln ziehen, an der Ecke größer/kleiner ziehen, mit ✕ ausblenden. Vorlagen: *Jarvis*, *Kompakt*, *Wand-Tablet*. Handy und Tablet haben je ein eigenes Layout. |
| **Vollbild** | **VOLLBILD ⛶**, ohne HA-Leisten (ideal fürs Wand-Tablet) |
| **System-Check** | im CORE-Log auf **⚙ CHECK**: zeigt, was Pulse gefunden hat und was fehlt, mit Reparatur per Klick |
| **Müllabfuhr einrichten** | Missionen → **🗑 Müllabfuhr einrichten** → ICS-Link deiner Stadt einfügen |
| **Missionen** | ✓ = erledigt, ⏰ = später erinnern, „zurückholen“ holt ausgeblendete wieder |
| **CORE etwas beibringen** | Sag einen Satz, den CORE nicht kennt (z. B. „Kinoabend“). Er fragt, ob er ihn lernen soll, und du sagst, was er tun soll. „Was hast du gelernt?“ zeigt alles, „Vergiss Kinoabend“ löscht es. |
| **Falsche Antwort melden** | „Das war falsch“ |

---

## 2. Im visuellen Editor

Dashboard bearbeiten → Pulse-Karte → **Bearbeiten**:
Sprache, Auto-Erkennung, System-Check-Hinweis, Maskottchen, Stimme, Begrüßungen, Wetter, Außentemperatur, maximale Leistung und Warnschwellen.

---

## 3. Für Feintuning: YAML

Karte bearbeiten → **Im Code-Editor anzeigen**. Du trägst nur ein, was du ändern willst, alles andere bleibt automatisch.

### Räume
```yaml
# Sensor pro Raum selbst festlegen (auch ein Thermostat climate.* geht)
room_sensors:
  Küche:
    temperature: climate.kueche
    humidity: sensor.kueche_luftfeuchte
  Wohnzimmer:
    temperature: sensor.wohnzimmer_temperatur

# Bereiche ausblenden
rooms_exclude: [Garage, Keller, Terrasse]
```
Komplett eigene Raumliste (ersetzt die Automatik):
```yaml
rooms:
  - name: Küche
    temperature: sensor.kueche_temperatur
    humidity: sensor.kueche_luftfeuchte
  - name: Bad
    temperature: sensor.bad_temperatur
```

### Strom
```yaml
max_power: 4000            # Watt, für die Auslastungsanzeige
power:
  - entity: sensor.hausverbrauch
    name: Haus
  - entity: sensor.waschmaschine_leistung
    name: Waschmaschine
    notify_done: true      # meldet „fertig“, wenn sie ausgeht
```

### Solar & Speicher (sonst aus dem Energie-Dashboard)
```yaml
solar_power: sensor.pv_leistung
battery_power: sensor.speicher_leistung
battery_soc: sensor.speicher_ladestand
grid_power: sensor.netz_leistung
grid_power_invert: false     # true, wenn Einspeisen/Bezug falsch herum ist
battery_power_invert: false  # true, wenn Laden/Entladen falsch herum ist
```

### Personen, Akkus, Wetter
```yaml
persons:
  - entity: person.anna
    name: Anna
batteries:
  - entity: sensor.handy_anna_battery_level
    name: Handy Anna
weather: weather.forecast_home
outdoor: sensor.aussen_temperatur
```

### Müll
```yaml
waste_calendar: calendar.abfall    # oder eine Liste von Kalendern
```

### Schnellaktionen (Leiste unten)
```yaml
actions:
  - name: Gute Nacht
    icon: mdi:weather-night
    service: script.turn_on
    target:
      entity_id: script.gute_nacht
  - name: Lichter aus
    icon: mdi:lightbulb-group-off
    service: light.turn_off
    target:
      entity_id: all
```

### Überwachung & Warnschwellen
```yaml
watch:                       # Geräte, die Pulse im Blick behält
  - entity: climate.wohnzimmer
    name: Heizung Wohnzimmer
    stale_hours: 3           # meldet sich, wenn so lange nichts kommt
humidity_high: 65            # % → „lüften“
temp_low: 17                 # °C → „Raum ist kalt“
battery_low: 20              # % → „Akku laden“
stale_hours: 6               # Sensor gilt danach als offline
```

### CORE & Stimme
```yaml
owner: Anna                  # so spricht CORE dich an
language: de                 # de / en (sonst wie Home Assistant)
tts_engine: tts.piper        # leer = Browser-Stimme
follow_up: true              # nach Rückfragen weiter zuhören
voice_log: true              # nicht Verstandenes protokollieren (nur lokal)
voice_log_list: todo.pulse_sprachlog
agent: conversation.mein_agent   # optional: eigener Gesprächsagent
```

### Darstellung
```yaml
height: 880                  # px
layout: jarvis               # Startvorlage: jarvis / kompakt / wand
layout_lock: false           # true = Layout-Knopf ausblenden (Kiosk)
auto_discover: true          # false = nur deine eigenen Angaben
system_check: true           # täglicher Hinweis bei Problemen
```

---

💡 **Tipp:** Leg die Karte in eine Ansicht vom Typ **Panel (1 Karte)**, dann bekommt sie den ganzen Platz.
🐞 Etwas passt nicht? → [Issue aufmachen](https://github.com/BadFameZz/sylvron-pulse/issues)
