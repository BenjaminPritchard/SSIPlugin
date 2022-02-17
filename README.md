# WordPress SSI Plugin

This is a fork of https://github.com/Associazione-Blockchain-Italia/SSIPlugin 

I changed it to remove the ability to issue a credential, and to allow verification only.

The idea is to prevent a user from seeing a certain page unless they are logged into Wordpress. To accomplish
this I used "Password Protect Pages" which is configured to redirect to `pluginform.php` when no WP user is logged in.

`pluginform.php` is the page inside of this plugin, and calls 

Once the credential is verified, the user is logged in using the wp_signon() function and then is redirected to the restriced page.

I also changed some of the text to be in English instead of Italian.

To actually issue the credential, I just used Trinsic Studio to create a Credential Template containing the fields `Identifier` and `Role`.

The `role` doesn't matter, but set the `identifier` to the userID of the WP user to log in.

Just for now, I also hardcoded the plugin to redirect to the restricted page after the credential is verified.


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
