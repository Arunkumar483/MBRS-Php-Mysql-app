# Meeting Room Booking System Application Deployment (Cloud Formation) 
---
## Architecture

<img src="https://user-images.githubusercontent.com/52123143/158527049-975c7a6c-ecd7-4305-a6f3-fa082b4eb6c4.png" width="50%">
## NESTED STACK

---

- 1.**Network  Substack**
    - COMPONENTS
        
        VPC
        
        INTERNET GATEWAY
        
        INTERNET GATEWAY ATTACHMENT
        
        4 PRIVATE SUBNETS
        
        2 PUBLIC SUBNETS
        
        PRIV/PUBLIC ROUTE TABLES
        
        PRIV/PUBLIC SUBNET ASSOCIATION
        
        NAT ROUTE
        
        IG ROUTE
        
        EIP 
        
        NAT GATEWAY
        
- 2.ALB Substack
    - Resources
        
        LAUNCH CONFIGURATIONS
        
        ALB SECURITY GROUP
        
        EC2 SECURITY GROUP
        
        AUTO SCALING
        
        ALB
        
        ALB LISTENERS
        
        ALB TARGET GROUP
        
- 3.RDS Substack
    - Resources
        
        RDS INSTANCE
        
        RDS SUBNET GROUP
        
        RDS SECURITY GROUP
        
    
- 4.D**eployment Stack**
    
    <aside>
    💡 we need to include all parameters in main Deployment file and the parameters in the substack will inherit the parameters in the main deployment file.
    
    </aside>
    

## User data for Web Instances

```bash
#!/bin/bash
yum update -y
yum install git -y
amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
yum install -y httpd mariadb-server
systemctl start httpd
systemctl enable httpd
usermod -a -G apache ec2-user
chown -R ec2-user:apache /var/www
chmod 2775 /var/www
cd tmp
git clone https://github.com/Arunkumar483/MBRS-Php-Mysql-app.git
cd MBRS-Php-Mysql-app
#mysql -h [mrbs-1.cad5qx6ykx9u.us-east-1.rds.amazonaws.com](http://mrbs-1.cad5qx6ykx9u.us-east-1.rds.amazonaws.com/) -P 3306 -u mrbs --password=mrbs-password <tables.my.sql
cp -r web/* /var/www/html
```

Cloud formation 

<aside>
💡 Sub - use parameters or id in  placeholders in tags

GetAtt - after creation it gives the attribute of the particular resource

Ref -  use parameters or id in tags

</aside>

- Important links
    
    ## Subnet calculator
    
    [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-cidr.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-cidr.html)
    
    ## NAT Explanation
    
    [https://aws.amazon.com/premiumsupport/knowledge-center/nat-gateway-vpc-private-subnet/](https://aws.amazon.com/premiumsupport/knowledge-center/nat-gateway-vpc-private-subnet/)
