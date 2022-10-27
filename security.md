# TRAX LRS 3.0 - Security

> This document details potential changes coming with TRAX LRS 3.0. As we are in the early stage of its design, all the aspects described in this document are **subject to change**. At this stage, there is no guarranty that all the described features will be implemented.


## Securing user accounts

### Passwords

TRAX LRS stores user accounts information, including passwords.
Here is a list of measures to improve passwords protection:

- Passwords are encrypted with a *BCrypt* or *Argon2* algorithm. The work factor of these algorithms can be customized.
- Strong password rules can be activated and configured (e.g. min length, required special chars, etc). 
- The entered password is always masked on forms (password definition, sign-in, etc).
- A password expiration date can be defined.
- Users get notified by email when their password is changed.
- Users get notified by email when someone use their credentials to log-in from an unknown location or device.
- A password strength indicator can be displayed on the password definition form.
- Passwords can be checked against previous breach corpuses, dictionary words, repetitive or sequential chars (https://haveibeenpwned.com/API/v3).
- An external identity provider may be used to store passwords and manage authentication (OAuth/OpenID integration).

### Authentication

- The authentication is disabled during T seconds after N failed attempts. T and N are configurable.
- A 2 factors authentication (2FA) based on email can be activated.
- A Captcha can be enabled on the login form.
- An option can be enabled to avoid users opening multiple sessions. 
- Developers can implement a custom middleware to secure authentication with their own rules, without breaking the code of the application.


## Securing APIs

TRAX LRS supports **Basic HTTP authentication** as it is the most common way for xAPI clients to authenticate.
This is also the less secured as the client identifier and password are just encoded in the headers, but not encrypted.
So this protocol should be used only for back-end to back-end communications in a secured infrastructure.

TRAX LRS also supports the **CMI5 communication protocol** which is based on the delivery of a temporary token.
This protocol is used when a content running in a browser needs to communicate directly with the LRS.
CORS policy can be configured and permissions are restricted for such clients.

Other authentication protocols may be supported by TRAX LRS 3.0, including OAuth as defined in the xAPI spec.
Options still have to be explored and confirmed. 


## OWASP

The Open Web Application Security Project (OWASP) is a nonprofit foundation that works to improve the security of software.
OWASP provides guidelines to secure applications, covering different languages and frameworks, including PHP and Laravel.

We will review these guidelines to improve security with TRAX LRS 3.0.
All the adopted security measures will be documented.

Further information: https://cheatsheetseries.owasp.org/cheatsheets/Laravel_Cheat_Sheet.html


## xAPI Sec

As stated in the xAPI spec:

> Security beyond authentication (including the interpretation of OAuth authorization scopes) is beyond the current scope of this document and left to the individual LRS provider as an implementation detail.

The xAPIsec initiative provides additional information regarding security and privacy concerns.
We will check the outputs of this working group to identify additional security improvements for TRAX LRS.
All the adopted security measures will be documented.

Further information: https://github.com/xapisec/xapisec


## PHP / Laravel

Security support for PHP 7.4 will end in novembre 2022 and security support for PHP 8.0 will end 1 year later.
Considering that TRAX LRS 3.0 could be released in stable version during de 2nd part of 2023,
support for PHP 7.4 and 8.0 could be dropped. 

At this time, a stable version of Laravel 10 will be available (feb. 2023) and PHP 8.1 will be required.
As TRAX LRS will be developed with Laravel 10, PHP 8.1 will also become a requirement.

PHP versions: https://www.php.net/supported-versions.php
Laravel versions: https://laravel.com/docs/master/releases#support-policy
