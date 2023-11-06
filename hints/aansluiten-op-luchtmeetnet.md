# Aansluiten op luchtmeetnet

> Als gebruiker wil ik dat de app aangesloten is op de Luchtmeetnet API, zodat ik de luchtkwaliteitsindex per locatie kan ophalen

- Gebruik de luchtmeetnet API:
  - https://api.luchtmeetnet.nl/open_api/concentrations?formula=LKI&longitude={1}&latitude={2}
  - LKI staat hierin voor 'LuchtKwaliteitIndex' 
  - De longitude `{1}` en latitude `{2}` moeten gevuld worden met de coordinaten van de meetlocatie

### Hints
- Voeg een `JSON Structure` toe aan de module

    <img src="../img/add-json-structure.png"  width="750">
    
    ```json
    {
        "data": [
            {
                "formula": "LKI",
                "value": 2.40000009536743,
                "timestamp_measured": "2023-04-29T08:00:00+02:00"
            }
        ]
    }
    ```

- Geef de velden een logische `Custom name`

    <img src="../img/json-structure.png"  width="750">

- Voeg een import mapping toe, gebaseerd op de `JSON Structure`, vink alleen het onderste `(Object)` en de bijbehorende attributen aan, en klik op "OK"

  <img src="../img/import-mapping-1.png"  width="750">

- Klik bovenaan op 'Map automatically' en check daarna het `Domain model`!

- Maak een nieuwe `Microflow` aan om de luchtkwaliteit mee op te halen
- Voeg een `Input Parameter` toe van het type `Object`, kies hierbij je locatie entiteit
- Voeg een `Call REST` actie toe aan de microflow
- Stel de Location in op de locatie die in de userstory staat beschreven, met parameters voor `{1}` en `{2}`

  <img src="../img/call-rest-1.png"  width="750">

- Stel bij Response de Import mapping in die je net hebt aangemaakt
- Sla de response op in een variable en stel die in als output van de `microflow`

  <img src="../img/call-rest-2.png"  width="750">

- Done!
