# Aureus – Überleben in der CF-Lunge

Ein kleines Arcade-Lernspiel für den Browser, entstanden in der Vibe-Coding-Challenge.
Du steuerst eine Kolonie *Staphylococcus aureus* durch den zähen Schleim einer Lunge
mit zystischer Fibrose (CF), sammelst Nährstoffe und weichst dem Immunsystem aus.

## Spielen

`index.html` im Browser öffnen – fertig. Keine Installation, keine Abhängigkeiten.

Mit GitHub Pages (Settings → Pages → Branch `main`) ist das Spiel auch online spielbar.

## Steuerung

| Aktion | Tastatur | Maus / Touch |
|---|---|---|
| Bewegen | Pfeiltasten oder W A S D | Zeiger bzw. Finger folgen |
| Pause | P oder Esc | Pause-Knopf oben rechts |
| Starten | Leertaste oder Enter | „Infektion starten“ |

## Regeln

**Sammeln**
- **Nährstoff** (türkis): +10 Punkte
- **Eisen (Fe)** (orange): +50 Punkte
- **Häm-Jackpot** (goldener Stern „200“): +200 Punkte – sehr selten und 1,3× schneller als alles andere
- **Biofilm** (grün): 5 Sekunden Schutz – gerammte Immunzellen geben Bonuspunkte

**Ausweichen** – fünf Gegner, von klein nach groß:
Antikörper → Komplement → Neutrophil → Makrophage → NET (Neutrophilen-Netz).
Jeder Treffer kostet ein Leben, du hast drei.

**Bosskampf** – alle 10 Level: Nach einer Warnung (Blinken + Sirene, Ton über 🔊 abschaltbar)
schiebt sich ein Boss von oben ins Bild. 30 Sekunden überleben – dabei gibt es **doppelte Punkte**,
und die Level-Uhr steht still. Vier Bosse wechseln sich ab, ab Level 50 werden sie stärker:

| Level | Boss | Angriff |
|---|---|---|
| 10, 50, 90 … | *Pseudomonas aeruginosa* | spuckt gezielt Pyocyanin, LasA und HQNO |
| 20, 60 … | *Burkholderia cepacia* | springt umher, feuert Fächer-Salven |
| 30, 70 … | *Aspergillus fumigatus* (Schimmelpilz) | lässt Sporen regnen |
| 40, 80 … | Antibiotika-Therapie | rotierende β-Laktam-Spirale |

**Belohnung nach jedem Boss:** Fehlen normale Herzen, werden sie wieder auf drei aufgefüllt.
Sind alle drei noch da, gibt es stattdessen ein **goldenes Herz**. Goldene Herzen werden bei
Treffern zuerst verbraucht, kommen nicht zurück und lassen sich stapeln.

Alle 20 Sekunden steigt das Level: Es kommen mehr Immun-Gegner, sie werden schneller,
und der Anteil der großen wächst. Dein Rekord wird im Browser gespeichert.

## Leaderboard

Nach dem Game Over kann man seinen Namen eintragen; das Spiel zeigt die **Top 3** aller
Spielerinnen und Spieler (pro Name zählt der beste Score). Gespeichert wird in einer
[Supabase](https://supabase.com)-Datenbank – Spieler brauchen dafür kein Konto, nur Internet.
Ohne Verbindung läuft das Spiel normal weiter, nur ohne Leaderboard.

Einträge lassen sich im Supabase-Dashboard unter *Table Editor → scores* ansehen und löschen.

## Biologischer Hintergrund (vereinfacht)

- Bei CF ist der CFTR-Kanal defekt; der Atemwegsschleim wird zäh und schlecht abtransportiert.
- *S. aureus* gehört zu den frühesten und häufigsten Keimen in CF-Lungen, besonders bei Kindern.
- Das goldene Pigment Staphyloxanthin schützt vor reaktiven Sauerstoffspezies der Neutrophilen.
- Im Biofilm überleben die Bakterien Antibiotika und Immunangriffe besser.
- Der Körper hält Eisen knapp („nutritional immunity“), deshalb ist es für Bakterien wertvoll.
- *P. aeruginosa* verdrängt *S. aureus* in CF-Lungen oft mit der Zeit – u. a. mit der Protease LasA
  („Staphylolysin“), dem Atmungshemmer HQNO und dem Giftpigment Pyocyanin.
- *Burkholderia cepacia* ist sehr resistent und kann das gefährliche „Cepacia-Syndrom“ auslösen.
- *Aspergillus fumigatus* ist ein Schimmelpilz, dessen Sporen eingeatmet werden; manche CF-Patienten
  entwickeln eine allergische Reaktion (ABPA).
- β-Laktam-Antibiotika wie Flucloxacillin blockieren den Zellwandaufbau von *S. aureus*; MRSA ist resistent.

Das Spiel ist stark vereinfacht und nicht maßstabsgetreu.

## Balance anpassen

Alle Werte stehen gesammelt im Script von `index.html` unter dem Kommentar `Level-Balance`:

- `LEVEL_SECONDS` – Dauer eines Levels
- `enemyInterval` – Abstand zwischen Immun-Gegnern
- `enemySpeed` / `itemSpeed` – Geschwindigkeiten
- `SHIP_SPEED` / `SHIP_ACCEL` – Tempo und Beschleunigung der eigenen Kolonie
- `IRON_CHANCE` – wie oft Eisen kommt
- `BOSS_EVERY` / `BOSS_SECONDS` / `WARN_SECONDS` – wann und wie lange der Boss kommt
- `BOSSES` – die vier Bosse mit Bewegung, Angriff und Feuerrate; `shotSpeed` – Tempo der Geschosse
- `HEME_CHANCE` / `hemeSpeed` – Häufigkeit und Tempo des Häm-Jackpots
- `ENEMY_SIZES` / `sizeRatio` – Gegnergrößen und wie schnell große häufiger werden
