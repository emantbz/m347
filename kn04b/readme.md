# KN04 – Docker Compose (Teil B)

## Ziel

In diesem Teil wurden die bereits in KN02 erstellten und auf Docker Hub publizierten Images verwendet. Im Gegensatz zu Teil A wird kein Dockerfile mehr benötigt, da die fertigen Images direkt heruntergeladen und gestartet werden.

---

# Docker Compose Datei

Datei:

```text
docker-compose.yml
```

Verwendete Images:

```yaml
image: emanfarukmujanovic/m347:kn02b-web
image: emanfarukmujanovic/m347:kn02b-db
```

Dadurch müssen keine Images mehr lokal gebaut werden.

---

# Unterschiede zu Teil A

| Teil A | Teil B |
|----------|----------|
| build: ./web | image: emanfarukmujanovic/m347:kn02b-web |
| Dockerfile notwendig | Kein Dockerfile notwendig |
| Image wird lokal gebaut | Image wird von Docker Hub geladen |

---

# Netzwerk

Für Teil B wurde ein eigenes Docker-Netzwerk mit einem anderen IP-Bereich verwendet.

Beispiel:

```yaml
subnet: 172.20.0.0/16
ip_range: 172.20.5.0/24
gateway: 172.20.5.254
```

Dadurch befinden sich beide Container im selben Netzwerk.

---

# Test info.php

URL:

```text
http://localhost:8084/info.php
```

Die Seite wurde erfolgreich geladen.

## Screenshot

![B1](assets/info-php.png)

*Abbildung B1: Ausgabe von info.php mit sichtbaren Netzwerkinformationen.*

---

# Test db.php

URL:

```text
http://localhost:8084/db.php
```

Beim Aufruf erscheint ein Fehler.

## Screenshot

![B2](assets/db-php-failed.png)

*Abbildung B2: Fehlermeldung beim Zugriff auf die Datenbank.*

---

# Warum tritt der Fehler auf?

Das Web-Image wurde bereits in KN02 erstellt und enthält die damalige Datenbankkonfiguration.

Im Image wurde die Datenbank über eine feste IP-Adresse angesprochen:

```php
$servername = "172.19.0.2";
```

Docker Compose erstellt jedoch ein neues Netzwerk und vergibt neue IP-Adressen.

Dadurch existiert die ursprünglich gespeicherte IP-Adresse nicht mehr und die Verbindung zur Datenbank schlägt fehl.

---

# Wie kann man das Problem lösen?

Statt einer festen IP-Adresse sollte der Containername verwendet werden.

Beispiel:

```php
$servername = "kn04b-db";
```

oder der entsprechende Service-Name aus der Docker-Compose-Datei.

Docker stellt innerhalb eines Netzwerks automatisch einen DNS-Dienst bereit und kann Containernamen auflösen.

Dadurch bleibt die Verbindung auch dann bestehen, wenn sich die IP-Adresse eines Containers ändert.

---

# Verwendete Befehle

```bash
docker compose up -d

docker compose ps

docker compose down
```

---

# Fazit

Die Verwendung publizierter Images vereinfacht das Deployment, da keine lokalen Builds mehr notwendig sind. Der Fehler in db.php zeigt jedoch, weshalb feste IP-Adressen in Containerumgebungen vermieden werden sollten. Die bessere Lösung ist die Verwendung von Containernamen innerhalb eines gemeinsamen Docker-Netzwerks.