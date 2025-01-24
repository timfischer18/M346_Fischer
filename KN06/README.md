
# Projekt: Java-Applikation mit Nginx und MongoDB

## Einleitung
In diesem Projekt wurde eine Java-Applikation installiert, konfiguriert und mit einem Nginx-Server sowie MongoDB Atlas verbunden. Die Applikation wurde so eingerichtet, dass sie über einen Reverse Proxy auf Port 80 erreichbar ist. Zudem wurde die Verbindung zu einer MongoDB-Datenbank erfolgreich hergestellt.

---

## Anforderungen
- **Betriebssystem:** Ubuntu
- **Applikation:** Java Spring Boot
- **Webserver:** Nginx
- **Datenbank:** MongoDB Atlas (SaaS)

---

## Schritte

### 1. Installation der notwendigen Tools
- **Java installieren:**
  ```bash
  sudo apt update
  sudo apt install -y openjdk-17-jre-headless
  ```

- **Nginx installieren:**
  ```bash
  sudo apt install -y nginx
  ```

- **Netzwerk-Tools:**
  ```bash
  sudo apt install -y net-tools
  ```

---

### 2. Bereitstellung der Applikation
- Die Datei `shopdemo-1.0.0.jar` wurde in das Verzeichnis `/var/www/app` kopiert:
  ```bash
  sudo cp /path/to/shopdemo-1.0.0.jar /var/www/app/
  ```

- **Berechtigungen anpassen:**
  ```bash
  sudo chown www-data:www-data /var/www/app/shopdemo-1.0.0.jar
  sudo chmod 644 /var/www/app/shopdemo-1.0.0.jar
  ```

---

### 3. Konfiguration der Produktionsumgebung
- Die Datei `production.properties` wurde im Verzeichnis `/var/www/app/` erstellt und konfiguriert:
  ```properties
  spring.data.mongodb.uri=mongodb+srv://timfischer:y9S2q8YUcTC6Wvvv@kn10.ihk8m.mongodb.net/shopDB?retryWrites=true&w=majority
  server.port=5001
  ```

- **Berechtigungen anpassen:**
  ```bash
  sudo chown www-data:www-data /var/www/app/production.properties
  sudo chmod 644 /var/www/app/production.properties
  ```

---

### 4. Starten der Applikation
Die Applikation wurde mit folgendem Befehl manuell gestartet:
```bash
sudo java -jar /var/www/app/shopdemo-1.0.0.jar --spring.config.additional-location=/var/www/app/production.properties
```

Die Logs zeigten an, dass die Applikation erfolgreich gestartet wurde und auf Port 5001 läuft.

![](./screenshots/application-logs.png)

---

### 5. Einrichtung von Nginx als Reverse Proxy
- **Nginx-Konfiguration:** Die Datei `/etc/nginx/sites-available/default` wurde wie folgt angepasst:
  ```nginx
  server {
      listen 80;

      server_name _;

      location / {
          proxy_pass http://127.0.0.1:5001;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
  }
  ```

- **Konfiguration testen:**
  ```bash
  sudo nginx -t
  ```
  Ausgabe: `nginx: configuration file /etc/nginx/nginx.conf test is successful`

![](./screenshots/nginx-test.png)

- **Nginx neu laden:**
  ```bash
  sudo systemctl reload nginx
  ```

---

### 6. Test der Anwendung
- **Swagger-UI:** Die Swagger-UI ist über die URL `http://IP/swagger-ui/index.html` erreichbar.

![](./screenshots/swagger-ui.png)

- **API-Endpunkte:** Die Endpunkte können direkt über die Swagger-UI getestet werden.

![](./screenshots/api-endpoint-test.png)

---

### 7. Verbindung zur MongoDB testen
Die Verbindung zur MongoDB wurde über `mongosh` geprüft:
```bash
mongosh "mongodb+srv://timfischer:y9S2q8YUcTC6Wvvv@kn10.ihk8m.mongodb.net/shopDB"
```
- **Daten anzeigen:**
  ```bash
  use shopDB
  db.products.find().pretty()
  ```

![](./screenshots/mongodb-collection.png)


---

## Fazit
Das Projekt wurde erfolgreich umgesetzt:
- Die Java-Applikation wurde bereitgestellt und konfiguriert.
- Nginx leitet Anfragen auf Port 80 korrekt an die Applikation weiter.
- Die Verbindung zur MongoDB-Datenbank funktioniert.
- Swagger-UI und API-Endpunkte sind erreichbar.

### Offene Fragen oder Verbesserungsmöglichkeiten:
- Automatisierung mit einem Systemd-Service für die Applikation.
- Optimierung der Sicherheitsgruppen und Firewalleinstellungen.
