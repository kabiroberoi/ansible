### Deploy Thrid Party KeyPair and Associate with Identity Domains.

OAuth aids in the security of service access. OAM offers an API-based method for setting OAuth Services.

Post the creation of Identity Domain, Resource and Client, OAM provides the flexibility of signing Token using the third-party Certificates.

There is minimum version of OAM Bundle Patch 12.2.1.4.210920 and later releases, for the Token signing using the third party keys.

As, part of the execution, the key pair needs to be uploaded to OAM using the OAM API:

curl --location --request POST 'https://<admin-host>:<admin-port>/oam/services/rest/ssa/api/v1/keypairadmin/keypair?password=<your_password>&aliasName=<alias_name>' \
--header 'Content-Type: application/x-pkcs12' \
--data-binary '@/<location>/<file_name>.p12'

Post uploading, associate the key pair with the identity domain, using the below API method:

curl --location -g --request PUT 'https://<admin-host>:<admin-port>/oam/services/rest/ssa/api/v1/oauthpolicyadmin/oauthidentitydomain?name=<identity_domain>' \
--header 'Authorization: Basic dGVzdDp0ZXN0=' \
--header 'Content-Type: application/json' \
--data-raw '{
    "defaultSigningKeyPair": "KeyPair1",
    "keyPairRolloverDurationInHours": "24"
}'

keyPairRolloverDurationInHours - Is the duration where primary (currently associated keypair) and newly associated keypair are present in system.
defaultSigningKeyPair - Is the name of alias name, provided while uploading the key pair.

As part of the Automation, following are the pre-requsites:
- The key pair should be created using p12 file (.p12 contains both the private and the public key)
- The p12 KeyPair to be uploaded to OAM, should be present under ~/oam_ansible/idm-basic-configuration/oam/resources/wallet
- Key pair rollover window should be defined
- Key passphrase should be updated in Vault file. There is variable configured in code key_passphrase, this needs credentials of the p12 store.

To upload the keypair and associate with identity domain use *keypair_rotation* role in *play_oam_config.yml* ansible script.

1. Go to the *~/oam_ansible/idm-basic-configuration/oam/environments/<env_name>/group_vars/oauth_config.yml* file and update the variables:
- key_rollover_duration - duration to keep the current associated keypair be active along with newly associated key pair
- keystore_name - The name of p12 file
- key_pair_name - Alias Name to be created for the p12 tobe uploaded
2. Validate the p12 file is available in *~/oam_ansible/idm-basic-configuration/oam/resources/wallet* directory
3. Have the main *play_oam_config.yml* updated and have only *play_oam_config.yml* role active and comment all other roles under *Configure OAM from ansible server*
