# BLOG  Post No.2

Scripts to create a an instance template from a Packer built image followed by the creation of a Managed Instance Group with a minimum of 3 Apache Server (no public IP) with a burst to 5 instances running. It also opens port 80 and 443 in firewall rules for default network and is fronted by a Global Load Balancer.

Scripts uses a GCP service account and a JSON file with your account token and VARS defined in variables.tf

# Prep
1) Create a service account and get the JSON file.
  * Follow step 2 and 3 from my [previous blog post](https://www.cloudops.com/2018/02/how-to-deploy-consul-in-gcp-using-terraform-your-first-step-towards-devops-automation/)
2) Clone this [repo](https://github.com/sveronneau/gcp-mig-lb.git)

# Steps
1) Install [Packer](https://www.packer.io) and [Terraform](https://www.terraform.io)
3) Update the *apache.json* and *variables.tf* files  with your personal information
4) packer validate apache.json
5) packer build apache.json
6) terraform init
7) terraform plan -out gcp-mig-lb.out
8) terraform apply
9) Wait or a bit (few minutes)  and open a Browser with the IP of your GCP Load Balancer
  * The IP of your GCP Load Balancer can be found in: Network Services / Load balancing
  * Click on http-lb-url-map and look in the Frontend section, protocol HTTP.  You'll see your Public IP there.
10) Open your browser http://frontend_public_ip
  * Hit refresh and you'll see that you are going randomly to your Apache servers

# Cleanup
1) terraform destroy
2) In Compute Engine / Images
  * Delete your custom image
