# wazuh-security-install
wazuh-security-install step by step (wazuh indexer, wazuh server, wazuh dasboard)


curl -sO https://packages.wazuh.com/4.3/wazuh-install.sh

curl -sO https://packages.wazuh.com/4.3/config.yml

Edit ./config.yml and replace the node names and IP values with the corresponding names and IP addresses. You need to do this for all the Wazuh server, the Wazuh indexer, and the Wazuh dashboard nodes. Add as many node fields as needed.

nodes:
  # Wazuh indexer nodes
  indexer:
    - name: node-1
      ip: <indexer-node-ip>
    # - name: node-2
    #   ip: <indexer-node-ip>
    # - name: node-3
    #   ip: <indexer-node-ip>

  # Wazuh server nodes
  # Use node_type only with more than one Wazuh manager
  server:
    - name: wazuh-1
      ip: <wazuh-manager-ip>
    # node_type: master
    # - name: wazuh-2
    #   ip: <wazuh-manager-ip>
    # node_type: worker

  # Wazuh dashboard node
  dashboard:
    - name: dashboard
      ip: <dashboard-node-ip>
      
      

# bash wazuh-install.sh --generate-config-files

Copy the wazuh-install-files.tar file to all the servers of the distributed deployment, including the Wazuh server, the Wazuh indexer, and the Wazuh dashboard nodes. This can be done by using, for example, scp.


#bash wazuh-install.sh --wazuh-indexer node-1

#bash wazuh-install.sh --start-cluster





curl -sO https://packages.wazuh.com/4.3/wazuh-certs-tool.sh

curl -sO https://packages.wazuh.com/4.3/config.yml



#bash ./wazuh-certs-tool.sh -A


tar -cvf ./wazuh-certificates.tar -C ./wazuh-certificates/ .
rm -rf ./wazuh-certificates




NODE_NAME=<indexer-node-name>




mkdir /etc/wazuh-indexer/certs
tar -xf ./wazuh-certificates.tar -C /etc/wazuh-indexer/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./admin.pem ./admin-key.pem ./root-ca.pem
mv -n /etc/wazuh-indexer/certs/$NODE_NAME.pem /etc/wazuh-indexer/certs/indexer.pem
mv -n /etc/wazuh-indexer/certs/$NODE_NAME-key.pem /etc/wazuh-indexer/certs/indexer-key.pem
chmod 500 /etc/wazuh-indexer/certs
chmod 400 /etc/wazuh-indexer/certs/*
chown -R wazuh-indexer:wazuh-indexer /etc/wazuh-indexer/certs





systemctl daemon-reload
systemctl enable wazuh-indexer
systemctl start wazuh-indexer



curl -k -u admin:admin https://<WAZUH_INDEXER_IP>:9200


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






