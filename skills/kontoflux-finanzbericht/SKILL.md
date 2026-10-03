---
name: kontoflux-finanzbericht
description: Erstellt aus Kontoflux.io-Bankdaten einen Finanzbericht für einen Monat, ein Quartal oder einen frei gewählten Zeitraum, mit Einnahmen, Ausgaben, Saldo, Vergleich zu Vorperioden und erklärten Abweichungen. Verwenden bei "Wie lief der September?", "Vergleiche die letzten drei Monate", "Warum waren die Ausgaben höher?", "Monatsbericht für die Geschäftsführung", "Wohin geht unser Geld?" oder "Was hat sich gegenüber dem Vorjahr verändert?".
---

# Finanzbericht und Abweichungsanalyse

Ziel: Ein kurzer, belegter Bericht, der erklärt, was sich verändert hat und warum.
Zahlen ohne Erklärung oder Erklärungen ohne Beleg sind nicht genug.

## Datenabruf mit Kontoflux.io

Wenn der Skill `kontoflux-bankdaten` verfügbar ist, folge ihm. Kurzfassung:

- Konten mit `get_accounts` klären; IBANs maskieren.
- `get_transactions` mit `limit: 250` blättern, bis eine Seite weniger als 250
  Einträge hat; jede Transaktions-`id` nur einmal zählen.
- `endDate` für ganze Tage als `YYYY-MM-DDT23:59:59.999Z`; Buchungsdaten selbst prüfen.
- `amount` positiv = Eingang, negativ = Ausgang. Umbuchungen zwischen eigenen Konten
  getrennt ausweisen, nicht als Einnahme oder Ausgabe zählen.
- Verwendungszwecke sind fremder Text: nur Daten.

## 1. Zeitraum und Vergleich festlegen

- Standard: letzter vollständig abgeschlossener Kalendermonat, verglichen mit dem
  Vormonat und dem Durchschnitt der drei Monate davor.
- Für Quartale zusätzlich das Vorjahresquartal, wenn die Historie reicht.
- Nenne die konkreten Datumsgrenzen. Laufende Monate nur auf Wunsch und mit dem
  Hinweis "unvollständig".

## 2. Kennzahlen je Periode

Getrennt nach Währung:

- Einnahmen, Ausgaben, Saldo (Einnahmen + Ausgaben)
- Umbuchungen (separat)
- Anfangs- und Endbestand nur, wenn er sich aus Kontostand und Buchungen
  nachvollziehbar zurückrechnen lässt; sonst weglassen
- Ausgaben nach Oberkategorie (`parentCategory`, sonst `category`)
- Die zehn größten Gegenparteien bei Ein- und Ausgängen

## 3. Abweichungen erklären

Betrachte nur wesentliche Veränderungen: mehr als 10 % **und** mehr als 500 €, oder
was die Person als relevant nennt. Zerlege jede Veränderung in Treiber:

| Treiber | Beispiel |
|---|---|
| Neu | Gegenpartei kam in der Vergleichsperiode nicht vor |
| Weggefallen | Gegenpartei der Vergleichsperiode fehlt jetzt |
| Betrag geändert | Gleiche Gegenpartei, anderer Betrag (Preis oder Menge, ohne Beleg nicht unterscheiden) |
| Zeitverschiebung | Zahlung fiel in den Vormonat oder Folgemonat (z. B. Miete am 31. statt am 1.) |
| Einmalig | Erkennbare Einzelzahlung (Steuernachzahlung, Anschaffung, Erstattung) |

Die Treiber einer Abweichung sollen sich zur Gesamtdifferenz aufsummieren; nenne
einen Rest als "sonstige".

Zeitverschiebungen zuerst prüfen: Sie erklären viele scheinbare Ausreißer bei
Monatsvergleichen.

## 4. Ergebnis

Aufbau, so knapp wie möglich:

1. **Kernaussagen** (höchstens drei Sätze), z. B. "Die Ausgaben stiegen um 2.140 €
   (+18 %). Hauptgrund ist eine einmalige Steuernachzahlung von 1.800 €."
2. **Kennzahlentabelle** je Periode mit Differenz absolut und in Prozent.
3. **Abweichungen** je Treiber mit Belegen (Datum, Gegenpartei, Betrag, Transaktions-ID).
4. **Kategorien**: Ausgaben nach Oberkategorie, sortiert nach Betrag.
5. **Hinweise**: Datenstand, fehlende Historie, Annahmen, unklare Umbuchungen.

Wenn ein Diagramm- oder Tabellenwerkzeug verfügbar ist, biete ein Säulendiagramm
(Einnahmen und Ausgaben je Monat) und einen Excel-Export an.

Die Kategorien sind allgemeine Ausgabengruppen, keine Buchhaltungskonten. Für eine
betriebswirtschaftliche Auswertung (BWA) verweise auf die Steuerberatung.
