curl -sO https://packages.wazuh.com/4.3/wazuh-install.sh
curl -sO https://packages.wazuh.com/4.3/config.yml

root@wazuh-indexer-1:/home/agung# sudo nano config.yml

edit config.yml seperti di bawah





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







