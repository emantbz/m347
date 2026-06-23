# KN05 – Arbeit mit Speicher

# Teil A – Bind Mount

## Ziel

In diesem Teil wurde gezeigt, dass ein Ordner auf dem Host-System direkt mit einem Container geteilt werden kann. Änderungen an Dateien auf dem Host sind dadurch sofort im Container sichtbar.

---

# Script erstellen

Datei:

```text
script.sh
```

Inhalt:

```bash
#!/bin/sh

echo "Hallo von Eman"
```

---

# Container starten

```bash
docker run -dit --name kn05-bind -v $(pwd):/scripts busybox
```

---

# Container öffnen

```bash
docker exec -it kn05-bind sh
```

---

# Script ausführen

```bash
sh /scripts/script.sh
```

Ausgabe:

```text
Hallo von Eman
```

---

# Script ändern

Neuer Inhalt:

```bash
#!/bin/sh

echo "Hallo von Eman Version 2"
```

---

# Script erneut ausführen

```bash
sh /scripts/script.sh
```

Ausgabe:

```text
Hallo von Eman Version 2
```

---

# Beobachtung

Die Änderung wurde direkt im Container sichtbar, obwohl die Datei nur auf dem Host-System geändert wurde.

Dies zeigt, dass der Container und der Host denselben Speicherbereich verwenden.

---

# Verwendete Befehle

```bash
mkdir kn05a
cd kn05a

nano script.sh

docker run -dit --name kn05-bind -v $(pwd):/scripts busybox

docker exec -it kn05-bind sh

sh /scripts/script.sh
```

---

# Screencast

Datei:

```text
assets/kn05a-screencast.mov
```

Der Screencast zeigt:

1. Erstellung des Scripts auf dem Host
2. Start des Containers mit Bind Mount
3. Ausführung des Scripts im Container
4. Änderung des Scripts auf dem Host
5. Erneute Ausführung im Container
6. Sichtbare Änderung der Ausgabe

---

# Fazit

Bind Mounts eignen sich besonders für Entwicklungsumgebungen. Dateien können direkt auf dem Host bearbeitet werden und stehen dem Container sofort in der aktuellen Version zur Verfügung.