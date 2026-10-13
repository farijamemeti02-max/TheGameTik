# GAMETIKA – Deploy für Netlify + Render

## Inhalt
- `index.html`: Frontend; Videoauswahl wird bereits im Browser auf maximal 35 MB geprüft.
- `server.js`: Express API, PostgreSQL, Registrierung/Login, Profile, Avatar- und Video-Upload, Feed, Likes, Kommentare, Folgen/Freunde, Nachrichten, Live-Chat und Admin-Endpunkte.
- `render.yaml`: Render Blueprint mit Healthcheck und persistentem Upload-Datenträger.
- `netlify.toml`: Netlify veröffentlicht die `index.html` aus diesem Ordner.

## Wichtig
Ein Code-Check kann Syntaxfehler und offensichtliche Konfigurationsprobleme prüfen. Echte Registrierung, Datenbank, Uploads und Chat sind erst nach Deployment mit eingerichteter PostgreSQL-Datenbank und gesetzten Render-Variablen live testbar. Die vorhandene Frontend-API-Adresse lautet weiterhin `https://gameti.onrender.com`.

## Render
1. Den kompletten Ordnerinhalt in ein GitHub-Repository hochladen.
2. In Render ein PostgreSQL-Datenbankprojekt erstellen und die interne `DATABASE_URL` als Umgebungsvariable des Webdienstes setzen.
3. Blueprint aus `render.yaml` erstellen oder beim bestehenden Webdienst die Build Command `npm install` und Start Command `npm start` setzen.
4. Variablen setzen: `DATABASE_URL`, `JWT_SECRET` (mindestens 32 Zeichen), `UPLOAD_DIR=/var/data/uploads`, `ADMIN_USERNAME`, `ADMIN_PASSWORD`. Ein persistenter Datenträger muss unter `/var/data` gemountet sein, wenn Upload-Dateien Deployments überleben sollen.
5. Deploy abwarten und `https://DEIN-DIENST.onrender.com/health` öffnen. Erwartet: `{"ok":true,"database":"connected"}`.

## Netlify
1. Netlify mit demselben GitHub-Repository verbinden.
2. Publish directory `.` setzen; kein Build-Schritt erforderlich.
3. Die Frontend-Konstante `API_BASE` muss exakt auf die Render-URL zeigen. Aktuell ist sie `https://gameti.onrender.com`. Falls dein Dienst eine andere URL hat, diese Zeile in `index.html` anpassen und neu deployen.

## Upload-Limit
Serverseitig maximal `35 * 1000 * 1000` Bytes (35 MB) je Video, zusätzlich im Browser geprüft. Unterstützte Endungen: MP4, WebM, MOV. Reverse proxies/hosting limits können zusätzliche Grenzen setzen.
