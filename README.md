# Opstarten
* Download het zip bestand van [WeTransfer](https://we.tl/t-axNGEGaOPh).en pak dat uit (liefst niet in een OneDrive map)
* Installeer de Mendix modeler met het .exe bestand
* Open het bestand `COS.mpr`
* Klik het inlogscherm weg
* Alle benodigde logica, schermen en data staat in de module `AirMonitor`

# User stories
* [Legenda](#Legenda)
* [Locatie omschrijving](#Locatie-omschrijving)
* [Favoriete locaties](#Favoriete-locaties)
* [Metingen top 10](#Metingen-top-10)

## Legenda
Als gebruiker wil ik een tabel hebben van de stofnamen en hun omschrijving zodat ik deze als legenda kan gebruiken.
- Pagina aanpassen

### Uitwerking
- Voeg op de pagina `Component_Legend`een `data grid` toe in het vak `Main`
- Dubbelklik op het grid en kies de juiste entiteit op het tabblad `Data Source` (laat het type op database staan)
- Laat Mendix het datagrid automatisch voor je invullen
- Verwijder de knoppen `New` en `Edit` van het datagrid
- Klik op `Run locally` bovenin de modeller
- Done!

## Locatie-omschrijving
Als gebruiker wil ik de omschrijving van de locatie van luchtmeetnet.nl zien als ik deze aanklik in de lijst met alle locaties, zodat ik wat meer achtergrond informatie heb.
- Domeinmodel aanpassen
- REST Data mapping aanpassen
- Pagina aanpassen

### Uitwerking
- De omschrijving van de locatie wordt al aangeleverd door de API van luchtmeetnet.nl, deze wordt alleen nog niet ingeladen in het datamodel.
- Voeg een String-attribuut toe op het Location object, bijvoorbeeld 'Description', zet de lengte op unlimited
- Open de import mapping `IM_Station` in de map API > Mappings
- Klik op `Select elements` en zet een vinkje bij NL onder description en klik op OK
- Dubbelklik op het rode bolletje aan de rechterkant van mapping
- Map het de waarde naar het juiste attribuut
- Voeg een `text area` widget toe aan de rechterkant van het `Location_Overview` scherm
- Kies het juiste attribuut en verberg het label
- Klik op `Run locally` bovenin de modeller
- Done!

## Favoriete-locaties
Als gebruiker wil ik een locatie kunnen markeren als favoriet, zodat deze bovenaan in de lijst komt te staan.
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
- Microflow aanpassen
- Pagina aanpassen

### Uitwerking
- Open de microflow `DS_GetMeasurements_Top5`
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
