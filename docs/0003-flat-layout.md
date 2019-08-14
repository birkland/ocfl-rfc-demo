# RFC 0003: Flat Layout

* Category: Specification
* Obsoleted by: n/a
* Obsoletes: n/a

*Note:  This is not a real RFC.  It's an example of its kind*

## Overview

This RFC defines a layout URI `https://birkland.github.io/ocfl-rfc-demo/0003-flat-layout` for use in [ocfl_layout.json](https://ocfl.io/draft/spec/#root-structure).  When this URI is specified, the OCFL root is structured as a flat hierarchy, where each directory directly underneath the OCFL storage root is an OCFL object root.

Directory names are a function of the object ID, and may be encoded via one of the methods prescribed in this RFC (e.g character encoding for the sake of filesystem compatibility).  The encoding algorithm is specified by a URL parameter

## URI structure

An `ocfl_layout.json` file MUST be present in the OCFL object root.  The `url` value MUST contain a URL that begins with `https://birkland.github.io/ocfl-rfc-demo/0003-flat-layout`, and MAY contain an `encoding` query parameter that describes how the ocfl object ID is encoded into a directory name.  The value
of the `description` field is arbitrary and irrelevant as far as this spec is concerned, a value of "Flat Layout" is sufficient, but copying the contents of this RFC
would also be appropriate.

The `encoding` parameter MUST be one of the following values, or absent in the case of "no encoding":

* `url`.  The directory names shall be determined by the urlencoded value of the ocfl object ID.
* `pairtree`.  The directory names shall be the value determined by [pairtree cleaning](https://tools.ietf.org/html/draft-kunze-pairtree-01#section-3)
* {`sha1`, `sha256`, `sha512`}.  If `encoding` is a digest algorithm, the directory names shall be determined by the digest value of the ocfl object ID.

## Example

### sha encoded

This `ocfl_layout.json` file specifies a flat layout using sha1 encoding

```json
{
    "url": "https://birkland.github.io/ocfl-rfc-demo/0003-flat-layout?encoding=sha1",
    "description": "Flat Layout"
}

An OCFL object with id `ark:12345/6` would have its path computed as follows:

* Calculate a sha1 of the ID:  `75b3b936989b76068528e71e4615808a2221dc3f`

On the filesystem, an ocfl object root containing an object with that ID would look like:
```plaintext
[storage_root]
                ├── 0=ocfl_1.0
                ├── ocfl_layout.json
                └── 75b3b936989b76068528e71e4615808a2221dc3f
                    ├── 0=ocfl_object_1.0
                    └── ...
