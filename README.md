# get-heatpump-metrics

Script for retrieving metrics from a luxtronic heatpump.

## Description

get-heatpump-metrics connects to the websocket port of a luxtronic heatpump
and retrieves metrics.

It could be used with telegraf to push metrics to InfluxDB.

## Dependencies

* Python 3
* websockets

## Usage 

```
usage: get-heatpump-metrics [-h] [-p PIN] [-P PORT] host

retrieve metrics from luxtronic heatpump

positional arguments:
  host                  hostname or address of heatpump

options:
  -h, --help            show this help message and exit
  -p PIN, --pin PIN     secret pin
  -P PORT, --port PORT  port of websocket interface
```

## Example output

```
$ ./get-heatpump-metrics 192.168.150.254 | jq .
{
  "Temperaturen": {
    "Vorlauf": 31.7,
    "Rücklauf": 26.2,
    "Rückl.-Soll": 30.4,
    "Heissgas": 52.4,
    "Außentemperatur": 9.9,
    "Mitteltemperatur": 12.6,
    "Warmwasser-Ist": 51.2,
    "Warmwasser-Soll": 50,
    "Wärmequelle-Ein": 12.3,
    "Vorlauf max.": 70,
    "Ansaug VD": 11,
    "VD-Heizung": 55.7,
    "Überhitzung": 7.3,
    "Überhitzung Soll": 7,
    "TFL1": 26.2
  },
  "Eingänge": {
    "ASD": 1,
    "EVU": 1,
    "MOT": 1,
    "HD": 10.7,
    "ND": 4.31,
    "Durchfluss": 1230
  },
  "Ausgänge": {
    "HUP": 43.4,
    "Ventil.-BOSUP": 1,
    "Verdichter": 1,
    "ZUP": 1,
    "AO 1": 5,
    "AO 2": 5,
    "Freq. Sollwert": 3567,
    "Freq. aktuell": 3509,
    "Ventilatordrehzahl": 6.8
  },
  "Betriebsstunden": {
    "Betriebstund. VD1": 1521,
    "Impulse Verdichter 1": 334,
    "Betriebstunden ZWE1": 91,
    "Betriebstunden WP": 1521,
    "Betriebstunden Heiz.": 1493,
    "Betriebstunden WW": 28
  },
  "Anlagenstatus": {
    "Revision": 9015,
    "Inverter SW Version": 45,
    "Bivalenz Stufe": 1,
    "Leistung Ist": 7.25,
    "Abtaubedarf": 42.1
  },
  "Wärmemenge": {
    "Heizung": 7817.3,
    "Warmwasser": 216.9,
    "Gesamt": 8034.2
  },
  "Eingesetzte Energie": {
    "Heizung": 2022.4,
    "Warmwasser": 56.3,
    "Gesamt": 2078.7
  }
}
```

## Telegraf configuration

```
[[inputs.exec]]
  commands = ["/usr/local/bin/get-heatpump-metrics", "-p", "123456", "192.168.2.123"]
  interval = "5m"
  timeout = "15s"
  data_format = "json"
  name_suffix = "_heatpump"
```

## Copyright

Markus Benning &lt;ich@markusbenning.de&gt;

