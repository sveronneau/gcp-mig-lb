# BLOG  Post No.2

Script to create a an instance template from a Packer built image followed by the creation of a Managed Instance Group with a minimum of 3 Apache Server (no public IP) with a burst to 5 instances running. It also opens port 80 and 443 in firewall rules for default network and is fronted by a Global Load Balancer.

Scripts uses a GCP service account and a JSON file with your account token and VARS defined in variables.tf

Once all is setup, hit the Load Balancer Public IP

# Steps
1) Install [Packer]: https://www.packer.io and [Terraform]: https://www.terraform.io
2) Clone this repo
3) Update the *apache.json* and *variables.tf* files  with your personal information
4) packer apache.json
5) terraform init
6) terraform apply
