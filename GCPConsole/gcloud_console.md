# gcp_console
Modules related to console of gcloud

1. To find active accounts
- gcloud auth list
2. To run the active account
- gcloud config set account `ACCOUNT`
3. To list the project id
- gcloud config list project
4. Understand Compute Engine Zone and Region in detail
- https://docs.cloud.google.com/compute/docs/instances
- https://docs.cloud.google.com/compute/docs/regions-zones
- gcloud compute regions list
- gcloud compute regions describe <Provide Region Name>
- gcloud compute zones list
- gcloud compute machine-types list --filter="Give name of machine"
5. Region is specific location with one or more Zones
Example: Western Europe: europe-west1 (Region) which has Zones as below 
-- europe-west1-b
-- europe-west1-c
-- europe-west1-d
6. Remember if we want to attach an persistent disk to any VM instance both has to be in 
same zone
7. Try to set region and zone (Here we are just giving the place holder values later we will add)
- gcloud config set compute/region Region
- gcloud config set compute/zone Zone
8. To view the region and zone
- gcloud config get-value compute/region
- gcloud config get-value compute/zone
9. To view the project id
- gcloud config get-value project
10. To view the project details
- gcloud compute project-info describe --project <projectId>
or
- gcloud compute project-info describe --prject $(gcloud config get-value project)
11. To get all the help commands
- gcloud compute instances create --help
- gcloud -h
- gcloud config -help or gcloud config -h (Both has same results)
- gcloud compute instances create --help
12. To store Region, Zone, ProjectId
- export Project_Id = $(gcloud config get-value project)
- export ZONE = $(gcloud config get-value compute/zone)
- export REGION = $(gcloud config get-value compute/region)
13. To verify the varibles we can run echo command
echo -e "Project ID: $Project_Id"\nZONE: $ZONE\n REGION: $REGION"
14. How to create VM using gcloud console
    https://docs.cloud.google.com/compute/docs/machine-resource
We have different types of machines
Examples:
E2: offers e2-micro, e2-small, and e2-medium shared-core machine types with 2 vCPUs for short periods of bursting.
N1: offers f1-micro and g1-small shared-core machine types which have up to 1 vCPU available for short periods of bursting.
- gcloud compute instances create myfirstvm --machine-type e2-micro --zone $ZONE
15. To list the configuration
- gcloud config list
16. To view all the properties 
- gcloud config list --all
17. To list all the components 
- gcloud components list
18. To list instances, to list instance w.r.t name in case if we have many
- gcloud compute instance list
- gcloud compute instance list --filter="name=('myfirstvm')
19. Verify the firewall rules
- gcloud compute firewall-rules list
- gcloud compute firewall-rules list --filter="NETWORK='dev-network'"
- gcloud compute firewall-rules list --filter="NETWORK:'default' AND ALLOW:'icmp'" 
- #(Internet Control Message Protocol) 
20. Connecting to VM using gcloud
- gcloud compute ssh myfirstvm --zone $ZONE
21. Install web server example nginx
- sudo apt update && sudo apt install -y nginx
- ps auwx | grep nginx (verify NGIX is running or not)
22. To access and hit the vm will fail. As nginx web server on tcp:80
- Add tags to created vm
- Add a firewall rule 
- gcloud compute instances add-tags myfirstvm --tags https-server,http-server
23. Verify the added firewall
- gcloud compute firewall-rules list --filter=ALLOW:'80'
- ZONE=$(gcloud compute instances list --filter="name=myfirstvm" --format="value(zone)")
- IP=$(gcloud compute instances describe myfirstvm --zone=$ZONE --format="value(networkInterfaces[0].accessConfigs[0].natIP)")
  curl http://$IP
- curl http://$(gcloud compute instances list --filter=name:myfirstvm --format='value(IP)')
24. View logs
- gcloud logging logs list
- gcloud logging read "resource.type=gce_instance AND labels.instance_name='myfirstvm'" --limit 2

