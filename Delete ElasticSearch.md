# Delete ElasticSearch from Ubuntu 18.04

    sudo apt-get remove --purge elasticsearch
    sudo rm -rf /etc/elasticsearch
    sudo rm -rf /var/lib/elasticsearch
    sudo rm /etc/apt/sources.list.d/elasticsearch-7.x.list*


## error : could not get lock /var/lib/dpkg/lock

    sudo lsof /var/lib/dpkg/lock

  **It’s possible that the commands don’t return anything, or return just one number. 
  If they do return at least one number, use the number(s) and kill the processes like this 
  (replace the <process_id> with the numbers you got from the above commands):**

    sudo kill -9 <process_id>
    
You can now safely remove the lock files using the commands below:

    sudo rm /var/lib/apt/lists/lock




> www.singularix.com/trac/blog/remove-elk-stack-ubuntu-16-04-xenial
