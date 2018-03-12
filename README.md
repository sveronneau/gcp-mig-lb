# BLOG  Post No.2

Scripts to create a an instance template from a Packer built image followed by the creation of a Managed Instance Group with a minimum of 3 Apache Server (no public IP) with a burst to 5 instances running. It also opens port 80 and 443 in firewall rules for default network and is fronted by a Global Load Balancer.

Scripts uses a GCP service account and a JSON file with your account token and VARS defined in variables.tf

Once all is setup, hit the Load Balancer Public IP

# Steps
1) Install [Packer](https://www.packer.io) and [Terraform](https://www.terraform.io)
2) Clone this [repo](https://github.com/sveronneau/gcp-mig-lb.git)
3) Update the *apache.json* and *variables.tf* files  with your personal information
4) packer validate apache.json
5) packer build apache.json
6) terraform init
7) terraform plan -out gcp-mig-lb.out
8) terraform apply
9) Open a Browser with the IP of your GCP Load Balancer
  * The IP of your GCP Load Balancer can be found in: Network Services / Load balancing /  
