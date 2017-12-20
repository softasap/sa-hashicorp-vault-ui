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





