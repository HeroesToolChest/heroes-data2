# Heroes Data
[![Release](https://img.shields.io/github/release/HeroesToolChest/heroes-data2.svg)](https://github.com/HeroesToolChest/heroes-data2/releases/latest)

This repository contains the Heroes of the Storm data files that are extracted with [Heroes Data Parser](https://github.com/HeroesToolChest/HeroesDataParser). The image files are stored in the [Heroes Images](https://github.com/HeroesToolChest/heroes-images) repository.

For versions before `2.55.16.97039`, see the archived repository [heroes-data](https://github.com/HeroesToolChest/heroes-data).

## Versions
In the `data` and `gamestring` directories, the immediate json files, referred to as the base JSON files, will be in "full" or "patch" form. The "patch" form is a JSON patch created from the previous version of the file as noted in the `.hdp.json` file's `depends-on` property. The "full" form can be created from the [json patch apply](https://github.com/HeroesToolChest/HeroesDataParser#json-patch-apply) command.

There can be multiple "patch" versions in a row. Creating the "full" JSON file requires that the [json patch apply](https://github.com/HeroesToolChest/HeroesDataParser#json-patch-apply) command be run multiple times, starting from the `root-version`.

The `map` directories contain directories of the battlegrounds, each with JSON files in the "patch" form. The JSON patch is created from the "full" base JSON files of the same version (not a previous version). These patch files represent the differences between that battleground and the base JSON files.

For each base JSON file, there should be a battleground patch file, except for the `gamestrings_mapdata_*.json` files. If there is no patch file, there are no differences. If a battleground directory contains no patches then that battleground directory will not exist. Battleground directory names are derived from the `normalizedId` property in the `mapdata_*.json` data files.

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
    ...
  }
}
```

> [!NOTE]
> For how often a version will be in "full" form is up to discretion. If a version was skipped, meaing that `extracted` is `false`, there will *might* be a "full" version after, otherwise maybe around 10 "patch" form versions.

## Data Extraction
The following options was used for extraction of data and images:
```
-e all:i -l all --gs-replace-constant-vars --gs-replace-style-vars --gs-preserve-constant-vars --gs-preserve-style-vars --localized-text extract
```
All data types across all locales are extracted, along with all images. Color variable names are resolved and preserved in gamestrings, and gamestrings are separated into their own JSON files.

To add the gamestrings back to the data files, use the [localized-text import](https://github.com/HeroesToolChest/HeroesDataParser/#localized-text-import) command.

To modify the format of the gamestrings, use the [gamestring-text format](https://github.com/HeroesToolChest/HeroesDataParser/tree/develop-v5#gamestring-text-format) command.

## Releases
The releases will always contain the "full" base JSON files. Maps will always contain the JSON patch files.

## Json Data Parsing Library
[Heroes Element](https://github.com/HeroesToolChest/Heroes.Element) is a .NET library that can be used to parse the "full" JSON files.
