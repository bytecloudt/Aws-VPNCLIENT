# Aws-VPNCLIENT
Aqui dejare todos los comandos que se vieron durante el video.

To generate the server and client certificates and keys and upload them to ACM
1. Clone the OpenVPN easy-rsa repo to your local computer and navigate to the easy-rsa/
easyrsa3 folder.
$ git clone https://github.com/OpenVPN/easy-rsa.git
$ cd easy-rsa/easyrsa3
2. Initialize a new PKI environment.
$ ./easyrsa init-pki
3. To build a new certificate authority (CA), run this command and follow the prompts.
$ ./easyrsa build-ca nopass
4. Generate the server certificate and key.
$ ./easyrsa build-server-full server nopass
5. Generate the client certificate and key.
Make sure to save the client certificate and the client private key because you will need them
when you configure the client.
$ ./easyrsa build-client-full client1.domain.tld nopass
You can optionally repeat this step for each client (end user) that requires a client certificate and
key.
6. Copy the server certificate and key and the client certificate and key to a custom folder and then
navigate into the custom folder.
Before you copy the certificates and keys, create the custom folder by using the mkdir
command. The following example creates a custom folder in your home directory.
$ mkdir ~/custom_folder/
$ cp pki/ca.crt ~/custom_folder/
$ cp pki/issued/server.crt ~/custom_folder/
7
AWS Client VPN Administrator Guide
Mutual authentication
$ cp pki/private/server.key ~/custom_folder/
$ cp pki/issued/client1.domain.tld.crt ~/custom_folder
$ cp pki/private/client1.domain.tld.key ~/custom_folder/
$ cd ~/custom_folder/
7. Upload the server certificate and key and the client certificate and key to ACM. Be sure to
upload them in the same Region in which you intend to create the Client VPN endpoint. The
following commands use the AWS CLI to upload the certificates. To upload the certificates using
the ACM console instead, see Import a certificate in the AWS Certificate Manager User Guide.
$ aws acm import-certificate --certificate fileb://server.crt --private-key
 fileb://server.key --certificate-chain fileb://ca.crt
$ aws acm import-certificate --certificate fileb://client1.domain.tld.crt --
private-key fileb://client1.domain.tld.key --certificate-chain fileb://ca.crt
