## Run ansible playbooks <a name="markdown-header-ansible-playbooks"></a>

### Deploy configuration to OID

To deploy configuration to the OID you should use *play_oid_config.yml* ansible script.  

1. Go to the *~/oam_ansible/idm-basic-configuration/oid* and edit *play_oam_config.yml*
2. Choose which roles you want to use and uncomment them. Other roles which shouldn't be used during deployment need to be commented.
3. Define on which environment you want to deploy the configuration. It is defined in "-i" parameter:
   - DEV environment: *environments/dev/hosts*
   - TEST environment: *environments/test/hosts*
   - ABN environment: *environments/abn/hosts*
   - PROD environment: *environments/prod/hosts*
4. Define which *vault.yml / vault_secret.sh* files you want to use. It is defined in "--vault-id" parameter:
   - DEV environment: *--vault-id dev@environments/dev/inventories/vault_secret.sh --vault-id dev-oauth@environments/dev/inventories/vault_oauth_secret.sh*
   - TEST environment: *--vault-id test@environments/test/inventories/vault_secret.sh --vault-id test-oauth@environments/test/inventories/vault_oauth_secret.sh*
   - ABN environment: *--vault-id abn@environments/abn/inventories/vault_secret.sh --vault-id abn-oauth@environments/abn/inventories/vault_oauth_secret.sh*
   - PROD environment: *--vault-id prod@environments/prod/inventories/vault_secret.sh --vault-id prod-oauth@environments/prod/inventories/vault_oauth_secret.sh*
5. Run ansible script as follows:
 Example for DEV:
    ```bash
    ansible-playbook play_oid_config.yml --vault-id dev@environments/dev/inventories/vault_secret.sh --vault-id deeoid@environments/dev/inventories/vault_secret.sh -i environments/dev/hosts
    ```
