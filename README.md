# Inforium Vragenlijst

Demonstratie van browserlokale vragenlijsten. Gebruik fictieve gegevens.

## Live demo en directe invullinks

De startpagina opent de korte demo: vier optionele vragen over bezoekreden, medicijnen, operaties en verwachtingen op twee invulpagina’s. Afronden mag ook zonder antwoorden. De resultaatpagina biedt dezelfde deelopties als de ESS-demo: ingestelde portaalroute (ZODOS-website, zonder automatische overdracht), deelmenu met PDF + JSON, e-mailconcept aan Mediwebs en losse downloads.

- [Korte demo (standaard)](https://inforium-vragenlijst.maroei.chatgpt.site/)
- [Directe korte invullink](https://inforium-vragenlijst.maroei.chatgpt.site/korte-demo)

- [Algemene medische intake](https://inforium-vragenlijst.maroei.chatgpt.site/algemene-intake)
- [Bestaande Epworth-demo](https://inforium-vragenlijst.maroei.chatgpt.site/invullen/epworth)
- [Linkoverzicht voor Inforium](https://inforium-vragenlijst.maroei.chatgpt.site/voorbeelden)
- [Vragenlijstbouwer](https://inforium-vragenlijst.maroei.chatgpt.site/maken)

De invullink bevat geen patiëntgegevens. Een Inforium kan direct verwijzen naar een van de invulroutes. Antwoorden blijven in React-geheugen van het browservenster, zonder localStorage, database of automatische verzending. Sluiten of vernieuwen wist de antwoorden. De invuller maakt en downloadt zelf PDF en JSON. Automatische terugkoppeling naar Inforium/EPD is niet geïmplementeerd.

## Algemene intake

Negen onderdelen: identiteit en bezoekreden, medische voorgeschiedenis, operaties/opnames, medicatie/bloedverdunners, allergieën, roken/vapen, alcohol/middelen, dagelijks functioneren en aanvullende informatie. Condities bepalen de zichtbare vervolgvragen. Verborgen antwoorden worden uitgesloten van samenvatting en export. Geen klinische scoring; alleen een schatting van pakjaren voor sigaretten als beide invoervelden bekend zijn.

Nederlands en Engels. Voorlezen via de spraakfunctie van de browser; beschikbaarheid en stem hangen af van apparaat/browser. PDF-uitvoer gebruikt een eenvoudige meerpagina-renderer met ASCII-transliteratie. De intake is een eigen concept, geen gevalideerde meetvragenlijst. De bestaande Epworth-demo vermeldt haar bron en licentievoorbehoud.

## Ontwikkelen

Node.js >=22.13, Linux met flock, curl en GNU timeout.

```sh
npm run install:ci
npm run dev
npm run build
node --test tests/*.test.mjs
```

Routes staan in `app/`, de modulaire intakedefinitie en validatie in `lib/intake.ts`, de bestaande ESS-definitie in `lib/questionnaire.ts`, en de lokale PDF-renderer in `lib/pdf.ts`.

De live demo draait via [ChatGPT Sites](https://inforium-vragenlijst.maroei.chatgpt.site/) en [GitHub Pages](https://mediwebs.github.io/inforium-vragenlijst/). GitHub bevat een zelfstandige index.html en de volledige broncode in inforium-vragenlijst-broncode.zip. Beide gebruiken dezelfde React-componenten. Bij wijzigingen moeten de broncodekopie en beide publicaties afzonderlijk worden bijgewerkt. Productiegebruik met echte patiëntgegevens vereist een afzonderlijk ingerichte en gecontroleerde zorgworkflow.


## QR-samenvatting en afdruk
De resultaatpagina van de korte demo kan lokaal een QR-code met geselecteerde, bewerkbare tekst maken en als PNG downloaden. Limiet: 800 UTF-8-bytes inclusief code, versie, vraagnummers en antwoorden, foutcorrectie M, witte marge van vier modules. Lange tekst wordt nooit afgekapt. Iedereen die de QR scant kan de inhoud lezen; er is geen opslag of QR-generatiedienst. Print antwoorden opent het browser-afdrukvenster met alle antwoorden en een eventuele QR-code.

De laatste vraag over verwachtingen en belangrijke informatie is optioneel en heeft maximaal 200 tekens. Afronden is mogelijk wanneer de complete QR-samenvatting binnen 800 UTF-8-bytes past; een teller en melding helpen bij inkorten. Geen antwoorden worden stilzwijgend afgekapt.

## Gelijke versies op Sites en GitHub Pages
Alle drie vragenlijsten hebben lokale QR-samenvatting en printuitvoer. Epworth gebruikt expliciete selectie en Radix data-state="checked" voor de gekleurde antwoordvakjes. GitHub Pages bundelt dezelfde React-componenten: ?form=short, ?form=epworth, ?form=intake, ?form=examples en ?form=builder. De linkbouwer bewaart de GitHub-projectmap; gegenereerde #q-links blijven werken. Lange medische intake/ESS-antwoorden kunnen boven de QR-grens van 800 bytes uitkomen; selecteer minder onderdelen of kort de samenvatting zelf in. PDF, JSON en print bevatten alle antwoorden.

## Genummerde QR-uitvoer
Codes: INF-KORT-001 = korte demo, INF-ESS-001 = Epworth, INF-INTAKE-001 = medische intake. Vragen hebben vaste nummers; bij conditionele vervolgvragen worden nummers niet opnieuw verdeeld. QR bevat alleen `<vragenlijstcode>` en `<vraagnummer>: <antwoord>`, geen titel of vraagtekst. Bij Epworth zijn 9 en 10 de totaalscore en duiding. Zelfgemaakte vragenlijsten krijgen een code op basis van hun definitie-ID. De volledige tekst blijft in PDF/print beschikbaar. QR-grens blijft 800 UTF-8-bytes.

## Intakevragenlijstbouwer
De bouwer start met de algemene medische intake, inclusief beide talen, tekst- en keuzevragen en condities. Een wijziging aan het standaardvoorbeeld krijgt een eigen code; verdere wijzigingen behouden die code en verhogen de versie. Bij toevoegen, verwijderen en verplaatsen nummert de bouwer alle vragen doorlopend opnieuw. De invullink bewaart de definitie in #intake=; antwoorden blijven lokaal. Op Sites en GitHub Pages opent deze link de intakecomponent met de bewerkte definitie en PDF/JSON/QR/print. Ongeldige definities worden afgewezen.

## Meerdere vragenlijsten bewaren
De intakebouwer heeft **Mijn vragenlijsten**: opslaan, wijzigingen opslaan, openen, verwijderen, nieuwe lijst starten en kopiëren met een eigen code. Formulierdefinities worden expliciet in browser-localStorage bewaard (maximaal 100); patiëntantwoorden blijven alleen in het invulvenster. Opslag is per browser/apparaat en per website-origin, dus GitHub Pages en Sites delen de opgeslagen lijsten niet. Exporteer/importeer een vragenlijst als JSON voor een reservekopie of overdracht naar een ander apparaat. Import controleert de definitie en bewaart geen meegeleverde antwoorden. Opslagproblemen worden zichtbaar gemeld. Gegenereerde invullinks blijven zelfstandig werken.

Bij het starten, openen of opslaan van een eigen intake maakt de bouwer direct een zelfstandige invullink, met **Open in browser** en **Kopieer link**. Elke opgeslagen lijst heeft daarnaast **Bewerken** en **Open invullink**. De URL bevat de formulierdefinitie en werkt zonder toegang tot de browseropslag van de maker. Bewerken en opnieuw opslaan genereert een nieuwe URL; eerder gedeelde links blijven hun oorspronkelijke definitie openen.

## Antwoordtypen, volgorde en pagina’s
De bouwer ondersteunt korte/lange tekst, getal, datum, e-mail, telefoon, één keuze met knoppen of keuzelijst, meerdere keuzes, ja/nee/weet ik niet en een schaal 0–10. Optieteksten zijn in beide talen bewerkbaar. Elke sectie is één invulpagina; pagina’s kunnen worden hernoemd, toegevoegd, verplaatst, gesplitst en samengevoegd. Vragen verplaatsen via pijlen, positienummer of paginakeuze. De nummers worden doorlopend bijgewerkt; verborgen vervolgvragen behouden tijdens invullen hun vastgelegde nummer. Voorwaarden blijven gekoppeld aan vraag-ID’s; een hoofdvraag moet voor de vervolgvragen staan. Verwijderen van een hoofdvraag verwijdert ook haar afhankelijke vervolgvragen.

Een bewerkingsronde verhoogt automatisch de laatste versiecomponent (bijvoorbeeld 1.1 → 1.2). Opslaan, een invullink maken of JSON exporteren legt deze versie vast; de volgende wijziging krijgt weer een hogere versie. Eerder gedeelde links houden hun oorspronkelijke inhoud. Intake-QR’s bevatten `code@versie` plus vraagnummers en antwoorden. PDF en JSON gebruiken de vragenlijsttitel in de bestandsnaam; intake en korte-demo-uitvoer voegen ook de versie toe. Definitie-export eindigt op `-vragenlijst.json`. Meerdere keuzes worden in resultaat-JSON als een array van antwoordcodes en als leesbare tekst opgenomen.
