Install MSR 2.9,

docker run -it --rm \
  mirantis/dtr:2.9.16 install \
  --dtr-external-url 172.19.118.221 \
  --ucp-node brettos-vital-skink-docker-msr-0.novalocal \
  --ucp-username vital-skink \
  --ucp-password 2PJGD7fmeZ1gzvp8BT5b \
  --ucp-url 172.19.120.131 \
  --ucp-insecure-tls
  
Push an image.

Install MSR 3.X Swarm.
  
Set to Read Only,
curl -k -u allowed-tarpon:3aUc7D1Bw9VRMmo4QUBg -X POST "https:///172.19.123.172/api/v0/meta/settings" -H "accept: application/json" -H "content-type: application/json" -d "{ \"readOnlyRegistry\": true }"

Sub out --rm with --name for better tracking in docker ps -a. 

Steps using copy mode,
  
Verify

  docker container run \
-it \
--name verify \
--mount source=dtr-registry-000000000001,target=/storage \
registry.mirantis.com/msr/mmt:latest \
verify msr  \
--source-mke-url http://172.19.113.242  \
--source-username allowed-tarpon \
--source-password 3aUc7D1Bw9VRMmo4QUBg \
--source-url http://172.19.123.172 \
--storage-mode inplace \
--source-insecure-tls \
/migration

Estimate

docker run \
-it \
--name estimate \
--mount source=dtr-registry-000000000001,target=/storage \
-v /home/ubuntu:/migration:Z \
registry.mirantis.com/msr/mmt:latest \
estimate msr  \
--source-mke-url http://172.19.113.242  \
--source-username allowed-tarpon \
--source-password 3aUc7D1Bw9VRMmo4QUBg \
--source-url http://172.19.123.172 \
--storage-mode inplace \
--source-insecure-tls \
/migration

Extract

docker run \
-it \
--name extract \
--mount source=dtr-registry-000000000001,target=/storage \
-v /home/ubuntu:/migration:Z \
registry.mirantis.com/msr/mmt:latest \
extract msr \
--source-mke-url http://172.19.113.242  \
--source-username allowed-tarpon \
--source-password 3aUc7D1Bw9VRMmo4QUBg \
--source-url http://172.19.123.172 \
--storage-mode inplace \
--source-insecure-tls \
/migration


Steps using in place mode,

docker run \
-it \
-v /home/ubuntu:/migration:Z \
registry.mirantis.com/msr/mmt:latest \
estimate msr  \
--source-mke-url http://172.19.120.131  \
--source-username vital-skink \
--source-password 2PJGD7fmeZ1gzvp8BT5b \
--source-url http://172.19.118.221 \
--storage-mode inplace \
--source-insecure-tls \
/migration

Extract/Inplace Mode

docker run \
--rm -it \
-v /home/ubuntu:/migration:Z \
registry.mirantis.com/msr/mmt:latest \
extract msr \
--source-mke-url http://172.19.120.131  \
--source-username vital-skink \
--source-password 2PJGD7fmeZ1gzvp8BT5b \
--source-url http://172.19.118.221 \
--storage-mode inplace \
--source-insecure-tls \
/migration

