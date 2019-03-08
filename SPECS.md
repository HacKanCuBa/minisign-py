# Minisign Specifications

Copied from [minisign](https://jedisct1.github.io/minisign/)'s website.  
Currently following version *0.8*.

## Signature format

```
untrusted comment: <arbitrary text>
base64(<signature_algorithm> || <key_id> || <signature>)
trusted_comment: <arbitrary text>
base64(<global_signature>)
```

* signature\_algorithm: Ed
* key\_id: 8 random bytes, matching the public key
* signature (PureEdDSA): ed25519(`<file data>`)
* signature (HashedEdDSA): ed25519(Blake2b-512(`<file data>)`)
* global\_signature: ed25519(`<signature>` || `<trusted_comment>`)

## Public key format

```
untrusted comment: <arbitrary text>
base64(<signature_algorithm> || <key_id> || <public_key>)
```

* signature\_algorithm: Ed
* key\_id: 8 random bytes
* public\_key: Ed25519 public key

## Secret key format

```
untrusted comment: <arbitrary text>
base64(<signature_algorithm> || <kdf_algorithm> || <cksum_algorithm> ||
       <kdf_salt> || <kdf_opslimit> || <kdf_memlimit> || <keynum_sk>)
```

* signature\_algorithm: Ed (or ED for prehashed)
* kdf\_algorithm: Sc
* cksum\_algorithm: B2
* kdf\_salt: 32 random bytes
* kdf\_opslimit: `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_SENSITIVE`
* kdf\_memlimit: `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_SENSITIVE`
* keynum\_sk: `<kdf_output>` ^ (`<key_id>` || `<secret_key>` || `<public_key>` || `<checksum>`), 104 bytes
* key\_id: 8 random bytes
* secret\_key: Ed25519 secret key
* public\_key: Ed25519 public key
* checksum: Blake2b-256(`<signature_algorithm>` || `<key_id>` || `<secret_key>` || `<public_key>`), 32 bytes

