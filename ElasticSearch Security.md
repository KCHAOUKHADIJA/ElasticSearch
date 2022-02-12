# Basic security setup
## I. Generate the Certificate Authority
#### Configuring TLS between nodes is the basic security setup to prevent unauthorized nodes from accessing to your cluster

### 1. Use the elasticsearch-certutil tool to generate a CA for your cluster > Certificate Authority
> this can be done in any node

     cd /usr/share/elasticsearch
     sudo ./bin/elasticsearch-certutil ca
     Accept the default file name, which is elastic-stack-ca.p12. 
     Enter a password for your CA.
### 2. Generate a certificate and private key for the nodes in your cluster
    sudo ./bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12
    Enter the password for your CA
    Create a password for the certificate and accept the default file name.
> The output file is a keystore named elastic-certificates.p12. This file contains a node certificate, node key, and CA certificate.
###### --ca <ca_file>
    Name of the CA file used to sign your certificates. 
    The default file name from the elasticsearch-certutil tool is elastic-stack-ca.p12.

### 3. On every node in your cluster, copy the elastic-certificates.p12 file to the $ES_PATH_CONF directory.
     sudo su : change to the root user
     cp /usr/share/elasticsearch/elastic-certificates.p12 /etc/elasticsearch
     sudo -i -u user


## II. Encrypt internode communications with TLS
#### Now that you’ve generated a certificate authority and certificates, you’ll update your cluster to use these files.

>Complete the following steps for each node in your cluster. 

### 1. Open the etc/elasticsearch/elasticsearch.yml file and make the following changes:
    sudo nano /etc/elasticsearch/elasticsearch.yml
    
    cluster.name: my-cluster
    node.name: node-1
    xpack.security.transport.ssl.enabled: true
    xpack.security.transport.ssl.verification_mode: certificate 
    xpack.security.transport.ssl.client_authentication: required
    xpack.security.transport.ssl.keystore.path: elastic-certificates.p12
    xpack.security.transport.ssl.truststore.path: elastic-certificates.p12
    
### 2. If you entered a password when creating the node certificate, run the following commands to store the password in the Elasticsearch keystore:
    sudo ./bin/elasticsearch-keystore add xpack.security.transport.ssl.keystore.secure_password
    sudo ./bin/elasticsearch-keystore add xpack.security.transport.ssl.truststore.secure_password
    
### 3. Complete the previous steps for each node in your cluster

### 4. On every node in your cluster, start Elasticsearch
     in case of permission error , run :
     sudo chmod -R 777 /etc/elasticsearch/elastic-certificates.p12
     then restart elasticsearch

#### In addition to configuring TLS on the transport interface of your Elasticsearch cluster, you configure TLS on the HTTP interface for both Elasticsearch and Kibana.

# Set up basic security for the Elastic Stack plus secured HTTPS traffic

## III. Encrypt HTTP client communications for Elasticsearch
### 1. On every node in your cluster, stop Elasticsearch and Kibana if they are running.

### 2. On any single node, run the Elasticsearch HTTP certificate tool to generate a Certificate Signing Request (CSR)
     cd /usr/share/elasticsearch
     sudo ./bin/elasticsearch-certutil http
     
> This command generates a .zip file that contains certificates and keys to use with Elasticsearch and Kibana.

      When asked if you want to generate a CSR, enter n.
      When asked if you want to use an existing CA, enter y.
      Enter the path to your CA.  /usr/share/elasticsearch/elastic-stack-ca.p12 
      Enter the password for your CA.
      Enter an expiration value for your certificate.
      When asked if you want to generate one certificate per node, enter y.

      Each certificate will have its own private key, and will be issued for a specific hostname or IP address.

      When prompted, enter the name of the first node in your cluster. Use the same node name that you used when generating node certificates.
      Enter all hostnames used to connect to your first node. These hostnames will be added as DNS names in the Subject Alternative Name (SAN) field in your certificate.

      List every hostname and variant used to connect to your cluster over HTTPS.

      Enter the IP addresses that clients can use to connect to your node.
      Repeat these steps for each additional node in your cluster.
      
### 3. After generating a certificate for each of your nodes, enter a password for your private key when prompted.

### 4. Unzip the generated elasticsearch-ssl-http.zip file. This compressed file contains one directory for both Elasticsearch and Kibana.
      /elasticsearch
      |_ README.txt
      |_ http.p12
      |_ sample-elasticsearch.yml 
      
      /kibana
      |_ README.txt
      |_ elasticsearch-ca.pem
      |_ sample-kibana.yml
      
### 5. On every node in your cluster, complete the following steps:
      Copy the relevant http.p12 certificate to the /etc/elasticsearch
  #### Edit the elasticsearch.yml file to enable HTTPS security and specify the location of the http.p12 security certificate.
      xpack.security.http.ssl.enabled: true
      xpack.security.http.ssl.keystore.path: http.p12
    
  #### Add the password for your private key to the secure settings in Elasticsearch.
      sudo ./bin/elasticsearch-keystore add xpack.security.http.ssl.keystore.secure_password

  #### Start Elasticsearch.
  
## Set the password for the ElasticSearch Built-in users
    sudo nano /etc/elasticsearch/elasticsearch.yml 
    add this :  xpack.security.enabled: true
    sudo systemctl restart elasticsearch
    /usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto

> copy them in a secure place

## Access elasticsearch

curl -u elastic:<ur password> https://localhost:9200 -k






