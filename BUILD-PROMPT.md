```json
{
  "board_name": "ArduinoUnoProtoShield",
  "one_liner": "Ein stabiles Prototyping-Shield für den Arduino Uno R3 mit großzügiger THT-Rasterfläche, dedizierten I2C- und SPI-Sensor-Schnittstellen, Status-LEDs, Reset-Taster und integriertem Signalgeber.",
  "market_gap": "Gängige Prototyping-Boards haben oft unübersichtliche Pins oder zu kleine Rasterflächen. Dieses Shield bietet beschriftete Rand-Schnittstellen für Displays und Sensoren (I2C/SPI) sowie eine klar strukturierte THT-Lötmatrix, um fliegende Wackelkontakte beim Basteln zu verhindern.",
  "confidence": "high",
  "price_eur": 9,
  "target_enclosure": "Direktmontage als Shield auf Arduino Uno R3 / Standard-Gehäuse für Arduino Uno",
  "injection_notes": "keine"
}
```

## BUILD-PROMPT

### 1. Projekt-Setup & Mechanik
- **Projektname:** `ArduinoUnoProtoShield`
- **Abmessungen:** Standard Arduino Uno R3 Shield Formfaktor (68.6 mm × 53.3 mm). Ecke oben links abgeschrägt, Ausbuchtung für USB/Power-Buchse anpassen (Keepout).
- **Befestigungsbohrungen:** 4× M3 (Ø 3.2 mm) mit den exakten, normierten Arduino-Uno-Koordinaten (relativ zum Nullpunkt unten links der USB-Schnittstelle):
  - Loch 1: X = 13.97 mm, Y = 2.54 mm
  - Loch 2: X = 15.24 mm, Y = 50.80 mm
  - Loch 3: X = 66.04 mm, Y = 7.62 mm
  - Loch 4: X = 66.04 mm, Y = 35.56 mm
- **Lagen-Stackup:** 4 Lagen (F.Cu für Signale, In1.Cu als durchgehende GND-Plane, In2.Cu als durchgehende 5V/3.3V Versorgungs-Plane, B.Cu für Signale).

### 2. Schaltplan (Netzliste)
Die Pins werden über vier Standard-Stift-/Buchsenleisten (für die Verbindung zum Arduino Uno R3) kontaktiert.
- **CN_PWR (Power Header, 1x08):**
  - Pin 1: NC
  - Pin 2: IOREF
  - Pin 3: RESET -> Netz `RESET`
  - Pin 4: 3.3V -> Netz `3.3V`
  - Pin 5: 5V -> Netz `5V`
  - Pin 6: GND -> Netz `GND`
  - Pin 7: GND -> Netz `GND`
  - Pin 8: VIN -> Netz `VIN`
- **CN_AD (Analog Header, 1x06):**
  - Pin 1: A0 -> Netz `A0`
  - Pin 2: A1 -> Netz `A1`
  - Pin 3: A2 -> Netz `A2`
  - Pin 4: A3 -> Netz `A3`
  - Pin 5: A4 -> Netz `A4` (auch I2C SDA)
  - Pin 6: A5 -> Netz `A5` (auch I2C SCL)
- **CN_DIG_L (Digital Low Header, 1x08):**
  - Pin 1: D0/RX -> Netz `D0`
  - Pin 2: D1/TX -> Netz `D1`
  - Pin 3: D2 -> Netz `D2`
  - Pin 4: D3 -> Netz `D3`
  - Pin 5: D4 -> Netz `D4`
  - Pin 6: D5 -> Netz `D5`
  - Pin 7: D6 -> Netz `D6`
  - Pin 8: D7 -> Netz `D7`
- **CN_DIG_H (Digital High Header, 1x10):**
  - Pin 1: D8 -> Netz `D8`
  - Pin 2: D9 -> Netz `D9`
  - Pin 3: D10 -> Netz `D10` (SPI CS)
  - Pin 4: D11 -> Netz `D11` (SPI MOSI)
  - Pin 5: D12 -> Netz `D12` (SPI MISO)
  - Pin 6: D13 -> Netz `D13` (SPI SCK)
  - Pin 7: GND -> Netz `GND`
  - Pin 8: AREF -> Netz `AREF`
  - Pin 9: SDA -> Netz `A4` (Hardware SDA Brücke)
  - Pin 10: SCL -> Netz `A5` (Hardware SCL Brücke)

#### Peripherie & Hilfsschaltungen
- **S1 (Reset-Taster):** THT-Taster zwischen Net `RESET` und `GND`.
- **D1 (Power-LED, Grün):** THT-LED 5mm. Anode über R1 (330 Ohm) an `5V`, Kathode an `GND`.
- **D2 (Status-LED, Gelb):** THT-LED 5mm. Anode über R2 (330 Ohm) an `D9`, Kathode an `GND`.
- **BZ1 (Buzzer):** THT Aktiv-Summer (12mm). Pin (+) über Jumper JP1 (1x02) an `D6`, Pin (-) an `GND`.
- **J_I2C (Schnittstelle, 1x04):** I2C-Sensor-Header. Pin 1: `GND`, Pin 2: `5V`, Pin 3: `A4` (SDA), Pin 4: `A5` (SCL).
- **J_SPI (Schnittstelle, 1x06):** SPI-Display-Header. Pin 1: `GND`, Pin 2: `5V`, Pin 3: `D13` (SCK), Pin 4: `D12` (MISO), Pin 5: `D11` (MOSI), Pin 6: `D10` (CS).
- **C1, C2 (Entkopplung):** 100 nF THT-Kondensatoren. C1 zwischen `5V` und `GND`, C2 zwischen `3.3V` und `GND`.
- **C3 (Pufferung):** 10 µF THT Elko zwischen `5V` (+) und `GND` (-).
- **Lötmatrix (Prototyping):** 4 parallel verlaufende, unkontaktierte Standard-Stiftleisten 1x12 (Pitch 2.54 mm), die eine freie 12x4 Rasterfläche zum Einlöten eigener Sensoren/ICs bilden.

