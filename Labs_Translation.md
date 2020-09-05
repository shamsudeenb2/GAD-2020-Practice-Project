#Lab:Google Cloud Fundamentals: Getting Started with Compute Engine

## Objectives:

In this lab, you will learn how to perform the following tasks:

- Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

- Create a Compute Engine virtual machine using the gcloud command-line interface.

- Connect between the two instances.

##Steps

1. Create virtual machine using the GCP Console
 
 gcloud compute instances create my-vm-1 --machine-type "n1-standard -1" --image-project "debian-cloud" --image "debian-9-stretch-v201902213"
   --subnet "default" --tags http
    
	 - Create firewall rules
		     gcloud compute firewall-rules create my-firewall-rule --allow http --target-tags "http"
	
2. Create a virtual machine using the gcloud command-line.

  gcloud compute zones list | grep us-central1
  
  gcloud config set compute/zone us-central1-b
  
  gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"

3. Connect between VM instances

      - Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network
      - connect to my-vm-2
			gcloud compute ssh my-vm-2
			
	  - ping my-vm-1 from my-vm-2
	      ping -c 4 my-vm-1 
		  
	  - Use the ssh command to open a command prompt on my-vm-1 from my-vm-2:
	       ssh my-vm-1
		   
	  - At the command prompt on my-vm-1, install the Nginx web server:
	       sudo apt-get install nginx-light -y
		   
	  - Use the nano text editor to add a custom message to the home page of the web server:
	       sudo nano /var/www/html/index.nginx-debian.html
		   
	  - Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:
	       Hi from SADISU
		   
	  - Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor.
	    
	  - Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:
	        curl http://localhost/
			
	  - To exit the command prompt on my-vm-1, execute this command:
	       exit
		   
	  - You will return to the command prompt on my-vm-2
	  
	  - To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:
	        curl http://my-vm-1/
			
       The response will again be the HTML source of the web server's home page, including your line of custom text.

# End 0f Google Cloud Fundamentals: Getting Started with Compute Engine.


#Lab: Google Cloud Fundamentals: Getting Started with GKE

## Objectives:
    In this lab, you learn how to perform the following tasks:

      - Provision a Kubernetes cluster using Kubernetes Engine.

      - Deploy and manage Docker containers using kubectl.
	  
##Steps
	1. Start a Kubernetes Engine cluster.
      
	  - For convenience, place the zone in the environment variable:
	       export MY_ZONE=us-central1-a.
		   
	  - Create a Kubernate cluster 'webfrontend' and Configure it to run 2 nodes:
	     gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
		 
	  - After the cluster is created, check your installed version of Kubernetes using the kubectl version command:
	      kubectl version
		  
	  - View your running nodes:
	      gcloud container clusters describe webfronend --zone $MY_ZONE
		  
	2. Run and deploy a container:
	   
	  - From your Cloud Shell prompt, launch a single instance of the nginx container.
	     kubectl create deploy nginx --image=nginx:1.17.10
		 
	  - View the pod running the nginx container:
	      kubectl get pods
		  
	  - Expose the nginx container to the Internet:
		 kubectl expose deployment nginx --port 80 --type LoadBalancer.
		 
	  - View the new service:
	     kubectl get services
		 
	  - Open a new web browser tab and paste your cluster's external IP address into the address bar. The default home page of the Nginx browser is displayed.
	  
	  - Scale up the number of pods running on your service:
	      kubectl scale deployment nginx --replicas 3
		  
	  - Confirm that Kubernetes has updated the number of pods:
	      kubectl get pods
		  
	  - Confirm that your external IP address has not changed:
	      kubectl get services
		  
	  - Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.


## End 0f Google Cloud Fundamentals: Getting Started with GKE.

#Lab: Creating Virtual Machines


 ## Objectives:
      In this lab, you explore the available options for VMs and see the differences between locations.

      In this lab, you learn how to perform the following tasks:

       - Create several standard VMs

       - Create advanced VMs

##Steps
    1. Create a utility virtual machine.
	    
		   - Click Details to the right of the Machine type list to see the breakdown of estimated costs.
		     gcloud compute machine-type list --filter="zone:(us-central1-c)"
			 
		   - Create VM
		     gcloud compute instances create my-utilityVm-1 --machine-type "n1-standard -1" --image-project "debian-cloud" --image "debian-9-stretch-v201902213"
               --zone "us-central1-c"  --no-address
			   
		   - On the VM instances page, click on the name of your VM.
		       gcloud compute instances describe my-utilityVm-1 --zone=us-central1-c
			   
	2. Create a Windows virtual machine.
	     
      - On the Navigation menu, click Compute Engine > VM instances.
		-Creating VM
		 1. create parsistent disk:
		     gcloud compute disks create windows-disk1 --image-project "windows-cloud" --type "pd-ssd" --zone "europe-west2-a" --image-family "windows-2016-core"
			 
			 
		 2. Click Create instance.
		      gcloud compute instances create my-window-vm --machine-type "n1-standard-2" --image-project "windows-cloud" --image-family "windows-2016-core" --boot-disk-device-name "windows-disk1" --boot-disk-type "pd-ssd" --zone "europe-west2-a" --subnet "default" --tags http
			   
		 3. Create firewall rules
		     gcloud compute firewall-rules create windows-firewall-rule --allow http --target-tags "http"
		   
	     4. set the password for the VM after the VM has been created:
		      gcloud compute reset-windows-password my-window-vm --zone "europe-west2-a"
			  
			   - You will be presented with a confirmation prompt:
			       Would you like to set or reset the password for [username] (Y/n)?
				   
			   - Once confirmed, confirmation of password reset will look as follows.
				    Resetting and retrieving password for [username] on [instance-name]
                    Updated [https://www.googleapis.com/compute/v1/projects/project-name/zones/zone/instances/instance-name].
                     ip_address: ip-address
                     password:   password
                     username:   username
					 
			   - Copy the provided password, and click CLOSE.
			   
			   
	3. Create a custom virtual machine
	
        - On the Navigation menu, click Compute Engine > VM instances.	
		   - Click Create instance.
		      gcloud compute instances create my-custom-vm --custom-vm-type "n2" --custom-cpu "6" --custom-memory "32GB" --image-project "debian-cloud" --image "debian-9-stretch-v201902213" --zone "us-west1-b"
			   
	
	4. Connect via SSH to your custom VM
	   - For the custom VM you just created, click SSH
	       gcloud compute ssh my-custom-vm
		   
	   - To see information about unused and used memory and swap space on your custom VM, run the following command:
	         free
	   - To see details about the RAM installed on your VM, run the following command:
	       sudo dmidecode -t 17
		   
	   - To verify the number of processors, run the following command:
	       nproc
		
	   - To see details about the CPUs installed on your VM, run the following command:
	        lscpu
			
	   - To exit the SSH terminal, run the following command:
	         exit
			 
			 
## End 0f Creating Virtual Machines