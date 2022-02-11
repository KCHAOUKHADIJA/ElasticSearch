# How to install kibana 

 ## 1. Install the Kibana Debian package :
  
    sudo apt-get update && sudo apt-get install kibana

 ## 2. Configure Kibana to start automatically when the system boots up, run the following commands:
  
    sudo /bin/systemctl daemon-reload
    sudo /bin/systemctl enable kibana.service

 ## 3. Kibana can be started and stopped as follows:
  
    sudo systemctl start kibana.service
    sudo systemctl stop kibana.service
  
 ## 4. Configure Kibana via the config file
  
    sudo nano /etc/kibana/kibana.yml 
  
      server.port: 5601
      server.host: 0.0.0.0
      elasticsearch.url: "http://localhost:9200"
      


