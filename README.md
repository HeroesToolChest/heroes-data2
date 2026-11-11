# Heroes Data
[![Release](https://img.shields.io/github/release/HeroesToolChest/heroes-data2.svg)](https://github.com/HeroesToolChest/heroes-data2/releases/latest)

This repository contains the Heroes of the Storm data files that are extracted with [Heroes Data Parser](https://github.com/HeroesToolChest/HeroesDataParser). The image files are stored in the [Heroes Images](https://github.com/HeroesToolChest/heroes-images) repository.

For versions before `2.55.16.97039`, see the archived repository [heroes-data](https://github.com/HeroesToolChest/heroes-data).

## Versions
In the `data` and `gamestrings` directories, the top-level JSON files, referred to as the base JSON files, will be in "full" or "patch" form. The "patch" form is a JSON patch created from the previous version of the file as noted in the `.hdp.json` file's `depends-on` property. The "full" form can be created with the [json-patch apply](https://github.com/HeroesToolChest/HeroesDataParser#json-patch-apply) command.

There can be multiple "patch" versions in a row. Creating the "full" JSON file requires that the [json-patch apply](https://github.com/HeroesToolChest/HeroesDataParser#json-patch-apply) command be run multiple times, starting from the `root-version`. A version with `extracted` set to `false` contains no data or gamestrings files and should be skipped when rebuilding.

The `data/maps` and `gamestrings/maps` directories contain a directory for each battleground, each with JSON files in the "patch" form. The JSON patch is created from the "full" base JSON files of the same version (not a previous version). These patch files represent the differences between that battleground and the base JSON files.

A battleground directory only has patch files for the base JSON files that differ on that battleground. A missing patch file means there are no differences, and a battleground with no differences at all has no directory. The `gamestrings_mapdata_*.json` files never have battleground patch files.

Each battleground directory is named with the battleground's `normalizedId` value from the `mapdata_*.json` data files.

### Meta JSON files
In the `heroesdata` directory, a `.version.json` file contains the following:
```json5
{
  // The latest version directory
  "latest": "2.55.17.97407_ptr",
  // The latest version directory that contains the "full" JSON files
  // Will be an equal or lower version than "latest"
  "latest-full": "2.55.16.97039",
  // The UTC datetime of the latest version extracted
  "date": "2026-07-02T01:29:58.4061443+00:00",
  // An array of the version directories (in order)
  "versions": [
    "2.55.16.97039",
    "2.55.17.97407_ptr"
  ]
}
```

In the version directories, a `.hdp.json` file contains the following:
```json5
{
  // The version of HDP used to extract the current files
  "hdp": "5.0.0",
  // The type of the base JSON files: "full" or "patch"
  "json": "patch",
  // If "json" is "patch", the prior version directory that the base JSON patch files depend on in order to be rebuilt into "full" JSON files
  "depends-on": "2.55.16.97039",
  // If "json" is "patch", the version directory containing the "full" JSON files used as the starting point for rebuilding the "full" JSON files
  // Will be an equal or lower version than "depends-on"
  "root-version": "2.55.16.97039",
  // If true, then this directory contains the data and gamestrings files
  "extracted": true,
  // The files that are in this directory
  "files": {
    // Base data files, keyed by data type
    "[data]": {
      "herodata": "herodata_97407.patch.json",
      // ...
      // Battleground patch files, keyed by battleground directory, then by data type
      "[maps]": {
        "alterac_pass": {
          "unitdata": "unitdata_97407.patch.json",
          "veterancydata": "veterancydata_97407.patch.json"
        },
        // ...
      }
    },
    // Base gamestrings files, keyed by locale (and "mapdata|<locale>" for the gamestrings_mapdata_*.json files)
    "[gamestrings]": {
      "enus": "gamestrings_97407_enus.patch.json",
      "mapdata|enus": "gamestrings_mapdata_97407_enus.patch.json",
      // ...
      // Battleground patch files, keyed by battleground directory, then by locale
      "[maps]": {
        "alterac_pass": {
          "enus": "gamestrings_97407_enus.patch.json",
          // ...
        },
        // ...
      }
    }
  }
}
```

> [!NOTE]
> How often a version is stored in "full" form is at the maintainer's discretion. If a version was skipped (`extracted` is `false`), the next version *might* be in "full" form; otherwise, expect a "full" version roughly every 10 "patch" versions.

## Data Extraction
The following options were used for extraction of data and images:
```
-e all:i -l all --gs-replace-constant-vars --gs-replace-style-vars --gs-preserve-constant-vars --gs-preserve-style-vars --localized-text extract
```
All data types across all locales are extracted, along with all images. Color variable names are resolved and preserved in gamestrings, and gamestrings are separated into their own JSON files.

To add the gamestrings back to the data files, use the [localized-text import](https://github.com/HeroesToolChest/HeroesDataParser#localized-text-import) command.

To modify the format of the gamestrings, use the [gamestring-text format](https://github.com/HeroesToolChest/HeroesDataParser#gamestring-text-format) command.

## JSON Schemas
The `schema` directory contains the JSON schema files for the "full" data and gamestrings JSON files, grouped into directories named after an HDP version (e.g. `schema/5.0.0`).

A new schema directory is only added when an HDP version changes the schema, so there is not a schema directory for every HDP version. To find the schema for a version directory, take the `hdp` property from its `.hdp.json` file and use the schema directory with the highest version that is equal to or lower than it.

For example, if the schema directories are `5.0.0` and `5.2.0`:
- Files extracted with HDP `5.0.4` use the `5.0.0` schemas
- Files extracted with HDP `5.2.0` or `5.2.3` use the `5.2.0` schemas

## Releases
The releases will always contain the "full" base JSON files. Maps will always contain the JSON patch files.

Each release includes the following files, in both `.zip` and `.tar.gz` formats:
- `heroes-data-<version>` - the `data` and `gamestrings` directories, including the `maps` directories
- `heroes-data-no-maps-<version>` - the `data` and `gamestrings` directories, without the `maps` directories

## JSON Data Parsing Library
[Heroes Element](https://github.com/HeroesToolChest/Heroes.Element) is a .NET library that can be used to parse the "full" JSON files.

## License
[MIT license](https://github.com/HeroesToolChest/heroes-data2/blob/main/LICENSE)
