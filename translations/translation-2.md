**Getting Started with Cloud Storage and Cloud SQL**    

*Deploy a web server VM instance*

* set region - gcloud compute zones list | grep us-central1
* set zone - gcloud config set compute/zone us-central1-a
* create VM - gcloud compute instances create "bloghost" --machine-type "e2-medium"
* allow http for the vm - gcloud compute instances add-tags bloghost --tags http-server
* add startup script - metadata=startup-script=apt-get\ update$'\n'apt-get\ install\ apache2\ php\ php-mysql\ -y$'\n'service\ apache2\ restart

*Create a Cloud Storage bucket using the gsutil command line*

* set location - export LOCATION=US
* create bucket - gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID
* get image from public cloud storage - gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png
* copy new image to bucket - gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
* allow everyone to read the new image - gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
  
*Create the Cloud SQL instance*

* create sql instance - gcloud sql instances create blog-db --root-password=password --region=us-central1
* add user account -  gcloud sql users create blogdbuser --instance=blog-db --password=userPassword
* add network connection - gcloud sql network create web-front-end --authorized-network=(ip address of instance)/32

*Configure an application in a Compute Engine instance to use Cloud SQL*

* SSH to bloghost - SSH bloghost
* change directory - cd /var/www/html
* add index.php - sudo nano index.php
* paste - 
    *<html>
    <head><title>Welcome to my excellent blog</title></head>
    <body>
    <h1>Welcome to my excellent blog</h1>
    <?php
    $dbserver = "CLOUDSQLIP";
    $dbuser = "blogdbuser";
    $dbpassword = "DBPASSWORD";
    // In a production blog, we would not store the MySQL
    // password in the document root. Instead, we would store it in a
    // configuration file elsewhere on the web server VM instance.

    $conn = new mysqli($dbserver, $dbuser, $dbpassword);

    if (mysqli_connect_error()) {
            echo ("Database connection failed: " . mysqli_connect_error());
    } else {
            echo ("Database connection succeeded.");
    }
    ?>
    </body></html>*
* save and exit nano - ctrl+x enter enter
* restart server - sudo service apache2 restart
* edit index.php - sudo nano index.php
* replace CLOUDSQLIP with SQL instance public ip
* replace DBPASSWORD with password
* save and exit nano - ctrl+x enter enter
* restart server - sudo service apache2 restart

*Configure an application in a Compute Engine instance to use a Cloud Storage object*

* SSH to bloghost - SSH bloghost
* change directory - cd /var/www/html
* edit index.php - sudo nano index.php
* paste url to h1
* paste <p>'<img src=''</p> right after the pasted url and add '> after the url
* save and exit nano - ctrl+x enter enter