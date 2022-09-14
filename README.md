# wazuh indexer 1 saja 

# yang di perlukan

wazuh-indexer = 2 (node/server)

wazuh-server / wazuh-manager = 1 (node/server)

wazuh-dasboard = 1 



curl -sO https://packages.wazuh.com/4.3/wazuh-install.sh

curl -sO https://packages.wazuh.com/4.3/config.yml

root@wazuh-indexer-1:/home/agung# sudo nano config.yml

edit config.yml seperti di bawah




![image](https://github.com/agungsoboru/wazuh-security-install/blob/main/gambar%20W/Capture.PNG)




root@wazuh-indexer-1:/home/agung# bash wazuh-install.sh --generate-config-files




root@wazuh-indexer-1:/home/agung# ls

wazuh-install-files.tar  wazuh-install.sh

root@wazuh-indexer-1:/home/agung#



root@wazuh-indexer-1:/home/agung# scp wazuh-install-files.tar user@172.x.x.36:/home/agung/

root@wazuh-indexer-1:/home/agung# scp wazuh-install-files.tar user@172.x.x.100:/home/agungsurya/



root@wazuh-indexer-1:/home/agung# bash wazuh-install.sh --wazuh-indexer node-1


root@wazuh-indexer-1:/home/agung# bash wazuh-install.sh --start-cluster


root@wazuh-indexer-1:/home/agung# curl -sO https://packages.wazuh.com/4.3/wazuh-certs-tool.sh

root@wazuh-indexer-1:/home/agung# curl -sO https://packages.wazuh.com/4.3/config.yml




root@wazuh-indexer-1:/home/agung# sudo nano config.yml

samakan seperti tadi

![image](https://github.com/agungsoboru/wazuh-security-install/blob/main/gambar%20W/Capture.PNG)




root@wazuh-indexer-1:/home/agung# bash ./wazuh-certs-tool.sh -A


root@wazuh-indexer-1:/home/agung# tar -cvf ./wazuh-certificates.tar -C ./wazuh-certificates/ .

root@wazuh-indexer-1:/home/agung# rm -rf ./wazuh-certificates



root@wazuh-indexer-1:/home/agung# scp wazuh-certificates.tar user@172.x.x.36:/home/agung/

root@wazuh-indexer-1:/home/agung# scp wazuh-certificates.tar user@172.x.x.100:/home/agungsurya/



root@wazuh-indexer-1:/home/agung# NODE_NAME=node-1

root@wazuh-indexer-1:/home/agung# mkdir /etc/wazuh-indexer/certs

root@wazuh-indexer-1:/home/agung# tar -xf ./wazuh-certificates.tar -C /etc/wazuh-indexer/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./admin.pem ./admin-key.pem ./root-ca.pem

root@wazuh-indexer-1:/home/agung# mv -n /etc/wazuh-indexer/certs/$NODE_NAME.pem /etc/wazuh-indexer/certs/indexer.pem

root@wazuh-indexer-1:/home/agung# mv -n /etc/wazuh-indexer/certs/$NODE_NAME-key.pem /etc/wazuh-indexer/certs/indexer-key.pem

root@wazuh-indexer-1:/home/agung# chmod 500 /etc/wazuh-indexer/certs

root@wazuh-indexer-1:/home/agung# chmod 400 /etc/wazuh-indexer/certs/*

root@wazuh-indexer-1:/home/agung# chown -R wazuh-indexer:wazuh-indexer /etc/wazuh-indexer/certs




root@wazuh-indexer-1:/home/agung# systemctl daemon-reload

root@wazuh-indexer-1:/home/agung# systemctl enable wazuh-indexer

root@wazuh-indexer-1:/home/agung# systemctl start wazuh-indexer


cek sudah berhasil apa belum

root@wazuh-indexer-1:/home/agung# curl -k -u admin:admin https://<WAZUH_INDEXER_IP>:9200

password terdapat di wazuh-install-files.tar extrat cari wazuh-password.txt


Output
{
  "name" : "node-1",
  "cluster_name" : "wazuh-cluster",
  "cluster_uuid" : "cMeWTEWxQWeIPDaf1Wx4jw",
  "version" : {
    "number" : "7.10.2",
    "build_type" : "rpm",
    "build_hash" : "e505b10357c03ae8d26d675172402f2f2144ef0f",
    "build_date" : "2022-01-14T03:38:06.881862Z",
    "build_snapshot" : false,
    "lucene_version" : "8.10.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "The OpenSearch Project: https://opensearch.org/"
}




# wazuh indexer 2 install

copas semua file tadi di indexer 1 ke indexer 2

root@wazuh-indexer-2:/home/agung# ls

config.yml wazuh-certificates.tar  wazuh-certs-tool.sh  wazuh-install-files.tar  wazuh-install.sh

root@wazuh-indexer-2:/home/agung# sudo nano config.yml

root@wazuh-indexer-2:/home/agung# bash wazuh-install.sh --wazuh-indexer node-2

root@wazuh-indexer-2:/home/agung# bash wazuh-install.sh --start-cluster


root@wazuh-indexer-2:/home/agung# NODE_NAME=node-2


root@wazuh-indexer-2:/home/agung# mkdir /etc/wazuh-indexer/certs

root@wazuh-indexer-2:/home/agung# tar -xf ./wazuh-certificates.tar -C /etc/wazuh-indexer/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./admin.pem ./admin-key.pem ./root-ca.pem

root@wazuh-indexer-2:/home/agung# mv -n /etc/wazuh-indexer/certs/$NODE_NAME.pem /etc/wazuh-indexer/certs/indexer.pem

root@wazuh-indexer-2:/home/agung# mv -n /etc/wazuh-indexer/certs/$NODE_NAME-key.pem /etc/wazuh-indexer/certs/indexer-key.pem

root@wazuh-indexer-2:/home/agung# chmod 500 /etc/wazuh-indexer/certs

root@wazuh-indexer-2:/home/agung# chmod 400 /etc/wazuh-indexer/certs/*

root@wazuh-indexer-2:/home/agung# chown -R wazuh-indexer:wazuh-indexer /etc/wazuh-indexer/certs


root@wazuh-indexer-2:/home/agung# systemctl daemon-reload

root@wazuh-indexer-2:/home/agung# systemctl enable wazuh-indexer

root@wazuh-indexer-2:/home/agung# systemctl start wazuh-indexer

cek server indexer yg ke 2

curl -k -u admin:admin https://<WAZUH_INDEXER_IP>:9200


# install Wazuh server

copy semua file tadi di indexer 1 atau 2 ke wazuh server


root@serverpkl:/home/agungsurya# bash wazuh-install.sh --wazuh-server wazuh-1

root@serverpkl:/home/agungsurya# systemctl daemon-reload

root@serverpkl:/home/agungsurya# systemctl enable wazuh-manager

root@serverpkl:/home/agungsurya# systemctl start wazuh-manager

root@serverpkl:/home/agungsurya# curl -so /etc/filebeat/filebeat.yml https://packages.wazuh.com/4.3/tpl/wazuh/filebeat/filebeat.yml

root@serverpkl:/home/agungsurya# sudo nano /etc/filebeat/filebeat.yml


root@serverpkl:/home/agungsurya# filebeat keystore create

root@serverpkl:/home/agungsurya# echo admin | filebeat keystore add username --stdin --force

root@serverpkl:/home/agungsurya# echo admin | filebeat keystore add password --stdin --force


root@serverpkl:/home/agungsurya# curl -so /etc/filebeat/wazuh-template.json https://raw.githubusercontent.com/wazuh/wazuh/4.3/extensions/elasticsearch/7.x/wazuh-template.json

root@serverpkl:/home/agungsurya# chmod go+r /etc/filebeat/wazuh-template.json



root@serverpkl:/home/agungsurya# curl -s https://packages.wazuh.com/4.x/filebeat/wazuh-filebeat-0.2.tar.gz | tar -xvz -C /usr/share/filebeat/module


root@serverpkl:/home/agungsurya# NODE_NAME=wazuh-1

root@serverpkl:/home/agungsurya# mkdir /etc/filebeat/certs

root@serverpkl:/home/agungsurya# tar -xf ./wazuh-certificates.tar -C /etc/filebeat/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./root-ca.pem

root@serverpkl:/home/agungsurya# mv -n /etc/filebeat/certs/$NODE_NAME.pem /etc/filebeat/certs/filebeat.pem

root@serverpkl:/home/agungsurya# mv -n /etc/filebeat/certs/$NODE_NAME-key.pem /etc/filebeat/certs/filebeat-key.pem

root@serverpkl:/home/agungsurya# chmod 500 /etc/filebeat/certs

root@serverpkl:/home/agungsurya# chmod 400 /etc/filebeat/certs/*

root@serverpkl:/home/agungsurya# chown -R root:root /etc/filebeat/certs



root@serverpkl:/home/agungsurya# systemctl daemon-reload

root@serverpkl:/home/agungsurya# systemctl enable filebeat

root@serverpkl:/home/agungsurya# systemctl start filebeat

root@serverpkl:/home/agungsurya# filebeat test output


# Wazuh dashboard install

copy semua file di indexer 1 atau 2 ke server wazuh dasboard

root@serverpkl:/home/agungsurya# bash wazuh-install.sh --wazuh-dashboard dashboard


akan keluar 

INFO: --- Summary ---
INFO: You can access the web interface https://<wazuh-dashboard-ip>
    User: admin
    Password: <ADMIN_PASSWORD>
INFO: Installation finished.
  
  
 ![image](https://github.com/agungsoboru/wazuh-security-install/blob/main/gambar%20W/Capture1.PNG)






