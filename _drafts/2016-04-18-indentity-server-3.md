---
layout: page
title: Walkthrough - Identity Server 3
---

This is following a walk-through on setting up a standalone implementation of Identity Server 3 (by Scott Brady)

## OpenID Connect Flows
The authorisation flows and how authentication is handled

### Authorization Code Flow
	- returns auth code that is exchanged for id & access token
	- requires client id and secret
	- does not expose tokens to User Agent
	- allows long lived access through refresh tokens

### Implicit Flow
	- request tokens without explicit client authentication
	- uses redirect uri to verify client identity
	- not lived tokens not allowed
	- simplest to implement (only needs to redirect user to the OpenID connect provider)
	- tokens are obtained from Authorisation endpoint

### Hybrid Flow
	- combination of both
	- allows user to immediately make use of the identity token
	- optionally allow user to retrieve authorisation code for long lived access

## Installation



## References
[Identity Server 3 Standalone Implementation Part 1](https://www.scottbrady91.com/Identity-Server/Identity-Server-3-Standalone-Implementation-Part-1)
[OpenID Connect Flows](https://www.scottbrady91.com/OpenID-Connect/OpenID-Connect-Flows)
[OpenID Connect Core 1.0](http://openid.net/specs/openid-connect-core-1_0.html#Authentication)