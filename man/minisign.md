# NAME

**minisign** - A dead simple tool to sign files and verify signatures.

## SYNOPSIS

**minisign** -G \[-p pubkey\] \[-s seckey\]

**minisign** -S \[-H\] \[-x sigfile\] \[-s seckey\] \[-c untrusted\_comment\] \[-t
trusted\_comment\] -m \<file\>

**minisign** -V \[-x sigfile\] \[-p
pubkeyfile \| -P pubkey\] \[-o\] \[-q\] -m file

## DESCRIPTION

**Minisign** is a dead simple tool to sign files and verify signatures.

It is portable, lightweight, and uses the highly secure Ed25519 *http://ed25519.cr.yp.to/* public-key signature system.

## OPTIONS

These options control the actions of **minisign**.

**-G**

:   Generate a new key pair

**-S**

:   Sign a file

**-V**

:   Verify that a signature is valid for a given file

**-m \<file\>**

:   File to sign/verify

**-o**

:   Combined with -V, output the file content after verification

**-H**

:   Combined with -S, pre-hash in order to sign large files

**-p \<pubkeyfile\>**

:   Public key file (default: ./minisign.pub)

**-P \<pubkey\>**

:   Public key, as a base64 string

**-s \<seckey\>**

:   Secret key file (default: \~/.minisign/minisign.key)

**-x \<sigfile\>**

:   Signature file (default: \<file\>.minisig)

**-c \<comment\>**

:   Add a one-line untrusted comment

**-t \<comment\>**

:   Add a one-line trusted comment

**-q**

:   Quiet mode, suppress output

**-Q**

:   Pretty quiet mode, only print the trusted comment

**-f**

:   Force. Combined with -G, overwrite a previous key pair

**-v**

:   Display version number

## EXAMPLES

### Creating a key pair

\$ **minisign** -G

The public key is printed and put into the **minisign.pub** file. The secret key is encrypted and saved as a file named
**\~/.minisign/minisign.key**.

### Signing a file

\$ **minisign** -Sm myfile.txt

Or to include a comment in the signature, that will be verified and
displayed when verifying the file:

\$ **minisign** -Sm myfile.txt -t .This comment will be signed as
well.

The secret key is loaded from **\${MINISIGN\_CONFIG\_DIR}/minisign.key**, **\~/.minisign/minisign.key**, or its path can be explicitly set with the **-s \<path\>** command-line switch.

### Verifying a file

\$ **minisign** -Vm myfile.txt -p \<pubkey\>

or

\$ **minisign** -Vm myfile.txt -p signature.pub

This requires the signature **myfile.txt.minisig** to be present in
the same directory.

The public key can either reside in a file (**./minisign.pub** by default) or be directly specified on the command line.

## Notes

**Trusted comments**

Signature files include an untrusted comment line that can be freely modified, even after signature creation.

They also include a second comment line, that cannot be modified without the secret key.

Trusted comments can be used to add instructions or application-specific metadata (intended file name, timestamps, resource identifiers, version numbers to prevent downgrade attacks).

**Compatibility with OpenBSD signify**

Signatures written by **minisign** can be verified using OpenBSD.s
**signify** tool: public key files and signature files are compatible.

However, **minisign** uses a slightly different format to store secret keys.

**Minisign** signatures include trusted comments in addition to
untrusted comments. Trusted comments are signed, thus verified, before being displayed.

This adds two lines to the signature files, that signify silently
ignores.

**Pre-hashing**

By default, signing and verification require as much memory as the size of the file.

Huge files can be signed and verified with very low memory requirements, by pre-hashing the content.

The -H command-line switch, in combination with -S, generates a
pre-hashed signature (HashEdDSA):

\$ **minisign** -SHm myfile.txt

Verification of such a signature doesn't require any specific switch: the appropriate algorithm will automatically be detected.

Signatures generated that way are not compatible with OpenBSD's **signify** tool and are not compatible with **Minisign** versions prior to 0.6.

# AUTHOR

Frank Denis (github \[at\] pureftpd \[dot\] org).

**minisign-py** by [HacKan](mailto:hackan@gmail.com). Check the [minisign-py repository](https://github.com/hackancuba/minisign-py/) for more information.

# COPYING

    This program is free software: you can redistribute it and/or modify it
    under the terms of the GNU General Public License as published by the
    Free Software Foundation, either version 3 of the License, or (at your
    option) any later version.

    This program is distributed in the hope that it will be useful, but
    WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General
    Public License for more details.

    You should have received a copy of the GNU General Public License along
    with this program. If not, see \<http://www.gnu.org/licenses/\>.
