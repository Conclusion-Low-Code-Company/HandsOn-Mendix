# Getting started (Als het goed is zijn de eerste drie stappen al voltooit)
* Maak [hier](https://signup.mendix.com/link/signup/?ref=135440) een Mendix account aan
* Download de Mendix Modeler via de Mendix [Marketplace](https://marketplace.mendix.com/link/studiopro/8.18.10) (kies versie 8.18.10)
* Installeer de Modeler
* Download het demo-project met de ZIP op deze Github pagina
* Pak de ZIP uit, bij voorkeur niet in een Netwerk of OneDrive folder
* Open het bestand `HandsOn-Mendix.mpr`
* Login met je nieuwe Mendix account
* Alle benodigde logica, schermen en data staat in de module `AirMonitor`

# User stories
* [Legenda](#Legenda)
* [Locatie omschrijving](#Locatie-omschrijving)
* [Favoriete locaties](#Favoriete-locaties)
* [Metingen top 10](#Metingen-top-10)

## Legenda
Als gebruiker wil ik een tabel hebben van de stofnamen en hun omschrijving zodat ik deze als legenda kan gebruiken.

Wat moet je allemaal doen?
- Pagina aanpassen

### Uitwerking
- Voeg op de pagina `Component_Legend` een `data grid` toe in tweede `row` van het `Layout grid`
- Dubbelklik op het grid en kies de juiste entiteit op het tabblad `Data Source` (laat het type op database staan)
- Laat Mendix het datagrid automatisch voor je invullen
- Verwijder de knoppen `New` en `Edit` van het datagrid
- Klik op `Run locally` bovenin de modeller
- Done!
- Extra: bekijk eens hoe eenvoudig je de `Column widths` kunt aanpassen om bijvoorbeed de `Description` meer zichtbaar te maken.

## Locatie-omschrijving
Als gebruiker wil ik de organisatie verantwoordelijk voor de locatie van luchtmeetnet.nl zien als ik deze aanklik in de lijst met alle locaties, zodat ik wat meer achtergrond informatie heb.

Wat moet je allemaal doen?
- Domeinmodel aanpassen
- REST Data mapping aanpassen
- Pagina aanpassen

### Uitwerking
- De organisatie verantwoordelijk voor de locatie wordt al aangeleverd door de API van luchtmeetnet.nl, deze wordt alleen nog niet ingeladen in het datamodel.
- Voeg een String-attribuut toe op het Location object (`Station`), bijvoorbeeld `Organization`
- Open de import mapping `IM_Station` in de map API > Mappings
- Klik op `Select elements` en zet een vinkje bij Organization en klik op OK
- Dubbelklik op het rode bolletje aan de rechterkant van mapping
- Map het de waarde naar het juiste attribuut
- Voeg de Organisatie toe aan de rechterkant van het `Location_Overview` scherm
- Dubbel klik op `{Location}` en `Edit` de `Caption`
	- Voeg `Organization` toe als `Parameter`
	- Pas de `Caption` aan zodat de waarde van `Organization` gebruikt word achter de `Location`
- Klik op `Run locally` bovenin de modeller
- Done!
- Extra: Bekijk wat nog meer een nuttige plek is voor het `Organization` veld en probeer deze toe te voegen
- Extra: Zorg dat je aan de linkerkant van het `Location_Overview` scherm ook kunt zoeken op het `Organization` veld 

## Favoriete-locaties
Als gebruiker wil ik een locatie kunnen markeren als favoriet, zodat deze bovenaan in de lijst komt te staan.

Wat moet je allemaal doen?
- Domeinmodel aanpassen
- Knop op pagina
- Microflow

### Uitwerking
- Voeg een boolean attribuut met een toepasselijke naam toe op 'Location' in het domein model (bijv. IsFavorite)
- Sorteer de `list view` op de pagina `Location overview` op het zojuist toegevoegde attribuut. Dubbelklik op de regel `Sort order` om de sortering aan te passen
- Dubbelklik op de link-knop met het > icoon
- Verander de actie in `Call a microflow` en maak een nieuw microflow aan `ACT_Location_ToggleFavorite`. Deze krijgt automatisch het station mee als parameter
- Voeg een `Change object` actie toe en wissel de waarde van het nieuwe attribuut om: not($Station/IsFavorite)
- Zet `Commit`  en `Refresh in Client` beiden op Yes
- Ga terug naar het scherm en verander het > icoon in het overzicht in een `star empty` icoon
- Kopieer en plak de link-knop en vervang hiervan het icoon in `star`
- Dubbelklik op de `star empty` link en klik op edit bij `visibility` 
- Kies voor `Based on attribute value` en stel dit zo in dat je de knop alleen ziet als de locatie geen favoriet is
- Doe precies het tegenovergestelde bij de andere knop
- Klik op `Run locally` bovenin de modeller
- Done!
	
## Metingen-top-10
Als gebruiker wil ik een top 10 van de hoogste metingen zien zodat ik deze locaties kan vermijden.
Ga ervan uit dat locaties met de meeste fijnstaf (PM25) het minst fris zijn

Wat moet je allemaal doen?
- Microflow aanpassen
- Pagina aanpassen

### Uitwerking
- Open de microflow `DS_GetMeasurements_Top10`
- Voeg hier een `Microflow call` action aan toe en kies de microflow `SUB_SearchMeasurements_Last1H`
- Vul de juiste parameter in
- Voeg een `List operation` actie toe en sorteer de lijst met alle meetwaarden op `Value` (aflopend), noem de lijst `SortedMeasurementList`
- Voeg een `Loop` toe en kies de `SortedMeasurementList` om over te itereren
- Voeg binnen de loop een `change list` actie toe en voeg de meting toe aan de Top10 lijst
- Voeg binnen de loop een `aggregate list` actie toe en tel het aantal object in de de Top10 lijst
- Voeg een `Exclusive split` toe en test of er al 10 metingen in de lijst zitten ($Count >= 10)
- Voeg een `Break` en een `Continue` actie toe aan de loop, verbind de `Break` met de `True` flow van de split en `Continue` met de false flow.
- Klik op `Run locally` bovenin de modeller
- Done!

## Extra
- Laat je niet limiteren tot wat er in de opdrachten staat, als je iets bedenkt kun je het maken!
