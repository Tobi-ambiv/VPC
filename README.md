# VPC
These shows how to create a VPC, 2 subnets which are public and private subnets, switching to the private subnet and pinging the private subnet from the public subnet(servers)
How to create aws vpc public and private subnet then ssh into them

go to services
locate networking and Content
select vpc
click on your vpc(you would see default vpc)
select create vpc
give it a name
select ipv4 CIDR manual input
use 10.0.0.0/16 in the box
select Amazon provided ipv6 CIDR block(to enable more options)
select us-east-1 region
tenancy - default
then we create
it automatically create route for us as seen in the image
no we have our VPC with Nacl, Route Table, Security group
back to vpc side menu and select subnet
select create subnet
click on the drop down on vpc and select the tobicloud_vpc(the vpc created)
input the details; subnet name,region(us-east-1a),ipv4(10.0.1.0/24) and hit create
create anoher subnet and give it a name, region(us-east-1b),ipv4(10.0.2.0/24) aand hit create

In other to make it assesible we need ec2 to launch in it with public address
so we select one from the 2 created subnet 
click on action and edit/modify
tick enable auto-assign public ipv4 address and save
so now we have public subnet 10.0.1.0/24 and private 10.0.2.0/24
now we add our internet gateway
click internet gateway,give it a name and create  
attach it to your vpc....NOTE you can only have one internet gateway per vpc
back to route to configure
create root table with name Publicroute into our vpc
select the public route and edit routes
add route insert 0.0.0.0/0 
insert our created IGW to our target
too add ipv6-add route and use ::/0 and use the created IGW on the target
and save
we  go to our subnet associations, edit and add our subnet(10.0.1.0/24) to our route
now we provision ec2 instance in our private subnet and public subnet
while creating your ec2, edit network and select your public subnet(10.0.1.0/24),create a new security group, create a pair key
create another ec2, edit network and selecxt our vpc, slect the private subnet(note autoassignip will be disaable), select the key pair
note the public ip, copy and ssh into it

>>
-go to ec2 and select your publicserver, copy the public IP
-click connect at the top and go to ssh client
-copy the example that looks like ssh -i "mynewkp" ec2-user@10.0.1.121
-paste on git & type yes
your ec2user prompt should display
exit to leave the machine
locate download directory
cat the key pair
copy the content
login to your public again
create a new file and paste the the content copied into it
save and exit
use "chmod 400 newfile"     this would give read permission to the file
make sure the file is saved on public ec2
ssh into your public subnet
and use this "ssh -i newfile ec2-user@privateIP"

>>to ping
tick public instance
select security and open security group
edit inbounds rules
add rule and select ALL ICMP-IPV4
under source type 0.0.0.0/0 (which means all)
save and ping your private ip from your public instance terminal
