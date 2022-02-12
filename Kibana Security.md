# Configure Kibana to connect to Elasticsearch with a password
#### Add the elasticsearch.username setting 
          sudo nano /etc/kibana/kibana.yml
          elasticsearch.username: "kibana"
          
#### Create the Kibana keystore:
          cd /usr/share/kibana
          sudo ./bin/kibana-keystore create
          
#### Add the password for the kibana_system user to the Kibana keystore:
          sudo ./bin/kibana-keystore add elasticsearch.password
          enter the password for the kibana_system user.
          
#### Restart Kibana
          sudo systemctl restart kibana

# Encrypt HTTP client communications for Kibana
#### Encrypt traffic between Kibana and Elasticsearch
###### When you ran the elasticsearch-certutil tool with the http option, it created a /kibana directory containing an elasticsearch-ca.pem file. 
#### Copy the elasticsearch-ca.pem file to the Kibana configuration directory 
          /etc/kibana
          
#### Specify the location of the security certificate for the HTTP layer
          sudo nano /etc/kibana/kibana.yml
          elasticsearch.ssl.certificateAuthorities: ["/etc/kibana/elasticsearch-ca.pem"]
          
#### Specify the HTTPS URL for your Elasticsearch cluster
          elasticsearch.hosts: ["https://localhost:9200"]
          
#### Final kibana.yml file
          server.host: 0.0.0.0
          elasticsearch.hosts: ["https://localhost:9200"]
          elasticsearch.username: "kibana"
          elasticsearch.password: "ur password"
          elasticsearch.ssl.certificateAuthorities: ["/etc/kibana/elasticsearch-ca.pem"]

#### Restart kibana
          sudo systemctl restart kibana
          go to localhost:5601
          login as elastic user


