This Role performs fowolling steps:
* fetches CSR from the host server
* sends CSR to CA to be proccesed.
* Creates server certificate using the CA
* Copies the server certificate and CA certificate to the host server
* Creates PKCS#12 container using the private key, server cert and CA cert on the host server


