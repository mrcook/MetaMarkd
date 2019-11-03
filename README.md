# MetaMarkd: An eBook Metadata Scheme

First Draft, 2019-11

* Latest version: [https://github.com/mrcook/MetaMarkd/blob/master/README.md](https://github.com/mrcook/MetaMarkd/blob/master/README.md)
* Authors: Michael R. Cook


Copyright © 2019 Michael R. Cook

This work is licensed under a [Creative Commons Attribution License](https://creativecommons.org/licenses/by/4.0/). This copyright applies to the MetaMarkd Metadata Specification. Regarding underlying technology, MetaMarkd uses the YAML specification, an open Web standard that can be freely used by anyone.

----

## Introduction

This specification describes the MetaMarkd vocabulary, defined as a dictionary of named properties using the human readable [YAML](https://yaml.org/spec/1.2/spec.html) specification.

The goal is to provide anyone producing Markdown (`.md`) based eBooks a simple way of specifying descriptive metadata about the work, which can then be parsed by automated tools during the conversion process.

The emphasis here is on _simple_. MetaMarkd is not intended to replace more exhaustive metadata vocabularies such as ONIX and MARC.

The metadata can then be placed in the `.md` document header, or in a separate `.yaml` file.


## Example

Further examples can be found in the [examples](/examples) directory.

```
identifiers:
  - type: UUID
    id: dadc889c-33d7-4cc9-bab8-0c765f4de041
title:
  - MetaMarkd Vocabulary
authors:
  - Michael R. Cook
published:
  - date: 2019-08
languages:
  - language: en
subjects:
  - name: Computer Programming
copyright:
  - year: 2019
    holders:
      - Michael R. Cook
summary: A short summary of the work.
description: A longer, more descriptive explanation of the work.
license: Creative Commons Attribution 4.0 International License (CC BY 4.0)
```


## Vocabulary


### `identifiers`

A list of numbers or strings that can be used to uniquely identify a work, such as an ISBN or UUID.

    At least one entry is required.

Each entry must include the following attributes:

* `type`: The type of identifier; ISBN, UUID, LCCN, etc.
* `id`: an unique string identifying the work.

Examples of common identifier types:

* `Coden`: ASTM standard E250 bibliographic code
* `DOI`: Digital Object Identifier
* `ISBN`: International Standard Book Number
* `LCCN`: Library of Congress Control Number
* `OCLC`: Online Computer Library Center number
* `UUID`: Universally Unique Identifier

It is recommended to include an UUID ([Universally Unique Identifier](https://en.wikipedia.org/wiki/Universally_unique_identifier)) as this is independent of tools or publishing platform.


### `title`

The main and subtitle(s) of the work, as an array of strings.

    At least one entry is required.

The first entry in the list is always the main title, with subsequent entries used for the subtitle(s).


### `authors`

A list of authors of the work, as an array of strings.

    At least one entry is required.


### `contributors`

A list of contributors to the work that do not fit in the `authors` section.

Each entry must include the following attributes:

* `name`: The full name of the contributor.
* `role`: The role they had in producing the work.

Values used for the `role` attribute must be one of those found in the [MARC Relator](https://www.loc.gov/marc/relators/relaterm.html) codes.

Example contributor codes:

* `edt`: Editor
* `ill`: Illustrator
* `trl`: Translator
* `aui`: Author of introduction, etc.


### `published`

A list of publication dates in reverse chronological order, along with the current edition and major changes made since the previous publication.

    At least one entry is required

    The `date` attribute is required.

The following attributes are available for this property:

* `date`: Date of publication as an ISO 8601 value.
* `edition`: A number or string defining the bibliographical edition.
* `changes`: An array of strings listing the major changes since the previous release.

Typical formats for the `date` would be: `YYYY`, `YYYY-MM`, and `YYYY-MM-DD`.


### `languages`

A list of languages used in the work.

    This property is optional, but recommended.

    The `language` attribute is required.

The following attributes are available for this property:

* `language`: As an ISO 3166-1 alpha-2 value (2 digits).
* `percent`: A decimal number from `1`-`100` indicating an approximate percentage that the language is used within the work.

If only one language is given, the `percent` value can be omitted. The `language` with the higher percentage is considered the _primary_ language.


### `subjects`

A list of subjects (genres) for the work.

    This property is optional, but recommended.

    The `name` attribute is required.

The following attributes are available for this property:

* `name`: The subject or genre name.
* `scheme`: The name of any formal scheme used.
* `code`: The scheme code.

If the `code` attribute is used, it is _strongly recommended_ to also provide the `scheme`.

There are a number of different specifications available such as ONIX, MARC, and Thema. The following would be an example using the Thema 1.3 specification:

```
- name: Short Stories
  code: FYB
  scheme: Thema v1.3
```

This specification gives no opinion on the values to be used for any of these attributes.


### `copyright`

The copyright year and names of the copyright holders.

    This property is optional, but strongly recommended.

Each entry must include the following attributes:

* `year`: A 4-digit year value, e.g. `2001`.
* `holders`: An array of strings listing the copyright holder names.


### `publisher`

The name of the publisher for this work.


### `illustrated`

A `true`/`false` value indicating if this work contains illustrations or other supporting images.


### `word_count`

A decimal value indicating the number of words in the work.

This specification gives no opinion on what constitutes a word.


### `series`

A list of series that the work belongs to.

Each entry must include the following attributes:

* `name`: The name of the series.
* `volume`: The volume number within the series as a decimal value.


### `movies`

A list of movies that are based on this work.

Each entry must include the following attributes:

* `title`: The movie title.
* `year`: A 4-digit year value, e.g. `2001`.


### `summary`

A short summary describing the work.

    This property is optional, but recommended.

No limit on the length is imposed by this specification, however, we recommend this not be more than more one sentence.


### `description`

A longer text describing the work.

    This property is optional, but recommended.

No limit on the length is imposed by this specification, however, we recommend this not be more than two or three short paragraphs.


### `keywords`

A short list of keywords describing the content of the work, as an array of strings.

No restriction on the number of entries or their values is imposed by this specification, however, we recommend only a handful of single word labels, from most to least importance.

An example for a novel about a Victorian cyberpunk romance between two androids might be:

```
keywords: [ cyberpunk, romance, android, victorian ]
```


### `excerpt`

A short excerpt from the work that can be used on a distribution platform.

No size limit is imposed by this specification, however, we recommend this not be more than a few hundred words.


### `license`

The license of the work if one needs to be specified.

Example licenses:

* Creative Commons license (e.g. `CC BY 4.0`)
* Public Domain
* Copyleft
