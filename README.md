# Laierdavid — Eventgalerie

Code-geschützte Kundengalerie für Eventfotos. Reines HTML/CSS/JS,
kein Server, keine laufenden Kosten.

## Design

Zwei Themes, umschaltbar über den Regler oben rechts (auch auf der
Code-Seite): dunkel mit Amber-Akzent und hell im Apple-Stil. Beim ersten
Aufruf richtet sich die Website nach der Systemeinstellung des Geräts.
Die Wahl landet als `?theme=dark` bzw. `?theme=light` in der Adresszeile.
Willst du ein Theme erzwingen, hänge das an den Kundenlink an.

## Dateien

```
index.html              Galerie (Codeeingabe, Raster, Lightbox, Downloads)
styles.css              Design (hell, Apple-Stil)
app.js                  Logik
admin.html              Werkzeug: Zugangscode-Hash + JSON-Block erzeugen
data/galleries.json     Galerien: Titel, Datum, Ort, Code-Hash, Dateiliste
fotos/<galerie-id>/     Fotos einer Galerie
```

Wichtig: Der Unterordner heißt genauso wie die `id` der Galerie,
z. B. `fotos/sommerparty26/`.

## Zwei Betriebsarten

In `data/galleries.json` steuert der Block `quelle` das Verhalten.

### A) manuell (Standard)

```json
"quelle": { "typ": "manuell" }
```

Die Dateien stehen in der Liste `fotos` der Galerie. Volle
Kontrolle über Reihenfolge, aber jede neue Datei muss eingetragen werden.

### B) github (automatisch)

```json
"quelle": {
  "typ": "github",
  "owner": "DEIN-GITHUB-NAME",
  "repo": "galerie",
  "branch": "main",
  "fotosOrdner": "fotos"
}
```

Die Website liest den Ordner `fotos/<id>/` selbst aus (öffentliche GitHub-API)
und sortiert nach Dateinamen. Du lädst also nur noch hoch — `galleries.json`
musst du nur bei einer **neuen** Galerie anfassen (Titel, Datum, Ort,
Code-Hash). Die Liste `fotos` dient dann als Reserve, falls die API gerade
nicht antwortet.

Dateinamen mit führender Nummer bestimmen die Reihenfolge und den angezeigten
Titel: `04-crowd-hoch.jpg` → „Crowd hoch".

## Neues Event einstellen

1. Bilder exportieren (JPEG, Langseite ca. 2000 px, Qualität ~85).
2. `admin.html` öffnen, Titel/Datum/Ort/Code eintragen → Code-Hash und
   JSON-Block kopieren, in `data/galleries.json` einfügen.
3. Ordner `fotos/<id>/` anlegen und die Fotos hochladen.
4. Kunden den Link mit Code schicken: `…/index.html?code=DEINCODE`

## Hosting (kostenlos)

| Ort | Grenze | Hinweis |
| --- | --- | --- |
| GitHub Pages | 100 MB pro Datei, Repo möglichst < 1 GB | Repo muss öffentlich sein; Automatikmodus B funktioniert nur hier |
| Cloudflare Pages | 25 MB pro Datei, 20 000 Dateien | schnell, gute Wahl für viele Fotos |
| Netlify Drop | 100 GB Traffic/Monat | Ordner einfach hineinziehen, kein Repo nötig |

## Sicherheitshinweis

Der Code schützt vor neugierigen Blicken, nicht gegen Fachleute: Auf einer
statischen Seite bleiben die Dateien über ihre direkte Adresse erreichbar.
Für echten Schutz braucht es signierte Links (z. B. Supabase Storage oder
Cloudflare R2 mit Signatur).
