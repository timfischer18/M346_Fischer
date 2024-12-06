
# Automatisierung und SSH-Key-Integration mit Cloud-Init

Dieses Dokument beschreibt die Schritte, die unternommen wurden, um SSH-Keys erfolgreich mit Cloud-Init zu integrieren und eine AWS-Instanz korrekt zu konfigurieren.

## Übersicht
Wir haben eine Cloud-Init-Datei verwendet, um eine Instanz zu konfigurieren. Diese Datei enthielt:
- Benutzererstellung
- SSH-Key-Integration
- Automatische Installation von Paketen

### Schritte im Detail

---

### **1. Cloud-Init-Datei erstellen**

Die Cloud-Init-Datei wurde wie folgt erstellt:

```yaml
---
users:
  - name: ubuntu
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: users, admin
    home: /home/ubuntu
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDNR9R127WUjX8ldgJe+lJ8EuPKKySGkLbg2P5tIhmE7k6KoqF/C6MytbZ4/1APQcdkf0bZ/seIbOeTAaD3ahY1ELywbw34jycTo6LpkkxLHhwO5pGDkEfyUSmKGxFY7fRFeRhxcDGJZ2e1nGEt85PhBUdgx2OerB7gsIKzgONmQq6Qyy1H9JZZ0mzLmYK7IsusS/C6mj7eSBX/eZcCFUKzP6FQ2Q1SKKKNobA5/WiHKrHI4B5/MqpYrqHXIJZyprWZopmmyR8u1DByIPlzWAowbJAnbg5Tax/z7M2gkbb63oHEO1u8dSP1HiqmWzHi0k/UzzLakcbNQoCsXQKtQ/2H aws-key-tim
      - |
        -----BEGIN RSA PUBLIC KEY-----
        MIIBCgKCAQEA8lshkw886qvWI7jKrK3F0TIWVawmw/nJy9mC6Uy205Yto13F+piE
        llc6z0FhJBeTj1glbVqVKchII8Hu5uxOc76SNGxoj63c6z5juUjIRSNIESSuVXSR
        FZefVwqJW+Lnqod3ML6W19pDFfH6tOFmPkqPJhhZWfhAfknb1pIHc3XtCjqF/KWU
        I3v6w+hqRxfSY7pdO+Q9ZJpUuGlJVJhpDN3OmeY/6YS48k0Wsijg/3WckxFUhmWk
        vXLHd0Lv2/fKqB/Vabxu/q4vkTTIdyGJCKTkSY4ZQscb2+rk0fPVnLzU/eJDQQNB
        lBAkJILhJS48fVkkdEcIpnzr+G/hMI/7FwIDAQAB
        -----END RSA PUBLIC KEY-----  # Öffentlicher Schlüssel der Lehrperson
ssh_pwauth: false
disable_root: true
package_update: true
package_upgrade: true
packages:
  - curl
  - wget
runcmd:
  - echo "Cloud-Init erfolgreich ausgeführt!"
  - echo "Benutzer: ubuntu" > /home/ubuntu/init.log
```

---

### **2. Cloud-Init in AWS-Instanz hochladen**

1. AWS Management Console öffnen.
2. Eine neue EC2-Instanz erstellen und die Cloud-Init-Datei in der Sektion **"Advanced Details" > "User data"** hochladen.
3. Eine Sicherheitsgruppe hinzufügen, die SSH-Zugriff erlaubt.

---

### **3. Verbindung zur Instanz herstellen**

#### Mit dem GUI-SSH-Key:
```bash
ssh -i "fischer.pem" ubuntu@<instance-ip>
```
**Erfolg:** Verbindung hergestellt.

#### Mit dem Cloud-Init-SSH-Key:
```bash
ssh -i "tim.pem" ubuntu@<instance-ip>
```
**Erfolg:** Verbindung hergestellt.

---

### **4. Fehlerbehebung und Logs**

Falls die Verbindung fehlschlägt, prüfe die Logs:
```bash
cat /var/log/cloud-init-output.log
```

Screenshot des Logs:
![](./images/cloud-init-log.png)

---

### **5. Manuelle Anpassungen**

Falls der Schlüssel nicht korrekt übernommen wird:
1. Verbinde dich mit dem GUI-SSH-Key.
2. Bearbeite die Datei `~/.ssh/authorized_keys`:
```bash
nano ~/.ssh/authorized_keys
```
3. Füge die öffentlichen Schlüssel hinzu.

Screenshot der `authorized_keys`:
![](./images/authorized_keys.png)

---

### **6. Überprüfung**

**Schritte zur Überprüfung:**
- Teste den Zugriff mit beiden privaten Schlüsseln.
- Verifiziere die Pakete:
```bash
curl --version
wget --version
```

Screenshot der SSH-Verbindung:
![](./images/ssh-connection-success.png)

---

### **Ergebnis**

- Die Konfiguration wurde erfolgreich durchgeführt.
- Beide SSH-Schlüssel funktionieren und die Instanz ist korrekt eingerichtet.

---

## Screenshots

### Cloud-Init Logs
![](./images/cloud-init-log.png)

### `authorized_keys` Datei
![](./images/authorized_keys.png)

### Erfolgreicher SSH-Zugriff
![](./images/ssh-connection-success.png)

---

Viel Erfolg bei der weiteren Arbeit mit Cloud-Init!
