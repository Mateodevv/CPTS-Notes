#### Shadow Format

|`cry0l1t3`|`:`|`$6$wBRzy$...SNIP...x9cDWUxW1`|`:`|`18937`|`:`|`0`|`:`|`99999`|`:`|`7`|`:`|`:`|`:`|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Username||Encrypted password||Last PW change||Min. PW age||Max. PW age||Warning period|Inactivity period|Expiration date|Unused|

If the password field contains a character, such as `!` or `*`, the user cannot log in with a Unix password. However, other authentication methods for logging in, such as Kerberos or key-based authentication, can still be used. The same case applies if the `encrypted password` field is empty. This means that no password is required for the login. However, it can lead to specific programs denying access to functions. The `encrypted password` also has a particular format by which we can also find out some information:

- `$<type>$<salt>$<hashed>`

As we can see here, the encrypted passwords are divided into three parts. The types of encryption allow us to distinguish between the following:

#### Algorithm Types

- `$1$` – MD5
- `$2a$` – Blowfish
- `$2y$` – Eksblowfish
- `$5$` – SHA-256
- `$6$` – SHA-512