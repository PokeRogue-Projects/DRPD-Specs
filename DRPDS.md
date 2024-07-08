# Daily Run Pathing Description Specifications

## Version 0.1.2
The keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://www.rfc-editor.org/info/bcp14) [RFC2119](https://datatracker.ietf.org/doc/html/rfc2119) [RFC8174](https://datatracker.ietf.org/doc/html/rfc8174) when, and only when, they appear in all capitals, as shown here.

This document is licensed under [The Gnu GPL License, version 3](https://www.gnu.org/licenses/gpl-3.0.en.html)

## Introduction
The Daily Run Pathing Description Specifications (DRPDS) defines a standardized, language agnostic structure to describe a path of the daily run of [pokerogue.net](https://pokerogue.net) .

It contains any and all information needed to display a daily run.

## Definitions
### DRPD Document
A self-contained resource which describes a Daily Run Path. The DRPD document MUST contain at least one date field, a waves field and an author field. A DRPD document uses and conforms to the Daily Run Pathing Description Specification.

## Specification
### Version
The Daily Run Pathing Description Specifications is versioned using a major.minor.patch versioning scheme. The major.minor portion of the version string (for example 3.1) SHALL designate the DRPDS feature set. .patch versions address errors in, or provide clarifications to, this document, not the feature set. Tooling which supports DRPDS `0.1` SHOULD be compatible with all DRPDS `0.1.*` versions. The patch version SHOULD NOT be considered by tooling, making no distinction between `0.1.0` and `0.1.1` for example.

Occasionally, non-backwards compatible changes may be made in minor versions of the DRPDS where impact is believed to be low relative to the benefit provided.

An DRPD document compatible with DRPDS `0.*.*` contains a required version field which designates the version of the DRPDS that it uses.

### Format
A Daily Run Pathing document that conforms to the Daily Run Pathing Description Specifications is itself a JSON object, which may be represented either in JSON or YAML format.

For example, if a field has an array value, the JSON array representation will be used:

```json
{
   "field": [ 1, 2, 3 ]
}
```
All field names in the specification are case-sensitive. This includes all fields that are used as keys in a map, except where explicitly noted that keys are case-insensitive.

The schema exposes two types of fields: Fixed fields, which have a declared name, and Patterned fields, which declare a regex pattern for the field name.

Patterned fields MUST have unique names within the containing object.

In order to preserve the ability to round-trip between YAML and JSON formats, YAML version 1.2 is RECOMMENDED along with some additional constraints:

-    Tags MUST be limited to those allowed by the JSON Schema ruleset.
-    Keys used in YAML maps MUST be limited to a scalar string, as defined by the YAML Failsafe schema ruleset.

### Document Structure
A DRPD document MUST be made up of a single document.

It is RECOMMENDED that the root DRPD document be named according to the date of the daily run and an optional keyword in the format YYYY-MM-DD_keyword : 2024-07-07_uxie.json or 2024-07-07_stakataka.yaml.

### Data Types
Data types in the DRPDS are based on the types supported by the [JSON Schema Specification Draft 2020-12](https://datatracker.ietf.org/doc/html/draft-bhutton-json-schema-00#section-4.2.1).

### Schema
In the following description, if a field is not explicitly **OPTIONAL**, it MUST be considered **REQUIRED**.

### DRPD Object
This is the root of the [DRPD Document](#drpd-document)

#### Fixed Fields
| Field Name | Type                                 | Description                                                                                          |
|------------|--------------------------------------|------------------------------------------------------------------------------------------------------|
| version    | `string`                             | This string MUST be the version number of the DRPD Specification that the DRPD document uses.        |
| title      | `string`                             | **OPTIONAL** An additional title to discriminate from other run on the same day.                     |
| authors    | \[`string`]                          | **OPTIONAL** The name of the pathers that worked on that run.                                        |
| date       | `string`                             | The date of the daily run in the format YYYY-MM-DD.                                                  |
| waves      | \[[Wave Object](#wave-object)]       | The waves of the daily run. This array MUST contain all 50 waves and be ordered by wave number       |
| starters   | \[[Pokemon Object](#pokemon-object)] | The starting pokemons of the run. The pokemon send in the first wave MUST be the first in this list. |

#### Example
```json
{
  "version": "0.1.2",
  "title": "Wo-chien route",
  "authors": ["Vicksin", "Javi", "RoulixStone"],
  "date": "2024-07-07",
  "waves": [],
  "starters": []
}
```

### Wave Object
#### Fixed Fields
| Field Name    | Type                              | Description                                                                          |
|---------------|-----------------------------------|--------------------------------------------------------------------------------------|
| id            | `integer`                         | The id of the run.                                                                   |
| reload        | `boolean`                         | Set to `true` if the run must be reloaded at the start of this wave.                 |
| type          | `string`                          | The type of the wave. The value MUST be "wild", "trainer" or "boss".                 |
| double        | `boolean`                         | Whether the wave is a double battle or not                                           |
| actions       | [`string`]                        | A list of all actions taken in order                                                 |
| shop          | `boolean`\|`string`               | The name of the item to buy, `false` if there is no shop at this wave (boss or flee) |
| biome         | `string`                          | Biome of the wave                                                                    |
| trainer       | [Trainer Object](#trainer-object)  | **OPTIONAL** This field MUST be completed if the wave is a "trainer" battle          |
| pokemon_left  | [Pokemon Object](#pokemon-object) | **OPTIONAL** This field MUST be completed if the wave is "wild" or "boss" battle     |
| pokemon_right | [Pokemon Object](#pokemon-object) | **OPTIONAL** This field MUST be completed if the wave is a double "wild" battle      |

#### Example
```json
{
  "id": 3,
  "reload": false,
  "type": "wild",
  "double": false,
  "action": [
    "Pokeball",
    "Transfer|Basculin All > Azumarill",
    "Transfer|Diggersby All > Azumarill"
  ],
  "shop": "Reload > Poke Ball",
  "biome": "Graveyard",
  "pokemon_left": {}
}
```

### Pokemon Object
#### Fixed Fields
| Field Name      | Type                           | Description                                                 |
|-----------------|--------------------------------|-------------------------------------------------------------|
| id              | `integer`                      | Pokedex ID of the pokemon                                   |
| name            | `string`                       | Species of the pokemon                                      |
| ability         | `string`                       | Ability of the pokemon                                      |
| isHiddenAbility | `boolean`                      | `true` if the ability of the pokemon is it's hidden ability |
| passive         | `string`                       | Passive of the pokemon                                      |
| nature          | `string`                       | Nature of the pokemon                                       |
| gender          | `string`                       | Gender of the pokemon                                       |
| rarity          | `string`                       | Biome rarity of the pokemon                                 |
| capture         | `boolean`                      | `true` if the pokemon is captured                           |
| level           | `integer`                      | Level of the pokemon                                        |
| items           | \[[Item Object](#item-object)] | List of the objects held by the pokemon                     |
| ivs             | [Iv Object](#iv-object)        | IVs of the pokemon                                          |

#### Example
```json
{
  "id": 708,
  "name": "phantump",
  "ability": "Frisk",
  "isHiddenAbility": false,
  "passive": "Ripen",
  "nature": "Careful",
  "gender": "female",
  "rarity": "Common",
  "capture": true,
  "level": 20,
  "items": [],
  "ivs": {}
}
```

### Iv Object
| Field Name | Type      | Description                                      |
|------------|-----------|--------------------------------------------------|
| hp         | `integer` | HP IV of the pokemon (between 0 and 31)          |
| atk        | `integer` | Attack IV of the pokemon (between 0 and 31)      |
| def        | `integer` | Defence IV of the pokemon (between 0 and 31)     |
| spatk      | `integer` | Spe.Attack IV of the pokemon (between 0 and 31)  |
| spdef      | `integer` | Spe.Defence IV of the pokemon (between 0 and 31) |
| spe        | `integer` | Speed IV of the pokemon (between 0 and 31)       |

#### Example
```json
{
  "hp": 15,
  "atk": 10,
  "def": 16,
  "spatk": 2,
  "spdef": 30,
  "spe": 25
}
```

### Trainer Object
#### Fixed Fields
| Field Name      | Type      | Description         |
|-----------------|-----------|---------------------|
| id              | `integer` | ID of the trainer   |
| name            | `string`  | Name of the trainer |
| type            | `string`  | Type of the trainer |

#### Example
```json
{
  "id": 34,
  "name": "BREEDER John",
  "type": "breeder_m"
}
```

### Item Object
#### Fixed Fields
| Field Name | Type      | Description          |
|------------|-----------|----------------------|
| id         | `integer` | ID of the item       |
| name       | `string`  | Name of the item     |
| quantity   | `integer` | Quantity of the item |

#### Example
```json
{
  "id": 5,
  "name": "Sitrus Berry",
  "quantity": 2
}
```

### Example of a complete DRPD document
```json
{
    "version": "0.1.0",
    "title": "Wo-chien route",
    "authors": ["Vicksin", "Javi", "RoulixStone"],
    "date": "2024-07-07",
    "waves": [
      {
        "id": 1,
        "reload": true,
        "type": "wild",
        "double": false,
        "action": [
          "Pokeball",
          "Transfer|Basculin All > Azumarill",
          "Transfer|Diggersby All > Azumarill"
        ],
        "shop": "Reload > Poke Ball",
        "biome": "Graveyard",
        "pokemon_left": {
          "id": 708,
          "name": "phantump",
          "ability": "Frisk",
          "isHiddenAbility": false,
          "passive": "Ripen",
          "nature": "Careful",
          "gender": "female",
          "rarity": "Common",
          "capture": true,
          "level": 20,
          "items": [],
          "ivs": {
            "hp": 15,
            "atk": 10,
            "def": 16,
            "spatk": 2,
            "spdef": 30,
            "spe": 25
          }
        }
      },
      {
        "id": 2,
        "reload": false,
        "type": "wild",
        "double": true,
        "action": [
          "Pokeball",
          "Transfer|Basculin All > Azumarill",
          "Transfer|Diggersby All > Azumarill"
        ],
        "shop": "Reload > Poke Ball",
        "biome": "Graveyard",
        "pokemon_left": {
          "id": 708,
          "name": "phantump",
          "ability": "Frisk",
          "isHiddenAbility": false,
          "passive": "Ripen",
          "nature": "Careful",
          "gender": "female",
          "rarity": "Common",
          "capture": true,
          "level": 20,
          "items": [],
          "ivs": {
            "hp": 15,
            "atk": 10,
            "def": 16,
            "spatk": 2,
            "spdef": 30,
            "spe": 25
          }
        },
        "pokemon_right": {
          "id": 708,
          "name": "phantump",
          "ability": "Frisk",
          "isHiddenAbility": false,
          "passive": "Ripen",
          "nature": "Careful",
          "gender": "female",
          "rarity": "Common",
          "capture": true,
          "level": 20,
          "items": [],
          "ivs": {
            "hp": 15,
            "atk": 10,
            "def": 16,
            "spatk": 2,
            "spdef": 30,
            "spe": 25
          }
        }
      },
      {
        "id": 3,
        "reload": false,
        "type": "trainer",
        "double": false,
        "action": [
          "Pokeball",
          "Transfer|Basculin All > Azumarill",
          "Transfer|Diggersby All > Azumarill"
        ],
        "shop": "Reload > Poke Ball",
        "biome": "Graveyard",
        "trainer": {
          "id": 34,
          "name": "BREEDER John",
          "type": "breeder_m"
        }
      },
      {
        "id": 4,
        "reload": false,
        "type": "boss",
        "double": false,
        "action": [
          "Pokeball",
          "Transfer|Basculin All > Azumarill",
          "Transfer|Diggersby All > Azumarill"
        ],
        "shop": "Reload > Poke Ball",
        "biome": "Graveyard",
        "pokemon_left": {
          "id": 708,
          "name": "phantump",
          "ability": "Frisk",
          "isHiddenAbility": false,
          "passive": "Ripen",
          "nature": "Careful",
          "gender": "female",
          "rarity": "Common",
          "capture": true,
          "level": 20,
          "items": [
            {
              "id": 52,
              "name": "Dragon claw",
              "quantity": 1
            }
          ],
          "ivs": {
            "hp": 15,
            "atk": 10,
            "def": 16,
            "spatk": 2,
            "spdef": 30,
            "spe": 25
          }
        }
      }
    ],
    "starters": [
      {
        "id": 708,
        "name": "phantump",
        "ability": "Frisk",
        "isHiddenAbility": false,
        "passive": "Ripen",
        "nature": "Careful",
        "gender": "female",
        "rarity": "Common",
        "capture": true,
        "level": 20,
        "items": [
          {
            "id": 5,
            "name": "Sitrus Berry",
            "quantity": 2
          },
          {
            "id": 6,
            "name": "Enigma Berry",
            "quantity": 1
          }
        ],
        "ivs": {
          "hp": 15,
          "atk": 10,
          "def": 16,
          "spatk": 2,
          "spdef": 30,
          "spe": 25
        }
      },
      {
        "id": 708,
        "name": "phantump",
        "ability": "Frisk",
        "isHiddenAbility": false,
        "passive": "Ripen",
        "nature": "Careful",
        "gender": "female",
        "rarity": "Common",
        "capture": true,
        "level": 20,
        "items": [
          {
            "id": 5,
            "name": "Sitrus Berry",
            "quantity": 2
          },
          {
            "id": 6,
            "name": "Enigma Berry",
            "quantity": 1
          }
        ],
        "ivs": {
          "hp": 15,
          "atk": 10,
          "def": 16,
          "spatk": 2,
          "spdef": 30,
          "spe": 25
        }
      },
      {
        "id": 708,
        "name": "phantump",
        "ability": "Frisk",
        "isHiddenAbility": false,
        "passive": "Ripen",
        "nature": "Careful",
        "gender": "female",
        "rarity": "Common",
        "capture": true,
        "level": 20,
        "items": [
          {
            "id": 5,
            "name": "Sitrus Berry",
            "quantity": 2
          },
          {
            "id": 6,
            "name": "Enigma Berry",
            "quantity": 1
          }
        ],
        "ivs": {
          "hp": 15,
          "atk": 10,
          "def": 16,
          "spatk": 2,
          "spdef": 30,
          "spe": 25
        }
      }
    ]
}
```

















