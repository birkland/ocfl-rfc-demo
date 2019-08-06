# RFC 0001: Pairtree Layout

* Category: Specification
* Obsoleted by: n/a
* Obsoletes: n/a

* Note:  This is not a real RFC.  It's an example of its kind *

## Overview

This RFC defines a pairtree layout URI `https://birkland.github.io/ocfl-rfc-demo/0001-pairtree-layout` for use in [ocfl_layout.json](https://ocfl.io/draft/spec/#root-structure).  When this URI is specified, the OCFL root layout MUST comply with the published [pairtree specification](https://tools.ietf.org/html/draft-kunze-pairtree-01), such that all
OCFL object roots are _properly encapsulated_ pairtree object directories.  

## Algorithm details

Given an OCFL object identifier, the OCFL object root path is calculated from that ID according to the [pairtree specification](https://tools.ietf.org/html/draft-kunze-pairtree-01) with the following clarifications:

* The OCFL object ID MUST undergo character encoding using the pairtree [cleaning](https://tools.ietf.org/html/draft-kunze-pairtree-00#section-3) algorithm.
* Terminal directories MUST be encapsulated in an identical manner throughout an OCFL root, using one of two techniques:
  * A terminal substring of the encoded ID with fixed length N, where N >= 3.
    * For encoded identifiers with length less than N but greater than 3, the whole encoded identifier shall be used.
    * For encoded identifiers less than three characters long, the terminal directory name shall be `obj`.
  * A constant value, such as "obj", where the length of this substring less than or equal to three characters long.
    * This value MUST be encoded according to pairtree [cleaning](https://tools.ietf.org/html/draft-kunze-pairtree-00#section-3)
* The layout MUST NOT use a [pairtree_prefix](https://tools.ietf.org/html/draft-kunze-pairtree-00#section-4).

## URI structure

An `ocfl_layout.json` file MUST be present in the OCFL object root.  The `url` value MUST contain a URL that begins with `https://birkland.github.io/ocfl-rfc-demo/0001-pairtree-layout`, and MAY contain an `encapsulation` query parameter that describes how all pairtree directories are encapsulated throughout the OCFL storage root.  The value
of the `description` field is arbitrary and irrelevant as far as this spec is concerned, a value of "Pairtree Layout" is sufficient, but copying the contents of this RFC
would also be appropriate.

Pairtree encapsulation MUST be consistent with the value of `encapsulation` in the URL as follows:

* If there is no `encapsulation` parameter, then all objects MUST be encapsulated in a directory named `obj`
* If `encapsulation` is an integer N, then all objects MUST be encapsulated in directories named with terminal substrings of length N
* If `encapsulation` is not an integer, then all objects MUST be encapsulated in directories named according to its value.

## Example

### Terminal Substring

This `ocfl_layout.json` file specifies a pairtree layout using terminal substring encapsulation with length 4:

```json
{
    "url": "https://birkland.github.io/ocfl-rfc-demo/0001-pairtree-layout?encapsulation=4",
    "description": "Pairtree Layout"
}
```

An OCFL object with id `ark:12345/6` would have its path computed as follows:

* First, the ID would be encoded via pairtree cleaning to `ark+12345=6`
* Next, the ID is divided into shorties:  `ar/k+/12/34/5=/6`
* Finally, this path is terminated with a length 4 substring (as per the `encapsulation` parameter): `ar/k+/12/34/5=/6/45=6`.  This directory is an OCFL root.  

On a filesystem, the resulting OCFL object corresponding to that ID would look like:

[storage_root]
                ├── 0=ocfl_1.0
                ├── ocfl_layout.json
                ├── ar
                |   └── k+
                |       └── 12
                |           └── 34
                |               └── 5=
                |                   └── 6
                |                       └──45=6
                |                           ├── 0=ocfl_object_1.0
                |                           └── ...