### 3. Bauteilliste & Footprints
1. **CN_PWR:** `Connector_PinSocket_2.54mm:PinSocket_1x08_P2.54mm_Vertical` (oder vorzugsweise das integrierte Footprint `Module:Arduino_UNO_R3_WithMountingHoles` verwenden, falls in der Bibliothek vorhanden).
2. **CN_AD:** `Connector_PinSocket_2.54mm:PinSocket_1x06_P2.54mm_Vertical`.
3. **CN_DIG_L:** `Connector_PinSocket_2.54mm:PinSocket_1x08_P2.54mm_Vertical`.
4. **CN_DIG_H:** `Connector_PinSocket_2.54mm:PinSocket_1x10_P2.54mm_Vertical`.
5. **S1:** `Button_Switch_THT:SW_PUSH_6mm_H5mm` (Standard 6x6 mm THT-Taster).
6. **D1, D2:** `LED_THT:LED_D5.0mm`.
7. **R1, R2:** `Resistor_THT:R_Axial_DIN0207_L6.3mm_D2.5mm_P7.62mm_Horizontal` (0.25W Handlöt-Widerstand).
8. **BZ1:** `Buzzer_Beeper:Buzzer_12x9.5mm` (Aktiv-Summer Pitch 7.6 mm).
9. **JP1:** `Connector_PinHeader_2.54mm:PinHeader_1x02_P2.54mm_Vertical` (Standard 2.54 mm Jumper).
10. **J_I2C:** `Connector_PinHeader_2.54mm:PinHeader_1x04_P2.54mm_Vertical`.
11. **J_SPI:** `Connector_PinHeader_2.54mm:PinHeader_1x06_P2.54mm_Vertical`.
12. **C1, C2:** `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm`.
13. **C3:** `Capacitor_THT:CP_Radial_D5.0mm_P2.00mm` (Elko 5 mm Durchmesser).
14. **J_PROTO1, J_PROTO2, J_PROTO3, J_PROTO4:** `Connector_PinHeader_2.54mm:PinHeader_1x12_P2.54mm_Vertical` (unbeschaltete Löt-Pads für das Lochraster).

### 4. Layout & Platzierungs-Vorgaben
- **Arduino-Header (CN_PWR, CN_AD, CN_DIG_L, CN_DIG_H):** Müssen exakt an den Rändern des Shields platziert werden, damit sie auf den Arduino Uno R3 passen (Achtung auf das asymmetrische 4.064 mm / 160 mil Spacing zwischen Digital High und Digital Low Header!). Bei Verwendung des kombinierten Shield-Footprints entfällt diese manuelle Ausrichtung.
- **Benutzerschnittstellen (Reset S1, LEDs D1/D2, Sensor-Header J_I2C, J_SPI):** An den oberen/seitlichen Rändern platzieren für maximale Zugänglichkeit im aufgesteckten Zustand.
- **Summer BZ1 & Jumper JP1:** Rechts oben nahe dem analogen Bereich platzieren.
- **Rasterlötfläche (J_PROTO1-4):** Im Zentrum des Boards als zusammenhängende Matrix platzieren (ca. 12x4 Pins bzw. nach verfügbarem Platz vergrößern).
- **Entkopplungskondensatoren (C1, C2):** Nah an den jeweiligen Spannungsversorgungspins von CN_PWR platzieren.

### 5. Routing & DRC-Vorgaben
- **Signal-Leiterbahnen:** Breite ≥ 0.4 mm.
- **Power-Leiterbahnen (5V, 3.3V, GND):** Breite ≥ 0.8 mm.
- **Clearance (Isolationsabstand):** ≥ 0.3 mm.
- **GND-Planes:** Eine durchgehende Kupferfläche auf Lage 2 (In1.Cu) erstellen und mit `GND` verbinden.
- **VCC-Plane:** Eine durchgehende Kupferfläche auf Lage 3 (In2.Cu) für die Stromschienen erstellen.

### 6. Design Rule Check & Export
1. Führe ein automatisches Routing (z.B. Freerouting / integrierter Router) auf den Außenlagen durch.
2. Führe den Design Rule Check (DRC) durch; behebe eventuelle Airwires oder Überlappungen.
3. Platziere eindeutige Bezeichner (`RESET`, `POWER`, `STATUS`, `I2C`, `SPI`, `PROTO-AREA`) gut lesbar auf dem Bestückungsdruck (F.SilkS).
4. Exportiere die Gerber-Daten (Kupfer, Lötstoppmaske, Bestückungsdruck) sowie die Bohrdaten (.drl) vollständig in das Verzeichnis `./gerbers`.

---

*Hinweis: Bitte fasse am Ende ehrlich zusammen, welche Schritte erfolgreich umgesetzt wurden und ob Abweichungen nötig waren.*
