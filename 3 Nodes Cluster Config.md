# How to Configure a 3-Node ElasticSearch Cluster

   ## 1.Configure hosts 
        sudo nano /etc/hosts
          192.168.0.1 node-1 
          192.168.0.2 node-2 
          192.168.0.3 node-3

   ## 2.Install ElasticSearch
        Download and install the public signing key :
            wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add –

        You may need to install the apt-transport-https package on Debian before proceeding:
           sudo apt-get install apt-transport-https

        Save the repository definition to /etc/apt/sources.list.d/elastic-7.x.list:
           echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list

        Install the Elasticsearch Debian package with:
          sudo apt-get update && sudo apt-get install elasticsearch

        Install Java
          sudo apt install default-jdk -y

   ## 3.Start ElasticSearch
        To configure Elasticsearch to start automatically when the system boots up, run the following commands:
            sudo /bin/systemctl daemon-reload
            sudo /bin/systemctl enable elasticsearch.service

        Elasticsearch can be started and stopped as follows:
            sudo systemctl start elasticsearch.service
            sudo systemctl stop elasticsearch.service

        curl -X GET http://localhost:9200/
        
   ## 4.Configure Ip address
        Reseau > reseau interne > ES network > Allow all
        Wired settings>identity : node1 connection
        Wired settings>IPV4> 192.168.0.1 – 24
        
   > repeat these 4 steps to all nodes
    
   ## 5.Configure yml file

        sudo nano /etc/elasticsearch/elasticsearch.yml

   ######  Configure ES node1
            cluster.name: mycluster
            node.name: "node-1"
            node.master: true
            node.data: true
            network.host: 0.0.0.0
            discovery.seed_hosts: ["192.168.0.1", "192.168.0.2","192.168.0.3"]
            cluster.initial_master_nodes: ["node-1", "node-2","node-3"]

   ######   Configure ES node2
            cluster.name: mycluster
            node.name: "node-2"
            node.master: true
            node.data: true
            network.host: 0.0.0.0
            discovery.seed_hosts: ["192.168.0.1", "192.168.0.2","192.168.0.3"]
            cluster.initial_master_nodes: ["node-1", "node-2","node-3"]

 ######     Configure ES node3
            cluster.name: mycluster
            node.name: "node-3"
            node.master: true
            node.data: true
            network.host: 0.0.0.0
            discovery.seed_hosts: ["node-1", "node-2","node-3"] 
            cluster.initial_master_nodes: ["node-1", "node-2","node-3"]

   > After the cluster forms successfully for the first time, remove the cluster.initial_master_nodes setting from each nodes' configuration. 

   ## If it doesn’t work, 
        •	Shut down all the nodes.
        •	Completely wipe each node by deleting the contents of their data folders.
        •	Configure cluster.initial_master_nodes as described above.
        •	Restart all the nodes and verify that they have formed a single cluster.
