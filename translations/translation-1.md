**Getting Started with Compute Engine** 


*Create a virtual machine using the GCP Console*

* set compute zone - gcloud config set compute/zone us-central1-a
* create vm instance - gcloud compute instances create "my-vm-1" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"
* allow http for the vm - gcloud compute instances 
add-tags my-vm-1 --tags http-server  

*Create a virtual machine using the gcloud command line*  

* display list of zones - gcloud compute zones list | grep us-central1
* set zone to us-central1-b - gcloud config set compute/zone us-central1-b
* create vm instance - gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"
* exit from shell - exit

*Connect between VM instances*

* ping my-vm-1 from my-vm-2 - ssh my-vm-2 = ping my-vm-1
* install ngnix server on my-vm-1 - sudo apt-get install nginx-light -y
* add message to homepage of server - sudo nano /var/www/html/index.nginx-debian.html
* Confirm that the server is serving the page - curl http://localhost/ 
* confirm that my-vm-2 can reach the web server on my-vm-1 - curl http://my-vm-1/