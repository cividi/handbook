title: Projects/Wohlen

# 014-Wohlen

This is repository for project 014-Wohlen, internal purposes only. It uses data ordered from Swisscom regarding traffic for requested segments. Details can be found in \_in/ folder.
Files received from Swisscom are processed to database (\_out/wohlen.db) and after that creating final excel file.
Requested segments are for 2 different week due to validation, used is only one week. Graphs were generated to help us decide which week we use (_see \_out/segments.md)._

## Base
1. [Area of Interest Selection](_in/01_aois.md)
2. [Weeks](_in/02_week-selection.md)

## Data Validation
1. [Week Charts](_out/segments.md)

## Output
0. [Perimeters map](https://sandbox.dfour.space/de/TQMHF/GACJ45/), based on [014-Wohlen-Perimeter.csv](_out/data/014-Wohlen-Perimeter.csv)
1. Numbers by segmentId and locationRef, column structure as specified in [output sample](_in/samples/MutschellenOutput_raw.csv)
2. Actual result for a client in [014-Wohlen-ALL.xlsx](_out/data/014-Wohlen-ALL.xlsx)
3. Spliced [014-Wohlen.csv](_out/data/014-Wohlen.csv) with all perimeters and direction types

[<img src="https://sandbox.dfour.space/downloads/snapshot-screenshots/GACJ45_2021-06-01_09-29-07Z.png" height="400px">](https://sandbox.dfour.space/de/TQMHF/GACJ45/)

## Data Model

Also see [Frictionless Data Package](https://spec.frictionlessdata.io) [014-Wohlen.package.json](_out/014-Wohlen.package.json) and [validation report](_out/014-Wohlen.package.validation.yaml).

| name                                 | type    | format  | bareNumber | description                                                                                                                                                             |
|--------------------------------------|---------|---------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gemeinde BfS Nummer                  | integer | default | true       | Nummer der Ziel/Herkunfts-Gemeinde des Bundesamtes für Statistik                                                                                                        |
| Gemeinde                             | string  | default | false      | Name der Gemeinde                                                                                                                                                       |
| Segment Swisscom ID                  | integer | default | true       | Siehe Perimeter                                                                                                                                                         |
| Segment Cividi ID                    | string  | default | false      | Siehe Perimeter                                                                                                                                                         |
| Direction                            | string  | default | false      | Fahrten mit Richtung/Typ Transit = transit, Einkommend = inward, Ausgehend = outward, Kombiniert = all                                                                  |
| Fahrten Total                        | number  | default | true       | Summe aller Fahrten, gemittelt zwischen Fahrten mit Ziel / Herkunft "Gemeinde"                                                                                          |
| Fahrten Total %                      | number  | default | true       | Anteil aller Fahrten mit Ziel/Herkunft "Gemeinde" im Vergleich zu allen Fahrten durch den Perimeter                                                                     |
| Fahrten Origins Total                | integer | default | true       | Summe aller regelmässigen und unregelmässigen Fahrten mit Herkunft "Gemeinde"*                                                                                          |
| Fahrten Origins Total %              | number  | default | true       | Anteil aller regelmässigen und unregelmässigen Fahrten mit Herkunft "Gemeinde" im Vergleich zu allen regelmässigen und unregelmässigen Fahrten mit Herkunft "Gemeinde"* |
| Fahrten Destinations Total           | integer | default | true       | Summe aller regelmässigen und unregelmässigen Fahrten mit Ziel "Gemeinde"*                                                                                              |
| Fahrten Destinations Total %         | number  | default | true       | Anteil aller regelmässigen und unregelmässigen Fahrten mit Ziel "Gemeinde" im Vergleich zu allen regelmässigen und unregelmässigen Fahrten mit Ziel "Gemeinde"*         |
| Fahrten Regelmässig                  | number  | default | true       | Summe der regelmässigen Fahrten gemittelt aus Fahrten mit Herkunft oder Ziel "Gemeinde"                                                                                 |
| Fahrten Regelmässig %                | number  | default | true       | Anteil der regelmässigen Fahrten mit Herkunft oder Ziel "Gemeinde" im Vergleich zu allen regelmässigen Fahrten mit Herkunft oder Ziel "Gemeinde"                        |
| Fahrten Unregelmässig                | number  | default | true       | Summe der unregelmässigen Fahrten gemittelt aus Fahrten mit Herkunft oder Ziel "Gemeinde"                                                                               |
| Fahrten Unregelmässig %              | number  | default | true       | Anteil der unregelmässigen Fahrten mit Herkunft oder Ziel "Gemeinde" im Vergleich zu allen unregelmässigen Fahrten mit Herkunft oder Ziel "Gemeinde"                    |
| Fahrten Origins Unregelmässig        | integer | default | true       | Regelmässige Fahrten mit Herkunft "Gemeinde"*                                                                                                                           |
| Fahrten Origins Unregelmässig %      | number  | default | true       | Anteil regelmässige Fahrten mit Herkunft "Gemeinde" im Vergleich zu allen regelmässigen Fahrten mit Herkunft "Gemeinde" durch den Perimeter*                            |
| Fahrten Origins Regelmässig          | integer | default | true       | Unregelmässigen Fahrten mit Herkunft "Gemeinde"*                                                                                                                        |
| Fahrten Origins Regelmässig %        | number  | default | true       | Anteil unregelmässigen Fahrten mit Herkunft "Gemeinde" im Vergleich zu allen unregelmässigen Fahrten mit Herkunft "Gemeinde" durch den Perimeter*                       |
| Fahrten Destinations Regelmässig     | integer | default | true       | Regelmässigen Fahrten mit Ziel "Gemeinde"*                                                                                                                              |
| Fahrten Destinations Regelmässig %   | number  | default | true       | Anteil regelmässige Fahrten mit Ziel "Gemeinde" im Vergleich zu allen regelmässigen Fahrten mit Ziel "Gemeinde" durch den Perimeter*                                    |
| Fahrten Destinations Unregelmässig   | integer | default | true       | Unregelmässigen Fahrten mit Ziel "Gemeinde"*                                                                                                                            |
| Fahrten Destinations Unregelmässig % | number  | default | true       | Anteil unregelmässige Fahrten mit Ziel "Gemeinde" im Vergleich zu allen unregelmässigen Fahrten mit Ziel "Gemeinde" durch den Perimeter*                                |


## Installation
Clone this repository:
```python
gh repo clone cividi/014-Wohlen
```
## Usage
To create database:
```python
python src/main.py
```

To create Excel file `014-Wohlen-ALL.xlsx`:
```python
python src/calculations_all.py
```

To create graphs or dual grpahs run:
```python
python _in/generate_graphs.py
python _in/dual_graphs.py
```

