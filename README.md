sa-hashicorp-vault-ui
=====================

[![Build Status](https://travis-ci.org/softasap/sa-hashicorp-vault-ui.svg?branch=master)](https://travis-ci.org/softasap/sa-hashicorp-vault-ui)

If for some reason you are feeling confused by using Vault console, that is basic role for 3rd party vault ui install.
By default ui is binded to localhost:8201 (next to vault port). It is highly not recommended to expose it outside of the vault box.


Example of usage:

Simple

```YAML

     - {
         role: "sa-hashicorp-vault-ui"
       }


```

Advanced

```YAML

     - {
         role: "sa-hashicorp-vault-ui"
       }


```

Configuring UI for initial run.
-------------------------------

Here and below, vault_ refers for shorthand file from  https://github.com/Voronenko/hashi_vault_utils toolkit,
namely
```BASH
#!/bin/sh
VAULT_BASE_URL=${VAULT_ADDR-http://localhost:8200}
echo vault $1 -address=$VAULT_BASE_URL $2 $3 $4 $5 $6 $7 $8 $9
vault $1 -address=$VAULT_BASE_URL $2 $3 $4 $5 $6 $7 $8 $9
```


Upon vault initialization it is recommended to use restricted tokens with expiration

For example,

```BASH
./vault_create_token_with_policy.sh root
```

Enable for role support

```BASH
./vault_ auth-enable approle
```

Prepare policy for vault UI

```BASH
vault policy-write --address=http://localhost:8200 goldfish ./goldfish_vault_policy.hcl

where policy is:

# [mandatory]
# store goldfish run-time settings here
# goldfish hot-reloads from this endpoint every minute
path "secret/goldfish" {
  capabilities = ["read", "update"]
}


# [optional]
# to enable transit encryption, see wiki for details
path "transit/encrypt/goldfish" {
  capabilities = ["read", "update"]
}
path "transit/decrypt/goldfish" {
  capabilities = ["read", "update"]
}


# [optional]
# for goldfish to fetch certificates from PKI backend
path "pki/issue/goldfish" {
  capabilities = ["update"]
}
```

Configure secret setup for app role
```BASH
./vault_ write auth/approle/role/goldfish role_name=goldfish policies=default,goldfish secret_id_num_uses=1 secret_id_ttl=5m period=24h token_ttl=0 token_max_ttl=0
```

```BASH
./vault_ write auth/approle/role/goldfish/role-id role_id=goldfish
```

```BASH
./vault_ write secret/goldfish DefaultSecretPath="secret/" UserTransitKey="usertransit" BulletinPath="secret/bulletins/"
```

```BASH
# [optional] to enable transit encryption:
# write key ServerTransitKey="goldfish" in the runtime settings (above)
# and run the following line to initialize the key:
./vault_ write -f transit/keys/goldfish
```

Navigate to vault-ui at your_host:8201

you should be asked for initial wrapped token to be produced with command
`vault write -f -wrap-ttl=5m auth/approle/role/goldfish/secret-id`

do it.

```BASH
./vault_ write -f -wrap-ttl=5m auth/approle/role/goldfish/secret-id
```

Note value for wrapping_token and use it in the UI (note you have 5m only)

Congratulations! Your vault_ui is now properly activated


Usage with ansible galaxy workflow
----------------------------------

If you installed the `sa-hashicorp-vault-ui` role using the command


`
   ansible-galaxy install softasap.sa-hashicorp-vault-ui
`

the role will be available in the folder `library/softasap.sa-hashicorp-vault-ui`
Please adjust the path accordingly.

```YAML

     - {
         role: "softasap.sa-hashicorp-vault-ui"
       }

```



requirements.yml snippet:

```YAML
- src: softasap.sa-hashicorp-vault-ui
  name: sa-hashicorp-vault-ui
```




Copyright and license
---------------------

Code is dual licensed under the [BSD 3 clause] (https://opensource.org/licenses/BSD-3-Clause) and the [MIT License] (http://opensource.org/licenses/MIT). Choose the one that suits you best.

Reach us:

Subscribe for roles updates at [FB] (https://www.facebook.com/SoftAsap/)

Join gitter discussion channel at [Gitter](https://gitter.im/softasap)

Discover other roles at  http://www.softasap.com/roles/registry_generated.html

visit our blog at http://www.softasap.com/blog/archive.html
