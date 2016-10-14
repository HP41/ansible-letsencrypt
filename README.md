# letsencrypt

### First, read Let's Encrypt's TOS and EULA. Only proceed if you agree to them.

## Ansible role to Let's Encrypt signed certs.
* Can use `standalone` or `webroot` method to acquire a certificate.
* Using `standalone` method, there's an option to copy the certificate to the final location if need be.
* The [Let's Encrypt client](https://github.com/letsencrypt/letsencrypt) will place the certificate and accessories in `/etc/letsencrypt/live/<first listed domain>/`. For more info, see the [Let's Encrypt documentation](https://letsencrypt.readthedocs.org/en/latest/using.html#where-are-my-certificates).

## Requirements 
* Ansible >= 2.1
* Guest machine: Debian
    - stretch
    - jessie
    - wheezy
* Guest machine: Ubuntu
    - xenial
    - trusty
    - precise

## Possible additional tasks that are not part of this role's responsibilities.
* Opening the necessary LetsEncrypt ports - 443 through the firewall.
	- If you'd like to use [debops.ferm](https://github.com/debops/ansible-ferm) you can use/modify `letsencrypt__debops_ferm_dependent_rules` (defined in defaults) to pass through to [debops.ferm](https://github.com/debops/ansible-ferm).

## Default Variables that can be overridden or used as-is when using this role::
* `letsencrypt_webroot_path`: The root path that gets served by your web server. Defaults to `/var/www`.
* `letsencrypt_email`: Email address to be registered with LetsEncrypt - Default=webmaster@{{ansible_domain}}.
* `letsencrypt_rsa_key_size`: Allows to specify a size for the generated key - Default=None
* `letsencrypt_cert_domains`: A *list* of domains you wish to get a certificate for - Default={{ansible_fqdn}}.
* `letsencrypt_server`: Sets the auth server - Default=None
* `letsencrypt_renewal_command_args`: Add arguments to the `letsencrypt renewal` command that gets run using cron.  For example, use the renewal hooks to restart a web server. Default=""
* `letsencrypt_authenticator`: `webroot` or `standalone` - Default=standalone
* `letsencrypt_standalone_copy_certificate`: When standalone, setting this to `True` will copy over the certificate/key files to a location of your choice. Default=False
* `letsencrypt_expand`: Add on `expand` to the command line argument so as to ensure the certificate expands to any other subdomains that's already being taken care of. Default=True
* `letsencrypt_test_certs`: Set to `True` to grab certificates from LetEncrypt staging certs. Default=True
* `letsencrypt_renewal_frequency`: Set the cron job timing.
* `letsencrypt_renewal_dict: `: Dictionary of information that'll be set in the renwal ini file.
* `letsencrypt_services_to_shutdown`: *list* of service names that needs to be shutdown before creation of certificates/keys and then started up - Default=None
* `letsencrypt_standalone_public_key_copy_locations_and_filename`: A **list** of locations to copy the **public** key to - Default=None
* `letsencrypt_standalone_private_key_copy_locations_and_filename`: A **list** of locations to copy the **private** key to - Default=None
* `letsencrypt_standalone_chain_key_copy_locations_and_filename`: A **list** of locations to copy the **chain** key to. Default - Default=None
* `letsencrypt_standalone_full_chain_key_copy_locations_and_filename`: A **list** of locations to copy the **full chain** key to - Default=None
* `letsencrypt__debops_ferm_dependent_rules`: Default simple rules to open up port 443 through firewall that can be referenced when using [debops.ferm](https://github.com/debops/ansible-ferm) role

## Testing
* I've tested this on Debian Jessie and Stretch boxes as well as Ubuntu trusty and xenial boxes using standlone method. 
* I've yet to check if renewal works as expected. 
* Please do try other methods and mention what works and what doesn't.