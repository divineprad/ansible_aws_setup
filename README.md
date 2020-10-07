# kodekloud_project_final

This project uses ansible to deploy a distributed webapp on AWS. It performs the following using ansible playbook

1) Creates RDS DB
2) Creates EC2 instances and related keys, security groups etc
3) Creates LoadBalancer above the EC2 instances
4) Restore the DB dump and deploy the code


The group_vars/all contains all project variables and since it contains some sensitive information like DB pass, it has been encrypted with ansible vault

It uses AWS dynamic inventory to fetch the EC2 instance details. 
