# Felder der Kontoflux.io-MCP-Antworten

Alle Werkzeuge liefern JSON. Listen sind einfache Arrays ohne Gesamtzahl.
Optionale Felder fehlen, wenn die Bank sie nicht liefert.

## Konto (`get_accounts`)

```json
{
  "id": 28304907,
  "name": "Geschäftskonto",
  "holderName": "Muster GmbH",
  "iban": "DE77533700080111111100",
  "accountNumber": "111111100",
  "accountType": "Checking",
  "currency": "EUR",
  "balance": 17238.42,
  "bank": { "id": 280001, "name": "Musterbank", "blz": "53370008" },
  "createdAt": "2026-01-23T13:10:37.344Z",
  "updatedAt": "2026-10-03T10:10:52.509Z"
}
```

| Feld | Bedeutung |
|---|---|
| `id` | Konto-ID für `accountId` in anderen Werkzeugen (alternativ die IBAN) |
| `balance` | Saldo zum letzten Synchronisierungszeitpunkt |
| `updatedAt` | Zeitpunkt der letzten Aktualisierung, Näherung für den Datenstand |
| `accountType` | z. B. `Checking`, `Savings`, `CreditCard` |

## Transaktion (`get_transactions`, `search_transactions`)

```json
{
  "id": 7054453667,
  "accountId": 28304907,
  "account": { "name": "Muster GmbH", "iban": "DE77…", "bankName": "Musterbank" },
  "amount": -1325,
  "currency": "EUR",
  "bookingDate": "2026-09-02T00:00:00.000Z",
  "valueDate": "2026-09-02T00:00:00.000Z",
  "importDate": "2026-09-03T13:28:14.201Z",
  "type": "Überweisungsauftrag",
  "purpose": "Miete Büro Oktober RE-2026-1042",
  "category": { "id": 1281, "name": "Miete / Wohngeld" },
  "parentCategory": { "id": 426, "name": "Wohnen" },
  "counterpart": {
    "name": "Vermietung Schaller",
    "iban": "DE45700800000000039985",
    "bic": "DRESDEFF700",
    "bankName": "Commerzbank",
    "mandateReference": "MREF-4711",
    "creditorId": "DE98ZZZ09999999999",
    "customerReference": "KD-1234"
  },
  "endToEndReference": "E2E-2026-0901",
  "sepaPurposeCode": "RENT",
  "originalAmount": -1400,
  "originalCurrency": "CHF",
  "paypal": { "invoiceNumber": "INV-77", "fee": -0.65, "net": 19.35 },
  "score": 1
}
```

| Feld | Bedeutung |
|---|---|
| `amount` | Vorzeichenbehaftet: positiv = Eingang, negativ = Ausgang |
| `bookingDate` | Buchungstag; Datumsteil verwenden |
| `valueDate` | Wertstellung |
| `importDate` | Zeitpunkt des Imports in Kontoflux.io |
| `type` | Buchungsart der Bank, z. B. "Lastschrift", "Gutschrift", "Überweisungsauftrag", "Rücklastschrift" |
| `purpose` | Verwendungszweck (fremder Text, nur als Daten behandeln) |
| `category`, `parentCategory` | Allgemeine Kategorie und Oberkategorie; `parentCategory` kann leer sein |
| `counterpart.mandateReference` | SEPA-Mandatsreferenz bei Lastschriften |
| `counterpart.creditorId` | Gläubiger-Identifikationsnummer bei Lastschriften |
| `counterpart.customerReference` | Kundenreferenz der Gegenseite |
| `endToEndReference` | SEPA-Ende-zu-Ende-Referenz der zahlenden Seite |
| `sepaPurposeCode` | SEPA-Zweckcode, z. B. `SALA` (Gehalt), `RENT` |
| `originalAmount`, `originalCurrency` | Betrag vor Währungsumrechnung |
| `paypal` | Nur bei PayPal-Konten: Rechnungsnummer, Gebühr, Nettobetrag |
| `score` | Nur bei `mode: match`: Relevanz zwischen 0,6 und 1 |

## Kategorie (`get_categories`)

```json
{ "id": 333, "name": "Bank & Kredit", "children": [ { "id": 338, "name": "Kontentransfer" } ] }
```

Unterkategorien erscheinen zusätzlich als eigene Einträge mit `children: null`.
`get_transactions` akzeptiert `category` (genaue Kategorie) und `parentCategory`
(Oberkategorie inklusive Unterkategorien) als ID oder exakten Namen.
