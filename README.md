# This repository contains code and instructions for setting up an infrastructure on Amazon Web Services (AWS) that includes the following components:
Creation of three EC2 instances with S3 full access.
Creation of an EFS (Elastic File System) and manual attachment to all EC2 instances.
Testing file visibility across all EC2 instances.
Create a S3 bucket.
Uploading a file to an S3 bucket using a shell script.
Setting up a cronjob on all instances to synchronize data with the S3 bucket.

# Prerequisites
Before running the code, make sure you have the following prerequisites:

* An AWS account with appropriate permissions to create EC2 instances, EFS, and S3 resources.
* AWS CLI installed and configured with access to your AWS account.
* YAML Code editor (e.g., Visual Studio Code) for editing the infrastructure-as-code file.

# Usage

Follow the steps below to set up the infrastructure and run the scripts:

Clone this repository to your local machine.

Open the "create_ec2_instances.yaml" file and update the following placeholders with your desired values:
Go into AWS CloudFormation and run the run the code and it will create EC2 instances automatically.
Once the EC2 instances are created, manually attach the EFS to each instance. To do this:

* Go to the AWS Management Console.
* Navigate to the EFS service and select "EFS(Elastic File System)".
* Select the EFS which you created and click on "Attach" 
* It will show you a command go to the EC2 instances and run the command --> exmaple:- sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-0fd4ae42b67d43e1a.efs.us-east-1.amazonaws.com:/ efs

Once the EFS is attached to the EC2 instances, you can test file visibility. SSH into one of the EC2 instances and create a file inside the mount path (/efs). Verify that the file is visible on all EC2 instances.

Edit the upload_to_s3.sh shell script and replace <your_bucket_name> with the name of your S3 bucket.
Run the upload_to_s3.sh script to upload the file to the S3 bucket:
# ./upload_to_s3.sh
Finally, to set up a cronjob on all EC2 instances for synchronizing data with the S3 bucket, follow these steps:

SSH into each EC2 instance.
Edit the crontab file using the "crontab -e" command.
Add the following entry to the crontab file:
* * * * * aws s3 sync efs/ s3://testing.bucket.cloud --include "*"
Save the crontab file.
Now, the cronjob will run every minute on all EC2 instances, synchronizing data with the S3 bucket.
