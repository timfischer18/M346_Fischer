# Einführung in die Virtualisierung

## Lernziele
- Erste Schritte in der Virtualisierung verstehen.
- Die Grenzen der Virtualisierung kennenlernen.

---

## Hypervisor

Ein **Hypervisor**, auch bekannt als Virtual Machine Monitor (VMM), ist eine Software, Firmware oder Hardware, die die Erstellung und Verwaltung von virtuellen Maschinen (VMs) ermöglicht. Ein Hypervisor trennt das Betriebssystem und die Anwendungen von der physischen Hardware, um mehrere Betriebssysteme gleichzeitig auf derselben Hardware auszuführen.

---

## Typen von Hypervisoren

### Typ 1: Bare-Metal Hypervisor
- **Definition**: Läuft direkt auf der physischen Hardware und benötigt kein Basis-Betriebssystem.
- **Eigenschaften**:
  - Hohe Leistung, da keine Zwischenschicht vorhanden ist.
  - Wird häufig in Server-Umgebungen eingesetzt.
  - Beispiele: VMware ESXi, Microsoft Hyper-V, Xen.
- **Vorteil**: Direkter Zugriff auf die Hardware für eine bessere Effizienz und Sicherheit.

### Typ 2: Hosted Hypervisor
- **Definition**: Läuft auf einem bestehenden Betriebssystem und nutzt dessen Ressourcen.
- **Eigenschaften**:
  - Einfach einzurichten und zu verwenden.
  - Wird oft für Entwicklungs- und Testzwecke verwendet.
  - Beispiele: VMware Workstation, VirtualBox.
- **Nachteil**: Weniger effizient, da ein Betriebssystem als Zwischenschicht vorhanden ist.

---

## Unterschiede zwischen Typ 1 und Typ 2

| Merkmal                 | Typ 1 (Bare-Metal)         | Typ 2 (Hosted)            |
|-------------------------|----------------------------|---------------------------|
| **Installation**        | Direkt auf Hardware        | Auf einem OS installiert  |
| **Leistung**            | Höher                     | Geringer                  |
| **Einsatzgebiet**       | Rechenzentren, Server      | Lokale Systeme, Tests     |
| **Beispiele**           | VMware ESXi, Hyper-V      | VirtualBox, VMware Workstation |

---

## Fazit
Hypervisoren sind essenzielle Technologien für die Virtualisierung. Während Typ-1-Hypervisoren eine leistungsstarke Lösung für Server und Rechenzentren bieten, eignen sich Typ-2-Hypervisoren besser für lokale Umgebungen und Testzwecke. Beide Typen haben ihre spezifischen Vor- und Nachteile und finden in verschiedenen Szenarien Anwendung.

---

**Autor**: [Ihr Name]