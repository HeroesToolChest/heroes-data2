# Heroes Data
[![Release](https://img.shields.io/github/release/HeroesToolChest/heroes-data2.svg)](https://github.com/HeroesToolChest/heroes-data2/releases/latest)

This repository contains the Heroes of the Storm data files that are extracted from [Heroes Data Parser](https://github.com/HeroesToolChest/HeroesDataParser). The images are stored in the [Heroes Images](https://github.com/HeroesToolChest/heroes-images) repository.

The version folders contain `data` and `gamestring` folders. The gamestrings in the `data` files have been extracted into the `gamestrings` files.

In the `heroesdata` directory, a `.version.json` file contains the following:
```json5
{
  // The latest version directory
  "latest": "2.55.16.97039",
  // The latest version directory that contains the "full" JSON files
  // Will be an equal or lower version than "latest"
  "latest-full": "2.55.16.96881",
  // The UTC datetime of the latest version extracted
  "date": "2026-05-11T15:49:11.608384+00:00",
  // An array of the version directories (in order)
  "versions": [
    "2.55.16.96881",
    "2.55.16.97039"
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
  // If true, then this directory contains the data and gamestrings files
  "extracted": true,
  // If "json" is "patch", the prior version directory that the base patch JSON files depend on in order to be rebuilt into "full" JSON files
  "depends-on": "2.55.16.96881",
  // If "json" is "patch", the version directory containing the "full" JSON files used as the starting point for rebuilding the "full" JSON files
  // Will be an equal or lower version than "depends-on"
  "root-version": "2.55.16.96881",
  // The files that are in this directory
  "files": {
    ...
  }
}
```

## Json Data Parsing Library

## Releases

## Heroes Data Parser Extraction
