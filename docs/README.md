# ocfl-rfc-demo

Demonstration/test of an OCFL RFC repository.

This is intended to demonstrate how a github repo can be used to collaborate, review, and publish "RFCs" related to OCFL.  Such RFCs are intended to be informational and citable, but outside the scope of the normal specification process.  Anybody may propose an RFC with a PR, and the community may review and discuss it before it is merged in.  They are intended to be mostly static once published, where substantial revisions of content beyond simple fixes warrant publishing a new RFC.  

Categories of RFCs may include:

* Specification.  A published specification pertinent to OCFL, but not in scope of the OCFL spec itself.  (That being said, incorporating an RFC into a future OCFL spec revision might not be out of the question when warranted!)
* Implementation pattern.  Describes a specific convention or technique for implementing OCFL
* Experimental.  Describes an approach being explored

This repository contains a couple samples, as well as an example pull request.

## RFCs

### Specification

* [RFC 0001: Pairtree Layout](0001-pairtree-layout.md).  Describes a layout URI and set of rules for creating pairtree directory layouts.
* [RFC 0003: Truncated N-tuple Layout](0003-truncated-ntuple-layout.md).  Describes a URI and set of rules for creating truncated n-tuple directory layouts.

### Implementation pattern

* [RFC 0002: Exclusive Deposit locking](0002-exclusive-deposit-locking.md).  Describes a technique for exclusive object-level locking within the `/deposit` directory for uncoordinated clients using a local filesystem.
