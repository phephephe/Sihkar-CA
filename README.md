# Sihkar
Very small, simple and 'sure' PKI using Ansible.

This project is not aimed to be used as such in (critical) production, this is after all my hobby project. But just may be you can take at least ideas from this and make your project more secure and easier to use from PKI point of view.

## Summary

This project called Sihkar performs multiple tasks related the PKI.

Following main Features are supported:
* Creates CA to the CA host server
* Generates Keys (PKCS#8) and CSR on the host server
* Copies CSR to CA
* Gets CA to generate the host server certificate based on the CSR
* Copies the host server certificate and CA certificate to the host server
* Generates PKCS#12 container on the host server


## CA

The CA is created to a host server defined by the inventory. Inventory hosts group called 'ca' is used for this.

The CA server has encrypted directory that contains the CA's  private key.

The CA private is stored using encrypted PKCS#8 format.


## Host Server
Private key is generated locally on the host server. 
The private key is stored using encrypted PKCS#8 structure. Used password is stored on Ansible Vault. Private is not copied out of the host server.

CSR is also generated there.

## Certification



## Usage


### Usage from other project




## Vault Content

Ansible vault needs to contain following variables containing very long passwords:
* CA's private key password
  * 'vault_ca_private_key_password_'+hostvars['ca'].inventory_hostname]
  * e.g. vault_ca_private_key_password_ca
* CA's enccrypted dir password
  * 'vault_ca_dir_encryption_password_'+inventory_hostname
  * e.g. vault_ca_dir_encryption_password_ca
* Server's private key password
  * 'vault_server_pki_private_key_password_'+inventory_hostname]
  * e.g. vault_server_pki_private_key_password_tester1

## Security of the CA

If Ansible Controller is compromised then all the hosts it manages are compromised as well. Therefore, used CA does not need to be more secure than Ansible Controller. 

When the CA is located on the Ansible Controller then only that Ansible Controller can be used to manage the hosts. This can be major problem.

This project can be modified by using the inventory to perform the CA installation to localhost so that Ansible controller could be the CA server as well. (NOT TESTED YET)

The CA is not offline CA, but not traditional online CA either. The CA is aimed to be used to supply certicates to hosts managed by the Ansible.

The CA Server should only have ssh connection enabled using ssh key authentication.

Separed hardening project will be used in the future to add additional security features.

### Key Security

Both directory encryption and private key passwords are stored to Ansible Vault located on Ansible Controller. 


The Ansible Vault is encrypted and password protected. The Ansible Vault is generated using the create_vault project. The Ansible Vault password is stored to a file. It is not realistic to user to enter 64 charters long password. This password file can be GPG encrypted using the create_vault project.

Additionally, the Ansible Controller could use encrypted usb memory stick to store the used Ansible Vault and mount it only when needed.

# Security of the host server's private key

The private key is stored using PKCS#8 and PKCS#12 both using password protection on the private key. In order to use the private key the party/software using it needs to have password. Normally this password is in clear text. Access permissions to the clear text password file is important security control here.

This is very typical situation but not very secure at the same time.

## Security Summary

Ansible Controller's security is paramount to the security of the hosts. 

CA's security does not nessary need to higher than the Ansible Controller. If this assumption is valid the CA Server here could be solution for your CA needs when using Ansible to manage hosts.

If this is not secure for you then please consider using real offline root CA.