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
      elasticsearch.hosts: ["http://localhost:9200"]
      
      ![image](https://user-images.githubusercontent.com/83924635/162066819-050232b0-92f4-46f6-96d0-c785907cf76d.png)
      ![image](https://user-images.githubusercontent.com/83924635/162066911-1536c0d6-0413-442b-9212-451e963e2761.png)



