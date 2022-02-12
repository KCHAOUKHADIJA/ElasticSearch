# Configure Kibana to connect to Elasticsearch with a password
#### Add the elasticsearch.username setting 
          sudo nano /etc/kibana/kibana.yml
          elasticsearch.username: "kibana_system"
#### Create the Kibana keystore:
          cd /usr/share/kibana
          sudo ./bin/kibana-keystore create
#### Add the password for the kibana_system user to the Kibana keystore:
          sudo ./bin/kibana-keystore add elasticsearch.password
          When prompted, enter the password for the kibana_system user.
#### Restart Kibana
          sudo systemctl restart kibana
          go to http://localhost:5601

