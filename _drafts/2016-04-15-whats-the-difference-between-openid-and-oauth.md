---
layout: post
title: OpenID or OAuth?
---

OpenID is about authentication (ie. proving who you are), OAuth is about authorisation (ie. to grant access to functionality/data/etc.. without having to deal with the original authentication).

OAuth could be used in external partner sites to allow access to protected data without them having to re-authenticate a user.

There are three ways to compare OAuth and OpenID:

	1. Their purposes
	OpenID was created for federated authentication, that is, letting a third-party authenticate your users for you, by using accounts they already have.
	OAuth was created to remove the need for users to share their passwords with third-party applications.

	The problem is with this separation of OpenID for authentication and OAuth for authorization is that both protocols can accomplish many of the same things.

	2. Their features
	Both protocols provide a way for a site to redirect a user somewhere else and come back with a verifiable assertion.

	OpenID - the most important feature of OpenID is its discovery process.
	OAuth - the most important feature of OAuth is the access token which provides a long lasting method of making additional requests.

	3. Their technical implementations
	The two protocols share a common architecture in using redirection to obtain user authorization. In OAuth the user authorizes access to their protected resources and in OpenID, to their identity. But that's all they share.

	Each protocol has a different way of calculating a signature used to verify the authenticity of the request or response, and each has different registration requirements.

ref: [SO - What's the difference between OpenID and OAuth?](http://stackoverflow.com/questions/1087031/whats-the-difference-between-openid-and-oauth)

	First the scenario for OpenID:
		User wants to access his account on example.com
		example.com (the “Relying Party” in OpenID lingo) asks the user for his OpenID
		User enters his OpenID
		example.com redirects the user to his OpenID provider
		User authenticates himself to the OpenID provider
		OpenID provider redirects the user back to example.com
		example.com allows the user to access his account

	And now the scenario for OAuth:
		User is on example.com and wants to import his contacts from mycontacts.com
		example.com (the “Consumer” in OAuth lingo) redirects the user to mycontacts.com (the “Service Provider”)
		User authenticates himself to mycontacts.com (which can happen by using OpenID)
		mycontacts.com asks the user whether he wants to authorize example.com to access his contacts
		User makes his choice
		mycontacts.com redirects the user back to example.com
		example.com retrieves the contacts from mycontacts.com
		example.com informs the user that the import was successful

ref: [OpenID versus OAuth from the user’s perspective](http://cakebaker.42dh.com/2008/04/01/openid-versus-oauth-from-the-users-perspective)

