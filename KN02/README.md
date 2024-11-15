# Umgang mit IAAS und AWS EC2 Instanzen

## Lernziele
- Einführung in die Nutzung von IAAS über AWS.
- Verstehen, wie virtuelle Server (Instanzen) in AWS erstellt und verwaltet werden.
- Umgang mit SSH-Keys, um auf virtuelle Server zuzugreifen.

---

## Aufgabenbeschreibung

### A) Umgang mit AWS Kurs (20%)
Erstellen Sie einen Account im AWS Academy Learner Lab und lernen Sie die Grundlagen der AWS-Umgebung.

- **Zugang zur Umgebung**:
  - Nachdem die Umgebung gestartet wurde (grüner Kreis), klicken Sie auf den AWS-Link, um auf die AWS-Konsole zuzugreifen.
  - Verfolgen Sie den Budgetverbrauch und die verbleibende Zeit, bevor die Umgebung heruntergefahren wird.
  - Starten und stoppen Sie die Umgebung nach Bedarf.
  
  ![AWS Academy Learner Lab](images/1.jpeg)

---

### B) Instanz erstellen (40%)
Erstellen Sie eine neue EC2-Instanz in AWS mit den folgenden Parametern:

- **Instanzname**: KN02
- **Betriebssystem (OS)**: Ubuntu 24.04
- **Instanztyp**: t2.micro
- **Key-Pair**: Erstellen Sie zwei Schlüssel (vorname1 und vorname2) und wählen Sie vorname1 für den Zugriff.
  
  #### Einstellungen der Instanz:
  - **Diskgröße**: Standardgröße für t2.micro Instanzen.
  - **Betriebssystem**: Ubuntu 24.04
  - **RAM-Größe**: 1 GB (t2.micro Standard)
  - **Anzahl der CPUs**: 1 (t2.micro Standard)

  ![Instanz erfolgreich gestartet](images/2.jpeg)

- **Instanzenübersicht**:
  - Die Instanz KN02 wird als aktiv und im Status "Läuft" angezeigt. Die öffentliche IP-Adresse ist sichtbar.

  ![Instanzenliste](images/3.png)

---

### C) Zugriff mit SSH-Key (40%)
Verwenden Sie SSH-Schlüssel, um sicher auf die Instanz zuzugreifen.

#### Schritte:
1. **SSH-Key Verständnis**:
   - Der private Schlüssel wird lokal gespeichert und darf nicht verloren gehen. AWS speichert den privaten Schlüssel nicht; auf dem Server ist nur der öffentliche Schlüssel hinterlegt.

2. **SSH-Verbindung zur Instanz**:
   - Verbindung zur Instanz mithilfe des ersten Schlüssels (vorname1).
   - Falls die Verbindung mit dem zweiten Schlüssel (vorname2) versucht wird, schlägt sie fehl, da nur vorname1 für den Zugriff autorisiert ist.
   
   - **Erfolgreicher SSH-Zugriff mit vorname1**:
     ![SSH Zugriff mit vorname1](images/4.png)

   - **Fehlgeschlagener SSH-Zugriff mit vorname2**:
     ![SSH Fehler mit vorname2](images/5.png)

3. **Schlüsseldetails**:
   - In den Details der Instanz ist sichtbar, dass der verwendete Schlüssel **vorname1** ist.

---

**Zusammenfassung**:
Durch die Nutzung von AWS EC2 und die Verwaltung von SSH-Schlüsseln konnte die Einrichtung einer sicheren Verbindung zur IAAS-Umgebung erfolgreich getestet werden. Diese grundlegenden Schritte in AWS bieten Einblicke in die Verwaltung und Sicherheit von virtuellen Servern.

---

**Autor**: [Ihr Name]