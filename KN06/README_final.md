
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
## Volumen/Typ wechsel
### **Wechsel von Volumen**
![Load Balancer Screenshot](./screenshots/volumechange.png)
![Load Balancer Screenshot](./screenshots/volumeterminal.png)
![Load Balancer Screenshot](./screenshots/medium.png)
## Horizontale Skalierung mit Auto Scaling

### **1. Einrichtung eines Load Balancers**
- Ein **Application Load Balancer** wurde erstellt, um den Traffic auf mehrere Instanzen zu verteilen.
- **DNS-Name des Load Balancers:** `KN06-LoadBalancer-123456.elb.amazonaws.com`
- **Target Group:**
  - Health Check Path: `/swagger-ui/index.html`
  - Verknüpfte Instanzen: Zwei Instanzen (`KN06` und eine zusätzliche Kopie).


**Screenshot: Swagger-UI über den Load Balancer**  
![Swagger-UI Screenshot](./screenshots/swagger_ui_load_balancer.png)

---

### **2. Horizontale Skalierung mit Auto Scaling**
#### **2.1 Launch Template**
- Ein **Launch Template** wurde basierend auf dem bestehenden AMI erstellt.
- **Details:**
  - Instanztyp: `t2.medium`
  - Sicherheitsgruppen: HTTP (Port 80) und 5001 für internen Traffic.
  - VPC und Subnet: Gleich wie die bestehende Umgebung.

#### **2.2 Auto-Scaling-Gruppe**
- Eine Auto-Scaling-Gruppe wurde eingerichtet, um sicherzustellen, dass immer **mindestens 2 Instanzen** aktiv sind:
  - **Desired Capacity:** 2
  - **Minimum Capacity:** 2
  - **Maximum Capacity:** 5
  - **Scaling Policies:** 
    - Neue Instanz starten, wenn die CPU-Auslastung über 75% steigt.
    - Eine Instanz entfernen, wenn die CPU-Auslastung unter 25% fällt.

**Screenshot: Auto Scaling Gruppe Konfiguration**  
![Auto Scaling Gruppe Screenshot](./screenshots/auto_scaling_group.png)

---

### **3. Test der Auto-Scaling-Funktionalität**
- Eine Instanz wurde manuell gestoppt, um den Ausfallschutz zu testen.
- **Ergebnis:** Die Auto-Scaling-Gruppe startete automatisch eine neue Instanz, um die gewünschte Anzahl von 2 Instanzen beizubehalten.

**Screenshot: Neue Instanz durch Auto Scaling**  
![Neue Instanz Screenshot](./screenshots/new_instance_auto_scaling.png)


---

## Fazit
- Die Java-Applikation wurde erfolgreich bereitgestellt und kann horizontal skaliert werden.
- Mit Auto Scaling ist die Umgebung vor Ausfällen geschützt.
- Der Load Balancer verteilt den Traffic gleichmäßig auf die verfügbaren Instanzen.

