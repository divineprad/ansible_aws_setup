# kodekloud_project_final

This project uses ansible to deploy a distributed webapp on AWS. It performs the following using ansible playbook

1) Creates RDS DB
2) Creates EC2 instances and related keys, security groups etc
3) Creates LoadBalancer above the EC2 instances
4) Restore the DB dump and deploy the code


Following is the structure.

There are 3 roles created

1) Role named aws which performs the following actions

Creates RDS instance

Clones the code from my repo (which is basically the sample code provided in kodekloud repo)

Modified the DB details in the code with the one I created above

Restores the sql file provided to the RDS instances

Creates a keypair, copies the private key to a chosen location

Creates security group for the EC2 instances, and opens port 80 to all and port 22 to the ansible server alone

Creates two ec2 instances

Add the public IP of the instances to inventory. I used dynamic inventory.



2) Role named webapp which 

Creates a folder on the EC2 instances to copy code

Deploys the code to that folder

Starts webserver on both instances


3) Role named loadbalancer which 

Creates the loadbalancer

Add the loadbalancer to the security group of EC2 instances so that they can communicate

Attach the instances to the loadbalancer


It uses AWS dynamic inventory to fetch the EC2 instance details. 


The group_vars/all contains all project variables and since it contains some sensitive information like DB pass, it has been encrypted with ansible vault. The variables present in the group_vars/all file are given below.

    # RDS Configuration Variables
    RDS_Instance_Name: # name of the RDS instance required
    RDS_Instance_Type: db.t2.micro
    RDS_DB_Name: # name of the database required
    RDS_DB_Username: #Database username
    RDS_DB_Password: # Database password

    PROJECT_ROOT: /home/ansible/kodekloud_project   # A location in ansible server where the plays are located
      


 # Ec2 sshkey, security group and instance creation variable

    ec2_key: # Name of the SSH key pair used
    ec2_ami: # The AMI ID chosen for the EC2 instances
    instance_type: t2.micro
    subnet_id:  #Subnet ID used
    vpc_id: # VPC ID used
    region: # Region used
    sec_group: # Security group of the EC2 instances
    sec_group_desc: # Description of Security Group for EC2 WebServers
    Tag_Name: Webserver
  # hostpath: /home/ansible/kodekloud_project/inventory.txt     (was not used)
  #  hoststring: "ansible_ssh_user=ec2-user ansible_ssh_private_key_file={{ PROJECT_ROOT }}/aws-private.pem"   (was not used)
    LB_Name: # Name of loadbalancer


# vars file for webapp role


    repo: https://github.com/divineprad/Sample_Web_App.git
    checkout_folder: /home/ansible/kodekloud_project/code   #Location in ansible server where code will be checked out
 

