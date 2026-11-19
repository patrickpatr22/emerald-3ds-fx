# Emerald 3DS – FX-Boost (Testversion)

Emerald 3DS v0.5.0 mit übertrieben aufgedrehten Effekten in der Voxel-Welt.
Alle Änderungen hängen am Schalter `CTR_VOXEL_FX_BOOST` in
`3ds_port/src/voxel/ctr_voxel.h` (`0` = wieder genau das Original).

## Was anders ist

| Effekt | Original | FX-Boost |
|---|---|---|
| Sonnenstrahlen (God Rays) | schwach, nur obere linke Ecke | 4× stärker, schmaler, fast über den ganzen Bildschirm |
| Lichtflecken unter Bäumen | kaum sichtbar | 3,5× mehr Kontrast |
| Bloom (Leuchten) | 0,07, nur sehr Helles | 3× stärker, viel mehr leuchtet (Schwelle 0,85 → 0,70) |
| Schatten und Farben | weich | Sonne wärmer und heller, Schatten kühler und dunkler |
| Staub im Sonnenlicht | 32 Partikel | 160 auf dem New 3DS (72 auf dem Old 3DS), größer, höher in der Luft |
| Glühwürmchen | – | Nachts 96 (44) grün-goldene Lichter, die über dem Boden schweben und pulsieren |
| Dunst in der Ferne | 0,14 | etwas mehr an klaren Tagen |

Glühwürmchen gibt es nur bei klarem Himmel draußen, nicht bei Regen, Sand,
Asche, Nebel, Schnee oder unter der Erde.

## Deutsche Version bauen (Windows mit WSL)

Die Programmdatei muss zur Sprache deines Datenpakets passen, und die deutsche
braucht beim Bauen dein eigenes, sauberes **Pokémon Smaragd-Edition**-ROM
(SHA-1 `61c2eb2b380b1a75f0c94b767a2d4c26cd7ce4e3`, dasselbe wie für den
Emerald-3DS-Builder). Das ROM bleibt auf deinem PC.

### 1. Ubuntu unter Windows einrichten (einmalig)

PowerShell **als Administrator** öffnen:

```powershell
wsl --install -d Ubuntu
```

PC neu starten, „Ubuntu“ aus dem Startmenü öffnen und Benutzername und
Passwort festlegen.

### 2. Werkzeuge installieren (einmalig, im Ubuntu-Fenster)

```sh
sudo apt update
sudo apt install -y build-essential g++ libpng-dev python3 python3-pil git wget
wget https://apt.devkitpro.org/install-devkitpro-pacman
chmod +x install-devkitpro-pacman
sudo ./install-devkitpro-pacman
sudo dkp-pacman -S --needed 3ds-dev
```

Bei der Paketauswahl einfach Enter drücken (alle). Danach das Ubuntu-Fenster
schließen und neu öffnen.

### 3. Bauen

ROM-Pfad anpassen: `C:\Users\Name\Downloads\smaragd.gba` heißt in Ubuntu
`/mnt/c/Users/Name/Downloads/smaragd.gba`.

```sh
source /etc/profile.d/devkit-env.sh
git clone https://github.com/patrickpatr22/emerald-3ds-fx
cd emerald-3ds-fx
python3 tools/bootstrap.py --make -j8 --python python3 --german-rom "/mnt/c/Users/Name/Downloads/smaragd.gba"
```

Das dauert beim ersten Mal 10–30 Minuten. Fertig ist es, wenn diese Datei
existiert:

```sh
ls -lh build/upstream/3ds_port/emerald3ds.3dsx
cp build/upstream/3ds_port/emerald3ds.3dsx /mnt/c/Users/Name/Desktop/
```

### 4. Auf die SD-Karte

1. SD-Karte in den PC, Ordner `/3ds/emerald3ds/` öffnen.
2. **Sichern:** `emerald3ds.sav` und `Emerald3DS.3dsx` an einen sicheren Ort
   kopieren.
3. Die neue `emerald3ds.3dsx` vom Desktop in `/3ds/emerald3ds/` kopieren und
   die alte `Emerald3DS.3dsx` ersetzen (gleicher Name, Groß-/Kleinschreibung
   egal). Diese Datei bringt ihre Spieldaten selbst mit.
4. Im Homebrew Launcher starten.

Zurück zum Original: die gesicherte `Emerald3DS.3dsx` wieder hineinkopieren.

## Testen

- **Nacht und Glühwürmchen:** OPTION → ENHANCEMENTS → DAY / NIGHT auf AUTO,
  CLOCK SPEED auf 60X (ein Tag dauert dann 24 Minuten).
- **Bildrate:** den FPS-Zähler in den Optionen des unteren Bildschirms
  einschalten.
- **Gute Orte:** Route 101–104 und Baumhausen (Bäume, Sonne), Route 111
  (Wüste: Sandsturm-Bloom).
- **Screenshots:** Rosalina-Menü (L + Steuerkreuz runter + Select) →
  „Take screenshot“, landet in `/luma/screenshots/`.
