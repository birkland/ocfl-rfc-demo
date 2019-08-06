# RFC 0002: Exclusive deposit locks

* Category: Implementation Pattern
* Obsoleted by: n/a
* Obsoletes: n/a

* Note:  This is not a real RFC.  It's an example of its kind *

## Overview

This RFC describes a conventions and algorithm for staging content in the `/deposit` directory that has the following properties:

* A client who stages content intended for the creation of a new ocfl object (or version) obtains an exclusive object-level lock.
  * In other words, no two un-coordinated clients can mistakenly both stage content intended for the same (conflicting) update to an OCFL object.  One will always fail to obtain a lock.
* As locks are scoped to individual objects, clients updating different objects do not interfere with one another.
* This algorithm only works on a locally-attached filesystem with atomic rename operations.  It is NOT appropriate for cloud object stores

## Algorithm details

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Dolor sed viverra ipsum nunc aliquet bibendum enim. In massa tempor nec feugiat. Nunc aliquet bibendum enim facilisis gravida. Nisl nunc mi ipsum faucibus vitae aliquet nec ullamcorper. Amet luctus venenatis lectus magna fringilla. Volutpat maecenas volutpat blandit aliquam etiam erat velit scelerisque in. Egestas egestas fringilla phasellus faucibus scelerisque eleifend. Sagittis orci a scelerisque purus semper eget duis. Nulla pharetra diam sit amet nisl suscipit. Sed adipiscing diam donec adipiscing tristique risus nec feugiat in. Fusce ut placerat orci nulla. Pharetra vel turpis nunc eget lorem dolor. Tristique senectus et netus et malesuada.

Etiam tempor orci eu lobortis elementum nibh tellus molestie. Neque egestas congue quisque egestas. Egestas integer eget aliquet nibh praesent tristique. Vulputate mi sit amet mauris. Sodales neque sodales ut etiam sit. Dignissim suspendisse in est ante in. Volutpat commodo sed egestas egestas. Felis donec et odio pellentesque diam. Pharetra vel turpis nunc eget lorem dolor sed viverra. Porta nibh venenatis cras sed felis eget. Aliquam ultrices sagittis orci a. Dignissim diam quis enim lobortis. Aliquet porttitor lacus luctus accumsan. Dignissim convallis aenean et tortor at risus viverra adipiscing at.
