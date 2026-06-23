# KN05 – Arbeit mit Speicher

# Teil B – Named Volume

## Ziel

In diesem Teil wurde gezeigt, dass mehrere Container auf denselben Speicher zugreifen können. Dazu wurde ein gemeinsames Docker Named Volume verwendet.

---

# Volume erstellen

```bash
docker volume create shared-volume
```

---

# Container 1 starten

```bash
docker run -dit --name c1 -v shared-volume:/data busybox
```

---

# Container 2 starten

```bash
docker run -dit --name c2 -v shared-volume:/data busybox
```

---

# Container 1 öffnen

```bash
docker exec -it c1 sh
```

---

# Datei erstellen und beschreiben

```bash
echo "Hallo von Container 1" >> /data/test.txt
```

---

# Datei lesen

```bash
cat /data/test.txt
```

Ausgabe:

```text
Hallo von Container 1
```

---

# Container 2 öffnen

```bash
docker exec -it c2 sh
```

---

# Datei lesen

```bash
cat /data/test.txt
```

Ausgabe:

```text
Hallo von Container 1
```

---

# Container 2 schreibt in dieselbe Datei

```bash
echo "Hallo von Container 2" >> /data/test.txt
```

---

# Datei erneut lesen

```bash
cat /data/test.txt
```

Ausgabe:

```text
Hallo von Container 1
Hallo von Container 2
```

---

# Container 1 liest dieselbe Datei

```bash
docker exec -it c1 sh

cat /data/test.txt
```

Ausgabe:

```text
Hallo von Container 1
Hallo von Container 2
```

---

# Beobachtung

Beide Container konnten dieselbe Datei lesen und verändern.

Dies zeigt, dass beide Container auf dasselbe Docker Volume zugreifen.

---

# Verwendete Befehle

```bash
docker volume create shared-volume

docker run -dit --name c1 -v shared-volume:/data busybox

docker run -dit --name c2 -v shared-volume:/data busybox

docker exec -it c1 sh

docker exec -it c2 sh

echo "Hallo von Container 1" >> /data/test.txt

echo "Hallo von Container 2" >> /data/test.txt

cat /data/test.txt
```

---

# Screencast

Datei:

```text
assets/kn05b-screencast.mov
```

Der Screencast zeigt:

1. Erstellung des Named Volumes
2. Start von Container 1 und Container 2
3. Schreiben einer Datei in Container 1
4. Lesen der Datei in Container 2
5. Schreiben zusätzlicher Daten in Container 2
6. Lesen der aktualisierten Datei in Container 1
7. Nachweis, dass beide Container denselben Speicher verwenden

---

# Fazit

Named Volumes ermöglichen es mehreren Containern, dieselben Daten zu verwenden. Änderungen eines Containers werden sofort für andere Container sichtbar. Diese Technik wird häufig bei Datenbanken oder gemeinsam genutzten Applikationsdaten eingesetzt.