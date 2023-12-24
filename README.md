# aws-practice-project

-----------------------------------------------------------------------------------------------------
# AUTHOR - YATIN GAMBHIR

# TITLE - AWS PRACTICE PROJECT

# DATE - 24 DECEMBER 2023

# DESC - IN THIS PROJECT I HAVE HOSTED THE APPLICATION IN EC2 INSTANCE HOSTED IN A PRIVATE SUBNET AND ACCESSED THE APPLICATION USING A ALB IN A PUBLIC SUBNET
-----------------------------------------------------------------------------------------------------

STEP 1 - LOGIN TO YOUR AWS MANAGEMENT CONSOLE

STEP 2 - CHOSE YOUR PREFFERED REGION TO HOST THE RESOURCES. MAKE SURE THAT THE REGION YOU HAVE SELECTED HAS THE DESIRED LIMIT OR THE QUOTA. DO CHECK IT BEFORE DEPLOYING THE RESOURES USING SERVICE QUOTA. I HAVE SELECTED US EAST (OHIO)

STEP 3 - SEARCH FOR VPC AND CREATE A VPC WITH 2 PUBLIC SUBNETS AND 2 PRIVATE SUBNETS IN 2 AVAILABILITY ZONES. ALSO ADD 2 NAT GATEWAY IN EACH AVAILABILITY ZONE TO CONNECT TO THE PRIVATE SUBNET AND DO NOT ATTACH S3 WITH THE ENDPOINT.
ALLOW THE 8000 PORT FOR ACCESING THE PYTHON HTTP SERVER , 22 PORT FOR SSH INTO THE SERVER

STEP 4 - AFTER CREATING THE VPC CREATE THE AUTO SCALING GROUP IN THE PRIAVTE SUBNET ONLY AND MAP IT WITH BOTH THE AVAILABILITY ZONES AND ADD THE FOLLOWING CONFIGURATIONS:
DESIRED CAPACITY - 2
MINIMUM CAPACITY - 1
MAXIMUM CAPACITY - 4
FINISH CREATING ASG, YOUR 2 EC2 INSTANCES WILL AUTOMATCICALLY UP AND RUNNING.

STEP 5 - IN THIS STEP GO TO THE INSTANCES AND CHECK WHETHER YOUR INSTANCES ARE RUNNING OR NOT AND YOU CAN CHECK THAT THERE IS NO PUBLIC IP ADDRESS ASSIGNED TO YOU INSTANCE BECAUSE THEY ARE HOSTED IN THE PRIVATE SUBNET. NOW CREATE AN INSTANCE AS A "BASTION-HOST" OR A JUMP-SERVER WHERE YOU WILL BE ABLE TO ACCESS THE INSTANCE IN YOUR PRIVATE SUBNET. HOST YOUR BASTION-HOST IN A PUBLIC SUBNET AND ALLOW THE SSH PORT, AND LOGIN INTO THER SERVER USING THE AWS KEY-PAIR LOGIN.

STEP 6 - NOW LOGIN OR SSH INTO YOUR BASTION HOST USING THE BELOW COMMAND AND FROM THIS BASTION HOST SECRUELY COPY THE .pem FILE INTO YOUR "bastion-host" INSTANCE.

1ST COMMAND TO LOGIN INTO YOUR BASTION-HOST INSTANCE:
ssh -i "<path_of_your_key-pair_login.pem>" "username_of_ec2_instance@public_ip_address"

2ND COMMAND TO SECURELY COPY THE .pem FILE INTO YOUR "bastion-host" INSTANCE:
scp -i "<path_of_your_key-pair_login.pem>" "<path_of_your_key-pair_login.pem>" "username_of_ec2_instance@public_ip_address":/home/ubuntu

AFTER SUCCUESSFULLY COPYING THE .pem FILE INTO YOUR BASTION-HOST INSTANCE. NOW LOGIN INTO YOUR ANOTHER SERVER FROM THIS JUMP-SERVER

STEP 7 - TO LOGIN INTO YOUR NEXT SERVER USE THE SSH COMMAND FROM YOUR BASTION-HOST TERMINAL AND USE THE PRIVATE IP ADDRESS OF YOUR RESPECTIVE SERVER.
ssh -i "<path_of_your_key-pair_login.pem>" "username_of_ec2_instance@private_ip_address"
NOW YOU WILL BE ABLE TO LOGIN TO YOUR SERVER FROM YOUR BASTION-SERVER. NOW DEPLOY A SIMPLE HTML WEBPAGE FOR YOUR PRACTICE PURPOSE IN THE 1ST SERVER IN PRIVATE SUBNET, AND ALLOW THE PYTHON HTTP SERVER ON PORT 8000 USING THE BELOW COMMAND.

python3 -m http.server 8000

STEP 8 - NOW YOUR PYTHON SERVER IS NOW UP AND RUNNING SO TO ACCESS THE APPLICATION CREATE A LOAD BALANCER(ALB) IN A PUBLIC SUBNET TO ACCESS THE APPLICATION AND MAKE SURE THE TARGET GROUP PORT IS 8000 AND DEPLOY THE ALB USING THE TARGET GROUP. INCLUDE BOTH THE SERVER 1 AND SERVER 2 CREATED BY THE ASG IN YOUR TARGET GROUP USING THE 8000 PORT.
ALSO ALLOW THE PORT 80 IN YOUR SECURITY GROUP INBOUND RULE FOR ACCESSING THE APPLICATION.

STEP 9 - ONCE YOUR ALB IS DEPLOYED IN THE PUBLIC SUBNET YOU CAN ACCESS THE WEBPAGE USING THE DNS NAME OF YOUR ALB.

FINALLY YOUR APPLICATION IS LIVE AND RUNNING AND CAN BE ACCESSED FROM THE PRIAVTE SUBNET.

YOU CAN ALSO REPEAT THE SAME PROCESS FOR THE SERVER 2 FROM SETP 7 TO DEPLOY THE WEBPAGE IN YOUR SERVER 2 ALSO AND YOU CAN CHECK THE TRAFFIC IS DISTRIBUTED IN BOTH THE SERVERS AND YOUR APPLICATION IS ACCESSIBLE ONCE YOU RELOAD THE WEBPAGE.

NOTE - AFTER SUCCSFULL CREATION AND ACCESSING YOUR APPLICATION, DO DELETE THE INSTANCES AND THE VPC ALONG WITH THE AUTO SCALING GROUP. FIRSTLY DELETE THE ASG THEN YOUR INSTANCES WILL AUTOMATICALLY GETS TERMINATED. DELETE THE NAT GATEWAY AND THE ALB AND THEN YOU WILL BE ABLE TO DELETE THE VPC.
