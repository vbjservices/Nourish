# Nourish 5.0 — eigen server + AI + centrale database

Dit is een **werkende, zelf te hosten applicatie**, geen al-gehoste dienst. De app heeft een HTML-interface, een FastAPI-server, een SQLite-database, accounts en optionele OpenAI-chat. Je hebt een eigen server nodig om via meerdere apparaten te synchroniseren.

## Starten op je computer

Installeer Python 3.11+ en open de map `Nourish-5.0` in een terminal.

```bash
python -m pip install -r requirements.txt
# Linux/macOS:
export OPENAI_API_KEY="jouw-api-sleutel"
# Windows PowerShell:
$env:OPENAI_API_KEY="jouw-api-sleutel"
python -m uvicorn server.app:app --host 127.0.0.1 --port 8000
```

Ga naar **http://127.0.0.1:8000** in je browser, kies **Account / sync**, en maak een gebruikersaccount aan. Je kunt Nourish ook zonder account gebruiken, maar dan is er alleen lokale opslag en lokale eenvoudige maaltijdherkenning.

**AI is optioneel:** zonder `OPENAI_API_KEY` is geen live chat mogelijk. De app zegt dat expliciet. De sleutel blijft op de server; zet hem nooit in de HTML.

## Op je telefoon / andere apparaten

Hosting op een bereikbare server met **HTTPS** is vereist. Alleen op `127.0.0.1` is de app niet bereikbaar vanaf je telefoon. Host bijvoorbeeld via een eigen VPS of een platform met **persistente opslag** (SQLite-database moet op een duurzaam volume staan), voeg de `OPENAI_API_KEY` als geheime environment variable toe en zet HTTPS voor de app. Open dezelfde HTTPS-URL op je apparaten en log met hetzelfde Nourish-account in.

## Gegevens en beperkingen

- De **server** slaat voor elk account een document op met producten, recepten, dagboek, supplementen en chatgeschiedenis. Databasebestand: `nourish.sqlite3` of via `NOURISH_DB`.
- Bij eerste login op een leeg account vraagt de app of bestaande lokale gegevens moeten worden overgenomen.
- Bij gelijktijdige bewerkingen op twee apparaten kan een conflict optreden. De app **blokkeert dan overschrijven**, zodat de nieuwere serverversie niet stilletjes verloren gaat. Exporteer dan eerst lokale gegevens, herlaad en importeer handmatig als nodig. **Dit is geen real-time multi-device samenwerking**.
- Foto's worden in de JSON-data opgeslagen. Grote aantallen foto's vragen op termijn object storage en een uitgebreidere databasearchitectuur.
- Er is geen geautomatiseerde e-mail-wachtwoordreset; bewaar je wachtwoord zorgvuldig.
- De hostingbeheerder beheert de server en kan database-inhoud technisch inzien. Maak versleutelde back-ups. Gebruik HTTPS voor externe toegang.
- AI krijgt de meegestuurde product-, recept- en voedingscontext te zien. De server stuurt deze context naar de OpenAI API. Er worden geen automatische registraties gedaan zonder jouw bevestiging. De AI mag suggesties doen, maar de rekenmodule gebruikt de opgeslagen productwaarden.
- De bestaande etiket-OCR gebruikt optioneel een extern geladen browserbibliotheek. Controleer herkende waarden.
- Deze versie is bedoeld als **persoonlijk prototype**. Voor publieke uitrol ontbreken o.a. rate limiting, e-mailverificatie, herstelprocedures, monitoring en een uitgebreide security audit.

## Geen server beschikbaar?

Je kunt ook `static/index.html` rechtstreeks openen voor de oude lokale functies. Cloudsync en AI werken **alleen** via de hierboven gestarte server.
