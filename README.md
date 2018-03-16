# Managed Instance Group and Load Balancer on GCP with Packer and Terraform.

Scripts to create a an instance template from a Packer built image followed by the creation of a Managed Instance Group with a minimum of 3 Apache Server (no public IP) with a burst to 5 instances running. It also opens port 80 and 443 in firewall rules for default network and is fronted by a Global Load Balancer.

Scripts uses a GCP service account and a JSON file with your account token and VARS defined in variables.tf

# Prep
1) Create a service account and get the JSON file.
  * Follow step 2 and 3 from my [previous blog post](https://www.cloudops.com/2018/02/how-to-deploy-consul-in-gcp-using-terraform-your-first-step-towards-devops-automation/)
  * Make sure you have the following roles (Compute Admin, Service Account User, Storage Admin)

2) Clone this [repo](https://github.com/sveronneau/gcp-mig-lb.git)
3) Update the *apache.json* and *variables.tf* files  with your GCP account information
4) Install [Packer](https://www.packer.io) and [Terraform](https://www.terraform.io) in the gcp-mig-lb folder to make things simple.

# Steps
1) packer validate apache.json
2) packer build apache.json
3) terraform init
4) terraform plan
5) terraform apply
6) Wait or a bit (few minutes)  and open a Browser with the IP of your GCP Load Balancer
  * The IP of your GCP Load Balancer can be found in: Network Services / Load balancing
  * Click on http-lb-url-map and look in the Frontend section, protocol HTTP.  You'll see your Public IP there.
  * Open your browser http://frontend_public_ip
  * Hit refresh and you'll see that you are going randomly to your Apache servers

# What does the Terraform script do?
From the custom image, a template will be created and from that, we can create a managed instance group. This will deploy 3 identical instances based on our baked image. It will also inject metadata that will create a static web page that contains the server’s name, internal and external IP. Create a Firewall rule that will allow HTTP traffic to come to that group. This is useful if we want it all to reach our servers. Create a front-end and back-end service fronted by a load balancer and forwarding rules.

# Testing Auto-Healing
You can easily see this in action by going into (Compute Engine / Instance groups / apache-rmig ) and select one of the instance and delete it.  You'll see a new one taking its place automatically.

# Testing Auto-Scaling
The stress tool as been installed in the golden image we've baked.  To stress an instance and trigger auto-scaling, go into (Compute Engine / Instance groups / apache-rmig ) and click SSH under the Connect option of the instance of your chosing to go in. Once you are inside the instance, just run (stress -c 4) and CPU utilization will spike to 100% on that instance and will trigger auto-scaling after a minute.  When you terminate the stress tool, the scale-down process can take up to 10 minutes.

# Cleanup
1) terraform destroy
2) In Compute Engine / Images
  * Delete your custom image
