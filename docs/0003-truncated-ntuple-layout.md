# RFC 0003: Truncated N-tuple Layout

* Category: Specification
* Obsoleted by: n/a
* Obsoletes: n/a

*Note:  This is not a real RFC.  It's an example of its kind*

## Overview

This RFC defines a truncated n-tuple layout URI `https://birkland.github.io/ocfl-rfc-demo/0003-truncated-ntuple-layout` for use in [ocfl_layout.json](https://ocfl.io/draft/spec/#root-structure).  A truncated n-tuple tree provides a depth-limited filesystem hierarchy based on the first few digits of an ocfl object ID.  Optionally, the ID can be encoded (e.g using a hash) for the purpose of generating balanced trees and avoiding illegal characters on a given filesystem.  This layout is a good choice for fiesystems that degrade in performance with large directory listings (NTFS, ext4, etc).

## Algorithm details

The truncated n-tuple layout creates a directory hiearachy based upon a number of fixed-length substrings generated from an ocfl object ID.  Required parameters are `N`
(the number of characters in a tuple), and `D` (depth; the number of tuples used for naming directories).  

Starting with an empty path relative to the ocfl object root:

1. Optionally, encode the ID according to one of the allowable [encodings](#Encodings) allowed in this RFC
2. Perform the following step `D` times:
   * If there are at least N+1 characters remaining in the ID, remove the first N characters from the beginning of the encoded ID, and append this to the path as a path segment. Otherwise, append the value `_` to the path, and go to step 3.
3. Append the full encoded object ID to the path as its final path segment.  This shall be the OCFL object root for the object with the given ID.

### Encodings

Allowed encodings are as follows:

| name     | description           |
| -------- | --------------------- |
| none     | No encoding           |
| sha1     | sha1 hash algorithm.  Encoded values are all lowercase   |
| sha256   | sha256 hash algorithm Encoded values are all lowercase   |
| sha512   | sha512 hash algorithm Encoded values are all lowercase   |
| url      | URL encoding          |
| pairtree | Pairtree cleaning. Escaped hex sequences are all lowercase   |

## URI structure

An `ocfl_layout.json` file MUST be present in the OCFL object root.  The `url` value MUST contain a URL that begins with `https://birkland.github.io/ocfl-rfc-demo/0003-truncated-ntuple-layout`, and MUST contain query parameters `n` and `depth` representing tuple length `N` and depth `D`, respectively.  A parameter `encoding` MAY be provided.
If present, it MUST be one of the values listed in the [encodings](#Encodings) table.  If `encoding` is not present, then OCFL object IDs are used in the path generation without
any encoding.

## Examples

### Short identifiers

The following table uses a tuple size `N` of 3, and depth `D` of 2.  It illustrates the paths that result from short identifiers.  The last table entry has a length greater than `N * D`, and is therefore not short.

| Encoded ID | Resulting OCFL object root path |
| ---------- | --------------------------------|
| a          | `_/a`                           |
| ab         | `_/ab`                          |
| abc        | `_/abc`                         |
| abca       | `abc/_/abca`                    |
| abcab      | `abc/_/abcab`                   |
| abcabc     | `abc/_/abcabc`                  |
| abcabca    | `abc/abc/abcabca`               |

### URL specification

This `ocfl_layout.json` file specifies a truncated n-tuple layout with depth of 2, tuple length of 2, and sha1 encoding

```json
{
    "url": "https://birkland.github.io/ocfl-rfc-demo/0003-truncated-ntuple-layout?n=2&depth=2&encoding=sha1",
    "description": "Truncated n-tuple Layout"
}
```

An OCFL object with id `ark:12345/6` would have its path computed as follows:

* First, the ID would be encoded via sha1 to `da39a3ee5e6b4b0d3255bfef95601890afd80709`
* Next, intermediate directories are created based on the first two 2-tuples :  `da/39`
* Finally, this path is terminated with the encoded ID: `da/39/da39a3ee5e6b4b0d3255bfef95601890afd80709`

On a filesystem, the resulting OCFL object corresponding to that ID would look like:

```plaintext
[storage_root]
                ├── 0=ocfl_1.0
                ├── ocfl_layout.json
                ├── da
                |   └── 39
                |       └── da39a3ee5e6b4b0d3255bfef95601890afd80709
                |           ├── 0=ocfl_object_1.0
                |           └── ...
```
