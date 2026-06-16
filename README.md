<img src="resources/img/sva-logo.png" alt="SVA Logo" align="right" width="160">
<!-- TODO @Jan: SVA-Logo unter resources/img/sva-logo.png ablegen -->

# Agentic AI Bootcamp – HR Edition

**Tag 2 · 24. Juni · IBM Ehningen**
Ein gemeinsames Hands-on-Lab von **IBM** und **SVA**

> Willkommen! In diesem Repository finden Sie alles, was Sie für den praktischen Teil des Bootcamps brauchen: Beispiel-Use-Cases, importierbare Tools (OpenAPI-Spezifikationen) und eine Schritt-für-Schritt-Anleitung, um Ihren ersten eigenen HR-Agenten in **IBM watsonx Orchestrate (wxO)** zu bauen – inklusive Anbindung an eine **Personio**-Umgebung.

---

## Inhaltsverzeichnis

1. [Was haben wir heute vor?](#1-was-haben-wir-heute-vor)
2. [Was ist Personio – und wie funktioniert die Anbindung?](#2-was-ist-personio--und-wie-funktioniert-die-anbindung)
3. [Wie integriere ich die bereitgestellten Tools?](#3-wie-integriere-ich-die-bereitgestellten-tools)
4. [Wissensdatenbank: HR-FAQ & Richtlinien testen](#4-wissensdatenbank-hr-faq--richtlinien-testen)
5. [Beispiel-Use-Cases](#5-beispiel-use-cases)
6. [Hilfreiche Links](#hilfreiche-links)

---

## 1. Was haben wir heute vor?

Heute bauen Sie **selbst** einen KI-Agenten in watsonx Orchestrate – mit Fokus auf **HR-Automatisierung**.

Aufbauend auf den Grundlagen vom ersten Tag geht es heute darum, selbst Hand anzulegen: Sie verstehen, wie ein Agent *denkt*, *Tools auswählt* und *Aktionen* in einem Fremdsystem ausführt. Als Spielwiese dient eine angebundene Personio-Umgebung, in der Ihr Agent z. B. Mitarbeiterdaten nachschlagen oder Abwesenheiten anlegen kann.

**Der Ablauf heute:**

1. **Grundlagen & Setup** – Wir richten gemeinsam Ihren Zugang ein und verbinden Ihre Umgebung mit Personio (siehe [Abschnitt 2](#2-was-ist-personio--und-wie-funktioniert-die-anbindung)).
2. **Tools importieren** – Sie binden die bereitgestellten Tools über eine OpenAPI-Spezifikation an Ihren Agenten an (siehe [Abschnitt 3](#3-wie-integriere-ich-die-bereitgestellten-tools)).
3. **Eigenen Agenten entwerfen** – Sie gestalten Rolle, Anweisungen (Instructions) und Verhalten Ihres Agenten selbst. Die [Beispiel-Use-Cases](#5-beispiel-use-cases) dienen Ihnen als Inspiration und Startpunkt – nicht als Vorgabe.
4. **Testen & iterieren** – Sie probieren Ihren Agenten im Chat aus, beobachten seine Reasoning-Schritte und schärfen Instructions und Tool-Auswahl nach.

> 💡 **Wichtig:** Die kreative Arbeit – *welchen* Agenten Sie bauen und *wie* er sich verhält – liegt bei Ihnen. IBM und SVA stellen die Infrastruktur, die Tools und die Beispiele bereit. Wir sind den ganzen Tag für Fragen vor Ort.

**Was Sie mitbringen sollten:**

- Einen Laptop mit aktuellem Browser
- Ihren Zugang zu watsonx Orchestrate (erhalten Sie vor Ort)
- Die **E-Mail-Adresse**, mit der Sie sich für das Bootcamp angemeldet haben (wird für die Personio-Anbindung benötigt – siehe unten)

---

## 2. Was ist Personio – und wie funktioniert die Anbindung?

### Kurz: Was ist Personio?

[Personio](https://www.personio.de/) ist eine weit verbreitete HR-Software für kleine und mittelständische Unternehmen. Sie bündelt typische Personalprozesse an einem Ort – u. a. **Mitarbeiterverwaltung**, **Abwesenheits- und Urlaubsmanagement**, **Recruiting** und **Gehaltsabrechnung**.

Für unser Bootcamp ist vor allem eines wichtig: Personio lässt sich von außen ansteuern. Das heißt, Ihr Agent kann auf Knopfdruck HR-Aufgaben in Personio erledigen – zum Beispiel eine Abwesenheit eintragen oder Mitarbeiterdaten nachschlagen.

### Wie funktioniert die Anbindung?

Damit Sie sich um die technischen Details nicht kümmern müssen, haben wir die Verbindung zu Personio bereits vorbereitet. Stellen Sie es sich wie einen **Vermittler** vor, der zwischen Ihrem Agenten und Personio sitzt:

```
   Ihr Agent    ───►   Vermittler   ───►   Personio
 (watsonx                                 (HR-Daten)
  Orchestrate) ◄───                 ◄───
```

Dieser Vermittler erledigt im Hintergrund die Anmeldung bei Personio und stellt Ihrem Agenten **fertige Bausteine („Tools")** bereit – jedes Tool steht für eine konkrete HR-Aktion, etwa „Mitarbeiter suchen" oder „Abwesenheit anlegen". Sie wählen einfach aus, welche dieser Bausteine Ihr Agent nutzen darf.

**Das bedeutet für Sie:**

- Sie müssen sich **nicht mit Passwörtern, Zugängen oder Technik** beschäftigen – das übernimmt der Vermittler.
- Sie arbeiten mit **verständlichen Bausteinen**, nicht mit roher Technik.
- Sie können sich ganz auf das konzentrieren, was zählt: **Welche HR-Aufgabe soll Ihr Agent übernehmen?**

### Einrichtung – machen wir live gemeinsam 🤝

> **Wichtiger Hinweis:** Die Verbindung Ihrer Umgebung mit Personio richten wir **gemeinsam vor Ort** ein – Sie müssen hier nichts vorbereiten.

Die Zuordnung Ihres Zugangs erfolgt über **genau die E-Mail-Adresse, mit der Sie sich für das Bootcamp angemeldet haben**. Halten Sie sie also bereit. Wir gehen den Einrichtungsschritt Schritt für Schritt zusammen durch, damit am Ende jede:r Teilnehmer:in eine funktionierende Personio-Anbindung hat.

<!-- TODO @Jan: Falls es vorbereitende Schritte gibt (z. B. Connection-Anlage in wxO, Eingabe der Mailadresse in einem Formular), hier konkret ergänzen. -->

---

## 3. Wie integriere ich die bereitgestellten Tools?

Die Tools werden über eine **OpenAPI-Spezifikation** (`.yaml`-Datei) in Ihren Agenten importiert. Das funktioniert direkt in der Oberfläche von watsonx Orchestrate – ganz ohne Code.

> 📄 **Die Datei, die Sie brauchen:** [`<DATEINAME>.yaml`](./<DATEINAME>.yaml)
> <!-- TODO @Jan: Exakten Dateinamen der Proxy-Spec eintragen und die Datei ins Repo legen. -->

### Schritt-für-Schritt-Anleitung

**Schritt 1 – Agenten öffnen und zum Toolset wechseln**

Öffnen Sie Ihren Agenten im Agent-Builder von watsonx Orchestrate und scrollen Sie zum Bereich **Toolset**. Klicken Sie dort auf das **`+`** (bzw. **„Add tool" / „Tool hinzufügen"**).

![Schritt 1: Im Toolset des Agenten auf das Plus-Symbol klicken](resources/img/01-toolset-add-tool.jpg)

---

**Schritt 2 – „Import" über OpenAPI wählen**

Wählen Sie im erscheinenden Dialog die Option zum **Import über eine OpenAPI-Spezifikation** (**„OpenAPI"** bzw. **„Import from a file / OpenAPI"**).

![Schritt 2: Import-Option „OpenAPI" auswählen](resources/img/02-select-openapi.jpg)

---

**Schritt 3 – YAML-Datei hochladen**

Laden Sie die bereitgestellte Datei **`<DATEINAME>.yaml`** hoch (per Drag-and-drop oder über den Datei-Dialog).

![Schritt 3: Die YAML-Spezifikation hochladen](resources/img/03-upload-yaml.jpg)

---

**Schritt 4 – Tools (Operationen) auswählen**

watsonx Orchestrate liest die Spezifikation ein und zeigt Ihnen alle verfügbaren **Operationen** als Tools an. Setzen Sie die Haken bei den Tools, die Ihr Agent verwenden soll.

> 💡 **Tipp:** Importieren Sie nur die Tools, die Ihr Use-Case wirklich braucht. Ein fokussiertes Toolset hilft dem Agenten, die passende Aktion zuverlässiger zu wählen.

![Schritt 4: Die gewünschten Tools aus der Spezifikation auswählen](resources/img/04-select-tools.jpg)

---

**Schritt 5 – Hinzufügen & testen**

Bestätigen Sie die Auswahl (**„Add" / „Hinzufügen"**). Die Tools erscheinen nun im Toolset Ihres Agenten. Testen Sie anschließend im Chat, ob Ihr Agent die Tools im passenden Moment aufruft.

![Schritt 5: Die importierten Tools im Toolset des Agenten](resources/img/05-tools-added.jpg)

---

> ✅ **Geschafft!** Ihr Agent kann ab jetzt über die importierten Tools mit Personio interagieren.
>
> Wenn ein Tool nicht wie erwartet aufgerufen wird, prüfen Sie zuerst die **Tool-Beschreibung** und die **Instructions** Ihres Agenten – sie sind der wichtigste Hebel, damit der Agent versteht, *wann* er *welches* Tool nutzen soll.

---

## 4. Wissensdatenbank: HR-FAQ & Richtlinien testen

Neben den Tools (die *Aktionen* in Personio ausführen) kann Ihr Agent auch **Fragen beantworten** – auf Basis einer Wissensdatenbank (**Knowledge Base**).

Dafür liegt in diesem Repository eine **FAQ-Datei** bereit:

> 📄 **Datei:** [`<FAQ-DATEINAME>`](./<FAQ-DATEINAME>)
> <!-- TODO @Jan: Exakten Dateinamen der FAQ-Datei eintragen und Datei ins Repo legen. -->

Sie enthält typische **HR-FAQs und Richtlinien** (z. B. Fragen rund um Urlaub, Krankmeldung, Arbeitszeiten oder interne Abläufe). Sie können diese Datei in eine **Knowledge Base** Ihres Agenten laden und damit ausprobieren, wie gut Ihr Agent passende Antworten findet und formuliert.

So lassen sich beide Bausteine kombinieren:

- **Tools** → für *Aktionen* (etwas in Personio tun)
- **Knowledge Base** → für *Wissen* (Fragen zu Richtlinien & Abläufen beantworten)

> 📝 **Anleitung folgt.** Die Schritt-für-Schritt-Anleitung zum Anlegen der Knowledge Base und zum Hochladen der FAQ-Datei ergänzen wir noch.
> <!-- TODO @Jan/SVA: Schritt-für-Schritt-Anleitung (Knowledge Base anlegen, FAQ-Datei hochladen, im Chat testen) ergänzen – ggf. mit Screenshots unter resources/img/. -->

---

## 5. Beispiel-Use-Cases

Die folgenden Use-Cases dienen Ihnen als Inspiration und Startpunkt. Sie müssen sie nicht 1:1 umsetzen – kombinieren, erweitern oder erfinden Sie gerne eigene Szenarien.

<!-- TODO @Jan/SVA: Use-Cases ausformulieren. Vorschlag pro Use-Case: Titel, kurzer Nutzen,
     benötigte Tools, Beispiel-Dialog. Platzhalter unten als Gerüst. -->

### Use-Case 1: _<Titel>_

> _Platzhalter – wird ergänzt._

- **Was der Agent tut:** _<Beschreibung>_
- **Benötigte Tools:** _<Tool A, Tool B>_
- **Beispiel-Prompt:** _„<Beispielanfrage eines Nutzers>"_

### Use-Case 2: _<Titel>_

> _Platzhalter – wird ergänzt._

- **Was der Agent tut:** _<Beschreibung>_
- **Benötigte Tools:** _<Tool A, Tool B>_
- **Beispiel-Prompt:** _„<Beispielanfrage eines Nutzers>"_

### Use-Case 3: _<Titel>_

> _Platzhalter – wird ergänzt._

- **Was der Agent tut:** _<Beschreibung>_
- **Benötigte Tools:** _<Tool A, Tool B>_
- **Beispiel-Prompt:** _„<Beispielanfrage eines Nutzers>"_

---

## Hilfreiche Links

- **watsonx Orchestrate – Produkt-Doku:** https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base
- **Personio:** _<LINK FOLGT>_
  <!-- TODO @Jan: Personio-Link/Umgebung ergänzen, sobald Zugang vorhanden. -->

---

<sub>Bei Fragen während des Bootcamps: Sprechen Sie Ihr IBM- oder SVA-Team direkt an. Viel Erfolg beim Bauen! 🚀</sub>
