# Random Notes

This is a fork of https://github.com/Associazione-Blockchain-Italia/SSIPlugin , slightly modified to fit my specific use case.

I worked with this plugin as a proof of concept. I wanted to see if it could be used to restrict access to a certain page on my website, using my preferred workflow.

That workflow is as follows:

- Users cannot access my videos page without verifying using a Trinsic wallet.
- Users can only get a credential from me, that I issue manually in Trinsic Studio and then email to them.
- Protecting the actual video page is done using a different plugin, `Password Protect Pages`. 
- When the user hits the protected (video) page, `Password Protect Pages` is setup to redirect to `pluginform.php` in here if they are not logged in already.
- The point of this plugin is to request a credential from the user, and if it is valid, then log the user into Wordpress using the value the attribute `Identifier` in the credential presentation.
- It is not possible for users to sign up by themselves.

Misc Notes:

I changed this plugin to remove the ability to issue a credential; now it allow verification only.

The idea is to prevent a user from seeing a certain page unless they are logged into Wordpress. To accomplish
this I used "Password Protect Pages" which is configured to redirect to `pluginform.php` when no WP user is logged in.

`pluginform.php` is the page inside of this plugin, and calls https://api.trinsic.id/credentials/v1/verifications/policy 
to create a connectionless verification using the attributes: `role` and `Identifier`. 

Once that is done, https://api.trinsic.id/credentials/v1/verifications/ is polled looking for the verification state to become "Accepted". When that happens, a  user is logged in using the `wp_signon()` function and then is redirected to a (hard-coded) restricted page. (NOTE: this is a hack, and should at least be customizable.)

Note that a WP user should be setup in advance, and must have their password set to match what is in `authenticateUser()` in this plugin.

Make sure to have created a Credential Template in Trinsic Studio containing the fields `Identifier` and `Role`.  Then issue the credential via email to someone, filling out the `role` and `identifier` attributes.

(The `role` doesn't matter, but set the `identifier` to the userID (not username) of the WP user to log in.)

I also changed some of the text to be in English instead of Italian.


The original text follows below:


## Introduction

This plugin enables registration and authentication of users on WordPress sites using the Self Sovereign Identity model.

The advantages of using this plugin are:

- It can significantly increase security for all WordPress sites. WordPress powers 35% of the Internet in 2020. With the current system, all the WordPress sites have a repository of usernames and password, which are subject to serious risk of hacking and generate costs. The engine is frequently updated for security reasons, but the updates are often not rolled out with the same speed, generating risks.

- It can enable a decentralized online login system based on privacy-by design: users will login minimizing the display of personal data and without relying on centralized parties for storage and management.

- It can have high scalability and impact: it reaches a very huge audience, contributing to raise awareness and increase knowledge about SSI among users, and also helping the emergence of good practices for online services.

## Prerequisites

WordPress 5 and PHP 7

## How it works

The plugin has been implemented using the Trinsic REST API (https://trinsic.id) and requires registration with the API in order to provide the Access token of the Organization whith issues the credentials and the DefinitionID of the credential.

For our test implementation we have used Credential ID Xw9jQyfGdYzCbiRvXpWYrt:3:CL:153208:default with Schema ID Xw9jQyfGdYzCbiRvXpWYrt:2:Autenticazione Standard:1.0 both of which can be found in the Sovrein Staging blockchain (https://indyscan.io/tx/SOVRIN_STAGINGNET/domain/153212).

The registration use case emits a credential with the currently selected default role in the General Settings page and sends it to the user.

The login use case allows the user to login with the previously created credential.

The credentials can be subsequently revoked in the settings page (and the corresponding WordPress user is consequently deleted).

## Improvements

Some additional functionalities and features that may be implemented at a later stage of development are listed below:

- WordPRess internationalization support

- The designed UI is minimal and sufficiently intuitive, but the quality of the graphics can certainly be improved to provide a better user experience.

- Implementation of the WordPress role system in the claims

- Registration process involving a request by the user and subsequent approval by the administrator, with verification and assignment of the correct role.

- Interoperability with other SSI wallet systems (e.g. Hyperledger Aries and Verity by Evernym)
