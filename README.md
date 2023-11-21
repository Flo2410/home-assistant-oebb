

# Get information about next departures

A sensor platform which allows you to get information about departures from a specified OEBB stop.






## Example configuration.yaml

```yaml
sensor:
  - platform: oebb
    L: vs_scotty.vs_liveticker
    evaId: 317040
    productsFilter: 1011111111011
```

## Configuration variables

key | description
-- | --
**platform (Required)** | The platform name.
**evaID (Required)** | OEBB Stop ID
**dirInput (Optional)** | OEBB stop ID - filter journey stopping there
**boardType (Optional)** | ? 
**tickerID (Optional)** | ? 
**start (Optional)** | ? 
**eqstop (Optional)** | ? 
**showJourneys (Optional)** | ? number of journeys to retreive
**additionalTime (Optional)** | ? offset to query next journeys

## Sample overview

![Sample overview](overview.png)

you will need custom frontend plugins "auto-entities" and "config-template-card" for this:

```
type: custom:auto-entities
card:
  square: false
  type: grid
  title: Abfahrt Siedlerstraße
  columns: 1
card_param: cards
filter:
  include:
    - entity_id: sensor.oebb_journey_*
      options:
        type: custom:config-template-card
        variables:
          _state: states['this.entity_id'].state
          _startTime: states['this.entity_id'].attributes.startTime
          _lastStop: states['this.entity_id'].attributes.lastStop
          _line: states['this.entity_id'].attributes.line
          _status: states['this.entity_id'].attributes.status
          _delay: states['this.entity_id'].attributes.delay
        entities:
          - this.entaity_id
        card:
          type: tile
          entity: this.entity_id
          name: ${_lastStop}
          color: '${_delay > ''0'' ? ''red'' : ''green''}'
          state_content:
            - line
            - state
            - delay
  exclude:
    - state: unavailable
    - attributes:
        line: Bus 221
    - attributes:
        line: S 60
sort:
  method: state
  count: 15
```

## Notes

To retreive the parameters please vitit the website, choose your stop and check the iframe url:
https://fahrplan.oebb.at/bin/stboard.exe/
Additional info on the parameters can also be found on the getStationBoardDataOptions() docu in the mymro repo - Thanks!

Sources: 
https://github.com/mymro/oebb-api

This platform is using the [OEBB API]([https://fahrplan.oebb.at/bin/stboard.exe/]) API to get the information.

